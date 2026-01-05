# Equity Risk Forecasting with Stochastic Processes

## Course Context
This project applies the core stochastic processes covered in the course—Brownian motion, Poisson processes, and Markov chains—to a practical financial risk management problem: **Value-at-Risk (VaR) forecasting and backtesting**.

Rather than focusing only on theory, the project evaluates how these stochastic models perform when applied to real market data.

---

## Motivation

Risk teams use statistical models to answer a simple but critical question:

> *How much could we lose tomorrow, and how often will that estimate be wrong?*

Value-at-Risk (VaR) provides a loss threshold that should only be exceeded with small probability (e.g., 5% or 1%).  
However, VaR is only meaningful if its predictions are validated against realized market outcomes.

This project investigates how well common stochastic models forecast downside risk and where their assumptions break down.

---

## Data and Setup

- Daily adjusted prices for six U.S. equities and indices  
- Sample period: **January 2015 – December 20, 2025**
- Daily log returns defined as:

$$
r_t = \log\left(\frac{S_t}{S_{t-1}}\right)
$$

All models are calibrated using a **rolling 252-day window**, reflecting standard industry practice.

---

## Models Studied (Linked to Course Topics)

### 1. Geometric Brownian Motion (Brownian Motion)

The baseline model assumes prices follow:

$$
dS_t = \mu S_t \, dt + \sigma S_t \, dW_t
$$

- Captures continuous random fluctuations
- Assumes constant volatility and normally distributed returns
- Used as a benchmark model

---

### 2. Jump Diffusion (Poisson Process)

To allow for sudden large price movements:

$$
dS_t = \mu S_t \, dt + \sigma S_t \, dW_t + J_t \, dN_t
$$

- $N_t$ is a Poisson process with intensity $\lambda$
- Models rare but large shocks (e.g., crises, earnings)
- Direct application of Poisson processes from the course

---

### 3. Markov Regime-Switching Volatility (Markov Chains)

Volatility switches between discrete regimes:

$$
\sigma_t \in \{\sigma_{\text{low}}, \sigma_{\text{high}}\}
$$

- Regimes evolve according to a discrete-time Markov chain
- Captures volatility clustering observed in financial markets
- Implemented using a simple two-state structure for interpretability

---

## Risk Forecasting and Backtesting

For each trading day:
1. Calibrate model parameters using the previous 252 days
2. Simulate next-day returns using Monte Carlo methods
3. Estimate VaR and Expected Shortfall (ES) at 95% and 99%
4. Compare forecasts to realized next-day returns

A VaR breach occurs when:

$$
r_{t+1} < \text{VaR}_\alpha
$$

Backtesting evaluates whether observed breach rates align with expected probabilities.

---

## Key Findings

- At the 95% level, regime-switching volatility produces the most stable breach rates
- At the 99% level, all models underestimate tail risk, particularly during stress periods
- Jump diffusion improves intuition around extreme events but is sensitive to rolling calibration
- Model failures are systematic, highlighting limitations of Gaussian-based assumptions


