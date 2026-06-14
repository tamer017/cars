# Car Price Prediction — Regression Analysis

> **End-to-end regression pipeline predicting used car market prices from specifications, comparing Linear Regression, Ridge, Lasso, Random Forest, and XGBoost with feature engineering and SHAP explainability.**

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![Scikit-learn](https://img.shields.io/badge/Scikit--learn-1.0+-orange.svg)](https://scikit-learn.org/)
[![XGBoost](https://img.shields.io/badge/XGBoost-1.6+-blue.svg)](https://xgboost.readthedocs.io/)

---

## Overview

This project builds a **used car price prediction system** using regression techniques. The dataset contains vehicle specifications (make, model, year, mileage, engine size, fuel type) and target sale prices. The goal is to train a model accurate enough for pricing guidance at dealerships or online marketplaces.

---

## Dataset

| Feature | Type | Description |
|---|---|---|
| `year` | Numerical | Manufacturing year |
| `mileage` | Numerical | Odometer reading (km) |
| `engine_size` | Numerical | Engine displacement (L) |
| `fuel_type` | Categorical | Petrol / Diesel / Electric / Hybrid |
| `transmission` | Categorical | Manual / Automatic / Semi-Auto |
| `make` | Categorical | Manufacturer (BMW, Toyota, Ford...) |
| `model` | Categorical | Specific model name |
| `price` | **Target** | Sale price (£) |

---

## Feature Engineering

```python
# 1. Car age (more intuitive than year)
df['age'] = 2024 - df['year']

# 2. Log-transform price (right-skewed)
df['log_price'] = np.log1p(df['price'])

# 3. Mileage per year (depreciation proxy)
df['mileage_per_year'] = df['mileage'] / (df['age'] + 1)

# 4. Premium brand flag
premium_brands = ['BMW', 'Mercedes', 'Audi', 'Porsche', 'Lexus']
df['is_premium'] = df['make'].isin(premium_brands).astype(int)

# 5. Encode categoricals
df = pd.get_dummies(df, columns=['fuel_type', 'transmission'])
```

---

## Model Comparison

| Model | RMSE (£) | MAE (£) | R² |
|---|---|---|---|
| Linear Regression | 4,821 | 3,102 | 0.73 |
| Ridge (α=10) | 4,798 | 3,087 | 0.74 |
| Lasso (α=1) | 4,834 | 3,118 | 0.73 |
| Random Forest | 2,943 | 1,876 | 0.89 |
| **XGBoost** | **2,611** | **1,654** | **0.92** |

**XGBoost** achieves the best performance: RMSE £2,611 and R²=0.92.

---

## SHAP Feature Importance

Top features by mean |SHAP value|:
1. `age` — most important (depreciation dominates price)
2. `mileage` — strong negative impact
3. `engine_size` — positive for larger engines
4. `is_premium` — significant premium brand effect
5. `fuel_type_Electric` — positive premium for EVs

---

## XGBoost Hyperparameters (Final)

```python
xgb_model = XGBRegressor(
    n_estimators=500,
    max_depth=6,
    learning_rate=0.05,
    subsample=0.8,
    colsample_bytree=0.8,
    reg_alpha=0.1,
    reg_lambda=1.0,
    random_state=42
)
```

---

## Installation

```bash
git clone https://github.com/tamer017/cars.git
cd cars
pip install scikit-learn xgboost pandas numpy matplotlib shap jupyter
jupyter notebook
```

---

## Skills & Concepts

`Regression` `XGBoost` `Random Forest` `Ridge` `Lasso` `Feature Engineering` `SHAP` `Price Prediction` `Log Transformation` `Cross-Validation` `Automotive Analytics`

---

## Author

**Ahmed Tamer Assy** — [GitHub](https://github.com/tamer017) | Machine Learning Researcher @ Volkswagen AG
