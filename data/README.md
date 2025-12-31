# Dataset Description

The dataset used in this study was retrieved from the USGS Earthquake Catalogue and
includes all global seismic events with magnitudes between **1.0 and 4.0**, recorded
between **1 August 2025 and 20 November 2025**.

The dataset contains **19,958 seismic events** across **22 attributes**, covering
event metadata, spatial location, magnitude, depth, and several uncertainty and
quality-control measures.

---

## Data Source

- Provider: United States Geological Survey (USGS)
- Catalogue: Global Earthquake Catalogue
- Access method: Web query and CSV export

**Note:**  
Raw data files are **not included** in this repository due to size and licensing
considerations. The dataset can be fully reproduced using the query URL below.

---

## Query Parameters

- Magnitude range: 1.0–4.0
- Time window: 2025-08-01 to 2025-11-20
- Global spatial coverage
- Ordered by event time (UTC)

### Direct Query URL
https://earthquake.usgs.gov/earthquakes/map/?extent=-89.72981,-374.0625&extent=89.72981,734.76563&range=search&timeZone=utc&search=%7B%22name%22:%22Search%20Results%22,%22params%22:%7B%22starttime%22:%222025-8-1%2000:00:00%22,%22endtime%22:%222025-11-20%20%22,%22minmagnitude%22:1,%22maxmagnitude%22:4,%22orderby%22:%22time%22%7D%7D


---

## Schema and Preprocessing Decisions

| Field Name | Data Type | Description | Preprocessing |
|----------|-----------|-------------|---------------|
| id | string | Unique event identifier | Dropped |
| time | string | Event origin time (UTC, ISO-8601) | Converted to datetime |
| latitude | float | Epicentral latitude (degrees) | — |
| longitude | float | Epicentral longitude (degrees) | — |
| depth | float | Event depth (km) | — |
| mag | float | Event magnitude | — |
| magType | string | Magnitude scale (ml, mb, md, etc.) | — |
| nst | float | Number of contributing stations | Retained |
| gap | float | Largest azimuthal gap between stations | Retained |
| dmin | float | Distance to nearest station (degrees) | Retained |
| rms | float | RMS of travel-time residuals | Retained |
| net | string | Reporting seismic network | Retained |
| horizontalError | float | Horizontal location uncertainty (km) | Retained |
| depthError | float | Depth uncertainty (km) | Retained |
| magError | float | Magnitude uncertainty | Retained |
| magNst | float | Stations contributing to magnitude | Retained |
| status | string | Processing status | Dropped (target leakage risk) |
| locationSource | string | Location reporting network | Dropped |
| magSource | string | Magnitude reporting network | Dropped |
| updated | string | Last update timestamp | Dropped |
| place | string | Human-readable location description | Feature engineered |
| type | string | Seismic event type (target variable) | Retained |

---

## Class Distribution

The target variable (`type`) is **severely imbalanced**, with earthquake events
dominating the dataset and minority classes such as icequakes and explosions
appearing sparsely. This imbalance motivated the use of:

- Cost-sensitive learning
- Macro-F1 and Weighted-F1 evaluation
- Confusion-matrix-based analysis

---

## Reproducibility

All preprocessing steps, feature engineering, and modelling decisions are fully
documented in the project notebook:

`notebooks/seismic_ml_pipeline.ipynb`

