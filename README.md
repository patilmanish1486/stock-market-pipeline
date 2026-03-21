# Stock Market Data Pipeline — Azure Data Engineering Project

![ADF](https://img.shields.io/badge/Azure%20Data%20Factory-0089D6?style=flat&logo=microsoft-azure&logoColor=white)
![Databricks](https://img.shields.io/badge/Databricks-FF3621?style=flat&logo=databricks&logoColor=white)
![PySpark](https://img.shields.io/badge/PySpark-E25A1C?style=flat&logo=apache-spark&logoColor=white)
![Status](https://img.shields.io/badge/Pipeline%20Status-Succeeded-brightgreen)

## Overview

An end-to-end cloud data pipeline built on Azure that ingests real-time
stock market data for 5 companies (AAPL, MSFT, GOOGL, AMZN, TSLA) via
the Alpha Vantage API and processes it through a 3-layer Medallion
Architecture (Bronze → Silver → Gold) using Azure Data Factory,
Databricks, and PySpark.

---

## Architecture
```
Alpha Vantage API (5 stocks)
        │
        ▼
┌─────────────────────┐
│  BRONZE LAYER       │  Azure Data Factory
│  Raw CSV ingestion  │  ForEach + Copy Activity
│  Azure Data Lake    │  5 × daily stock CSVs
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│  SILVER LAYER       │  Databricks PySpark
│  Clean + Transform  │  12 columns including
│  Parquet output     │  derived metrics
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│  GOLD LAYER         │  Databricks SQL
│  Analytics ready    │  5 analytical queries
│  Business insights  │  Parquet output
└─────────────────────┘
```

---

## Tech Stack

| Tool | Purpose |
|---|---|
| Azure Data Factory | Pipeline orchestration — ForEach + Copy activities |
| Azure Data Lake Gen2 | Layered storage (Bronze / Silver / Gold) |
| Alpha Vantage API | Real-time stock market data source |
| Databricks | PySpark transformations and SQL analytics |
| PySpark | Large-scale data cleaning and feature engineering |
| SQL | Analytical queries on Gold layer |
| Python | Pipeline scripting and automation |
| GitHub | Version control |

---

## Dataset

- Source: Alpha Vantage free API
- Stocks: AAPL, MSFT, GOOGL, AMZN, TSLA
- Records: 500 rows (100 daily records per stock)
- Columns per stock: timestamp, open, high, low, close, volume

---

## Silver Layer — Derived Columns Added

| Column | Description |
|---|---|
| daily_return_pct | % change from previous close |
| price_change | Absolute price change |
| moving_avg_7day | 7-day rolling average close price |
| daily_range | High minus low for the day |
| is_positive_day | Whether close > previous close |

---

## Key Findings (Gold Layer)

| Insight | Finding |
|---|---|
| Best avg daily return | GOOGL — 0.126% per day |
| Most volatile stock | TSLA — stddev 2.506 |
| Most consistent stock | AMZN — 52% positive trading days |
| Total records processed | 500 rows across 5 stocks |
| Pipeline layers | Bronze → Silver → Gold |

---

## Pipeline Screenshots

### ADF Pipeline Canvas
![ADF Pipeline](<img width="1918" height="821" alt="1st pipeline" src="https://github.com/user-attachments/assets/a4843c3f-9bac-4d4a-a08d-f82d3d4f8dee" />)

### ADF Monitor — Successful Run
![ADF Monitor](screenshots/adf_monitor_run.png)

### Databricks Silver Transformation
![Silver Layer](screenshots/databricks_silver_notebook.png)

### Databricks Gold Key Findings
![Gold Layer](screenshots/databricks_gold_findings.png)

### Bronze Container — 5 CSV Files
![Bronze Container](screenshots/bronze_container.png)

### Silver Container — Partitioned by Symbol
![Silver Container](screenshots/silver_container.png)

---

## Project Structure
```
stock-market-pipeline/
│
├── notebooks/
│   ├── 00_mount_datalake.ipynb       # Mount Azure Data Lake
│   ├── 01_silver_transformation.ipynb # PySpark cleaning + enrichment
│   └── 02_gold_layer.ipynb           # SQL analytics queries
│
├── output/
│   └── bronze/                        # Raw CSV files (5 stocks)
│
├── screenshots/                       # Pipeline evidence
└── requirements.txt
```

---

## How to Run
```bash
# 1. Clone the repo
git clone https://github.com/patilmanish1486/stock-market-pipeline.git

# 2. Set up Azure resources
# - Create Resource Group: rg-stock-pipeline
# - Create Storage Account with ADLS Gen2
# - Create bronze / silver / gold containers
# - Create Azure Data Factory instance

# 3. Get Alpha Vantage API key (free)
# https://www.alphavantage.co/support/#api-key

# 4. Run ADF pipeline pl_bronze_stock_ingestion
# 5. Run Databricks notebooks in order:
#    00_mount_datalake → 01_silver_transformation → 02_gold_layer
```

---

## What I Learned

- Building parameterised ADF pipelines with ForEach and Copy activities
- Ingesting real-time API data into Azure Data Lake Gen2
- PySpark transformations — window functions, lag, moving averages
- SQL analytics on Parquet data in Databricks
- Medallion Architecture pattern for data lake design
- Cost-optimised cloud pipeline design on pay-as-you-go Azure

---

*Built by Manish Patil — Final Year IT Engineering Student, SPPU, Pune*
*Connect on [LinkedIn](https://linkedin.com/in/YOUR_LINKEDIN_HERE)*
