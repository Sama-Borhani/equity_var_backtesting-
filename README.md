# Equity VaR Backtesting with Stochastic Processes

This project evaluates how different stochastic models perform in forecasting
downside risk using rolling Value-at-Risk (VaR) backtesting.

## Models
- Geometric Brownian Motion (GBM)
- Jump Diffusion (Poisson jumps)
- Markov Regime-Switching Volatility (2-state)

## Methodology
- Daily log returns from U.S. equities (2015–2024)
- Rolling 252-day calibration window
- 1-day VaR and Expected Shortfall at 95% and 99%
- Backtesting via exception (breach) rates

## Key Findings
- Regime-switching volatility performs best at VaR(95%) across most tickers
- All models underestimate tail risk at VaR(99%), especially during stress
- Jump Diffusion improves tail modeling conceptually but is sensitive to rolling calibration

## How to Run
1. Run `01_data_features.ipynb`
2. Run model notebooks `02`–`04`
3. Run `05_model_comparison.ipynb` for final results
