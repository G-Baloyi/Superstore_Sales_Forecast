# Sales & Demand Forecasting — Superstore Dataset

---

## Objective

Predict future monthly sales using historical order data from the Superstore dataset. This notebook walks through every step — data loading, cleaning, feature engineering, model training, evaluation, and business-friendly visualisation — so the results can be presented directly to a store owner, startup founder, or business manager.

---

## Notebook Contents

| Section | Description |
|---|---|
| 1. Data Loading & Exploration | Load the CSV, inspect shape, columns, and summary statistics |
| 2. Data Cleaning & Preprocessing | Parse dates, check for duplicates, missing values, and loss-making orders |
| 3. Time-Based Feature Engineering | Aggregate to monthly sales and build 8 forecasting features |
| 4. Exploratory Data Analysis | Visualise trends, seasonality, category and regional breakdowns |
| 5. Forecasting Models | Train and compare Linear Regression, Random Forest, and Gradient Boosting |
| 6. Model Evaluation & Error Analysis | MAE, RMSE, MAPE, R² comparison + actual vs predicted + residuals |
| 7. Future Sales Forecast | Recursive 6-month forecast with 95% confidence band |
| 8. Feature Importance | Which features drive the forecast most |
| 9. Category-Level Breakdown | Sales trends per product category with trend lines |
| 10. Business Insights & Planning | Inventory, cash flow, staffing, and discount recommendations |

---

## Dataset

| Property | Detail |
|---|---|
| **Source** | [Superstore Sales Dataset — Kaggle](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final) |
| **File** | `Sample_-_Superstore.csv` |
| **Period** | January 2014 – December 2017 |
| **Records** | 9,994 transactions · 21 columns |
| **Pre-cleaned** | Yes — no missing values or duplicates |

> Place `Sample_-_Superstore.csv` in the same directory as the notebook before running.

---

## Feature Engineering

Daily transactions are aggregated to **monthly totals**, then the following features are engineered for the model:

| Feature | Why it helps |
|---|---|
| `month_num` | Sequential integer — captures the long-run upward trend |
| `month` | Calendar month (1–12) — raw intra-year seasonality signal |
| `quarter` | Quarter of year (1–4) — captures quarter-end purchasing spikes |
| `year` | Annual level shift |
| `sin_month` / `cos_month` | Smooth cyclic encoding — avoids an artificial Jan→Dec jump |
| `lag_1` | Sales from 1 month ago — autoregressive signal |
| `lag_3` | Sales from 3 months ago — medium-term autoregressive signal |
| `rolling_mean_3` | 3-month rolling average — smoothed recent trend |

---

## Models & Evaluation

Three models are trained on the same feature set and compared side by side:

| Model | Role |
|---|---|
| **Linear Regression** | Simple, interpretable baseline (features scaled with `StandardScaler`) |
| **Random Forest** | Ensemble — captures non-linear patterns and feature interactions |
| **Gradient Boosting** | Ensemble — typically the strongest performer on tabular time-series |

**Train / Test split:** First 80% of months for training, last 20% held out for evaluation — temporal order is strictly preserved to prevent data leakage.

**Evaluation metrics:**

| Metric | What it measures |
|---|---|
| **MAE** | Average dollar error per month — easy to explain to stakeholders |
| **RMSE** | Penalises large errors more heavily than MAE |
| **MAPE** | Average percentage error — intuitive for business reporting |
| **R²** | Proportion of variance explained (1.0 = perfect) |

The best-performing model (lowest MAPE) is automatically selected for the 6-month forecast.

---

## Forecast

The best model is extended **6 months into the future** using a **recursive multi-step approach** — each predicted month is fed back as the `lag_1` feature for the next prediction. A **95% confidence band** is calculated from the residual standard deviation of the test period.

---

## Visualisations Produced

| File | Description |
|---|---|
| `eda_overview.png` | 2×2 panel — monthly trend, category lines, seasonality bars, regional bars |
| `model_comparison.png` | MAE / RMSE / MAPE bar chart across all three models |
| `actual_vs_predicted.png` | Time-series actual vs predicted + residuals bar chart |
| `sales_forecast.png` | Business-friendly 6-month forecast with confidence band and value annotations |
| `feature_importance.png` | Side-by-side feature importance for Random Forest and Gradient Boosting |
| `category_breakdown.png` | Monthly sales per category with linear trend lines and summary annotations |

---

## Key Business Insights

1. **Consistent upward trend** — sales grew steadily from 2014 to 2017 across all categories and regions.
2. **Strong Q4 seasonality** — September, November, and December spike every year. Plan inventory and staffing 6–8 weeks ahead of Q4.
3. **Technology is the growth engine** — fastest-growing category with the steepest trend slope.
4. **West and East dominate** — together they generate over 61% of total revenue.
5. **~30% of orders are loss-making** — driven by excessive discounting, especially in Furniture. A discount policy review is recommended.
6. **Use the confidence band for risk management** — plan budgets against the lower bound and stock orders against the mid-point forecast.

---

## How to Run

**1. Install dependencies**

```bash
pip install pandas numpy scikit-learn matplotlib seaborn jupyter
```

**2. Place the dataset in the project folder**

```
Sales_Forecasting_Superstore.ipynb
Sample_-_Superstore.csv          ← place here
```

**3. Launch Jupyter and run all cells**

```bash
jupyter notebook Sales_Forecasting_Superstore.ipynb
```

Run cells top to bottom — each section builds on the previous one.

---

## Dependencies

```
pandas
numpy
scikit-learn
matplotlib
seaborn
jupyter
```

---

## Authors

**Goitsemang Baloyi**

---
