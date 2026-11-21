# PM2.5 Estimation in the Kathmandu Valley Using Tree-Based Ensemble Machine Learning

Comparative analysis of Random Forest, Gradient Boosting, XGBoost, and LightGBM for estimating daily PM2.5 concentrations from satellite-derived and environmental variables in the Kathmandu Valley, Nepal.

*Author:* Gremmy Dhungana, B.E. Geomatics Engineering, Tribhuvan University (2026)

## Overview

The Kathmandu Valley is one of the most polluted regions in South Asia, but it has only a small number of ground monitoring stations. This project tests how well tree-based ensemble machine learning models can estimate daily PM2.5 using freely available satellite and climate data matched to ground measurements.

Four models are tuned with Bayesian hyperparameter optimization and compared on a held-out test set.

## Data

Ground-level PM2.5 from five monitoring stations in the Kathmandu Valley, matched by date and station to the following predictors (extracted with Google Earth Engine):

| Feature | Description |
|---|---|
| AOD | Aerosol optical depth (MODIS) |
| NDVI | Vegetation index (Landsat) |
| LST_Celsius | Land surface temperature (MODIS) |
| Precip_mm | Precipitation (CHIRPS) |
| SoilMoist_m3m3 | Soil moisture |
| month_sin, month_cos | Cyclical encoding of the month, to represent seasonality |

The target variable is pm2.5.


## Methods

1. *Preprocessing:* parse dates and create cyclical month features.
2. *Split:* random 80% training / 20% test split (random_state=42).
3. *Tuning:* [Optuna](https://optuna.org/) with 60 trials per model, maximizing mean R² over 5-fold cross-validation on the training set.
4. *Evaluation:* the best configuration of each model is retrained on the full training set and scored on the test set using R², RMSE, and MAE.
5. *Interpretation:* feature importance (normalized and averaged across models) and actual-versus-predicted scatter plots.

## Results

Test-set performance:

| Model | R² | RMSE (µg/m³) | MAE (µg/m³) |
|---|---|---|---|
| *Gradient Boosting* | *0.733* | *19.81* | *13.61* |
| XGBoost | 0.732 | 19.87 | 13.77 |
| LightGBM | 0.711 | 20.61 | 14.48 |
| Random Forest | 0.696 | 21.15 | 14.09 |

Gradient Boosting and XGBoost performed almost identically. AOD and seasonality were the most influential predictors. All models tended to underestimate high-pollution episodes.


