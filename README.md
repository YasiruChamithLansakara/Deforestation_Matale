# Deforestation Analysis - Matale District, Sri Lanka

A comprehensive satellite-based deforestation monitoring and prediction system for Matale District using Landsat imagery and machine learning.

## 📋 Overview

This project analyzes temporal deforestation patterns in Matale District, Sri Lanka, using Landsat Surface Reflectance data (2013-2025). It combines remote sensing indices (NDVI, NDBI) with advanced predictive models to monitor forest loss, identify high-risk zones, and forecast future deforestation trends.

### Key Features

- **Monthly Deforestation Monitoring**: Automated processing of Landsat imagery to detect deforested areas
- **Risk Mapping**: Spatial analysis identifying vulnerable forest zones using proximity and vegetation health metrics
- **Predictive Modeling**: Multi-model forecasting (Prophet, ARIMA, Random Forest) for deforestation projections through 2030
- **Temporal Analysis**: Time series visualization and trend analysis of forest cover changes
- **Automated Pipeline**: End-to-end workflow from satellite data to actionable insights

## 🛰️ Data Sources

- **Satellite Imagery**: Landsat 8/9 Surface Reflectance (30m resolution)
  - Bands: Blue (B2), Green (B3), Red (B4), NIR (B5), SWIR1 (B6)
  - Temporal Coverage: 2013 - 2025
  - Source: USGS Earth Explorer

- **Administrative Boundaries**: GADM Sri Lanka (gadm41_LKA.gpkg)
  - Area of Interest: Matale District divisions

## 📁 Project Structure

```
Deforestation_Matale/
├── AOI/
│   ├── gadm41_LKA.gpkg                 # Sri Lanka administrative boundaries
│   └── matale_District_AOI.geojson     # Matale district boundary
├── Code/
│   ├── AOI Matale District.ipynb       # Extract district boundary
│   ├── Deforestation Analysis.ipynb    # Main deforestation analysis
│   ├── Deforestation Risk & Vulnerability Mapping/
│   │   └── Deforestation Risk & Vulnerability Mapping.ipynb
│   └── Deforestation Prediction Models/
│       └── Deforestation Prediction Models.ipynb
├── Processed_Monthly/                  # Monthly deforestation outputs
│   ├── Deforested_*.tif                # Binary deforestation masks
│   ├── NDVI_*.tif                      # Vegetation index maps
│   ├── NDBI_*.tif                      # Built-up/bare soil index
│   └── Monthly_Deforestation_Stats.csv # Time series statistics
├── Deforestation_Risk_Analysis/        # Risk assessment outputs
│   ├── deforestation_risk_score.tif
│   ├── deforestation_risk_zones.tif
│   ├── proximity_risk.tif
│   └── forest_vulnerability.tif
└── Model_Comparison/                   # Prediction model results
    ├── Model_Comparison_Results.csv
    ├── Deforestation_Forecast_2030.csv
    └── *.png                           # Visualizations
```

## 🔬 Methodology

### 1. Deforestation Detection
- **NDVI (Normalized Difference Vegetation Index)**: Measures vegetation health
  ```
  NDVI = (NIR - Red) / (NIR + Red)
  ```
- **NDBI (Normalized Difference Built-up Index)**: Detects bare soil/deforested areas
  ```
  NDBI = (SWIR1 - NIR) / (SWIR1 + NIR)
  ```
- **Classification**: Pixels with NDBI > 0.1 are classified as deforested

### 2. Risk Assessment
- **Proximity Analysis**: Distance to existing deforested areas using Euclidean distance transform
- **Forest Vulnerability**: Inverse of NDVI indicating degraded forest health
- **Risk Scoring**: Weighted combination of proximity risk and vegetation vulnerability
- **Zone Classification**: 5-tier risk levels (0=already deforested, 1-5=increasing risk)

### 3. Predictive Models
- **Prophet**: Facebook's time series forecasting with seasonality modeling
- **ARIMA**: Autoregressive Integrated Moving Average for trend analysis
- **Random Forest**: Ensemble learning with temporal features (month, year, quarter)
- **K-Means Clustering**: Temporal pattern discovery and segmentation

## 🚀 Getting Started

### Prerequisites

```bash
pip install rasterio geopandas numpy pandas matplotlib seaborn scipy scikit-learn prophet statsmodels tqdm
```

### Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd Deforestation_Matale
```

2. Download Landsat imagery and organize by year:
```
D:\Satellite Image Processing\Satellite Images\
├── 2013/
│   └── LC08_L2SP_*_SR_B*.TIF
├── 2014/
├── ...
└── 2025/
```

3. Run the analysis notebooks in sequence:

**Step 1: Extract AOI**
```bash
jupyter notebook "Code/AOI Matale District.ipynb"
```

**Step 2: Monthly Deforestation Analysis**
```bash
jupyter notebook "Code/Deforestation Analysis.ipynb"
```

**Step 3: Risk Mapping**
```bash
jupyter notebook "Code/Deforestation Risk & Vulnerability Mapping/Deforestation Risk & Vulnerability Mapping.ipynb"
```

**Step 4: Predictive Modeling**
```bash
jupyter notebook "Code/Deforestation Prediction Models/Deforestation Prediction Models.ipynb"
```

## 📊 Outputs

### Deforestation Statistics
- Monthly time series CSV with deforested area (km²)
- Trend visualizations and statistical summaries
- Annual and seasonal pattern analysis

### Risk Maps
- GeoTIFF rasters showing deforestation risk levels
- Proximity-based vulnerability maps
- Forest health degradation zones

### Forecasts
- Short-term predictions (12 months ahead)
- Long-term projections through 2030
- Model performance comparisons (MAE, RMSE, R²)
- Confidence intervals for predictions

## 📈 Results & Insights

The analysis provides:
- **Temporal Trends**: Identification of deforestation acceleration/deceleration periods
- **Spatial Hotspots**: High-risk zones requiring immediate conservation action
- **Future Scenarios**: Data-driven projections for forest management planning
- **Model Accuracy**: Comparative evaluation of forecasting approaches

## 🛠️ Technical Details

- **Coordinate System**: WGS84 (EPSG:4326)
- **Spatial Resolution**: 30 meters (Landsat)
- **Temporal Resolution**: Monthly composites
- **Processing**: Cloud-masked surface reflectance data
- **File Format**: GeoTIFF for rasters, GeoJSON for vectors, CSV for statistics

## 📝 Notes

- `.tif` files are excluded from version control due to size (see `.gitignore`)
- Satellite imagery must be pre-downloaded from USGS Earth Explorer
- Processing time varies with dataset size (~5-10 minutes per year)
- Models trained on 85% of data, tested on remaining 15%

## 🤝 Contributing

Contributions are welcome! Areas for enhancement:
- Additional predictive models (LSTM, XGBoost)
- Real-time data integration via Google Earth Engine
- Web-based interactive dashboard
- Automated alert system for high-risk zones

## 📄 License

This project is available for academic and research purposes.

## 👤 Author

Geospatial Analysis Project - Matale District Deforestation Monitoring

## 🔗 References

- Landsat Data: [USGS Earth Explorer](https://earthexplorer.usgs.gov/)
- GADM Boundaries: [GADM Database](https://gadm.org/)
- Remote Sensing Indices: Zha et al. (2003), Tucker (1979)

---

**Last Updated**: December 2025  
**Study Area**: Matale District, Central Province, Sri Lanka  
**Contact**: yasiruchamithlansakara@gmail.com
