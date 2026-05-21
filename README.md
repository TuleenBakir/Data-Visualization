# OW Yellow Caps — NYC Yellow Taxi Analysis
### Data Quality Assessment · Exploratory Data Analysis · Executive Dashboard
**NYC TLC Yellow Taxi · January – March 2025**

---

## Overview

This project delivers a full-scale data quality assessment, cleaning pipeline, exploratory data analysis (EDA), and interactive executive dashboard for NYC TLC Yellow Taxi trip records covering Q1 2025. Starting from 11.2 million raw records, the pipeline produces a validated, analysis-ready dataset and surfaces operational insights across demand patterns, revenue, geography, payment behaviour, and the impact of the CBD Congestion Fee introduced on January 5, 2025.

---

## Project Files

| File | Description |
|------|-------------|
| `01_Data_Cleaning.ipynb` | Full cleaning and validation pipeline — raw parquet → clean parquet |
| `02_Univariate_EDA.ipynb` | Single-variable distributions and summaries (11 figures) |
| `03_Bivariate_EDA.ipynb` | Two-variable relationships and correlations (12 figures) |
| `04_Multivariate_EDA.ipynb` | Three-or-more variable interactions and causal DAGs (9 figures) |
| `dashboard.html` | Interactive executive BI dashboard (opens in any browser) |
| `Project_Master_Clean.parquet` | Clean dataset — 10.32M rows, 33 columns |
| `OW_Yellow_Caps_Executive_Insights_Report.docx` | Written executive report |
| `OW_Yellow_Caps_Technical_Insights_Report.docx` | Written Technical report |

---

## Dataset Summary


| Metric | Value |
|--------|-------|
| Source | NYC TLC Yellow Taxi Trip Records |
| Period | January 1 – March 31, 2025 |
| Raw rows | 11,198,026 |
| Rows after cleaning | **10,318,198** (92.14% retained) |
| Rows dropped | 879,828 (7.86%) |
| Columns — original | 22 |
| Columns — derived | 11 |
| Final column count | 33 |

---

## Data Cleaning Pipeline (`01_Data_Cleaning.ipynb`)

### Drop rules

| Flag | Count | % of Raw | Root cause |
|------|-------|----------|------------|
| FLAG_NEG_FARE | 535,497 | 4.78% | Refund / dispute records with negative fare |
| FLAG_ZERO_DIST | 294,386 | 2.63% | Meter engaged and disengaged at same location |
| FLAG_ZERO_PAX | 69,122 | 0.62% | Non-Flex trips with 0 passengers |
| FLAG_ZERO_DURATION | 29,197 | 0.26% | Instantaneous trips — meter error |
| FLAG_LONG_DURATION | 3,811 | 0.034% | Duration > 300 min — meter left running |
| FLAG_EXTREME_DIST | 548 | 0.005% | Distance > 100 miles — implausible |
| FLAG_EXTREME_FARE | 146 | 0.001% | Fare > $500 — extreme outlier |
| FLAG_OVER_PAX | 47 | <0.001% | Passenger count > 6 — exceeds capacity |

### Correction (not dropped)

| Flag | Count | Action |
|------|-------|--------|
| FLAG_CBD_EARLY | 402 | CBD fee set to $0 — vendor submitted fee before Jan 5 effective date |

### Retained with flag

| Flag | Count | Reason |
|------|-------|--------|
| FLAG_FLEX_NULL | 2,263,749 | MNAR — passenger count and RatecodeID are null by design for all Flex Fare (payment_type = 0) trips. No imputation applied. |

### Sensitivity analysis

Changing the duration cap from 300 minutes (base) to 120 minutes (strict) removes 2,716 additional rows and shifts the median total amount by $0.02 ($20.93 → $20.91). All conclusions are stable under either threshold.

---

## Notebooks

### 02 — Univariate EDA
Single-variable distributions covering: fare amount, total amount, trip distance, trip duration, pickup hour, day of week, month, passenger count, payment type, rate code, vendor, tip amount, congestion surcharge, CBD fee, and airport fee.

Key outputs:
- Median fare: **$13.35** (P25 = $8.50, P75 = $20.50)
- Median total: **$20.70** (mean = $26.60 — right-skewed by long trips)
- Median distance: **1.74 mi** · Median duration: **12.0 min**
- Credit card: 71.0% · Flex Fare: 17.3% · Cash: 10.2%
- Curb Mobility (VendorID 2): 78.8% of all trips
- Standard rate code: 93.9% of non-Flex trips
- Median CC tip: $3.92 (large spike at $0 — ~2.8M CC trips tipped nothing)

### 03 — Bivariate EDA
Two-variable relationships: fare × distance, total × distance, duration × distance, payment type × revenue, hour × demand, day-of-week × demand, zone × revenue, pre/post CBD × metrics.

Key outputs:
- Pearson r (distance ↔ fare): **0.728** — strongest single predictor
- Pearson r (distance ↔ total): **0.673**
- OLS slope (standard rate): **$6.50 per mile**
- Thursday 18:00 is the single busiest hour across Q1: **122,673 trips**
- Saturday total trips (Q1): 1,723,168 — highest of any weekday
- Zone 132 (JFK): $31.27M total revenue — highest of any pickup zone
- Pre-CBD avg total: $27.14 · Post-CBD: $26.58 (−2.06%)
- Pre-CBD avg fare: $18.12 · Post-CBD: $17.53 (−3.25%)

### 04 — Multivariate EDA
Three-or-more variable interactions: hour × zone × demand, geography × payment × revenue, distance × duration × total (PCA), month × hour × demand, pre/post CBD × geography × fare, vendor × payment × revenue, pair plots, weekend vs weekday × hour × payment, and two causal DAGs.

Key outputs:
- Top 3 hour × zone combos: Zone 161 Friday 18:00 → 46,268 | 17:00 → 45,353 | 19:00 → 39,022
- March Saturday demand: **721,363 trips** — highest month × day cell
- CBD fare change by zone: all within ±3%; JFK is the only major zone with a positive post-CBD change (+2.27%)
- CC share gap: weekday 72–75% vs weekend 66–70% at every hour of the day
- **Figure 11A (Fare Formation DAG):** Zone → Rate Code → Fare → Total; Distance → Duration → Fare → Total; Payment Type → Tip → Total; Post-Jan-5 Policy → CBD Fee → Total
- **Figure 11B (Demand Driver DAG):** Hour / DoW / Month → Trip Demand → Zone Revenue; CBD Policy → Reduced Demand; Zone + Hour → Payment Mix → Tip Rate

---

## Dashboard (`dashboard.html`)

Open in any modern browser — no server or installation required.

| Page | Content |
|------|---------|
| Overview | Q1 KPI cards, monthly trips, vendor share, payment mix, rate code distribution |
| Demand & Time | Hourly demand, day-of-week bars, monthly evolution, heatmaps (Day × Hour, Month × DoW), top 5 zone hourly profiles, CC share by hour |
| Revenue & Fares | Fare and total distributions, avg total by distance bin, avg fare by rate code |
| Geography | Top 20 zones by revenue and trip count, avg revenue per trip |
| Payment & Tips | Payment type frequency and avg totals, passenger count, tip histogram, congestion and CBD fee distributions |
| CBD Congestion Fee | Pre vs post Jan 5 metric comparison, % fare change by zone (top 15) |
| Data Quality | Flag count chart, flag prevalence bars, issue register table, sensitivity analysis |
| Causal DAGs | Figure 11A (Fare Formation) and Figure 11B (Demand Driver) rendered as interactive SVG graphs |
| Key Insights | Priority action cards, all findings consolidated by section with full data tables |

---

## Key Findings

**Demand**
- Peak window: Thursday–Friday 17:00–19:00. Thursday 18:00 = 122,673 trips fleet-wide.
- Busiest month × day: March Saturday (721,363 trips). March demand is 19.3% above January.
- Saturdays have the highest Q1 total (1.72M trips) but the evening-rush peak belongs to Thursday.
- Times Square (Zone 230) shows a distinctive late-night surge pattern unique among top zones.

**Revenue**
- JFK Airport (Zone 132) earns $72.42 per trip — 3.06× more than Midtown Center ($23.67/trip) despite 16% fewer trips.
- LaGuardia (Zone 138) earns $67.50/trip — the 2nd highest revenue-per-trip zone.
- The 1–2 mile bin is the most common trip length (2.89M trips) with an avg total of $19.27.
- At 30–50 miles, avg total reaches $143.21 — driven almost entirely by airport flat-rate and negotiated trips.

**Payment & Tips**
- Credit card trips average $28.35 vs $23.78 for cash — a $4.57 gap partly explained by tip recording and longer average distances.
- 10.2% of trips pay cash but contribute zero recorded tip data — revenue reporting structurally underestimates rider generosity.
- 66.6% of non-Flex trips carry exactly one passenger; pairs (2 pax) = 22.6%.

**CBD Congestion Fee**
- The fee introduced on January 5, 2025 produced no significant demand suppression. All zone-level fare changes are within ±3%.
- The apparent 2–3% drop in avg fare post-CBD reflects a ride-mix shift back to shorter commuter trips after the 4-day New Year holiday period (Jan 1–4), not a fee-driven pricing effect.
- 7.32M post-CBD trips (the large majority) pay the full $2.75 fee.

**Data Quality**
- 92.14% of raw records were retained after cleaning.
- The largest drop category was negative fares (535,497 rows, 4.78%) — a systematic vendor submission issue requiring upstream correction.
- Flex Fare records (20.22% of raw data) are MNAR: passenger count and rate code are missing by design. They are retained with an `is_flex_fare` flag throughout all notebooks.

---

## How to Run

**Requirements:** Python 3.9+, Jupyter, pandas, numpy, matplotlib, seaborn, scikit-learn, networkx, pyarrow.

```bash
pip install pandas numpy matplotlib seaborn scikit-learn networkx pyarrow jupyter
```

**Notebook order:**

```
01_Data_Cleaning.ipynb      # reads Project_master_clean.parquet
02_Univariate_EDA.ipynb     # reads yellow_taxi_clean.parquet
03_Bivariate_EDA.ipynb      # reads yellow_taxi_clean.parquet
04_Multivariate_EDA.ipynb   # reads yellow_taxi_clean.parquet
```

**Dashboard:** Open `dashboard.html` directly in a browser. All chart data is embedded — no backend required.

---

## Tech Stack

| Category | Tools |
|----------|-------|
| Language | Python 3 |
| Data | pandas, numpy, pyarrow |
| Visualisation | matplotlib, seaborn |
| ML / Stats | scikit-learn (PCA), scipy |
| Graph | networkx (DAG figures) |
| Dashboard | HTML, CSS, Chart.js, SVG |
| Environment | Jupyter Notebook |

---

## Team

- Tuleen Bakir
- Anas Gharaibeh


**Supervisor:** Dr. Osama Abdel Hay
