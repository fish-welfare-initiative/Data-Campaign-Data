# Continuous Water Quality Monitoring Data — Fish Welfare Initiative

This repository contains water quality data collected as part of [Fish Welfare Initiative's](https://www.fishwelfareinitiative.org/) satellite imagery data campaign (November 2025–January 2026). We deployed 16 continuous monitoring devices and used handheld ProDSS meters at fish farms in Eluru, Andhra Pradesh, India.

For full context on why we collected this data, what it includes, and what we're planning next, see our [blog post](https://www.fishwelfareinitiative.org/post/cm-data).

## Repository Structure

### Excel Files (Original Data)

These are the original `.xlsx` files with full formatting, including color-coded quality flags and multi-sheet structure. Download these if you want the complete, formatted dataset.

| File | Description |
|------|-------------|
| `Data Campaign Data.xlsx` | The full dataset: 17 pond time-series sheets, a ProDSS & Photometer sheet, a Key Events log, and a QC Flags legend. |
| `Comparison ProDSS vs Continuous Monitors.xlsx` | Side-by-side comparison of continuous monitor vs. ProDSS readings on satellite flyover days. |
| `Analysis ProDSS vs Continuous Monitors.xlsx` | Detailed daily ProDSS vs. continuous monitor comparison data from a validation study. |

### CSV Files

The `csv/` folder contains the same data exported as individual `.csv` files for easier programmatic access. GitHub can preview these directly.

**Pond time-series data** (17 files): `csv/[id].csv`
Each file contains continuous monitoring readings for one pond, with columns: Timestamp, DO (mg/L), pH, Temperature (°C). Readings were taken every 15 minutes. Each file also includes three QC flag columns (`QC_Flag_DateTime`, `QC_Flag_DO`, `QC_Flag_pH`) that reproduce the color-coded conditional formatting from the original Excel file as text labels. Flags are applied per-measurement, not per-row.

**Metadata and reference sheets:**

| File | Description |
|------|-------------|
| `csv/Key_Events.csv` | Device installation dates, calibration info, and key events during the campaign. |
| `csv/QC_Flags.csv` | Legend explaining the quality control flags used in the dataset (see below). |
| `csv/ProDSS_and_Photometer.csv` | Handheld ProDSS and photometer readings collected on satellite flyover days. Includes a `QC_Flag` column for flagged values. |

**Comparison/validation data:**

| File | Description |
|------|-------------|
| `csv/Continuous_Monitor_vs_ProDSS_Comparison.csv` | ProDSS vs. monitor readings on flyover days, with a `QC_Flag` column marking notable or large differences in specific measurements. |
| `csv/ProDSS_vs_Continuous_Monitors_Comparison.csv` | Daily comparison study data, with an `Event_Flag` column marking probe cleaning events and satellite flyover dates. |

## Quality Control Flags

The original Excel files use color-coded conditional formatting to flag potential data quality issues. In the CSV exports, these are converted to text in a dedicated flag column. The flags apply to **individual cells** (specific measurements), not entire rows.

| Flag | Meaning |
|------|---------|
| `time_gap_>20min` | Gap in recording >20 minutes — may indicate device issues. |
| `DO_<0.1` | Near-zero DO — possible probe issue or extreme event. |
| `DO_jump_>2` | DO changed by more than 2 mg/L within ~15 minutes — possible probe disturbance. |
| `DO_>10_before_noon` | DO above 10 mg/L before noon — possible sensor noise. |
| `pH_out_of_range` | pH below 6 or above 10 — unusual for pond aquaculture. |
| `pH_jump_>1` | Rapid pH change — possible probe disturbance. |
| `flyover_window` | Reading taken during satellite overpass (10:20–10:40 AM IST). |
| `notable_difference` | ProDSS vs. monitor difference above typical range. |
| `large_difference` | ProDSS vs. monitor difference substantially above typical range. |
| `probes_cleaned` | Probes were cleaned on this date. |
| `satellite_flyover_date` | Satellite flyover occurred on this date. |

### Zero-Value Readings

Some readings contain exact zero values (0) for DO, pH, and/or Temperature. These are equipment artifacts — typically caused by device resets, sensor initialization on startup, or outages — and do not represent real water quality measurements. They should be filtered out before analysis.

Most zero-value readings occur on January 8, 2026, across many devices simultaneously (likely a coordinated calibration or firmware event). A few ponds also have longer stretches of zeros: `eb2903bd` has a multi-day outage starting January 5, and `ac7bb683` has persistent pH=0 readings after being reinstalled on January 25 following a harvest. These zeros are not flagged by the QC conditional formatting rules but are noted in `csv/QC_Flags.csv`.

## Parameters Collected

| Parameter | Device |
|-----------|--------|
| Dissolved Oxygen (mg/L) | Continuous monitor and ProDSS handheld meter |
| pH | Continuous monitor and ProDSS handheld meter |
| Temperature (°C) | Continuous monitor and ProDSS handheld meter |
| Ammonia (TAN NH3-N) | Hanna Spectrophotometer (combined with pH and temperature) |
| Chlorophyll-a (µg/L) | ProDSS handheld meter |

## Notes

- All pond identifiers have been anonymized. The Pond IDs use the same anonymous keys as those in our [2026 ARA data sharing post](https://www.fishwelfareinitiative.org/aquatic-risk-assessment), so the datasets can be cross-referenced.
- Continuous monitors collected readings every ~15 minutes. ProDSS and ammonia data were collected on satellite flyover days (every 3–5 days).
- Monitors were cleaned approximately every 5 days, prioritizing the day before satellite flyovers. Algae buildup between cleanings may affect sensor accuracy.

## License

This data is shared publicly to support research into scalable fish welfare solutions. Please cite Fish Welfare Initiative if you use this data.
