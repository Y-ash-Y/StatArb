# Statistical Arbitrage: A Multi-Method Approach

A quantitative finance project implementing a statistical arbitrage (pairs trading) strategy using econometric models, stochastic processes, and machine learning techniques.

Developed at IIT Kanpur (EE798Z)  
Author: Yash Yadav

---

## Overview

This project builds a market-neutral trading strategy that exploits mean-reverting relationships between asset pairs.

The implementation focuses on improving realism and robustness through:

- Rolling hedge ratio estimation (removes lookahead bias)
- Ornstein-Uhlenbeck modeling for mean reversion
- Backtesting with transaction costs and risk controls
- Monte Carlo validation using block bootstrap
- Dynamic hedging via Kalman Filter

A detailed explanation of methodology and results is available in the full report:

**→ See:** `Statistical_Arbitrage_Final_Report.pdf`

---

## Key Results

- Identified cointegrated pair: **MSFT / TSLA**
- Sharpe Ratio:
  - Rolling OLS: **1.69**
  - Kalman Filter: **4.40**
- 100% profitable outcomes across 1,000 Monte Carlo simulations
- Mean-reversion half-life: ~31–51 trading days

---

## Methods Used

- Cointegration Testing (Engle-Granger)
- Rolling OLS Regression
- Ornstein-Uhlenbeck Process
- Kalman Filter (EM optimization)
- Monte Carlo Simulation (Block Bootstrap)
- LSTM (evaluated, not effective)
- Diebold-Mariano Test

---

## Tech Stack

- Python (NumPy, Pandas, Statsmodels)
- Machine Learning (TensorFlow / PyTorch)
- Quantitative Finance Models
