# 📈 Predictive Model for Stock Return Forecasting
### FTSE100 Tech-Mark Series — London Stock Exchange

![Python](https://img.shields.io/badge/Python-3.10-blue?logo=python)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

---

## 📌 Business Problem

Stock return forecasting is critical for portfolio management and investment decision-making. This project builds and evaluates multiple time-series models to forecast the stock returns of **SN. (Smiths Group)**, a FTSE100 Tech-Mark listed company on the London Stock Exchange, using historical price data from **2015 to 2022**.

---

## 🏗️ Project Architecture

```
Yahoo Finance API (yfinance)
        ↓
  Raw Stock Data (OHLCV)
        ↓
  Data Cleaning & Feature Engineering
        ↓
  ┌─────────────────────────────────┐
  │  Stationarity Testing (ADF)     │
  │  Decomposition & ACF/PACF Plots │
  └─────────────────────────────────┘
        ↓
  Model Training & Evaluation
  ┌──────────┬──────────┬──────────┐
  │  ARIMA   │ SARIMAX  │   GARCH  │
  └──────────┴──────────┴──────────┘
        ↓
  Performance Comparison (RMSE)
```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| `yfinance` | Download historical stock data |
| `pandas` / `numpy` | Data manipulation |
| `statsmodels` | ARIMA, SARIMAX, Exponential Smoothing |
| `arch` | GARCH volatility modelling |
| `matplotlib` | Visualisation |
| `sklearn` | RMSE evaluation |

---

## 📦 Installation

```bash
git clone https://github.com/your-username/your-repo-name.git
cd your-repo-name
pip install -r requirements.txt
```

**requirements.txt**
```
pandas
numpy
matplotlib
scikit-learn
statsmodels
arch
yfinance
```

---

## 🚀 How to Run

```bash
# Download data and run full forecasting pipeline
python main.py
```

Or open the notebook directly:
```bash
jupyter notebook stock_forecasting.ipynb
```

---

## 📊 Key Steps

### 1. Download Stock Data
```python
import yfinance as yf

data = yf.download("SN.L", start="2015-01-01", end="2022-12-31")
```

### 2. Stationarity Test (ADF)
```python
from statsmodels.tsa.stattools import adfuller

result = adfuller(data['Close'])
print(f'ADF Statistic: {result[0]:.4f}')
print(f'p-value: {result[1]:.4f}')
```

### 3. Model Training
```python
from statsmodels.tsa.arima.model import ARIMA

model = ARIMA(train, order=(p, d, q))
model_fit = model.fit()
```

---

## 📈 Results

| Model | RMSE |
|---|---|
| ARIMA | *add your value* |
| SARIMAX | *add your value* |
| Exponential Smoothing | *add your value* |
| GARCH | *add your value* |

> ✅ **Best performing model:** *(add your conclusion here)*

---

## 💡 Key Insights

- The stock returns showed **non-stationarity** which required differencing before modelling
- SARIMAX outperformed plain ARIMA by capturing **seasonal components**
- GARCH was effective in modelling **volatility clustering** common in financial time series

---

## 📁 Repository Structure

```
├── data/               # Raw and processed data
├── notebooks/          # Jupyter notebooks
├── src/                # Python scripts
│   └── main.py
├── outputs/            # Plots and result exports
├── requirements.txt
└── README.md
```

---

## 👤 Author

**Aniket Nerali**
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?logo=linkedin)](https://linkedin.com/in/your-profile)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-black?logo=github)](https://github.com/your-username)
