# 🚖 OW Yellow Caps — NYC TLC Yellow Taxi Analysis & Data Quality Assessment

## 🌟 Overview
This project presents a full-scale **data quality assessment, cleaning pipeline, and exploratory data analysis (EDA)** for the NYC TLC Yellow Taxi dataset covering **January–March 2025**.

Using over **11.2 million taxi trip records**, the project transforms raw operational data into a reliable, analysis-ready dataset through systematic validation, anomaly detection, preprocessing, and statistical analysis.

The work combines:
- Data Cleaning & Validation
- Data Quality Assessment
- Exploratory Data Analysis (EDA)
- Business-Oriented Insights
- Visualization & Reporting

---

# 📂 Project Structure

```text
Project
│
├── 📘 README.md
├── 📄 OW_Yellow_Caps_TOR.pdf
│
├── 📁 notebooks/
|   ├── 00_visuals.ipynb
│   ├── 01_Data_Cleaning.ipynb
│   ├── 02_Univariate_EDA.ipynb
│   ├── 03_Bivariate_EDA.ipynb
│   └── 04_Multivariate_EDA.ipynb
|
├── 📁 dataset/
│   ├── Project_master_clean.parquet
│  
│
└── 📁 outputs/
    ├──
    ├── OW_Yellow_Caps_Executive_Report.pdf/
    └── Data_Quality_Report_1 .pdf/
```

---

# 📊 Dataset Summary

| Metric | Value |
|--------|-------|
| Source Dataset | NYC TLC Yellow Taxi Trips |
| Source File | `Project_master_clean.parquet` |
| Raw Rows | 11,198,026 |
| Clean Rows | 10,429,583 |
| Rows Removed | 768,443 (~6.9%) |
| Duplicate Trips Removed | ~769,000 |
| Columns (Clean Dataset) | 39 |
| Original Columns | 22 |
| Derived Columns | 17 |
| Date Range | Jan 1 – Mar 31, 2025 |

---

# 🧠 Project Objectives

- Detect and resolve major data quality issues
- Build a robust cleaning and validation pipeline
- Identify inconsistencies and impossible trip records
- Generate meaningful business insights using EDA
- Produce a trustworthy dataset for BI and analytics workflows

---

# 🔎 Key Data Quality Checks

✔️ Field validity checks  
✔️ Invalid categorical values detection  
✔️ Negative and impossible numerical values  
✔️ Temporal consistency validation  
✔️ Trip duration verification  
✔️ Cross-field financial logic validation  
✔️ Duplicate trip detection  
✔️ Missing data analysis  
✔️ Outlier detection using statistical methods  

---

# 🧹 Cleaning Decisions

| Step | Rule | Action |
|------|------|--------|
| Temporal scope | Jan 1 – Mar 31 2025 only | Removed out-of-range records |
| Invalid duration | dropoff ≤ pickup | Removed |
| Negative financials | fare/total/tip/tolls < 0 | Removed |
| Trip distance | ≤ 0 or > 200 miles | Removed |
| Passenger count | 0 or > 9 (non-Flex Fare) | Removed |
| CBD fee logic | CBD fee before Jan 5 2025 | Removed |
| Duplicate trips | Exact match on 6 key fields | Removed |
| Long trips > 24h | Extreme duration | Flagged & kept |
| Airport fee inconsistencies | Fee outside airport zones | Flagged & kept |
| Tip on non-card payments | Logical inconsistency | Flagged & kept |
| Extreme outliers | IQR method (k=3) | Flagged & kept |

---

# ⚠️ Missing Data Analysis

`passenger_count` and `RatecodeID` contain null values for **Flex Fare** trips (`payment_type = 0`).

The missingness was classified as:

> **MAR — Missing At Random**

No imputation was applied to preserve data integrity.

---

# 📈 Exploratory Data Analysis (EDA)

## 1️⃣ Univariate Analysis
Analysis of single variables and distributions:
- Trip distance
- Fare amount
- Passenger count
- Payment type
- Trip duration
- Pickup hour distribution
- Tip percentage
- Airport fees
- Vendor trends

📊 **13 visualizations**

---

## 2️⃣ Bivariate Analysis
Relationships between pairs of variables:
- Fare vs distance
- Distance vs duration
- Payment type vs tipping
- Pickup hour vs demand
- CBD fee impact
- Airport trips vs total amount

📊 **12 visualizations**

---

## 3️⃣ Multivariate Analysis
Complex interactions among multiple variables:
- Demand patterns across weekday/hour
- Fare behavior by payment type & distance
- Airport activity trends
- Tip behavior across trip categories
- Time-based financial patterns

📊 **11 visualizations**

---

# 📌 Key Analytical Findings

## 🚕 Demand Patterns
- Peak demand occurs during:
  - **Weekdays:** 6–7 PM
  - **Saturday nights:** 10 PM–1 AM

---

## 💳 Payment Behavior

| Payment Type | Approx. Share |
|--------------|---------------|
| Credit Card | ~67% |
| Cash | ~20% |
| Flex Fare | ~13% |

Credit card trips consistently show higher tipping rates.

---

## 💰 Tipping Insights
- Strong positive relationship between:
  - Fare amount ↔ Tip amount
- Credit-card trips above **$30** show:
  - **>75% tipping rate**

---

## 📍 High-Demand Zones
Top activity areas include:
- Midtown Manhattan
- JFK Airport
- LaGuardia Airport
- Upper East Side
- Penn Station

---

## 📈 Strong Correlations

| Variables | Correlation |
|-----------|-------------|
| Distance ↔ Fare | 0.89 |
| Distance ↔ Duration | 0.78 |

---

## 🏙️ CBD Fee Impact
After implementation of the CBD surcharge on **Jan 5, 2025**:
- Average trip total increased
- Overall trip volume remained relatively stable

---

# 🛠️ Tech Stack

## Languages & Libraries
- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Jupyter Notebook

## Techniques Used
- Data validation
- Statistical analysis
- Outlier detection
- Missing data analysis
- Exploratory data analysis
- Business intelligence reporting

---

# ⚙️ How to Run

## 1️⃣ Data Cleaning

Run:

```bash
01_Data_Cleaning.ipynb
```

Requires raw parquet dataset:

```text
/mnt/user-data/uploads/Project_master_clean.parquet
```

Prebuilt outputs already exist in:

```text
/mnt/user-data/outputs/
```

---

## 2️⃣ EDA Notebooks

Run notebooks in order:

```bash
02_Univariate_EDA.ipynb
03_Bivariate_EDA.ipynb
04_Multivariate_EDA.ipynb
```

They automatically load:

```text
yellow_taxi_clean.parquet
```

---

# 💡 Key Insight

A significant portion of the dataset contained **systematic inconsistencies rather than random noise**, demonstrating that:

> 🚨 Data quality validation is essential before performing any analytical or business intelligence tasks.

Reliable insights depend directly on reliable data.

---

# 👨‍🏫 Supervision

**Dr. Osama Abdel Hay**

---

# 👥 Team

- Tuleen Bakir  
- Anas Gharaibeh  
- Tala Beirouti  
- Raneem Mahmoud  
- Lana Al-Sutari  

---

# 🎯 Final Note

This project demonstrates the importance of preprocessing, validation, and analytical rigor in large-scale real-world datasets.

By combining data quality assessment with exploratory analytics, the project delivers a trustworthy foundation for:
- Business Intelligence
- Predictive Analytics
- Operational Reporting
- Future Machine Learning Applications

---

## ⭐ If you found this project useful, consider starring the repository.
