# Analytics-Project

# Climate Change Analytics: CO₂, Surface Temperature, Sea Level & Disaster Frequency  
---

## Project Summary
This project investigates the **interplay between atmospheric carbon dioxide (CO₂) concentration, global surface temperature change, mean sea level change, and climate-related disaster frequency**. It combines **public web-hosted datasets** and a **live ArcGIS REST API (GeoJSON)** to build a reproducible analysis pipeline, perform EDA, engineer features, and quantify relationships through **correlation, trend analysis, regression, and spatial visualization**.

---

## Research Goals
The notebook answers the following:
1. How strongly are **CO₂ concentrations** associated with **global surface temperature**?
2. How strongly is **surface temperature** associated with **mean sea level**?
3. Is there a measurable association between **mean sea level** and **climate-related disaster frequency**?
4. How do these patterns vary across **time** and **geography**?

---

## Data Sources (4 Collections)
This project uses **four climate-related datasets**, sourced from public repositories and APIs:

### Dataset 1 — Annual Surface Temperature Change (Country-level, wide → long)
- Loaded from a public GitHub CSV (raw link used in the notebook)
- Transformed into tidy long format using **melt/reshape**
- Used for trend, correlation, and spatial insights

### Dataset 2 — Atmospheric CO₂ Concentrations (Monthly → yearly averages)
- Loaded from a public GitHub CSV
- Converted Year+Month into datetime, then aggregated to **yearly mean CO₂**
- Filtered to an analysis window (1992–2022) for comparability

### Dataset 3 — Mean Sea Level Changes (Year-level)
- Loaded from a public GitHub CSV
- Cleaned and prepared for year-level merging with other indicators

### Dataset 4 — Climate-related Disasters Frequency (API / GeoJSON)
- Extracted via **ArcGIS REST API** endpoint returning **GeoJSON**
- Retrieved using Python **`requests.get()`**
- Parsed and transformed into analysis-ready tabular format (wide → long)

> Project reference site used for climate data sources: IMF Climate Change Data page  
> https://climatedata.imf.org/pages/climatechange-data

---

## Tech Stack & Tools

### Environment
- Python
- Jupyter Notebook

### Core Libraries
- **pandas** (data ingestion, cleaning, reshaping, merges, grouping)
- **numpy** (numeric operations)
- **matplotlib** (visualization)
- **seaborn** (EDA visualizations, pairplot, heatmaps)
- **requests** (API-based data extraction for GeoJSON)
- **json** (API response handling)
- **geopandas** (spatial joins and world-map plotting)

### Modeling / ML (Used in Regression Section)
- **scikit-learn**
  - `PolynomialFeatures`
  - `LinearRegression`
  - `make_pipeline`
  - `cross_val_score` (cross-validation)

---

## Methods & Techniques Implemented

### 1) Data Loading & Acquisition
- Remote CSV ingestion using `pd.read_csv(<raw_github_url>)`
- REST API ingestion via `requests.get(<arcgis_endpoint>)` returning GeoJSON

### 2) Data Cleaning & Wrangling
- Column subsetting and schema simplification
- Null handling and basic validation checks
- Data type corrections
- **Reshaping (wide → long)** using melting to make datasets tidy
- Year extraction / datetime creation (CO₂ dataset)

### 3) Exploratory Data Analysis (EDA)
Performed on each dataset:
- Univariate analysis (numerical + categorical)
- Bivariate analysis (category vs numeric)
- Multivariate analysis (time × geography × metric)
- Visuals used include: histograms, KDE plots, bar charts, line charts, scatter plots, heatmaps

### 4) Feature Engineering / Data Preparation
- Aggregations by **Year** and **Country**
- Binning / categorization:
  - Countries binned by temperature patterns (temperature-based grouping)
  - Disaster-prone categorization by disaster type and frequency
- Creation of year-level summary tables for merging across indicators

### 5) Integrated Investigative Analysis
- Year-level merging across datasets:
  - CO₂ yearly averages + average surface temperature + average sea level + total disaster frequency
- **Correlation analysis**
- **Trend analysis**
- **Regression analysis**
  - Polynomial regression with cross-validation across degrees (1–5)
- **Spatial analysis**
  - World map visualizations using `geopandas` (Natural Earth low-res world boundaries)
- **Pair plot**
  - Multivariate relationship visualization across key indicators

---

## Key Results (from your notebook outputs)
From the merged year-level dataset, the project reports:

- **CO₂ vs Surface Temperature:** correlation = **0.9015869000674023**  
  → Strong positive relationship (high, near-linear association)

- **Surface Temperature vs Sea Level:** correlation = **0.8683914747663903**  
  → Strong positive relationship (high, non-linear association)

- **Sea Level vs Disaster Frequency:** correlation = **0.5016704860645078**  
  → Moderate positive relationship (suggests other contributing drivers beyond sea level alone)

Additional findings documented in the notebook:
- Countries near the **Arctic Circle** show higher observed surface temperature changes relative to many equatorial countries (as visualized via spatial analysis).
- CO₂ increased substantially over recent decades (notebook notes ~**85 ppm increase** over the last ~30 years).
- Many high-disaster countries in the top group are geographically **coastal / near oceans**, aligning with sea-level and storm risk narratives.

> Note: These are **observed statistical associations** from the datasets and do not, by themselves, prove causation.

---

## Reproducibility
- All data loading steps use **public URLs / API endpoints** directly in the notebook.
- No Postgres or external database storage is required.
- All transformations (cleaning, tidying, merges, plots, correlations, regression) are executed within the notebook.

---
