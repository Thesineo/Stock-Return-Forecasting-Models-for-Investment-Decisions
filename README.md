# 📈 Stock Return Forecasting Model — FTSE100 TechMark Series
### Investors Model for Investment Decision-Making | London Stock Exchange

![R](https://img.shields.io/badge/R-4.x-276DC3?logo=r)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)
![Dissertation](https://img.shields.io/badge/MSc-Business%20Analytics%20%26%20Decision%20Sciences-blue)

> **MSc Dissertation** | University of Leeds | Supervised by Dr. Wessam Abouarghoub

---

## 📌 Business Problem

The UK Tech sector holds a market capitalisation of £860 billion, making it the third largest globally. Accurate forecasting of stock returns for FTSE TechMark listed firms is essential for investors to make precise investment decisions. This study **comparatively evaluates ARIMA and GJR-GARCH(1,1) models** across 10 top UK technical firms listed on the FTSE100 TechMark index, using 7 years of daily stock price data (Jan 2015 – Dec 2022).

---

## 🏗️ Methodology Pipeline

```
Yahoo Finance API (quantmod)
        ↓
  Daily Closing Price Data (2015–2022)
  10 FTSE TechMark Firms
        ↓
  ┌──────────────────────────────────────┐
  │  Stationarity Testing (ADF Test)     │
  │  ACF / PACF Correlograms             │
  │  Kurtosis & Skewness Analysis        │
  └──────────────────────────────────────┘
        ↓
  Train: Jan 2015 – Dec 2020 (1,518 observations)
  Test:  Jan 2021 – Dec 2022 (503 observations)
        ↓
  ┌──────────────────┬────────────────────────┐
  │  ARIMA (p,d,q)   │  GJR-GARCH(1,1) [sstd] │
  │  Per-firm orders │  Asymmetric Volatility  │
  └──────────────────┴────────────────────────┘
        ↓
  Evaluation: RMSE & MAPE
  Multi-step Forecast: 2-Year Horizon
```

---

## 🏢 Companies Analysed

| Ticker | Company | Market Cap (GBP) | Sector |
|---|---|---|---|
| BA.L | BAE Systems | £27.98B | Defence Technology |
| FLTR.L | Flutter Entertainment | £28.79B | Online Betting |
| SN.L | Smith and Nephew PLC | £10.31B | Medical Technology |
| SGE.L | The Sage Group PLC | £9.41B | Software Solutions |
| SXS.L | Spectris PLC | £3.79B | Instrument Technology |
| RWS.L | Renishaw PLC | £2.90B | Scientific Technology |
| CCC.L | Computacenter PLC | £2.48B | Tech Service Provider |
| QQ.L | QinetiQ Group | £1.98B | Technology Services |
| GNS.L | Genus PLC | £1.58B | Genetic Technology |
| SPT.L | Spirent Communications | £1.01B | Communications Tech |

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| `quantmod` | Download historical stock data from Yahoo Finance |
| `rugarch` | GARCH model specification, fitting & forecasting |
| `tseries` | ADF stationarity test |
| `forecast` | ARIMA model fitting & multi-step forecasting |
| `PerformanceAnalytics` | Return calculation & visualisation |
| `xts` | Time series data handling |

---

## 📦 Setup (R)

```r
install.packages(c("quantmod", "rugarch", "tseries", 
                   "forecast", "PerformanceAnalytics", "xts"))
```

```r
# Download stock data
library(quantmod)
data <- getSymbols("SN.L", src = "yahoo",
                   from = as.Date("2015-01-01"),
                   to   = as.Date("2022-12-31"),
                   auto.assign = FALSE)
```

---

## 🔬 Key Methodology Steps

### 1. Stationarity Check (ADF Test)
```r
library(tseries)
adf.test(train)
# SN.L p-value = 0.088 → Non-stationary → Apply 1st differencing
```

### 2. ARIMA Model Fitting
```r
library(forecast)
fit <- arima(train, order = c(3, 1, 2))  # Best fit for SN.L
forecast(fit, h = 503)
```

### 3. GJR-GARCH(1,1) Fitting
```r
library(rugarch)
spec <- ugarchspec(
  mean.model     = list(armaOrder = c(0, 0)),
  variance.model = list(model = "gjrGARCH"),
  distribution.model = "sstd"             # Student-t best fit
)
m <- ugarchfit(data = return, spec = spec)
ugarchforecast(m, n.ahead = 503)
```

---

## 📊 ARIMA Model Orders — All Firms

| Company | ARIMA Order (p,d,q) | Log-Likelihood | AIC | BIC |
|---|---|---|---|---|
| The Sage Group (SGE.L) | (0,1,1) | -5683.71 | 11371.41 | 11382.06 |
| Smith & Nephew (SN.L) | (3,1,2) | -6868.53 | 13749.06 | 13781.01 |
| BAE Systems (BA.L) | (0,1,0) | -5279.25 | 10560.49 | 10565.82 |
| Computacenter (CCC.L) | (2,1,2) | -7195.33 | 14402.65 | 14434.60 |
| Genus PLC (GNS.L) | (0,1,1) | -7942.26 | 15890.53 | 15906.50 |
| QinetiQ Group (QQ.L) | (2,1,2) | -4383.76 | 8777.51 | 8804.13 |
| Flutter Entertainment (FLTR.L) | (2,1,2) | -9931.20 | 19874.39 | 19906.34 |
| Renishaw PLC (RWS.L) | (3,1,2) | -5612.27 | 11236.55 | 11268.49 |
| Spirent Communications (SPT.L) | (1,1,3) | -4241.46 | 8494.92 | 8526.87 |
| Spectris PLC (SXS.L) | (1,1,0) | -7806.06 | 15616.12 | 15626.77 |

---

## 📈 Model Evaluation Results (RMSE & MAPE)

> Forecast horizon: Jan 2021 – Dec 2022 (503 daily observations)

| Company | ARIMA RMSE | ARIMA MAPE (%) | GJR-GARCH RMSE | GJR-GARCH MAPE (%) | Winner |
|---|---|---|---|---|---|
| SGE.L | 10.2513 | 1.0876 | 7.0295 | 0.6624 | ✅ GARCH |
| SN.L | 22.3854 | 1.0547 | 19.0230 | 0.5243 | ✅ GARCH |
| BA.L | 7.8521 | 1.0536 | 5.3354 | 0.9528 | ✅ GARCH |
| CCC.L | 27.7668 | 1.3099 | 27.8386 | 1.2955 | ✅ GARCH |
| GNS.L | 45.4329 | 1.3024 | 35.5456 | 1.0004 | ✅ GARCH |
| QQ.L | 4.3510 | 1.0623 | 22.9720 | 1.9371 | ✅ ARIMA |
| FLTR.L | 168.5630 | 1.4013 | 114.3916 | 1.0597 | ✅ GARCH |
| RWS.L | 9.7850 | 1.6560 | 9.7316 | 1.6309 | ✅ GARCH |
| SPT.L | 3.9616 | 1.5850 | 1.7772 | 1.1464 | ✅ GARCH |
| SXS.L | 41.5620 | 1.2856 | 39.2270 | 1.2846 | ✅ GARCH |

---

## 💡 Key Findings

- **GJR-GARCH(1,1) outperforms ARIMA for 9 out of 10 firms**, producing lower RMSE and MAPE values across the 2-year test horizon
- Both models kept MAPE well **below 5%**, confirming accurate forecasting ability for all firms
- Among GARCH distributions, only the **Student-t (sstd) distribution** passed the Goodness-of-Fit test — Normal and GED distributions failed
- The **exception is QinetiQ (QQ.L)**, where ARIMA outperformed GARCH due to decreasing volatility in its stock movements
- ARIMA assigns a **unique order per stock** based on ACF/PACF analysis, whereas GJR-GARCH applies a **single unified model** across all firms

---

## 🗂️ Repository Structure

```
├── data/                   # Raw closing price datasets per firm
├── models/
│   ├── arima_models.R      # ARIMA fitting & forecasting
│   └── garch_models.R      # GJR-GARCH fitting & forecasting
├── outputs/
│   ├── forecasts/          # Predicted return plots
│   └── evaluation/         # RMSE & MAPE comparison tables
├── notebooks/              # Exploratory analysis
└── README.md
```

---

## ⚠️ Limitations

- Regime shifts from unexpected macro events (e.g. COVID-19, Brexit) can affect model accuracy
- Models rely solely on historical price data and do not incorporate **market sentiment** or news signals
- Algorithmic trading and external manipulation can create unpredictable short-term price swings

---

## 👤 Author

**Aniket Amar Nerali**


[![LinkedIn](https://www.linkedin.com/in/aniket-nerali)
[![GitHub](https://github.com/Thesineo)
