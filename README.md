# ⚡ Electricity Load Forecasting with XGBoost

Accurate electricity demand forecasting is critical for power system planning and operations.
This project implements an **end-to-end time series forecasting pipeline** to predict **aggregated total electricity demand** using historical consumption data and machine learning.

The solution combines **feature engineering**, **baseline modeling**, and an **XGBoost regressor**, achieving **over 94% error reduction** compared to a naive baseline.

---

## 📊 Dataset

* **Source:** Kaggle – *Electricity Load Diagrams 2011–2014*
* **Frequency:** Hourly
* **Coverage:** 2011–2014
* **Features:** Electricity consumption of 370+ customers
* **Target:** Aggregated total electricity load

The dataset is downloaded automatically using `kagglehub`.

---

## 🔄 Project Workflow

### 1️⃣ Data Preprocessing

* Parse semicolon-separated values and comma decimals
* Convert timestamps to `DateTimeIndex`
* Aggregate individual customer loads into a single time series
* Validate data integrity (missing, zero, negative values)

---

### 2️⃣ Exploratory Data Analysis (EDA)

* Long-term electricity demand trends
* Two-week demand visualization
* Average demand patterns:

  * Hourly (daily seasonality)
  * Weekly (weekday vs weekend behavior)
* Seasonal decomposition (trend, seasonal, residual)

---

### 3️⃣ Feature Engineering

Time-based features:

* Hour of day
* Day of week
* Month
* Weekend indicator

Lag features:

* Previous hour (`lag_1`)
* Same hour yesterday (`lag_24`)
* Same hour last week (`lag_168`)

Rolling statistics:

* 24-hour rolling mean & standard deviation
* 168-hour rolling mean

Rows with missing lag values are removed to prevent data leakage.

---

### 4️⃣ Train–Test Split

* **Training:** Up to **June 30, 2014**
* **Testing:** From **July 1, 2014** onward
* Chronological split ensures realistic forecasting conditions

---

### 5️⃣ Baseline Model

A persistence baseline using:

```
Prediction(t) = Load(t − 24)
```

This captures daily seasonality but ignores trend and nonlinear effects.

---

### 6️⃣ Machine Learning Model

**XGBoost Regressor**

Key parameters:

* `n_estimators = 500`
* `learning_rate = 0.05`
* `max_depth = 7`
* `subsample = 0.8`
* `colsample_bytree = 0.8`
* Objective: `reg:squarederror`

---

## 📈 Results

### 🔢 Quantitative Evaluation

| Model                 |        MAE ↓ |       RMSE ↓ |
| --------------------- | -----------: | -----------: |
| **Baseline (Lag-24)** |    94,123.07 |   118,049.83 |
| **XGBoost**           | **4,293.84** | **6,663.40** |

---

### 🚀 Performance Improvement

| Metric | Reduction |
| ------ | --------- |
| MAE    | **95.4%** |
| RMSE   | **94.4%** |

---

### 📊 Visual Analysis

The results show:

* **Excellent alignment** between actual and predicted demand
* Accurate capture of:

  * Daily load cycles
  * Weekly seasonality
  * Seasonal demand shifts
* Strong short-term forecasting accuracy in 1-week zoomed views
* Clear identification of peak and off-peak hours

---

## 🧠 Key Insights

* Electricity demand exhibits strong **daily and weekly seasonality**
* Lagged and rolling features are highly informative
* Tree-based models handle nonlinear demand patterns effectively
* XGBoost vastly outperforms naive baselines in real-world forecasting scenarios

---

## 🛠️ Tech Stack

* Python
* Pandas, NumPy
* Matplotlib
* Statsmodels
* Scikit-learn
* XGBoost
* KaggleHub

---

## 🚀 Future Work

* Hyperparameter tuning with time-series cross-validation
* Integration of weather data
* Probabilistic forecasting (quantiles)
* Deep learning models (LSTM, Temporal CNN)
* Multi-step forecasting horizons

---


---


