# Portfolio_Optimization

## Overview

Formulated an asset allocation framework using historical returns and investor views to generate optimal portfolios.

The project combines Monte Carlo simulation, Markowitz portfolio theory, CVXPY optimization and Black-Litterman allocation to analyze risk-return trade-offs across 15 NIFTY stocks using 3 years of historical market data.

---

## Objectives

- Generate and evaluate large sets of feasible portfolios
- Identify risk-efficient allocations
- Compare approximate and exact optimization techniques
- Visualize the Efficient Frontier and Capital Market Line (CML)
- Study the impact of practical portfolio constraints
- Incorporate investor views using Black-Litterman allocation
- Measure diversification using concentration metrics

---

## Dataset

- Universe: 15 NIFTY 50 stocks
- Data Source: Yahoo Finance (`yfinance`)
- Frequency: Daily adjusted close prices
- Lookback Period: 3 years

Stocks used:

- RELIANCE
- TCS
- HDFCBANK
- INFY
- ICICIBANK
- HINDUNILVR
- SBIN
- BHARTIARTL
- ITC
- KOTAKBANK
- LT
- AXISBANK
- ASIANPAINT
- MARUTI
- TITAN

---

## Methodology

### 1. Return & Risk Estimation

Daily log returns were computed from historical prices.

Expected annualized returns:

μ = mean(daily returns) × 252

Annualized volatility:

σ = std(daily returns) × √252

Covariance and correlation matrices were constructed to measure co-movement between stocks.

---

### 2. Monte Carlo Portfolio Simulation

Generated **1M random portfolios** using Dirichlet-distributed weights.

For each portfolio:

- Expected Return = wᵀμ
- Volatility = √(wᵀΣw)
- Sharpe Ratio = (Return − Rf) / Volatility

This creates the feasible portfolio universe and enables identification of high Sharpe-ratio portfolios.

---

### 3. Efficient Frontier

The Monte Carlo cloud was binned by risk levels and the highest-return portfolio from each bin was retained.

This provides an approximation of the Efficient Frontier.

An exact Efficient Frontier was later generated using CVXPY optimization.

---

### 4. Markowitz Optimization (CVXPY)

Implemented exact portfolio optimization using convex optimization.

#### Maximum Sharpe Portfolio

Objective:

Maximize

Sharpe = (wᵀμ − Rf) / √(wᵀΣw)

Solved using a convex reformulation in CVXPY.

#### Minimum Variance Portfolio

Objective:

Minimize

wᵀΣw

Subject to:

- Σw = 1
- w ≥ 0

---

### 5. Capital Market Line (CML)

Constructed the Capital Market Line using:

- Risk-free rate
- Exact tangency portfolio (maximum Sharpe portfolio)

The tangency point represents the most efficient risk-adjusted portfolio available.

---

### 6. Portfolio Constraints

To study practical investment scenarios, multiple optimization variants were implemented:

#### Case 1 — Unconstrained Markowitz
Maximum Sharpe portfolio without position limits.

#### Case 2 — 30% Position Cap
No individual stock may exceed 30% allocation.

#### Case 3 — 20% Position Cap
Stricter diversification constraint.

#### Case 4 — Sector Exposure Cap
Total sector allocation limited to 40%.

#### Case 5 — Target Return Portfolio
Minimum-risk portfolio achieving a 15% target return.

#### Case 6 — Risk Parity
Portfolio designed so that assets contribute more evenly to overall portfolio risk.

---

### 7. Black-Litterman Allocation

Implemented a Black-Litterman framework to combine:

- Market-implied equilibrium returns
- Investor views

#### Market Prior

Equilibrium returns:

Π = δΣw_market

where:

- δ = risk aversion coefficient
- Σ = covariance matrix
- w_market = proxy market portfolio

#### Investor View

View:

Banking sector outperforms IT sector by 5%.

#### Posterior Returns

Market beliefs and investor views are blended through a Bayesian update process to obtain posterior expected returns.

These updated returns are then used for portfolio optimization.

---

### 8. Diversification Analysis

Portfolio concentration was evaluated using the Herfindahl-Hirschman Index (HHI):

HHI = Σ(wᵢ²)

Lower HHI values indicate greater diversification.

Reference:

Equal-weight portfolio ≈ 0.067

---

## Results

### Efficient Frontier & Capital Market Line

<img width="1089" height="690" alt="cvpxy_exact frontier" src="https://github.com/user-attachments/assets/d3d35393-5707-4fda-a440-fb9697fb6ebf" />


The exact CVXPY frontier dominates the Monte Carlo approximation and identifies the true tangency portfolio.

---

### Portfolio Allocation Comparison

<img width="1589" height="490" alt="image" src="https://github.com/user-attachments/assets/2998be37-2410-4c29-aae3-da57e7c40c22" />


Comparison of portfolio weights under different optimization constraints.

---

### Strategy Comparison

| Strategy | Return | Risk | Sharpe | HHI |
|----------|---------|---------|---------|---------|
| Markowitz (Unconstrained) | 24.8% | 17.7% | 1.03 | 0.56 |
| Markowitz 30% Cap | 20.8% | 16.5% | 0.87 | 0.26 |
| Markowitz 20% Cap | 18.2% | 15.2% | 0.77 | 0.18 |
| Sector 40% Cap | 21.7% | 16.3% | 0.94 | 0.29 |
| Target Return 15% | 15.0% | 13.2% | 0.64 | 0.18 |
| Risk Parity | 4.6% | 12.1% | -0.16 | 0.07 |
| Black-Litterman | 19.7% | 24.0% | 0.55 | 0.81 |
| Black-Litterman + 30% Cap | 14.7% | 18.7% | 0.44 | 0.28 |

---

## Libraries Used

- NumPy
- Pandas
- SciPy
- CVXPY
- Matplotlib
- Seaborn
- Yahoo Finance

---

## Key Learnings

- Monte Carlo simulation provides intuition for feasible portfolio construction.
- CVXPY produces exact solutions to Markowitz optimization problems.
- Position and sector constraints improve diversification but may reduce expected returns.
- Black-Litterman enables incorporation of investor views while maintaining a market-consistent prior.
- Diversification can be quantified using concentration metrics such as HHI.
