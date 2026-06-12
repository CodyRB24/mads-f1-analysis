# Performance Under Pressure

**Analyzing Formula 1 driver and team performance under adverse weather conditions**

A University of Michigan Master of Applied Data Science (MADS) milestone project combining FastF1 lap-level telemetry with Open-Meteo historical weather archive data across 173 races and 189,248 individual laps.

---

## Project Overview

This project investigates how Formula 1 drivers and teams respond to adverse weather, with a focus on rainy conditions.
---

## Notebook Execution Order

Run the notebooks in the following sequence to avoid any errors. Each notebook produces output files that are consumed by those downstream.

### 1. `notebooks/1_race_data_acquisition.ipynb`
Fetches raw Formula 1 data using the FastF1 API for all race sessions from 2018–2025.

**Produces:**
- `data/f1_event_location_data.csv` — 173 races with lat/lon coordinates and UTC start times from FastF1
- `data/ff1_laps_data/laps_data_*.csv` — Per-year lap data from FastF1
- `data/ff1_weather_data/weather_data_*.csv` — Per-year session weather data from FastF1

**Note:**
- Optional to run as it involves time-consuming code. The data needed to run the other notebooks is created further downstream and stored for further runs.
---

### 2. `notebooks/2_race_data_cleaning.ipynb`
This notebook cleans the raw data from `notebooks/1_race_data_acquisition.ipynb` and creates the data that used for a majority of the analysis in the project

**Requires:** Output from Notebook 1 (`notebooks/1_race_data_acquisition.ipynb`)

**Produces:**
- `data/f1_lap_weather_data.pkl` — The cleaned and combined lap and weather dataset

---

### 3. `notebooks/3_f1_weather.ipynb`
Fetches a 2-hour weather window around each race start from the Open-Meteo Historical Archive API. It is used to validate weather flags against actual tyre compound data (intermediate and wet tyre compounds are the ground truth).

**Requires:** Output from Notebook 1 (`f1_event_location_data.csv`), Output from Notebook 2 (`f1_lap_weather_data.pkl`)

**Produces:**
- `data/f1_race_weather_data.csv` — Data of races with engineered weather features
- `data/wet_at_start_race_profiles.csv` — Simplified race wet/dry classification

---

### 4. `notebooks/4_f1_lap_precipitation.ipynb`
This notebook maps each lap to its corresponding weather window and computes per-lap and cumulative precipitation.

**Requires:** Output from Notebook 3 (`f1_race_weather_data.csv`) and output from Notebook 2 (`f1_lap_weather_data.pkl`)

**Produces:**
- `data/lap_precipitation_data.csv` — Data for 189,248 laps with weather bucket metadata

---

### 5. `notebooks/5_race_data_analysis.ipynb`
Analysis of lap performance under weather conditions after further data cleaning. Ranks drivers and teams by their relative time performance gain or loss in wet conditions.

**Requires:** Output from Notebook 2 (`f1_lap_weather_data.pkl`)

**Key outputs:** Driver and team rain time performance and visualisations
---

### 6. `notebooks/6_driver_ranking_and_analysis.ipynb`
Comprehensive driver performance rankings under pressure. Aggregates wet vs. dry finish positions and creates further features for effective visualisations.
**Requires:** Output from Notebook 2 (`f1_lap_weather_data.pkl`), Output from Notebook 3 (`wet_at_start_race_profiles.csv`)

**Key outputs:** Driver performance in dry v wet conditions and visualisations

---

## Data Directory

| File | Size | Description |
|---|---|---|
| `f1_event_location_data.csv` | 14 KB | 173 races with location coordinates |
| `f1_race_weather_data.csv` | 106 KB | Open-Meteo weather features per race |
| `lap_precipitation_data.csv` | 29 MB | Lap-wise precipitation data |
| `f1_lap_weather_data.pkl` | 48 MB | Master lap and weather dataset |
| `race_results_summary.csv` | 404 KB | Race-level results summary |
| `position_changes.csv` | 17 MB | Lap-level position changes |
| `driver_statistics.csv` | 5.3 KB | Driver-level summary |
| `fastf1_ground_truth.csv` | 5.1 KB | Ground truth from FastF1 |
| `wet_at_start_race_profiles.csv` | 16 KB | Race wet/dry classification |

---

## Setup

See `requirements.txt` for information on all package used. To install, enter the following code in your terminal:

```bash
pip install -r requirements.txt
```

---

## External Data Sources

- **[FastF1](https://docs.fastf1.dev/)**
- **[Open-Meteo Historical Archive](https://open-meteo.com/en/docs/historical-weather-api)**
- **[F1 Circuits (GitHub)]** 