# Dataset for: Spatiotemporal machine learning for PM2.5 prediction and implications for pollution mitigation in Greater London

**Author:** Anushka Pandey, Cambridge Centre for International Research
**DOI:** https://doi.org/10.5281/zenodo.24153394
**Licence:** Creative Commons Attribution 4.0 International (CC BY 4.0), with source-data attributions listed below.
**Contact:** anushkap.1906@gmail.com

## 1. Overview

This record contains the compiled dataset used to train and evaluate machine learning models that predict hourly PM2.5 concentrations across Greater London (January 2020 to December 2025), and to run a counterfactual analysis of human-influenced features (road length, major-road length, traffic flow and NDVI). It accompanies the manuscript of the same title (citation to be added once published).

## 2. Files

| File | Size | Description |
|---|---|---|
| `MainData.csv` | ~1.3 GB | One row per monitoring location per hour. Merges PM2.5 observations with meteorology, land cover, vegetation, road, traffic, spatial and temporal features (Section 3). |
| `london_boroughs.csv` | ~1.2 MB | Boundary points of the London boroughs, used to assign monitoring locations to boroughs (Section 4). |
| `README.md` | | This file. |

[Add any further files you upload, e.g. `code.zip`.]

## 3. `MainData.csv`

### 3.1 Structure

- One row = PM2.5 at one monitoring location in one hour (UTC).
- Rows: [number of rows]. Monitoring locations: 230 (239 OpenAQ sensors; sensors within 30 m of each other were merged into one location when their reporting periods did not overlap).
- Coverage: [first date] to [last date].
- Missing values: [state how missing values are coded, e.g. empty cells / `NaN`].
- [State whether the 22--26 March 2022 Saharan dust observations (2,064 rows) are included or already removed.]
- [State whether PM2.5 values are raw or winsorised. In the paper, winsorisation is applied during modelling, at the 99.5th percentile of the training targets (56.33 µg/m³).]

### 3.2 Key columns

| Column | Description | Unit / format |
|---|---|---|
| `date_x` | Calendar date of the observation | YYYY-MM-DD |
| `datetime` | Timestamp of the observation | ISO 8601, UTC |
| `pm25` | Observed PM2.5 concentration | µg/m³ |
| `latitude`, `longitude` | Location of the monitoring site | decimal degrees (WGS84) [confirm] |
| `ndvi_v3_original` | Copernicus Global Land Service 300 m NDVI, as supplied | unitless |
| `ndvi_was_interpolated` | Whether the NDVI value was interpolated | True / False |
| `ndvi_v3_final` | NDVI used as a model feature | unitless |
| `landcover_2020_*` | One 0/1 column per ESA WorldCover 2020 class (e.g. `landcover_2020_mangroves`, `landcover_2020_moss_lichen`) | 0 / 1 |
| `landcover_2020_group` | Grouped land cover: Built-up, Green/open space, or Tree cover | category |
| `landcover_2020_group_Built-up`, `landcover_2020_group_Green/open space`, `landcover_2020_group_Tree cover` | Mutually exclusive binary encoding of `landcover_2020_group` (used as model features) | 0 / 1 |
| `wind_speed_missing` | Flag for missing wind speed | 0 / 1 |
| `aod`, `aod_station_level`, `aod_cluster`, `aod_clustered`, `dust_camsera` | CAMS aerosol optical depth / dust fields explored during the study. **Not used as model features** (see paper, Section 3.1). | [units] |

### 3.3 Columns used as model features

The paper's input features are listed below. [Map each to its column name in `MainData.csv`.]

| Category | Feature | Column name | Unit |
|---|---|---|---|
| Meteorology | Temperature | [col] | [unit] |
| | Relative humidity | [col] | [unit] |
| | Wind speed | [col] | [unit] |
| | Solar irradiance | [col] | W/m² (hourly, de-accumulated) |
| | Rainfall | [col] | [unit] (hourly, de-accumulated) |
| Land cover and vegetation | Built-up, green/open space, tree cover | `landcover_2020_group_*` | 0 / 1 |
| | NDVI | `ndvi_v3_final` | unitless |
| Roads and traffic | Road length within 250 m | [col] | m |
| | Major road length within 250 m | [col] | m |
| | Nearest traffic AADF | [col] | vehicles/day |
| | Distance to nearest traffic count point | [col] | [unit] |
| Spatial | Distance to nearest monitoring station | [col] | [unit] |
| Temporal | Day of week (0--6) | [col] | integer |
| | Weekend | [col] | 0 / 1 |
| | Public holiday | [col] | 0 / 1 |

Hour of day, month, latitude/longitude and sensor ID were **not** used as model inputs. Lagged PM2.5 inputs (the PM2.5 observations of the previous *L* hours at the same location) are constructed from `pm25` during modelling and are not stored in this file.

### 3.4 Other columns

[List any remaining columns in the file and what they are, or state that they are intermediate / unused. The full list can be generated with the snippet in Section 8.]

### 3.5 Train / test split

The paper uses a chronological 80/20 split: the first 80% of observations (through 17 April 2025) form the training set and the remainder (rest of 2025) the test set. [State whether the file contains a split indicator, or how to reproduce the split.]

## 4. `london_boroughs.csv`

- Columns: `borough` (borough name) and `x`, `y` (boundary point coordinates).
- Each row is one boundary point; points are listed in order along each borough's outline.
- Coordinates look like British National Grid eastings and northings in metres (EPSG:27700) [confirm].
- Source: London Datastore (Greater London Authority) London borough boundaries.
- In the study, boundaries were intersected with the Greater London bounding box (longitude -0.510 to 0.334, latitude 51.286 to 51.692), and monitoring locations were assigned to boroughs by location. "Central London" is defined as the union of Westminster, Camden, Islington, Tower Hamlets, Hackney, Southwark and Lambeth.

## 5. Data sources, licences and attribution

| Data | Source | Licence / terms |
|---|---|---|
| PM2.5 observations | OpenAQ (https://explore.openaq.org) | Terms vary by data provider; see OpenAQ. |
| Meteorology | ERA5-Land hourly data, Copernicus Climate Change Service (https://doi.org/10.24381/cds.e2161bac) | Contains modified Copernicus Climate Change Service information. |
| Land cover | ESA WorldCover 10 m 2020 v100 (https://doi.org/10.5281/zenodo.5571936) | CC BY 4.0 |
| NDVI | Copernicus Global Land Service, NDVI 300 m, version 3 | Copernicus free, full and open data policy. |
| Aerosol / dust (explored, not used as features) | Copernicus Atmosphere Monitoring Service, EAC4 reanalysis | Contains modified Copernicus Atmosphere Monitoring Service information. |
| Road features | © OpenStreetMap contributors (https://www.openstreetmap.org) | Open Database Licence (ODbL) |
| Traffic (AADF) | UK Department for Transport road traffic statistics | Open Government Licence v3.0 |
| Borough boundaries | London Datastore, Greater London Authority | Check the licence stated on the dataset page; boundary data typically carries Ordnance Survey / ONS Crown copyright notices. |

Please consult each provider's terms before redistributing or reusing derived data.

## 6. Processing summary

1. PM2.5 hourly observations were downloaded for the Greater London bounding box and sensors within 30 m were merged when reporting periods did not overlap.
2. ERA5-Land precipitation and solar radiation (accumulated since 00:00 UTC) were de-accumulated by differencing; solar radiation at 00:00 UTC was set to zero.
3. Land cover (static, 2020) and NDVI were attached by location.
4. Road length and major-road length were computed within a 250 m radius of each location; traffic AADF and distance were taken from the nearest Department for Transport count point.
5. Temporal and spatial features were added (day of week, weekend, public holiday, distance to nearest monitoring station).

Modelling steps (normalisation using training-set statistics, median imputation, winsorisation, lagged PM2.5 inputs, hyperparameter tuning, SHAP and counterfactual analysis) are described in the manuscript.

## 7. Citation

If you use this dataset, please cite:

> Pandey, A. (2026). Dataset for: Spatiotemporal machine learning for PM2.5 prediction and implications for pollution mitigation in Greater London [Dataset]. Zenodo. https://doi.org/10.5281/zenodo.24153394

and the associated article [citation to be added once published].

## 8. Listing every column (for completing Section 3)

```python
import pandas as pd
df = pd.read_csv("MainData.csv", nrows=1000)
print(pd.DataFrame({"column": df.columns, "dtype": df.dtypes.astype(str).values}).to_string())
```
