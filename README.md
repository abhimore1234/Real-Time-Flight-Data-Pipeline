# ✈️ Real-Time Flight Data Medallion Pipeline

### Using Apache Airflow, Docker & Snowflake

An end-to-end Data Engineering project that ingests live flight data, processes it through Medallion Architecture (Bronze, Silver, and Gold), and delivers analytical insights through Snowflake dashboards.

---

## 📌 Project Overview

This project implements an end-to-end flight data pipeline using Apache Airflow and Snowflake.

The pipeline collects live flight information in JSON format, orchestrates data ingestion and transformation using Apache Airflow, and processes data through three layers of Medallion Architecture.

The final aggregated data is stored in Snowflake, where interactive dashboards are created to visualize flight analytics and key performance indicators (KPIs).

## 🏗️ System Architecture

**Data Flow:**

Live Flight API → Apache Airflow → Bronze → Silver → Gold → Snowflake → Snowflake Dashboard

## 🛠️ Technology Stack

| Technology | Purpose |
|---|---|
| Python | Data ingestion and processing |
| Apache Airflow | Workflow orchestration |
| Docker Compose | Containerization |
| REST API | Live flight data ingestion |
| JSON | Raw data format |
| CSV | Processed data format |
| Snowflake | Cloud Data Warehouse and Analytics |
| SQL | Data transformation and aggregation |
| Git & GitHub | Version control |

## 🥉 Medallion Architecture

### 1. Bronze Layer – Raw Data

- Collects raw flight information from the live flight API.
- Stores original JSON data.
- Maintains raw data for traceability.

**Storage:** `data/bronze/`

### 2. Silver Layer – Cleansed Data

- Cleans and processes raw flight records.
- Handles missing and invalid values.
- Performs data type conversions.
- Stores structured and processed flight data.

**Storage:** `data/silver/`

### 3. Gold Layer – Aggregated Data

- Generates business-ready aggregated data.
- Calculates important flight KPIs.
- Prepares analytical data for Snowflake.
- Supports reporting and dashboard creation.

**Storage:** `data/gold/`

## ❄️ Snowflake Data Warehouse

Snowflake is used as the final data warehouse for storing processed and aggregated flight data.

**Target Table:**

`FLIGHTS.FLIGHT_TABLE`

Snowflake also provides the analytical capabilities used to build interactive dashboards and visualize flight statistics.

## 📊 Snowflake Dashboard

The dashboard provides insights into flight operations through key performance indicators.

### Key Performance Indicators (KPIs)

- **Total Flights:** Total number of flight records.
- **Total Records:** Number of records processed.
- **Average Velocity:** Average flight velocity.
- **Total On Ground:** Number of aircraft currently on the ground.
- **Active Countries:** Number of countries represented in the flight data.

### Dashboard Features

- Flight activity monitoring.
- Average flight velocity analysis.
- Country-wise flight distribution.
- Interactive visualizations.
- Aggregated flight statistics.

## ⚙️ Workflow Orchestration

Apache Airflow manages the end-to-end pipeline workflow.

Docker Compose is used to run the Airflow environment and its supporting services.

Airflow coordinates the ingestion and processing tasks, manages execution, and helps automate the pipeline.

## 📁 Project Structure

```text
Real-Time-Flight-Data-Pipeline/
│
├── ARCHITECTURE/
│   └── architecture.png
│
├── dags/
│
├── scripts/
│
├── data/
│   ├── bronze/
│   ├── silver/
│   └── gold/
│
├── logs/
│
├── .env
├── .gitignore
├── requirements.txt
├── docker-compose.yml
└── README.md
```

## 🎯 Project Objectives

- Build an end-to-end Data Engineering pipeline.
- Automate data ingestion using Apache Airflow.
- Implement Medallion Architecture.
- Process semi-structured JSON flight data.
- Integrate Snowflake as a cloud data warehouse.
- Create analytical dashboards using Snowflake.
- Gain practical experience with modern Data Engineering technologies.

## 🔐 Security

- Credentials are maintained using environment variables.
- `.env` is excluded through `.gitignore`.
- Database credentials are not committed to GitHub.
- Snowflake access follows appropriate permission practices.

## 👨‍💻 Author

**Abhishek More**

- LinkedIn: [Abhishek More](https://www.linkedin.com/in/abhimore1234/)


---

