# MGNREGA Utilization Analysis

This project analyzes MGNREGA fund availability and expenditure across Indian states for financial years 2022-23 through 2025-26. It cleans state-level raw Excel exports, calculates utilization rates, stores the cleaned data in CSV and SQLite formats, and prepares a Power BI-ready dataset used for dashboard/report outputs.

## Data Coverage

- Raw source files: 116 `.xls` files
- States covered: 29
- Financial years covered: `22-23`, `23-24`, `24-25`, `25-26`
- Cleaned rows: 116 state-year records
- State codes: `AP`, `AR`, `AS`, `BR`, `CH`, `GA`, `GJ`, `HP`, `HR`, `JH`, `JK`, `KA`, `KL`, `MH`, `ML`, `MN`, `MP`, `MZ`, `NL`, `OD`, `PB`, `RJ`, `SK`, `TG`, `TN`, `TR`, `UK`, `UP`, `WB`

Each raw file follows the naming pattern: (State_Code)(Year).xls

## Workflow

The main workflow is implemented in `load_and_clean.ipynb`.

1. Load all files from `raw/*.xls`.
2. Parse state code and financial year from each filename.
3. Read the tabular data using `pandas.read_html`.
4. Keep district-level rows by filtering numeric serial numbers.
5. Aggregate availability and expenditure values.
6. Calculate utilization rate:
   utilization_rate = expenditure / availability * 100

7. Export the cleaned dataset to `cleaned_mgnrega.csv`.
8. Store the cleaned data in SQLite as the `utilization` table.
9. Add region mappings, anomaly flags, and year sort order for Power BI.
10. Export the dashboard-ready dataset to `mgnrega_powerbi.csv`.

## Key Findings

1. **National utilization is stable, not declining** — averaged ~100% across FY22-23 
   to FY25-26 (99.6% to 101.0%), with no clear improving or worsening trend at the 
   national level.

2. **Bihar, Rajasthan, Punjab, and Assam are persistent under-utilizers** — each 
   appeared in the bottom-5 utilizing states in 3 of the last 4 years (FY23-24 
   through FY25-26), with Rajasthan and Bihar showing a worsening trend 
   (Rajasthan: 96.2% → 89.3% → 86.8%; Bihar: 96.5% → 95.1% → 89.3%).

3. **West Bengal is a documented anomaly, not a governance failure** — utilization 
   spiked to 180-718% due to a central government fund suspension since March 
   2022, unrelated to state performance. Excluded from rankings; noted separately.


### `cleaned_mgnrega.csv`

Base cleaned dataset with these columns:

state | State code parsed from the raw filename 
year | Financial year parsed from the raw filename 
availability | Aggregated fund availability 
expenditure | Aggregated expenditure 
utilization_rate | Expenditure as a percentage of availability 

Region row counts in the Power BI export:

Central | 16 |
East | 16 |
North | 20 |
North-East | 32 |
South | 20 |
West | 12 |

### `mgnrega.db`

SQLite database generated from the cleaned dataset. The notebook writes the data to a table named:
utilization

### `MGNREGA.pbix`

Power BI dashboard/report file built from the cleaned Power BI export.

### `outputs/`

Contains exported report artifacts:

- `MGNREGA.pdf`
- `Screenshot 2026-09-21 182640.png`

## Analysis Notes

West Bengal (`WB`) is treated as an anomaly in the Power BI export. The notebook notes that West Bengal's utilization rate is unusually high because fund availability collapsed while expenditure remained high, reportedly due to central funding suspension and continued state-level payments.

The highest utilization values in the cleaned data are for West Bengal:

| State | Year | Utilization Rate |
| --- | --- | ---: |
| WB | 25-26 | 718.54 |
| WB | 24-25 | 252.12 |
| WB | 23-24 | 180.23 |

The notebook excludes `WB` from some comparative rankings so that this anomaly does not dominate the analysis.

## Requirements

The notebook uses:

- Python
- pandas
- sqlite3
- Jupyter Notebook
- Power BI Desktop, for opening or editing `MGNREGA.pbix`

