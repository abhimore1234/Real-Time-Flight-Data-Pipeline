## ENVIRONMENT VARIABLES CODE

```yaml
# ---------- POSTGRES ----------
POSTGRES_USER="abhi"
POSTGRES_PASSWORD="flight123"
POSTGRES_DB="flightdb"



# ---------- AIRFLOW ADMIN ----------
AIRFLOW_ADMIN_USER="abhi"
AIRFLOW_ADMIN_FIRSTNAME="Abhi"
AIRFLOW_ADMIN_LASTNAME="More"
AIRFLOW_ADMIN_EMAIL="abhimore475500@gmail.com"
AIRFLOW_ADMIN_PASSWORD="abhimore123456"
```

## DATA SOURCE 

### API
```yaml
https://opensky-network.org/api/states/all
```

### API DOCUMENTATION
```yaml
https://openskynetwork.github.io/opensky-api/rest.html
```
## AIRFLOW PIPELINE DAG STRUCTURE FOR BRONZE 

```python
import sys
from pathlib import Path
from datetime import datetime, timedelta
from airflow import DAG
from airflow.operators.python import PythonOperator

AIRFLOW_HOME = Path("/opt/airflow")

if str(AIRFLOW_HOME) not in sys.path:
    sys.path.insert(0, str(AIRFLOW_HOME))

from scripts.bronze_layer import run_bronze_ingestion

default_args = {
    "owner": "airflow",
    "retries": 0,
    "retry_delay" : timedelta(minutes=5),
}

with DAG(
    dag_id="flights_ops_medallion_pipe",
    default_args=default_args,
    start_date=datetime(2025, 12, 10),
    schedule_interval="*/30 * * * *",
    catchup=False,
) as dag:

    bronze = PythonOperator(
        task_id="bronze_ingest",
        python_callable=run_bronze_ingestion,
    )

```

## SQL SCRIPT FOR DATABASE, SCHEMA AND TABLE CREATION

### DATABASE
```sql
CREATE DATABASE IF NOT EXISTS FLIGHTS;
```

### SCHEMA
```sql
CREATE SCHEMA IF NOT EXISTS FLIGHTS_SCHEMA;
```

### TABLE
```sql
USE SCHEMA FLIGHTS_SCHEMA;

CREATE TABLE FLIGHT_TABLE (
    window_start TIMESTAMP,
    origin_country TEXT,
    total_flights INT,
    avg_velocity FLOAT,
    on_ground INT,
    load_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (window_start, origin_country)
);

```


## SNOWFLAKE DASHBOARD SCRIPTS
### 1. Total Flights
```sql
SELECT
    SUM(TOTAL_FLIGHTS) AS TOTAL_FLIGHTS 
FROM FLIGHTS.FLIGHTS_SCHEMA.FLIGHT_TABLE;
```
### 2. TOTAL_RECORDS
```sql
SELECT
    COUNT(*) AS TOTAL_RECORDS
FROM FLIGHTS.FLIGHTS_SCHEMA.FLIGHT_TABLE;
```
### 3. Average Velocity
```sql
SELECT
    ROUND(AVG(AVG_VELOCITY), 2) AS AVG_VELOCITY
FROM FLIGHTS.FLIGHTS_SCHEMA.FLIGHT_TABLE;
```
### 4. Active Countries
```sql
SELECT
    COUNT(DISTINCT ORIGIN_COUNTRY) AS ACTIVE_COUNTRIES
FROM FLIGHTS.FLIGHTS_SCHEMA.FLIGHT_TABLE;
```

### 5. Flight Trends
```sql
SELECT
    WINDOW_START,
    SUM(TOTAL_FLIGHTS) AS TOTAL_FLIGHTS
FROM FLIGHTS.FLIGHTS_SCHEMA.FLIGHT_TABLE
GROUP BY WINDOW_START
ORDER BY WINDOW_START;
```
### 6. Top 10 Countries by Flight Volume
```sql
SELECT
    ORIGIN_COUNTRY,
    SUM(TOTAL_FLIGHTS) AS TOTAL_FLIGHTS
FROM FLIGHTS.FLIGHTS_SCHEMA.FLIGHT_TABLE
GROUP BY ORIGIN_COUNTRY
ORDER BY TOTAL_FLIGHTS DESC
LIMIT 10;
```
### 7. Average Velocity Analysis
```sql
SELECT
    ORIGIN_COUNTRY,
    ROUND(AVG(AVG_VELOCITY), 2) AS AVG_VELOCITY
FROM FLIGHTS.FLIGHTS_SCHEMA.FLIGHT_TABLE
GROUP BY ORIGIN_COUNTRY
ORDER BY AVG_VELOCITY DESC
LIMIT 10;
```

### 8. Ground Activity Analysis
```sql

SELECT
    ORIGIN_COUNTRY,
    SUM(TOTAL_FLIGHTS) AS TOTAL_FLIGHTS
FROM FLIGHTS.FLIGHTS_SCHEMA.FLIGHT_TABLE
GROUP BY ORIGIN_COUNTRY
ORDER BY TOTAL_FLIGHTS DESC
LIMIT 10;
```


### 9.Flight Details
```sql
SELECT
    WINDOW_START,
    ORIGIN_COUNTRY,
    TOTAL_FLIGHTS,
    AVG_VELOCITY,
    ON_GROUND,
    LOAD_TIME
FROM FLIGHTS.FLIGHTS_SCHEMA.FLIGHT_TABLE
ORDER BY WINDOW_START DESC
LIMIT 100;
```
