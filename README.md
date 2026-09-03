# CSE437 Data Science Project

## Project Title

Predicting Seattle Building Site Energy Use Intensity Using Building and Property Characteristics

## Problem

This project predicts Seattle building Site Energy Use Intensity (SiteEUI) using physical, structural, location, and property-use characteristics.

Variables that directly reveal energy consumption were excluded to prevent target leakage.

## Dataset

City of Seattle Building Energy Benchmarking Data, 2015–Present.

Dataset source:

https://data.seattle.gov/Built-Environment/Building-Energy-Benchmarking-Data-2015-Present/teqw-tu6e/about_data

## Research Questions

1. Which physical and structural characteristics of buildings, such as building type, age, size, number of floors, and location, have the strongest relationship with Site Energy Use Intensity?

2. How accurately can Site Energy Use Intensity be predicted using only building and property characteristics, without using variables that directly reveal energy consumption?

3. Which machine-learning model performs best for predicting Site Energy Use Intensity, and for which types of buildings does the final model produce the largest prediction errors?

## Models

- Ridge Regression
- Histogram Gradient Boosting Regression

## How to Run

1. Install the required Python packages using:

   `pip install -r requirements.txt`

2. The original dataset is located in `data/raw/`.

3. Run the notebooks in numerical order:

   - `01_data_audit_and_eda.ipynb`
   - `02_preprocessing.ipynb`
   - `03_feature_engineering.ipynb`
   - `04_modeling_and_tuning.ipynb`
   - `05_evaluation_and_error_analysis.ipynb`

4. Each notebook should be executed from top to bottom.

## Final Model Result

The tuned Histogram Gradient Boosting model achieved:

- MAE: 17.685
- RMSE: 37.975
- R²: 0.385
- RMSLE: 0.476
- Log-scale R²: 0.411

## Report

The final report is available in:

`report/report.pdf`
