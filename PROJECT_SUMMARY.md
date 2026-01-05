# Equity Risk Forecasting with Stochastic Processes

## Objective
This project applies simple stochastic processes to the problem of **short-horizon financial risk forecasting**, specifically 1-day Value-at-Risk (VaR) and Expected Shortfall (ES).

The focus is not on deriving models, but on **testing their validity through rolling backtesting** on real equity data.

---

## Data and Return Construction

- Assets: 6 liquid U.S. equities and indices
- Sample: January 2015 – December  2025
- Prices are adjusted for splits and dividends

Daily log returns are defined as:
$$
r_t = \log\left(\frac{S_t}{S_{t-1}}\right)
$$

Log returns are used because they are additive over time and align naturally with continuous-time models.

---

## Modeling Assumptions

Across all models, the following assumptions are made:

1. Markets are frictionless (no transaction costs)
2. Returns are conditionally independent given model parameters
3. Parameters are **locally stationary** over a rolling 252-day window
4. Risk is evaluated at a **1-day horizon**, consistent with trading-desk risk management

---

## Stochastic Models (Course Alignment)

### 1. Geometric Brownian Motion (Brownian Motion)

The baseline model assumes prices follow:
$$
dS_t = \mu S_t\,dt + \sigma S_t\,dW_t
$$

Equivalently, log returns satisfy:
$$
r_{t+1} \sim \mathcal{N}\left(\mu \Delta t, \sigma^2 \Delta t\right)
$$

**Assumptions**
- Continuous paths
- Constant volatility
- Gaussian innovations

**Rationale**  
GBM serves as a benchmark model against which more complex dynamics are evaluated.

---

### 2. Jump Diffusion (Poisson Process)

To capture discontinuous price movements, jumps are added:
$$
dS_t = \mu S_t\,dt + \sigma S_t\,dW_t + J_t\,dN_t
$$

where:
- $N_t \sim \text{Poisson}(\lambda t)$
- $J_t \sim \mathcal{N}(\mu_J, \sigma_J^2)$

**Assumptions**
- Jumps occur independently of diffusion
- Jump arrivals follow a Poisson process
- Jump sizes are i.i.d.

**Rationale**  
This model relaxes the Gaussian tail assumption and reflects rare but extreme market events.

---

### 3. Markov Regime-Switching Volatility (Markov Chains)

Volatility evolves according to a latent Markov state:
$$
\sigma_t = \sigma_{X_t}, \quad X_t \in \{1,2\}
$$

with transition probabilities:
$$
\mathbb{P}(X_{t+1}=j \mid X_t=i) = p_{ij}
$$

**Assumptions**
- Finite number of volatility regimes
- Regime transitions are memoryless
- Returns are conditionally Gaussian within each regime

**Rationale**  
This captures volatility clustering without introducing continuous stochastic volatility.

---

## Risk Forecasting

For each trading day:
1. Parameters are estimated using the previous 252 returns
2. Next-day returns are simulated via Monte Carlo
3. VaR and ES are computed as:
$$
\text{VaR}_\alpha = \inf\{x : \mathbb{P}(R \le x) \ge \alpha\}
$$
$$
\text{ES}_\alpha = \mathbb{E}[R \mid R \le \text{VaR}_\alpha]
$$

---

## Backtesting Framework

A VaR breach (exception) occurs when:
$$
r_{t+1} < \text{VaR}_\alpha
$$

For a correctly specified model:
$$
\mathbb{P}(\text{breach}) = 1 - \alpha
$$

Observed breach rates are compared to theoretical expectations at 95% and 99%.

---

## Key Results

- Regime-switching volatility yields breach rates closest to theoretical levels at 95%
- All models materially underpredict tail risk at 99%
- Jump diffusion improves tail flexibility but is sensitive to rolling calibration
- Model failures are systematic, not sampling noise

---

## Conclusion

The project demonstrates that stochastic processes from the course translate directly into operational risk models.  
However, even structurally richer models remain limited by distributional assumptions, emphasizing the importance of **model validation through backtesting**.
