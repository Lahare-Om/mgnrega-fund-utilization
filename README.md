# MGNREGA Fund Utilization Gap Analysis

This project analyzes MGNREGA fund availability and expenditure across Indian 
states for FY 2022-23 through FY 2025-26, to identify which states consistently 
under-utilize allocated funds and whether that gap follows a pattern over time.

## Data Coverage

- Source: State MGNREGA MIS portal (financial statement reports, per state/year)
- 116 raw files: 29 states × 4 financial years (22-23 to 25-26)
- Cleaned to 116 state-year records

## Method

Raw files are HTML tables served with a `.xls` extension, each with an 
inconsistent multi-row header across states and years. For each file, district-
level rows are isolated (filtered by numeric serial number, excluding header/
total rows), and availability and expenditure are aggregated from these district 
rows directly — the portal's own pre-computed state-total row was found to be 
unreliable and is not used. Utilization rate is calculated as:
      utilization_rate = expenditure / availability * 100


Data is cleaned in Python (pandas), stored in SQLite for querying, and 
visualized in Power BI.

## Key Findings

1. **National utilization is stable, not declining** — averaged ~100% across 
   FY22-23 to FY25-26 (99.6%–101.0%), with no clear improving or worsening trend.

2. **Bihar, Rajasthan, Punjab, and Assam are persistent under-utilizers** — each 
   appeared in the bottom-5 utilizing states in 3 of the last 4 years (FY23-24 
   through FY25-26), with Rajasthan and Bihar showing a worsening trend 
   (Rajasthan: 96.2% → 89.3% → 86.8%; Bihar: 96.5% → 95.1% → 89.3%).

3. **West Bengal is a documented anomaly, not a governance failure** — 
   utilization spiked to 180–718% due to a central government fund suspension 
   since March 2022, unrelated to state performance. Excluded from main 
   rankings; noted separately.

## Outputs

- `cleaned_mgnrega.csv` / `mgnrega.db` — cleaned dataset (CSV and SQLite)
- `queries.sql` — core SQL queries (national trend, YoY change, rankings)
- `MGNREGA.pbix` + `outputs/` — Power BI dashboard file and exported views

## Requirements

Python, pandas, sqlite3, Power BI Desktop

## Future Work

District-level drill-down, population/election-year correlation checks, 
region-level comparison (tested — no significant pattern found once WB and 
Andhra Pradesh outliers are excluded).
