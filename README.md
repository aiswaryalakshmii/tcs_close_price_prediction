# Explainable ML Model to Predict Next-Day Close Price of TCS

This project aims to build a transparent and explainable machine learning model to predict the **next-day close price** of Tata Consultancy Services (TCS) stock. Rather than being a black box, the model prioritizes **interpretability** using SHAP (SHapley Additive exPlanations), aligning with real-world financial industry standards.

---

## Objective

- Predict TCS stock’s next-day close price using past price data and technical indicators.
- Evaluate and compare multiple regression models (XGBoost, Random Forest, Lasso).
- Ensure model interpretability using SHAP.


---

## Dataset

- Source: Historical TCS stock data (NSE)
- Columns used:
  - `Date`, `Open`, `High`, `Low`, `Close`, `Volume`
- Total records: ~5,000 daily entries since 2004

---

## Features Used

| Feature | Description |
|--------|-------------|
| `lag_1`, `lag_2`, `lag_3` | Close prices of previous 1, 2, and 3 days |
| `rolling_mean_3` | 3-day rolling mean of Close price |
| `rolling_std_5` | 5-day rolling standard deviation |
| `daily_return` | Percentage daily return |
| `rsi` | Relative Strength Index |
| `macd` | Moving Average Convergence Divergence |
| `target` | Next day’s close price (shifted -1) |

---

## Workflow

### Step 1: Data Cleaning
- Parsed `Date` column and sorted chronologically
- Removed missing values
- Converted prices to numeric where needed

### Step 2: Feature Engineering
- Created lag features
- Calculated rolling averages and std deviations
- Computed technical indicators using `ta` library

### Step 3: Model Building
- Models used:
  - `XGBoostRegressor`
  - `RandomForestRegressor`
  - `Lasso` 
- Data split: 80% train / 20% test (no shuffle)

### Step 4: Evaluation Metrics

| Model | RMSE | MAE | MAPE |
|-------|------|-----|------|
| XGBoost | 987.34 | 773.84 | 20.29% |
| Random Forest | 948.87 | 717.93 | 18.55% |
| Lasso | **47.62** | **34.23** | **0.95%** |

> Note: Lasso performed unusually well, likely due to strong linear relationships in engineered features + regularization + scaled inputs.

###  Step 5: Explainability with SHAP
To enhance transparency, SHAP (SHapley Additive exPlanations) was used to interpret the predictions made by the best-performing model.
By incorporating SHAP, this project ensures interpretability — a critical requirement for models used in financial forecasting and enterprise decision-making.
####  SHAP Summary Plot
- **Top features**:
  - `lag_3`, `rsi`, `macd`, `lag_1`, `lag_2`
- SHAP showed strong dependence on `lag_3`, confirming temporal influence.

#### SHAP Bar Plot (Mean Importance)
| Feature         | Mean SHAP Value |
|----------------|------------------|
| `lag_3`        | 252.14           |
| `rsi`          | 52.67            |
| `macd`         | 41.52            |
| `lag_1`        | 29.66            |
| `lag_2`        | 21.05            |
| `rolling_mean_3` | 15.57          |

#### SHAP Waterfall Plot
- Prediction explained for a single instance.
- Base value: ~2917.70 → Final prediction: ~3108.34
- Major driver: `lag_3` increased predicted value by ~171.61

#### Key Insights:

- Top Influencers: SHAP analysis revealed that `lag_3`, `rsi`, and `macd` were the most influential features driving the model’s predictions.

- Consistent Dependence on `lag_3`: The model heavily relied on the third-day lag value (`lag_3`), highlighting the persistence of short-term stock movement trends.

- Technical Indicators Matter: RSI (Relative Strength Index) and MACD (Moving Average Convergence Divergence) significantly influenced the predicted close price, suggesting that momentum and trend indicators add meaningful value.

- Waterfall Plot Insight: For an individual prediction, SHAP showed how the base value was adjusted by each feature, clearly explaining the reasoning behind the final predicted price.
---
#### Author  
Aiswarya Lakshmi  
MSc in Survey Research and Data Analytics  
International Institute for Population Sciences (IIPS), Mumbai
