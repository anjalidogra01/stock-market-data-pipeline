# 📈 Stock Market Data Pipeline — Medallion Architecture

A production-grade data engineering pipeline built with PySpark on Databricks, implementing the Medallion Architecture (Bronze → Silver → Gold) for 9 major Indian stocks from 2020–2025.

---

## 🏗️ Architecture

```text
yfinance API
    ↓
🥉 BRONZE LAYER
Raw data ingestion + schema enforcement + partitioning
    ↓
🥈 SILVER LAYER
Data cleaning + feature engineering + anomaly detection
    ↓
🥇 GOLD LAYER
Star schema + aggregations + advanced SQL analytics
    ↓
📊 Power BI Dashboard
```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|--------|---------|
| PySpark | Distributed data processing |
| Databricks | Cloud execution platform |
| Python | Data extraction & orchestration |
| SQL (SparkSQL) | Advanced analytics queries |
| yfinance | Stock data API |
| Power BI | Dashboard & visualization |

---

## 📦 Data

- **Stocks:** RELIANCE, TCS, INFOSYS, HDFC BANK, WIPRO, BAJAJ FINANCE, SUN PHARMA, MARUTI, ADANI
- **Period:** January 2020 — December 2025
- **Total Records:** 13,365 rows across 9 tickers

---

## 🥉 Bronze Layer

- Raw extraction from Yahoo Finance API
- Strict **StructType schema enforcement**
- Added metadata columns:
  - `Daily_Range`
  - `Mid_Price`
  - `Year`
  - `Month`
  - `Ingestion_Timestamp`
- Saved as **partitioned Parquet** (partitioned by Year & Ticker)
- Data quality validation report

---

## 🥈 Silver Layer

- Null value removal & data type validation

### Feature Engineering

- `Daily_Return_%` — daily percentage change using LAG
- `Is_Anomaly` — flagged if daily move > 3%
- `7_Day_MA` — 7 day moving average
- `30_Day_MA` — 30 day moving average
- `Volatility_30d` — 30 day rolling standard deviation

- PySpark Window functions used throughout

---

## 🥇 Gold Layer — Star Schema

```text
fact_stock_prices (13,365 rows)
│
└──── dim_ticker (9 rows)

gold_monthly_summary (648 rows)
gold_yearly_performance (54 rows)
gold_anomalies (1,129 rows)
```

---

## 📊 Advanced SQL Analytics

| Query | Concept Used |
|---------|-------------|
| Best performing stock per year | `DENSE_RANK() + PARTITION BY` |
| Month over month growth | `LAG() Window Function` |
| Top 3 anomaly days per stock | `CTE + ROW_NUMBER()` |
| Cumulative range & rolling avg | `SUM() OVER UNBOUNDED PRECEDING` |
| Stocks consistently above 30MA | `HAVING + CASE WHEN` |
| Year over year performance | `3 Chained CTEs + DENSE_RANK` |

---

## 🔍 Data Quality Validation

Validation report generated at every layer:

- Total row count
- Null row detection
- Schema column count check
- Pass/Fail status

---

## 💡 Key Engineering Decisions

- **Medallion Architecture** chosen for clear data lineage and layer separation
- **Partitioning by Year & Ticker** for query performance optimization
- **StructType schema enforcement** at ingestion to catch data issues early
- **Low cardinality columns removed** (e.g. source column) to optimize storage
- **Star Schema** in Gold layer for efficient analytical queries

---

## 📁 Project Structure

```text
stock-market-data-pipeline/
│
├── stock_pipeline.ipynb    # Main Databricks notebook
├── README.md               # Project documentation
```

---

##  How to Run

1. Create a free Databricks account at community.databricks.com
2. Import `stock_pipeline.ipynb` into your workspace
3. Run all cells top to bottom
4. All tables auto-created in Databricks metastore

---

## 👤 Author

Anjali Dogra  
Data Engineering Enthusiast
 
**LinkedIn:** https://www.linkedin.com/in/anjali-dogra-350bb1252/
