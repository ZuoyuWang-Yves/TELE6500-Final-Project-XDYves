
# Cluster-based Regression for RUL Prediction (CMAPSS dataset)

This repository contains an implementation of a predictive framework for estimating the Remaining Useful Life (RUL) of equipment based on time-series data. The framework combines exploratory data analysis (EDA), feature engineering, time-series segmentation, clustering, and regression modeling to provide robust predictions.

## Exploratory Data Analysis (EDA)
Perform EDA to preprocess the data and ensure stationarity, stabilizing non-stationary features where necessary.
Select features for clustering and modeling based on the following criteria:
1) The feature does not exhibit a simple mathematical relationship with RUL (e.g., linear or exponential trends).
2) The feature shows importance based on the weight calculation in Random Forest algorithm, or the feature is expected to have essential correlations with RUL, providing valuable information about the degradation process.

## Feature Engineering
For all features excluding the ones related to Time_in_Cycle, we compute rolling window statistics: Mean, Variance and Slope.
Use a rolling window of size 20, where the window ends at the corresponding data point. These rolling window features provide localized statistical insights into the operational state of the equipment.

## Time-Series Segmentation and Normalization
Segment each unit's data using the ruptures library to identify change points in the time-series data. Normalize each segment to a fixed length of 100 points using interpolation to ensure consistency.

## Clustering
Cluster the normalized segments for each selected feature (in EDA step) into three clusters using Dynamic Time Warping (DTW) and TimeSeriesKMeans.

## Model Building
For each cluster, train a linear regression model to predict RUL using the both the original columns and the rolling window statistics as features.
Store the regression models and cluster details for use during the prediction phase.

## Test Data Processing and Prediction
Process test data as follows:
- Stabilize the data using the same preprocessing steps applied during EDA.
- Compute rolling window statistics (mean, variance, slope) for each data point.
- Segment the test data and normalize the last segment.
- Assign the last segment to its corresponding cluster for each selected feature.

Generate predictions:
- For a test data point, use the three clusters it is assigned to (one per feature) to predict RUL, using the corresponding regression models for the clusters.
- Compute a weighted average of the predictions, where the weights are calculated based on the R^2 value of each regression model (indicating model performance), and the number of observations in the cluster (indicating model reliability).
- If we're inputing a whole testing dataset, for each given unit, we use a simpleExpSmoothing on the final prediction.


## Performance 
Performance of this framework working on each dataset can be found in the performance documentation.

