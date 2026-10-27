# Germany Electricity Generation Pipeline (Databricks)

A data pipeline built on Databricks that processes hourly German
electricity generation data (2020 to 2026).

## What I built
- **Bronze:** loaded the raw CSV into a table
- **Silver:** cleaned the data, converted German numbers, fixed timestamps, reshaped the table
- **Gold:** built summary tables (renewable share, yearly mix, hourly profile)
- **Dashboard:** charts built from the Gold tables

## Tools used
Databricks, PySpark, Delta Lake, SQL

## Dashboard
![Dashboard](dashboard.png)

## Data source
SMARD.de (Bundesnetzagentur)
