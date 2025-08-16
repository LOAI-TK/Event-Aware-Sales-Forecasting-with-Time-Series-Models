# Event Aware Sales Forecasting with Time Series Models
This repository contains an end-to-end pipeline for forecasting retail sales while incorporating calendar effects, Hijri-based holidays, and marketing events. The project leverages XGBoost with time-series features such as lags, rolling windows, and event-based indicators.

---

# Dataset

The dataset (sales_marketing.csv) contains daily sales by category and channel with event tags:

| Column          | Description                                      |
| --------------- | ------------------------------------------------ |
| `CAL_YEAR`      | Gregorian year                                   |
| `CAL_DATE`      | Gregorian date (daily)                           |
| `HIJRI_DATE`    | Hijri (Islamic) date                             |
| `CATEGORY_CODE` | Product category code                            |
| `CATEGORY_DESC` | Product category description                     |
| `CHANNEL_NAME`  | Sales channel (e.g., E-Commerce, Brick & Mortar) |
| `SALES_AMOUNT`  | Total daily sales (SAR)                          |
| `Main Event`    | Marketing campaign, promotion, or seasonal event |

---

#  Pipeline Overview
## 1️⃣ Preprocessing (1_preprocessing.py)

- Cleans and validates Gregorian and Hijri dates.

- Tags Islamic holidays:

- Ramadan

- Eid al-Fitr

- Eid al-Adha

- Removes sales outliers (≤ 1000 SAR).

- Aggregates daily sales by category & channel.

- Saves processed dataset → processed_sales.csv.


## 2️⃣ Feature Engineering (2_feature_engineering.py)

- Creates Gregorian features: weekday, weekend flag, month start/end, quarter, day of year, etc.

- Extracts Hijri month and aligns Ramadan campaigns.

- Maps raw marketing campaigns into 8 event categories:

- Ramadan_Campaign

- Eid

- Mega_Sale

- Product_Launch

- Campaign_Driven

- Back_To_School

- Seasonal_Event

- National_Event

- One-hot encodes event categories.

- Generates seasonal indicators (Is_Summer, Is_Winter, Is_Spring, Is_Vacation).

- Saves enriched dataset → sales_with_features.csv.


## 3️⃣ XGBoost Modeling (3_xgboost_model.py)

- Creates lag & rolling features (4, 8, 12 periods) per category-channel.

- Train/test split:

- Train: until Dec 2024

- Test: Jan–May 2025

- Trains XGBoost Regressor with tuned hyperparameters.

- Evaluates model with:

- MAE (Mean Absolute Error)

- RMSE (Root Mean Square Error)

- SMAPE (Symmetric Mean Absolute Percentage Error)

- Correlation between actual vs predicted sales

## Visualizations:

- Actual vs predicted sales

- Feature importance (top 20)

- Scatter plot (Actual vs Predicted)

- Exports predictions → sales_predictions_jan_may_2025.csv.

---

# Results

- Forecasts capture the effect of promotions, Ramadan, and Mega Sales.

- Model evaluation includes error metrics and correlation analysis.

- Predictions are plotted over time for validation.


# Future Work

- Add walk-forward validation instead of a single cutoff.

- Experiment with seasonal lags (24, 36 periods).

- Add interaction features (e.g., Ramadan × Weekend).

- Compare with Prophet, LSTM, or Temporal Fusion Transformers.

# Tech Stack

- Python 3.9+

- Pandas, NumPy → data wrangling

- Seaborn, Matplotlib → visualization

- Scikit-learn → metrics

- XGBoost → forecasting model
