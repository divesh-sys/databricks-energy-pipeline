<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=0,2,2,5,30&height=220&section=header&text=Germany%20Electricity%20Pipeline&fontSize=42&fontColor=ffffff&animation=fadeIn&desc=Databricks%20%7C%20PySpark%20%7C%20Delta%20Lake&descAlignY=68" width="100%" alt="Germany Electricity Pipeline banner" />
</p>

<h3 align="center">⚡ From raw SMARD data to a renewable-energy dashboard ⚡</h3>

<p align="center">
  An end-to-end data pipeline on <b>Databricks</b> that turns six years of hourly German
  power generation data into clear, business-ready insights.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Databricks-FF3621?style=for-the-badge&logo=databricks&logoColor=white" />
  <img src="https://img.shields.io/badge/PySpark-E25A1C?style=for-the-badge&logo=apachespark&logoColor=white" />
  <img src="https://img.shields.io/badge/Delta%20Lake-00ADD4?style=for-the-badge&logo=delta&logoColor=white" />
  <img src="https://img.shields.io/badge/SQL-336791?style=for-the-badge&logo=postgresql&logoColor=white" />
  <img src="https://img.shields.io/badge/Data-Germany%20%F0%9F%87%A9%F0%9F%87%AA-black?style=for-the-badge" />
</p>

---

## 📌 Project overview

Germany is changing how it produces electricity: more wind and solar, no nuclear, less coal.
This project answers questions such as:

- 🌱 How fast is the **renewable share** growing?
- ⚛️ How did the **generation mix** change after the nuclear phase-out?
- ☀️ At what time of day do **solar and wind** produce the most?

| | |
|---|---|
| **Data period** | Oct 2020 to Oct 2026 |
| **Resolution** | Hourly |
| **Energy sources** | 12 (wind, solar, biomass, hydro, nuclear, lignite, hard coal, gas, and more) |
| **Size** | about 53,000 hours, about 637,000 rows after reshaping |

---

## 🏗️ Architecture

```mermaid
flowchart LR
    A[📄 SMARD CSV] --> B[📦 Volume]
    B --> C[🥉 Bronze<br/>raw table]
    C --> D[🥈 Silver<br/>cleaned data]
    D --> E[🥇 Gold<br/>aggregates]
    E --> F[📊 Dashboard]
```

| Layer | What happens |
|---|---|
| 🥉 **Bronze** | Raw CSV loaded as a Delta table, column names cleaned, metadata added |
| 🥈 **Silver** | German numbers converted, timestamps converted to UTC, wide-to-long unpivot, energy category added, quality checks, MERGE upsert |
| 🥇 **Gold** | Monthly renewable share, yearly generation mix, hourly solar and wind profile, daily generation |

---

## 📊 Dashboard

[![Dashboard preview](dashboard.png)](dashboard.pdf)

*Click the image to open the full PDF.*

---

## 🔍 Key findings

- 🌱 **Renewables supplied 64.8% of German generation in September 2026**, and the monthly share shows a clear upward trend from 2021 to 2026, with peaks in summer.
- ⚡ **Renewable generation in 2025 totaled 257.7k GWh.**
- 🌬️ **Onshore wind is the largest single source (2021-2025)**, followed by lignite (braunkohle) and solar (photovoltaik).
- ☀️ **Solar peaks around midday** (about 11:00 to 14:00) and is near zero at night, while onshore wind output stays steady through the day.
- ⚛️ **Nuclear generation ended after the 2023 phase-out.**
- 🏭 **Coal generation declined from 2021 to 2025** for both lignite and hard coal.
- 📉 **Total yearly generation fell from 2021 to 2025**, as shown in the yearly mix chart.
- 🔄 **Windy days tend to have lower solar output.** This is likely a seasonal effect (windy winter days have little sun), not a direct cause.
- 🏆 **The top 20 days by renewable share all reach about 90% or more**, mostly in recent years.

---

## 🧠 Design decisions

| Decision | Why |
|---|---|
| Keep raw data in Bronze | Silver and Gold can be rebuilt without re-downloading |
| Store time in UTC | Avoids daylight-saving problems (missing and repeated hours) |
| Use MERGE in Silver | Re-running the notebook never creates duplicates (idempotent) |
| Run quality checks | Fails early on empty data, missing sources, or too many nulls |
| Exclude pumped storage from shares | Avoids double counting energy |

**Data quality note:** all null values were in nuclear (`kernenergie`) after the 2023 phase-out, so they were treated as no generation.

---

## 🛠️ Tech stack

- **Platform:** Databricks (Free Edition), Unity Catalog Volumes
- **Processing:** PySpark
- **Storage:** Delta Lake
- **Analytics:** Databricks SQL dashboards

---

## 📁 Repository structure

```
databricks-energy-pipeline/
├── 01_setup.py
├── 02_bronze_ingest.py
├── 03_silver_transform.py
├── 04_gold_aggregate.py
├── dashboard.png
├── dashboard.pdf
└── README.md
```

---

## ▶️ How to reproduce

1. Download hourly generation data for Germany from [smard.de](https://www.smard.de) (Downloadcenter)
2. Upload it to a Databricks Volume as `DE_generation.csv`
3. Run the notebooks in order: `01` to `04`
4. Build the dashboard from the Gold tables

---

## 📚 What I learned

- Building a medallion architecture (Bronze, Silver, Gold)
- Cleaning data with PySpark (number formats, timestamps, unpivot)
- Writing idempotent Silver loads with Delta Lake MERGE
- Turning Gold tables into a dashboard

---

## 🚀 Next steps

- Automate the pipeline with a Databricks Workflow
- Add weather data and analyze its effect on wind and solar output
- Forecast renewable generation with MLflow

---

## 🙋 About me

** [diveshjadhav1193@gmail.com]

---

<sub>**Data source:** Bundesnetzagentur | SMARD.de (check the license terms on the site)</sub>
