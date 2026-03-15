# Jacobs Flood Simulation Carbon Analysis

A data science internship project analysing the carbon footprint of large-scale hydraulic and flood-risk simulations run by Jacobs Engineering.

---

## Project Overview

Jacobs runs thousands of flood simulations annually using two software packages:
- **TUFLOW** — 2D hydraulic simulations (`.tlf` log files)
- **Flood Modeller 1D** — 1D river simulations (`.zzd` log files)

This project extracts performance data from 1000+ simulation log files, calculates energy consumption and carbon emissions, and presents findings through Power BI dashboards and Excel reports.

---

## Folder Structure
```
jacobs_project/
├── data/           ← simulation log files + Excel reference file
├── output/         ← generated CSV results
└── parser.py       ← main Python script
```

---

## What the Script Does

1. Reads all `.tlf` and `.zzd` files from the `data/` folder
2. Extracts key information from each file:
   - Simulation name
   - Computer name
   - Hardware type (CPU or GPU)
   - Simulation run time (clock time minus startup time)
   - Folder size (from Excel reference file)
3. Fills missing computer names by:
   - Matching ZZD files to their corresponding TLF file
   - Looking up the Excel reference file provided by Jacobs
4. Calculates carbon emissions for each simulation:
   - Energy (kWh) = Power (W) × Time (hours) / 1000
   - Carbon (kg CO₂) = Energy (kWh) × 0.125
5. Saves everything to a clean CSV for Power BI and Excel analysis

---

## Carbon Calculation Methodology

| Parameter | Value | Source |
|---|---|---|
| GPU power consumption | 300W | Typical NVIDIA GPU (assumed) |
| CPU power consumption | 150W | Typical Intel Xeon (assumed) |
| UK carbon intensity | 0.125 kg CO₂/kWh | National Grid ESO Live, 2024-2025 |

---

## Key Findings

- **1,000+** simulation log files analysed
- **239** TUFLOW TLF files (GPU simulations)
- **1,003** Flood Modeller ZZD files (CPU simulations)
- **GPU simulations generate ~54.94%** of total carbon despite fewer runs
- GPU uses **2x more power** than CPU (300W vs 150W)
- Computer names not logged in ZZD files — identified as a monitoring gap

---

## Known Limitations

- Hardware power values are assumed — actual specs not yet confirmed by Jacobs
- ZZD files do not log computer name — filled in from matching TLF files or Excel reference where possible
- ZZD files do not separate startup time from simulation time
- 22 ZZD files missing simulation time (incomplete runs or crashes)
- All simulations assumed to be UK-based (confirmed by Jacobs)

---

## How to Run

**1 — Install dependencies:**
```
pip install openpyxl
```

**2 — Add your files:**
- Place all `.tlf` and `.zzd` files into the `data/` folder
- Place the Excel reference file into the `data/` folder
- Update `EXCEL_FILE` in `parser.py` with your actual Excel filename

**3 — Run the script:**
```
python parser.py
```

**4 — Check results:**
- Open `output/simulation_summary.csv` in Excel or Power BI
- Columns: `simulation_name`, `computer_name`, `hardware`, `file_type`, `simulation_time_hours`, `simulation_time_hm`, `energy_kwh`, `carbon_kg`, `folder_size_mb`

---

## Output CSV Columns

| Column | Description |
|---|---|
| simulation_name | Name of the simulation (from filename) |
| computer_name | Name of the computer that ran it |
| hardware | CPU, GPU, or CPU (assumed) |
| file_type | TLF or ZZD |
| simulation_time_hours | Run time in decimal hours |
| simulation_time_hm | Run time in readable format (e.g. 6h 18m) |
| energy_kwh | Energy consumed in kWh |
| carbon_kg | Carbon generated in kg CO₂ |
| folder_size_mb | Simulation folder size from Excel reference |

---

## Tools Used

- **Python** — log file parsing and carbon calculations
- **Power BI** — interactive dashboards and charts
- **Excel** — data highlights and pivot tables
- **openpyxl** — reading Excel reference files

---

## Author

Internship project — Jacobs Engineering, 2026
