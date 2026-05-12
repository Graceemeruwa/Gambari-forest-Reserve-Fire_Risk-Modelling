# Gambari Forest Reserve Fire-Risk Analysis: A Reproducible Study

This repository presents a comprehensive and fully reproducible analysis of fire-risk within the Gambari Forest Reserve. The study integrates various geospatial and climatic datasets to characterize the fire regime, assess climate-fire relationships, validate existing risk maps, and develop data-driven predictive models. All analyses are performed within a Google Colab notebook, designed for end-to-end execution and transparent reproduction of results.

## 1. Introduction and Background

Forest fires pose a significant threat to biodiversity, ecosystem services, and human livelihoods globally. Effective fire management strategies rely on accurate assessments of fire risk, which in turn require a deep understanding of historical fire regimes and the underlying environmental drivers. The Gambari Forest Reserve, like many tropical forest ecosystems, is vulnerable to fire events, necessitating robust analytical approaches to inform conservation and management efforts.

This research aims to provide an in-depth characterization of the fire regime in Gambari Forest Reserve, evaluate the efficacy of existing Analytical Hierarchy Process (AHP) based fire-risk maps, and explore the potential of data-driven modeling techniques for improved fire-risk prediction. The study emphasizes transparency and reproducibility, offering a complete computational workflow implemented in a Google Colab environment.

## 2. Data Sources and Pre-processing

The analysis utilizes a diverse array of spatially explicit and temporal datasets:

*   **Fire Points**: MODIS (Moderate Resolution Imaging Spectroradiometer) active fire detections (M-C61) providing locations, acquisition dates, Fire Radiative Power (FRP), and confidence levels from 2001 to 2025.
*   **Geospatial Predictors**: Raster layers representing key environmental factors, including:
    *   **Vegetation Indices**: NDVI (Normalized Difference Vegetation Index) for 2005, 2015, and 2025.
    *   **Land Surface Temperature (LST)**: Annual averages for 2005, 2015, and 2025.
    *   **Topographic Variables**: Slope, Aspect, and Elevation derived from Digital Elevation Models.
    *   **Proximity Measures**: Distance to human settlements (Place), Roads, and Water bodies.
    *   **Land Use/Land Cover (LULC)**: Categorical map for 2025.
*   **AHP Fire-Risk Map**: A pre-existing 64-meter resolution AHP fire-risk raster for the study area.
*   **Climatic Data**: Annual aggregates of Precipitation, Relative Humidity (RH), and Surface Temperature (NASA POWER, 2000-2025).
*   **Stakeholder Questionnaire**: Survey responses detailing local perceptions of fire causes, impacts, and management challenges.

All geospatial data are reprojected to UTM Zone 31N (EPSG:32631) for consistent spatial analysis. Fire points are sampled against all raster layers to create a comprehensive fire-predictor dataframe.

## 3. Methodology

The analysis proceeds through several key stages:

1.  **Fire Regime Characterization**: Descriptive statistics of fire counts, seasonality, FRP distribution, and detection confidence. Spatial clustering is assessed using the Clark-Evans R statistic. Linear and Mann-Kendall trend tests are applied to annual fire counts.
2.  **Climate-Fire Temporal Coupling**: Quasi-Poisson regression models are used to investigate the relationship between annual fire counts and climatic variables (precipitation, relative humidity, temperature), including lagged effects of precipitation.
3.  **Climate-Raster Diagnostic**: Examination of pixel value distributions for climatic rasters to identify potential interpolation artifacts or lack of spatial variability at the reserve scale.
4.  **AHP Map Validation**: The existing AHP fire-risk map is validated against historical fire locations. Performance metrics include class-level percent-area, percent-fire, lift, Chi-square goodness-of-fit, and Area Under the Receiver Operating Characteristic Curve (AUC).
5.  **Data-Driven Model Comparison**: Four machine learning models—Logistic Regression, Random Forest, Gradient Boosting, and MaxEnt (implemented as L1-penalized logistic regression with hinge features)—are developed using a presence-absence dataset. Model performance is evaluated using in-sample AUC, random k-fold cross-validation, and spatial-block cross-validation.
6.  **Pseudo-Absence Sensitivity Analysis**: The impact of pseudo-absence generation strategy (absence:presence ratio and spatial exclusion radius) on logistic regression performance is systematically investigated.
7.  **Predictor Correlation and VIF Analysis**: Assessment of multicollinearity among continuous predictors using correlation matrices and Variance Inflation Factors (VIF).
8.  **Stratified Logistic Regression**: Logistic regression models are fitted separately for 'edge' and 'interior' zones (stratified by median distance to roads) to explore spatial heterogeneity in predictor effects.
9.  **High-Confidence Detection Subset Analysis**: The AHP and data-driven model validations are repeated using only high-confidence MODIS fire detections (≥ 80%) to assess robustness.
10. **Moran's I on Residuals**: Spatial autocorrelation in logistic regression residuals is tested using Moran's I to determine if spatial patterns remain unexplained by the models.
11. **AHP Weight Sensitivity**: The AHP risk surface is reconstructed under various weighting schemes (original, climate-halved, climate-zeroed, equal) and re-validated to understand the impact of criterion weights on performance.
12. **Stakeholder Perception Summary**: Analysis of questionnaire responses to summarize local perspectives on fire causes, benefits, impacts, and management challenges.

## 4. Key Findings and Implications

This research provides critical insights into fire dynamics and risk assessment in the Gambari Forest Reserve. Key findings include:

*   **Fire Regime Trends**: [Summarize 1-2 most important findings from §6, e.g., increasing/decreasing trend, strong seasonality, typical FRP values].
*   **Climate Influence**: [Summarize 1-2 most important findings from §7, e.g., significant climatic drivers, lagged effects].
*   **AHP Map Performance**: The AHP-derived fire-risk map generally exhibited [e.g., poor discriminatory power, unexpected lift inversion in high-risk classes], particularly with high-confidence fire detections, suggesting limitations in its predictive capacity or parameterization for this specific context.
*   **Data-Driven Model Efficacy**: Data-driven models, especially [e.g., Random Forest/Gradient Boosting], demonstrated [e.g., higher predictive accuracy, more consistent performance] compared to the AHP map, particularly when appropriate pseudo-absence generation strategies were employed.
*   **Pseudo-Absence Importance**: The sensitivity analysis highlighted the crucial role of pseudo-absence sampling, particularly the spatial exclusion radius, in influencing model performance metrics like AUC. This suggests that the definition of 'non-fire' areas significantly impacts model evaluation and development.
*   **Spatial Autocorrelation**: Moran's I analysis indicated [e.g., no significant residual spatial autocorrelation, suggesting that the chosen predictors adequately capture the spatial structure of fire occurrence].
*   **Stakeholder Perspectives**: [Summarize 1-2 key insights from §18, e.g., alignment/divergence between scientific findings and local perceptions, dominant perceived causes/impacts].

The findings underscore the importance of integrating empirical fire data with advanced modeling techniques for robust fire-risk assessment. They also suggest that while expert knowledge (as in AHP) is valuable, its translation into spatially explicit risk maps requires rigorous validation and may benefit from calibration with observed fire patterns and local ecological conditions.

## 5. Notebook Structure and Reproducibility

The accompanying Google Colab notebook (`Gambari_Fire_Risk_Analysis.ipynb`) is structured into logical sections corresponding to the methodology described above. Each section is introduced by a markdown cell and followed by Python code cells that perform the analysis.

**To reproduce this analysis:**

1.  **Clone this repository** or download the notebook.
2.  **Upload the project data archive** (e.g., `Gambari.zip`) to your Google Colab session, or mount your Google Drive and navigate to the project folder containing the data as described in the notebook's "Data location" section.
3.  **Run the notebook cells sequentially** from top to bottom. The notebook includes checks for required libraries and data paths to ensure a smooth execution.
4.  All generated figures and tables are saved to the `/content/output/` directory within the Colab environment (or a custom `OUT_DIR` if specified), including a `summary.json` file containing headline statistics.
