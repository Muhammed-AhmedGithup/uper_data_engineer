# 🚖 Uber Data Engineering Project

A complete end-to-end data engineering pipeline built on **NYC TLC Uber trip data**, covering data modeling, ETL transformation, and business intelligence reporting.

---

## 📌 Project Overview

This project transforms raw Uber trip records into a clean, analytics-ready **star schema** data model. The pipeline handles data ingestion, dimensional modeling, fact table construction, and finally a Power BI dashboard for visual insights.

---

## 📂 Project Structure

```
uber-data-engineering/
│
├── uber_data.csv                  # Raw source data (~100,000 trip records)
├── uper_data_engineer.ipynb       # ETL notebook — data modeling & transformation
├── uper_data_engineer.pbix        # Power BI dashboard
│
├── datetime_dim.csv               # Dimension: pickup & dropoff datetime breakdowns
├── pickup_location_dim.csv        # Dimension: pickup coordinates
├── dropoff_location_dim.csv       # Dimension: dropoff coordinates
├── passenger_count_dim.csv        # Dimension: number of passengers
├── trip_distance_dim.csv          # Dimension: trip distance
├── rate_code_dim.csv              # Dimension: rate code type
├── store_and_fwd_flag_dim.csv     # Dimension: store-and-forward flag
├── payment_type_dim.csv           # Dimension: payment method
└── fact_table.csv                 # Central fact table linking all dimensions
```

---

## 🗃️ Dataset

**Source:** NYC TLC (Taxi & Limousine Commission) Uber trip records

| Field | Description |
|---|---|
| `VendorID` | Trip vendor identifier |
| `tpep_pickup_datetime` | Pickup timestamp |
| `tpep_dropoff_datetime` | Dropoff timestamp |
| `passenger_count` | Number of passengers |
| `trip_distance` | Trip distance in miles |
| `pickup_longitude / latitude` | Pickup GPS coordinates |
| `dropoff_longitude / latitude` | Dropoff GPS coordinates |
| `RatecodeID` | Rate code applied to the trip |
| `store_and_fwd_flag` | Whether trip was stored before forwarding |
| `payment_type` | Payment method (card, cash, etc.) |
| `fare_amount` | Base fare |
| `extra`, `mta_tax`, `tip_amount`, `tolls_amount` | Additional charges |
| `improvement_surcharge` | Improvement surcharge |
| `total_amount` | Total trip cost |

> **~100,000 trip records** with 19 columns.

---

## 🏗️ Data Model (Star Schema)

The raw data is broken down into **8 dimension tables** and **1 central fact table**:

```
                    ┌─────────────────┐
                    │   fact_table    │
                    │─────────────────│
         ┌──────────┤  VendorID       ├──────────┐
         │          │  datetime_id    │          │
         │          │  passenger_     │          │
  datetime_dim      │  count_id       │   pickup_location_dim
         │          │  trip_distance_ │          │
         │          │  id             │          │
         └──────────┤  pickup_loc_id  ├──────────┘
                    │  dropoff_loc_id │
         ┌──────────┤  rate_code_id   ├──────────┐
         │          │  payment_       │          │
  dropoff_location  │  type_id        │  payment_type_dim
         │          │  fare_amount    │          │
         │          │  total_amount   │          │
         └──────────┤  ...            ├──────────┘
                    └─────────────────┘
```

**Dimension Tables:**

| Table | Key | Description |
|---|---|---|
| `datetime_dim` | `datetime_id` | Pickup/dropoff broken into day, month, year, hour, weekday |
| `pickup_location_dim` | `pickup_location_id` | Pickup longitude & latitude |
| `dropoff_location_dim` | `dropoff_location_id` | Dropoff longitude & latitude |
| `passenger_count_dim` | `passenger_count_id` | Passenger count values |
| `trip_distance_dim` | `trip_distance_id` | Trip distance values |
| `rate_code_dim` | `rate_code_id` | Rate code types |
| `store_and_fwd_flag_dim` | `store_and_fwd_flag_id` | Store-and-forward flag |
| `payment_type_dim` | `payment_type_id` | Payment method types |

---

## ⚙️ ETL Process

The transformation notebook (`uper_data_engineer.ipynb`) performs the following steps:

1. **Load raw data** — Read `uber_data.csv` using pandas
2. **Explore the data** — Check data types, unique values, nulls
3. **Parse datetimes** — Convert string timestamps to `datetime` objects
4. **Build dimension tables** — Extract unique values per dimension, assign surrogate keys
5. **Build datetime features** — Extract day, month, year, hour, weekday for both pickup and dropoff
6. **Build the fact table** — Merge all dimensions back onto the base data, keep only key columns + metrics
7. **Export results** — Save all dimension tables and the fact table as CSV files

---

## 📊 Power BI Dashboard

The `.pbix` file (`uper_data_engineer.pbix`) connects to the exported CSV files and provides:

- Trip volume over time
- Revenue breakdown (fare, tips, tolls, surcharges)
- Pickup & dropoff location heatmaps
- Passenger count distribution
- Payment type analysis
- Peak hours and weekday trends

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| Python 3 | Data transformation |
| pandas | Data manipulation & modeling |
| Jupyter Notebook | Development environment |
| Power BI | Data visualization & reporting |

---

## 🚀 How to Run

1. **Clone the repo**
   ```bash
   git clone https://github.com/your-username/uber-data-engineering.git
   cd uber-data-engineering
   ```

2. **Install dependencies**
   ```bash
   pip install pandas jupyter
   ```

3. **Run the notebook**
   ```bash
   jupyter notebook uper_data_engineer.ipynb
   ```

4. **Open the dashboard**  
   Open `uper_data_engineer.pbix` in Power BI Desktop.

---

## 📈 Key Insights (from the dashboard)

- The dataset covers NYC Uber trips with detailed financial and geographic breakdowns
- Datetime dimension enables time-series analysis down to the hour level
- Star schema design enables fast, efficient querying in BI tools

---

## 👤 Author

**Mohamed Ahmed**  
[GitHub](https://github.com/Muhammed-AhmedGithup) · [LinkedIn](https://www.hackerrank.com/profile/ma1927393)

---
