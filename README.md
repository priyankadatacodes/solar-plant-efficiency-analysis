# Solar Plant Performance & Efficiency Analysis

![Python](https://img.shields.io/badge/Python-Pandas-150458?logo=python\&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-8.0%2B-4479A1?logo=mysql\&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-Analytics-orange?logo=mysql)
![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?logo=powerbi\&logoColor=black)

## Executive Summary

End-to-end **solar plant performance analytics project** focused on energy generation, system efficiency, energy utilization loss, operational trends, and inverter performance.

The project uses **Python for data preparation, MySQL for data storage and querying, and Power BI for executive-level reporting**.

### Business Question

> **How efficiently is the solar plant generating energy, where is energy being lost, and which inverters require attention?**

---

## 1. Business Problem

Solar plant operators need reliable visibility into operational performance to understand:

* Total energy generation
* Overall system efficiency
* Energy utilization loss
* Daily and monthly performance trends
* Potential performance degradation
* Underperforming inverters

The project converts raw plant and weather data into **operational KPIs and management-ready visualizations**.

---

## 2. Dataset

**Source:** Solar Power Generation Dataset — Kaggle
**Data Type:** Time-series sensor data
**Granularity:** Plant-level operational data

### Key Variables

* DC Power
* AC Power
* Daily Energy Yield
* Irradiation
* Ambient Temperature
* Module Temperature
* Inverter ID

The dataset represents **plant-level operational data rather than customer-level data**.

---

## 3. Tech Stack

| Tool                     | Purpose                                       |
| ------------------------ | --------------------------------------------- |
| **Python**               | Data cleaning and preparation                 |
| **Pandas / NumPy**       | Data manipulation                             |
| **MySQL**                | Data storage and querying                     |
| **SQLAlchemy / PyMySQL** | Python–MySQL connectivity                     |
| **Power BI**             | Data modeling, DAX, and dashboard development |

---

## 4. Analytics Workflow

```text id="6f9h2k"
Raw Generation & Weather Data
          ↓
Python Data Preparation
          ↓
Data Validation & Cleaning
          ↓
MySQL Storage
          ↓
SQL / Data Extraction
          ↓
Power BI Data Model
          ↓
DAX KPI Calculation
          ↓
Executive Dashboard
          ↓
Operational Insights
```

The project follows a structured workflow from raw sensor data to an executive-level Power BI dashboard.

---

## 5. Data Preparation

Python was used to:

* Load generation and weather datasets
* Convert timestamps to datetime format
* Remove duplicate and invalid records
* Filter non-operational records using `DC Power > 0` and `AC Power > 0`
* Create derived analytical columns

The cleaned data was then stored in MySQL and used for Power BI analysis.

---

## 6. Key Performance Indicators

The dashboard tracks the following core metrics:

| KPI                                        | Purpose                                            |
| ------------------------------------------ | -------------------------------------------------- |
| **Total Energy Generated (kWh)**           | Measures overall energy production                 |
| **Average Panel Efficiency (%)**           | Tracks average conversion efficiency               |
| **System Energy Utilization Loss (%)**     | Measures energy lost during non-generating periods |
| **Peak Daily Energy Output (kWh)**         | Identifies highest daily production                |
| **Average Daily Specific Yield (kWh/day)** | Tracks daily generation performance                |

### Important KPI Definition

**System Energy Utilization Loss** represents downtime, curtailment, and non-generation periods.

It is **not intended to represent inverter conversion loss**.

---

## 7. Power BI Dashboard

The project uses a **single-page executive dashboard** focused on plant performance and operational monitoring.

### Dashboard Coverage

* Overall plant KPI cards
* Daily energy generation vs. system loss
* Average DC vs. AC power comparison
* Monthly average AC power trend
* Underperforming inverter analysis

The dashboard is designed to provide a concise operational view for management and business stakeholders.

### Dashboard Preview

![Solar Plant Dashboard](https://raw.githubusercontent.com/priyankadatacodes/solar-plant-efficiency-analysis/main/dashboard/solar_dashboard.png)

---

## 8. Key Insights

### Energy Loss

A significant portion of observed energy loss comes from **non-generating periods and downtime**.

### Plant Efficiency

System-level efficiency remains relatively stable across the analyzed period.

### Performance Trends

Monthly AC power trends provide a way to monitor potential **performance degradation over time**.

### Inverter Performance

Some inverters consistently perform below the overall average and can therefore be prioritized for operational investigation.

---

## 9. Business Impact

The analysis provides an operational framework to:

* Monitor solar plant performance
* Track energy generation trends
* Identify system-level energy loss
* Detect potentially underperforming inverters
* Support management-level performance monitoring

The project demonstrates how raw operational sensor data can be transformed into actionable analytics through Python, SQL, and Power BI.

---

## 10. Project Structure

```text id="4q6f8w"
solar-plant-efficiency-analysis/
│
├── dashboard/
│
├── data/
│
├── notebooks/
│
├── sql/
│
├── requirements.txt
└── README.md
```

The repository contains dedicated folders for dashboard assets, data, notebooks, and SQL analysis.

---

## 11. How to Run

### 1. Clone the Repository

```bash id="3wy5s0"
git clone https://github.com/priyankadatacodes/solar-plant-efficiency-analysis.git
cd solar-plant-efficiency-analysis
```

### 2. Install Dependencies

```bash id="5c2s4g"
pip install -r requirements.txt
```

### 3. Run the Analysis

Open the relevant notebooks from:

```text id="kz4vpc"
notebooks/
```

### 4. Review the Power BI Dashboard

Open the dashboard assets from:

```text id="b7a4v1"
dashboard/
```

---

## 12. Skills Demonstrated

### Data Analytics

* Energy Analytics
* Time-Series Analysis
* Operational Performance Analysis
* KPI Development
* Trend Analysis

### Python

* Pandas
* NumPy
* Data Cleaning
* Data Validation
* Feature Engineering

### SQL & Database

* MySQL
* SQL
* Data Storage
* Data Querying
* SQLAlchemy
* PyMySQL

### Business Intelligence

* Power BI
* DAX
* Data Modeling
* Executive Dashboards
* Operational Reporting

---

## 13. Limitations

* The analysis is based on the available Kaggle solar plant dataset.
* The dataset represents plant-level operational data.
* System Energy Utilization Loss should not be interpreted as inverter conversion loss.
* The project focuses on descriptive and operational analytics rather than predictive maintenance modeling.

---

## 14. Future Improvements

Potential extensions include:

* Automated performance monitoring
* Predictive inverter-failure analysis
* Anomaly detection
* Weather-adjusted generation benchmarking
* Automated daily/monthly reporting
* Forecasting of energy generation
* Advanced inverter-level performance modeling

---

## 15. Author

**Priyanka Lakra**
Data Analyst | Python · SQL · Power BI · Energy Analytics

**Portfolio:** [bloomindata.in](https://www.bloomindata.in/)

**GitHub:** [priyankadatacodes](https://github.com/priyankadatacodes)

---

## License

This project is available under the repository's existing license.
