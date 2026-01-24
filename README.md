# **Solar Plant Performance & Efficiency Analysis**

![Python](https://img.shields.io/badge/Library-Python-150458)
![SQL](https://img.shields.io/badge/SQL-MySQL-orange)
![Dashboard](https://img.shields.io/badge/Dashboard-PowerBI-yellow)

---

## Executive Summary

This project analyzes solar power plant generation data to understand **energy production, efficiency, and system-level energy loss**.

The main goal of this project is to practice **real-world data analyst skills** using an energy dataset.  
Python was used for data cleaning, MySQL for data storage, and Power BI for building an **executive-level dashboard**.

The dashboard provides a clear view of plant performance, trends, and underperforming inverters.

---

## Why I Built This Project

Solar plants generate a lot of operational data, but raw data alone is not useful for decision-making.

I built this project to learn and demonstrate:
- How to analyze solar plant performance data  
- How to calculate efficiency and energy loss correctly  
- How to track daily and monthly trends  
- How to identify underperforming solar inverters  

This is similar to real work done by a **solar or energy data analyst**.

---

## Business Context

Solar plant operators need quick answers to questions like:
- How much energy are we generating?
- How efficient is the plant overall?
- How much energy is being lost at the system level?
- Is plant performance degrading over time?
- Which inverters need attention?

This project converts raw sensor data into **simple KPIs and charts** that are easy for management to understand.

---

## Problem Statement

Analyze solar generation and weather data to:
- Measure total energy generation  
- Calculate system-level efficiency  
- Understand energy utilization loss  
- Track daily and monthly performance trends  
- Identify underperforming inverters  

Then present the results in a **single-page executive dashboard**.

---

## Dataset Overview

- **Source**: Solar Power Generation Dataset (Kaggle)
- **Data type**: Time-series sensor data
- **Main columns**:
  - DC power
  - AC power
  - Daily energy yield
  - Irradiation
  - Ambient temperature
  - Module temperature
  - Inverter ID

The dataset represents **plant-level operational data**, not customer-level data.

---

## Tools Used

- **Python** – Data cleaning and preparation  
- **Pandas & NumPy** – Data handling  
- **MySQL** – Data storage and querying  
- **SQLAlchemy & PyMySQL** – Python to MySQL connection  
- **Power BI** – Data modeling, DAX, and dashboard creation  

---

## Data Preparation

Using Python, I:
- Loaded generation and weather datasets  
- Converted timestamps to datetime format  
- Removed duplicates and invalid values  
- Filtered non-operational records (DC power > 0 and AC power > 0)  
- Created basic derived columns for analysis  

The cleaned data was stored in **MySQL** and used directly in Power BI.

---

## Analysis Approach

Steps followed in this project:
1. Clean and prepare data using Python  
2. Store cleaned data in MySQL  
3. Connect Power BI to the database  
4. Create DAX measures for KPIs  
5. Build a single-page executive dashboard  

---

## Key KPIs Tracked

- **Total Energy Generated (kWh)**  
- **Avg Panel Efficiency (%)**  
- **System Energy Utilization Loss (%)**  
- **Peak Daily Energy Output (kWh)**  
- **Avg Daily Specific Yield (kWh/day)**  

> Note: System Energy Utilization Loss is intentionally high.  
> It represents downtime, curtailment, and non-generation periods, not inverter conversion loss.

---

# **Dashboard Overview**
The Power BI dashboard is designed as a single-page executive view.
*Included Visuals*
KPI cards for overall performance
Daily energy generation vs system loss trend
Average DC vs AC power output comparison
Monthly average AC power trend (degradation analysis)
Underperforming solar inverters table
The dashboard is simple, clear, and focused on decision-making.

---
# **Key Insights**
A large portion of energy loss comes from non-generating periods and downtime
System-level efficiency remains relatively stable
Monthly AC power trends help identify degradation patterns
Some inverters consistently perform below average

---
# **Final Takeaway**
Solar plant performance cannot be judged using raw data alone.
This project shows how data analysis can turn operational data into useful insights for business and operations teams.

---
# **What This Project Shows**

Data cleaning using Python
SQL-based data handling using MySQL
Correct KPI calculation for energy analytics 
Executive-level dashboard creation using Power BI

---
# *Author*

Priyanka Lakra
Data Analyst (Fresher)
Python | SQL | Power BI | Energy Analytics
