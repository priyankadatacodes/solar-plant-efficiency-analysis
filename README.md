# Solar Plant Performance Analysis

![Python](https://img.shields.io/badge/Python-Pandas-150458?logo=python\&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-8.0%2B-4479A1?logo=mysql\&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-Analytics-orange?logo=mysql)
![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?logo=powerbi\&logoColor=black)

> This project was first published as "Solar Plant Performance & Efficiency Analysis". After a KPI audit, several KPIs were corrected or retired. This README describes the corrected version.

## Executive Summary

End to end solar plant performance analysis of a 22 inverter plant, using **Python for data preparation, MySQL for storage and validation queries, and Power BI for a single page dashboard**.

After building the first dashboard, I audited every KPI against the data. The audit showed that the energy total I had published was about 25 times too high, and that my "loss" KPI did not measure downtime. This repository contains the corrected analysis.

### Key Results

Plant 1, 15 May to 17 June 2020 (34 days):

* **Plant energy:** 5,380,274.19 kWh (about 5.38M), corrected from 132.70M kWh on the first dashboard
* **Best plant day:** 193,764 kWh on 20 May
* **AC/DC ratio:** about 9.78% for every inverter, not yet explained
* **Inverters below 95% of the fleet mean:** 2 (by average AC power)

### Business Question

> **How efficiently is the solar plant generating energy, where is energy being lost, and which inverters require attention?**

What the data can answer: how much energy the plant produced, which days were strongest, and which inverters sit below the fleet. What it cannot answer yet: where energy is lost, because my cleaning filter removed downtime rows and installed capacity is unknown.

## 1. Business Problem

Solar plant operators need reliable visibility into operational performance to understand:

* Total energy generation
* Best and weakest days
* Daily and monthly performance
* Which inverters differ from the rest of the fleet

The project converts raw inverter and weather sensor data into **operational KPIs and a management ready dashboard**, and then checks whether each KPI measures what its label claims.

## 2. Dataset

**Source:** [Solar Power Generation Data, Kaggle](https://www.kaggle.com/anikannal/solar-power-generation-data) (Plant 1 only, plant_id 4135001)
**Data type:** Time series sensor data
**Granularity:** Inverter level generation data (22 inverters) with plant level weather data, every 15 minutes, for 34 days

### Files

* **Plant_1_Generation_Data.csv:** date_time, plant_id, source_key (inverter), dc_power, ac_power, daily_yield, total_yield
* **Plant_1_Weather_Sensor_Data.csv:** irradiation, ambient_temperature, module_temperature

Neither file had null values or duplicate rows. The data has no installed capacity and no maintenance records.

## 3. Tech Stack

* **Python (Pandas, NumPy):** data cleaning, joins and derived columns
* **MySQL:** fact table, KPI queries and validation queries
* **SQLAlchemy and PyMySQL:** Python to MySQL connectivity
* **Power BI (DAX):** data model, measures and dashboard

## 4. Analytics Workflow

```text
Raw Generation and Weather Data
          ↓
Python Data Preparation
          ↓
Data Validation and Cleaning
          ↓
MySQL Storage (fact_solar_generation)
          ↓
SQL KPI Queries
          ↓
Power BI Data Model and DAX
          ↓
Dashboard
          ↓
KPI Audit and Validation Queries
          ↓
Corrected KPIs
```

## 5. Data Preparation

1. Load both CSVs, lower case the column names and convert date_time to a datetime type.
2. Check for nulls and duplicates (none found), then remove rows with negative values.
3. Join generation and weather data on timestamp and plant (68,774 rows).
4. Keep operating rows only, using DC power above 0 and AC power above 0 (36,823 rows).
5. Add derived columns: AC/DC ratio, DC minus AC, module minus ambient temperature, and DC per unit of irradiation.
6. Apply a sanity filter on the AC/DC ratio (0 to 30%), which removed nothing.
7. Export the cleaned file (36,823 rows by 16 columns) and load it into MySQL as fact_solar_generation.

***The operating row filter keeps night time zeros from distorting ratio KPIs, but it also removes every period when the plant produced nothing. That is why downtime and availability cannot be measured from this table.***

## 6. Key Performance Indicators

The dashboard shows four KPI cards:

* **Total Energy Generated (kWh):** one value per inverter day, then summed. 5,380,274.19 kWh. It is not a sum of readings.
* **Peak Plant Day Energy (kWh):** the best day for all inverters combined. 193,764 kWh on 20 May. It is not the best single inverter day.
* **Inverter AC/DC Ratio (%):** AC power divided by DC power. About 9.78. It is not panel efficiency, and it is the same for every inverter.
* **Inverters Below 95% of Fleet:** the number of inverters whose average AC power is below 95% of the fleet mean. Currently 2. It is a first screen, not a diagnosis.

### KPI Corrections After the Audit

* **Total Energy Generated:** the first version summed daily_yield over all rows (132.70M kWh). daily_yield is a running total per inverter per day, so energy was counted many times.
* **Peak Daily Energy Output:** the first version showed the highest single inverter day (9.16K kWh). It is now the best plant day.
* **Avg Panel Efficiency:** this was an inverter ratio, not panel efficiency, and a second measure had the identical formula. It is now called Inverter AC/DC Ratio.
* **System Energy Utilization Loss:** removed. It equalled 100 minus the AC/DC ratio (90.22 against 9.78), and my filter had already removed the downtime rows, so it could not measure downtime, curtailment or lost production.

### Important KPI Definition

***daily_yield is a running total per inverter per day. Do not sum it directly. Take one value per inverter day first, then sum across inverters.***

The AC/DC ratio is the same for every inverter, so it cannot rank inverters. What the DC and AC columns measure, and in which units, has not been confirmed.

## 7. Validation and Audit

I recomputed the dashboard values from the cleaned CSV, and they matched exactly. That showed the logic was wrong, not the data refresh. I then wrote validation queries in MySQL:

* Duplicate key and coverage check (22 inverters, 34 days)
* Energy per inverter day, then per plant day, using CTEs
* Reconciliation against the lifetime total_yield counter
* Inverter ranking against the fleet mean, using window functions
* Output per unit of irradiation, and the share of days each inverter sat below 95% of the fleet mean
* The AC/DC ratio split into irradiation bands with CASE
* A month comparison normalised for sunshine, and a 7 day moving average

The fix for the energy total is a two step query: collapse the running counter first, then add up the plant.

```sql
WITH inverter_day AS (
    SELECT date, inverter_id, MAX(daily_yield) AS kwh
    FROM fact_solar_generation
    GROUP BY date, inverter_id
)
SELECT date, ROUND(SUM(kwh), 0) AS plant_kwh
FROM inverter_day
GROUP BY date
ORDER BY date;
```

## 8. Power BI Dashboard

The project uses a **single page dashboard** for plant performance monitoring.

### Dashboard Coverage

* Four KPI cards (see section 6)
* Daily plant energy trend
* Average DC against AC power
* Monthly average AC power, May against June (17 days each)
* Inverter table sorted by output against the fleet mean, weakest first
* Month slicer

A few cosmetic rough edges remain. The trend chart legend lists two measures that return the same daily value, so the lines overlap, and the inverter table uses a colour scale, so the percentages are the evidence, not the colours.

### Dashboard Preview

![Solar Plant Dashboard](https://raw.githubusercontent.com/priyankadatacodes/solar-plant-efficiency-analysis/main/dashboard/solar_dashboard.png)
## 9. Key Insights

### Plant Energy

The plant produced 5,380,274.19 kWh over 34 days, with a best day of 193,764 kWh on 20 May. The first dashboard showed 132.70M kWh, about 25 times too high.

### AC/DC Ratio

The ratio is about 9.78% for all 22 inverters. That is far lower than the 96 to 99% best case inverter efficiency listed in a [solar inverter glossary](https://www.surgepv.com/glossary/solar-inverter), and it does not vary, so it cannot be used to judge inverter health until the DC and AC columns are understood.

### Monthly Comparison

Average AC power was 593.51 in May and 556.93 in June, with 17 days in each. This is a comparison, not a trend. The window is too short, and weather could explain the difference.

### Inverter Performance

Two inverters sit below 95% of the fleet mean in average AC power: bvBOhCH3iADSZry at 91.7% (527.00) and 1BY6WEcLGh8j5v7 at 93.0% (534.46). This is a first screen. I have not yet checked whether the gap persists across days.

## 10. Business Impact

The analysis shows how to:

* Report plant energy correctly from a running counter
* Compare inverters against the fleet mean
* Validate every KPI before putting it on a dashboard
* Name each KPI for what it actually measures

The project demonstrates how raw operational sensor data can be turned into trustworthy analytics through Python, SQL and Power BI, including how to find and fix mistakes in your own work.

## 11. Project Structure

```text
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

* **dashboard/:** Power BI file and dashboard screenshot
* **data/:** data files
* **notebooks/:** cleaning and preparation notebooks
* **sql/:** KPI queries and validation queries

## 12. How to Run

### 1. Clone the Repository

```bash
git clone https://github.com/priyankadatacodes/solar-plant-efficiency-analysis.git
cd solar-plant-efficiency-analysis
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Run the Analysis

Open the relevant notebooks from:

```text
notebooks/
```

### 4. Review the Queries and Dashboard

Open the SQL files from `sql/` and the dashboard assets from `dashboard/`.

## 13. Skills Demonstrated

### Data Analytics

* Energy Analytics
* Time Series Data
* KPI Definition and Validation
* Separating observation from interpretation

### Python

* Pandas
* NumPy
* Data Cleaning
* Feature Engineering

### SQL and Database

* MySQL
* CTEs
* Window Functions
* CASE Bands
* Reconciliation Queries
* SQLAlchemy and PyMySQL

### Business Intelligence

* Power BI
* DAX
* Data Modeling
* Single Page Dashboards

## 14. Limitations

* One plant and 34 days. There is no view of seasons or degradation, and a month to month difference cannot be called a trend.
* No installed capacity, so Performance Ratio could not be calculated and nothing is claimed about performance against design.
* The DC and AC scale is unresolved. The 9.78% ratio is flat across inverters and implausibly low, and the cause is not confirmed.
* Ratio figures use operating rows only, so downtime and availability are outside what they can say.
* Public data with no maintenance records. Inverters can be flagged as different from the fleet, but not explained.
* The analysis is descriptive. It shows association within one short window, not cause.

## 15. Future Improvements

* Confirm what the DC and AC columns measure, and their units
* Build availability from rows that keep zero output
* Check whether the two flagged inverters stay below the fleet across days
* Add installed capacity (kWp) to calculate Performance Ratio
* Weather adjusted benchmarking
* Anomaly detection
* Forecasting of energy generation

## 16. Author

**Priyanka Lakra**
Data Analyst | Python · SQL · Power BI · Energy Analytics

**GitHub:** [priyankadatacodes](https://github.com/priyankadatacodes)

**LinkedIn:** [priyanka-data-analytics](https://www.linkedin.com/in/priyanka-data-analytics/)

**YouTube:** [bloominpriyanka](https://www.youtube.com/@bloominpriyanka)

## License

This project is available under the repository's existing license.
