# Aircraft-Fuel-Consumption-Analysis-Prediction
# Aircraft Fuel Consumption Analytics & Predictive Modeling

## Project Overview
This project focuses on operational data analytics and predictive modeling for commercial aviation fuel consumption. Utilizing multi-feature time-series CSV data, the primary objective was to build a statistically rigorous, reproducible pipeline to isolate key drivers of fuel burn and establish a reliable forecasting baseline for corporate budget and logistics planning.

## Data Engineering & Statistical Diagnostics
To ensure data integrity and eliminate mathematical noise before modeling, a strict preprocessing and feature selection pipeline was implemented:
* **Data Curation:** Addressed correlation anomalies by utilizing heatmaps to identify and drop zero-variance features, eliminating `NaN` biases.
* **Feature Engineering:** Constructed localized target variables using lag features, temporal differencing (`.diff()`), and percentage change (`.pct`) transformations to stabilize non-stationary data.
* **Dimensionality Reduction:** Conducted an ordinary least squares (OLS) regression using Stepwise Backward Elimination to systematically drop statistically insignificant features and isolate core operational predictors.

## Model Evaluation & Addressing Spurious Regressions
Initial baseline modeling on absolute fuel metrics yielded an artificially inflated $R^2$ of 0.99. A deep-dive diagnostic revealed structural data leakage and spurious regression caused by the natural continuous upward/downward drift (trend) of sequential aviation flight paths. 

To correct this and build a production-ready model, the pipeline was re-architected:
* **Stationarity Transformation:** Converted the absolute fuel target into incremental changes utilizing temporal differencing and lag momentum mechanics to stabilize the mean and track true consumption momentum rather than static trends.
* **Anomaly Purging:** Conducted a structural variance audit across a 9-fold `TimeSeriesSplit` (TSCV) to identify and drop key operational anomaly rows (Indices: 5146, 5797) disrupting fold splits.
* **Final Model Architecture:** Benchmarked linear, tree, and ensemble architectures. Regularized **Lasso Regression** optimized via TSCV successfully bypassed high multi-collinearity to isolate true tracking capabilities, achieving a verified training $R^2$ of **0.9488**.

## Model Performance Visualization
The plot below highlights the model's final tracking performance, mapping the predicted fuel consumption curves directly against actual historical operational variances using Seaborn to communicate structural forecast errors:

<img width="1388" height="590" alt="output_98_0" src="https://github.com/user-attachments/assets/6bd28f4b-d6d1-4528-a0bb-21a2a41d3857" />

