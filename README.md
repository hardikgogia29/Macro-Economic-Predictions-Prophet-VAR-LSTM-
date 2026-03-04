# India Macroeconomic Forecasting

A comparison of LSTM, VAR, and Prophet forecasting models on 10 economic indicators for India.

## Project Overview

This project analyzes 26 years (2000-2025) of macroeconomic data across 180+ countries to forecast India's economic indicators. We compare three forecasting approaches to identify which model works best for different types of economic variables.

Key Finding: No single model is best for all variables. Different models excel at different economic patterns.

## Notebooks

| Notebook | Description |
|----------|-------------|
| 0__Executive_Summary | Synthesis of all findings |
| 1__Correlation_and_Non-Time_Series_Clustering | Exploratory analysis and country clustering |
| 2__TimeSeries_Clustering_and_Regression | DTW-based clustering of GDP patterns |
| 3__Granger_Causality | Causal relationships for India |
| 4__Prophet_Forecasting | Facebook Prophet implementation |
| 5__VAR_Model | Vector AutoRegression model |
| 6__LSTM | Deep learning LSTM implementation |
| 7__Model_Comparison_Enhanced | Model benchmarking and error analysis |

## Results

### Model Winners by Variable

| Variable | Winner | RMSE |
|----------|--------|------|
| GDP | VAR | 4.14 |
| Inflation | Prophet | 2.10 |
| Unemployment | LSTM | 0.25 |
| Current Balance | Prophet | 0.68 |
| Fiscal Balance | VAR | 0.60 |
| Debt | Prophet | 6.47 |
| Investment | VAR | 1.15 |
| GNS (Savings) | VAR | 0.94 |
| Exports | Prophet | 4.96 |
| Imports | Prophet | 5.55 |

### Model Performance Summary

Prophet: 5/10 wins - Best for trend-based variables (Inflation, Trade, Debt)
VAR: 4/10 wins - Best for multivariate relationships (GDP, Balances, Investment)
LSTM: 1/10 win - Best for autocorrelated series (Unemployment)

## Major Findings

### Prophet Fails Catastrophically on GNS (Savings)
RMSE: 9.35 (VAR: 0.94) - 9.94 times worse

Why: GNS has structural breaks from financial crises and policy changes. Prophet assumes trends continue but breaks down when structural changes occur. VAR captures relationships between variables which are more stable than raw trends.

### LSTM Fails on Investment Due to Data Scarcity
RMSE: 6.75 (VAR: 1.15) - 5.85 times worse

Why: Only 20 training sequences available (25 years divided by 5-year lag length). LSTM overfits on small datasets. VAR uses fewer parameters and is more stable with limited data.

### Prophet Fails on Unemployment Due to Autocorrelation
RMSE: 2.01 (LSTM: 0.25) - 8 times worse

Why: Unemployment is highly autocorrelated with sudden shocks. Prophet does not model lag dependencies. LSTM explicitly learns temporal patterns and handles shock-prone variables better.

## Recommendation

Use an ensemble approach combining all three models by variable type:

```
GDP -> VAR
Inflation -> Prophet
Unemployment -> LSTM
Current Balance -> Prophet
Fiscal Balance -> VAR
Debt -> Prophet
Investment -> VAR
GNS -> VAR
Exports -> Prophet
Imports -> Prophet
```

This ensemble approach likely outperforms any single model on 9-10 variables.

## Data

Time Period: 2000-2025 (26 years)
Countries: India + 9 Granger-causally significant countries
Indicators: 10 macroeconomic variables
Source: IMF/World Bank data

## Models

Prophet - Facebook's time series forecasting library
VAR - Vector AutoRegression from statsmodels
LSTM - Long Short-Term Memory neural network using TensorFlow

## Methodology

Train/Test Split: 2000-2020 (train), 2021-2025 (test)
Metrics: RMSE and MAE
Validation: Forward-looking test set with no temporal leakage
Hyperparameters: Mostly defaults

## How to Use

1. Start with 0__Executive_Summary.ipynb for findings
2. Review 7__Model_Comparison_Enhanced.ipynb for error analysis
3. Explore other notebooks for methodology details
4. Raw data is in data.csv
