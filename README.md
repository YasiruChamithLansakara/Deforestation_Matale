# Deforestation Analysis - Matale District, Sri Lanka

A comprehensive satellite-based deforestation monitoring and prediction system for Matale District using Landsat imagery and machine learning.

## 📋 Overview

This project analyzes temporal deforestation patterns in Matale District, Sri Lanka, using Landsat Surface Reflectance data (2013-2025). It combines remote sensing indices (NDVI, NDBI) with advanced predictive models to monitor forest loss, identify high-risk zones, and forecast future deforestation trends.

### Key Features

- **Cloud-Masked Monitoring**: Automated processing of Landsat imagery with intelligent cloud masking (>50% threshold)
- **Data Quality Assessment**: Multi-tier quality classification based on cloud coverage and valid pixel counts
- **Risk Mapping**: Spatial analysis identifying vulnerable forest zones using proximity and vegetation health metrics
- **Advanced Predictive Modeling**: Multi-model forecasting (Prophet, ARIMA, SARIMA, XGBoost, Random Forest) with cloud coverage as predictor
- **Temporal Analysis**: Time series visualization and trend analysis of forest cover changes
- **Long-term Projections**: Year-by-year forecasts through 2030 with scenario analysis
- **Automated Pipeline**: End-to-end workflow from satellite data to actionable insights

## 🛰️ Data Sources

- **Satellite Imagery**: Landsat 8/9 Collection 2 Surface Reflectance (30m resolution)
  - Bands: Blue (B2), Green (B3), Red (B4), NIR (B5), SWIR1 (B6)
  - Temporal Coverage: 2013 - 2025
  - Cloud Masking: Scenes with >50% cloud coverage excluded
  - Quality Metrics: Cloud coverage percentage and valid pixel counts tracked
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
│   ├── Monthly_Deforestation_Stats.csv # Time series statistics
│   ├── Cloud_Coverage_Stats.csv        # Cloud coverage metrics per scene
│   └── Composites/                     # Monthly composite imagery
├── Deforestation_Risk_Analysis/        # Risk assessment outputs
│   ├── deforestation_risk_score.tif
│   ├── deforestation_risk_zones.tif
│   ├── proximity_risk.tif
│   └── forest_vulnerability.tif
└── Model_Comparison/                   # Prediction model results
    ├── Model_Comparison_Results.csv
    ├── Deforestation_Forecast_2030.csv
    ├── Yearly_Projections_2030.csv
    ├── Deforestation_with_Clusters.csv
    └── *.png                           # Visualizations
```

## 🔬 Methodology

### 1. Data Quality Control & Cloud Masking
- **Cloud Filtering**: Scenes with >50% cloud coverage automatically excluded
- **Valid Pixel Tracking**: Monitor number of cloud-free pixels per scene
- **Quality Tiers**: Classify data quality as High (<30% clouds), Medium (30-50%), or Low (>50%)
- **Processing**: Only high and medium quality scenes used for analysis

### 2. Deforestation Detection
- **NDVI (Normalized Difference Vegetation Index)**: Measures vegetation health
  ```
  NDVI = (NIR - Red) / (NIR + Red)
  ```
- **NDBI (Normalized Difference Built-up Index)**: Detects bare soil/deforested areas
  ```
  NDBI = (SWIR1 - NIR) / (SWIR1 + NIR)
  ```
- **Classification**: Pixels with NDBI > 0.1 are classified as deforested

### 3. Risk Assessment
- **Proximity Analysis**: Distance to existing deforested areas using Euclidean distance transform
- **Forest Vulnerability**: Inverse of NDVI indicating degraded forest health
- **Risk Scoring**: Weighted combination of proximity risk and vegetation vulnerability
- **Zone Classification**: 5-tier risk levels (0=already deforested, 1-5=increasing risk)

### 4. Predictive Models (Cloud-Aware)
- **Prophet**: Time series forecasting with yearly seasonality and cloud coverage as additional regressor
- **ARIMA**: Autoregressive Integrated Moving Average (1,1,1) for trend analysis
- **SARIMA**: Seasonal ARIMA with monthly patterns (1,1,1)(1,1,1,12) for cyclical trends
- **XGBoost**: Gradient boosting with enhanced features (temporal + cloud coverage + valid pixels)
- **Random Forest**: Ensemble learning with 300 trees using temporal and quality features
- **K-Means Clustering**: Temporal pattern discovery incorporating cloud coverage patterns
- **Feature Engineering**: Month, year, quarter, cloud coverage %, and valid pixel counts
- **Model Selection**: Best model chosen by RMSE performance on test set (typically Random Forest)

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
- Monthly time series CSV with deforested area (km²) and cloud coverage metrics
- Data quality assessment per scene (High/Medium/Low)
- Valid pixel counts and cloud coverage percentages
- Trend visualizations and statistical summaries
- Annual and seasonal pattern analysis

### Risk Maps
- GeoTIFF rasters showing deforestation risk levels
- Proximity-based vulnerability maps
- Forest health degradation zones

### Forecasts
- Short-term predictions (12 months ahead) using Random Forest
- Long-term projections through 2030 with yearly breakdowns
- Model performance comparisons across 5 models (MAE, RMSE, R²)
- Feature importance analysis (temporal, cloud, and quality factors)
- Scenario analysis with cumulative increase projections
- Clustered temporal patterns (K-Means with 3 clusters)

## 📈 Results & Insights

The analysis provides:
- **Cloud-Corrected Trends**: Reliable temporal patterns with cloud-masked data ensuring accuracy
- **Data Quality Metrics**: Understanding of observation reliability based on cloud coverage
- **Spatial Hotspots**: High-risk zones requiring immediate conservation action
- **Future Scenarios**: Data-driven projections for forest management planning through 2030
- **Model Accuracy**: Comparative evaluation of 5 forecasting approaches with cloud-aware features
- **Feature Impact**: Quantified influence of cloud coverage and data quality on predictions

## 🛠️ Technical Details

- **Coordinate System**: WGS84 (EPSG:4326)
- **Spatial Resolution**: 30 meters (Landsat)
- **Temporal Resolution**: Monthly composites
- **Data Collection**: Landsat Collection 2 Surface Reflectance
- **Cloud Masking**: Automated filtering at 50% threshold with quality tracking
- **Processing Pipeline**: Cloud-aware feature engineering for predictive models
- **Model Training**: 85% train / 15% test split with temporal validation
- **File Format**: GeoTIFF for rasters, GeoJSON for vectors, CSV for statistics

## 📝 Notes

- `.tif` files are excluded from version control due to size (see `.gitignore`)
- Satellite imagery must be pre-downloaded from USGS Earth Explorer (Collection 2)
- Cloud masking automatically filters scenes with >50% cloud coverage
- Processing time varies with dataset size (~5-10 minutes per year)
- Models trained on 85% of cloud-masked data, tested on remaining 15%
- Cloud coverage and valid pixel counts used as predictive features

## 🤝 Contributing

Contributions are welcome! Areas for enhancement:
- Additional predictive models (LSTM, deep learning approaches)
- Enhanced cloud removal techniques (multi-temporal compositing)
- Real-time data integration via Google Earth Engine
- Web-based interactive dashboard
- Automated alert system for high-risk zones
- Integration of additional environmental variables (rainfall, temperature)

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
