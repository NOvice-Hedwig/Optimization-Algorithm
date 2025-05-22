# Constrained Mean-Variance Portfolio Optimization with Long-Short Structure and PCA Risk Model

This project implements a robust investment strategy using constrained mean-variance optimization. It incorporates long-short positions, cardinality constraints, and a PCA-based risk model to achieve optimal allocation. The final portfolio is evaluated under simulated market dynamics using Monte Carlo path simulation.

---

## Data Cleaning and Preprocessing

- Assets with any missing returns were removed.
- Daily returns were calculated as percent changes in closing prices.
- Outliers exceeding ±25% daily return were smoothed using a centered 5-day moving average.
- Assets requiring more than 20 such corrections were excluded.

Final asset universe size: **2,034**

---

## Risk Modeling via PCA

To reduce dimensionality and isolate systematic factors, I applied **Principal Component Analysis (PCA)** to the asset return covariance matrix:

- Used top **50 principal components** for low-rank approximation.
- This PCA-based risk model preserves dominant market factors while simplifying optimization.

---

## Optimization Model

The portfolio allocation is formulated as a **Mixed-Integer Quadratic Program (MIQP)** with the following structure:

### Variables
- `pj ≥ 0`: long weight of asset *j*
- `mj ≥ 0`: short weight of asset *j*
- `zj ∈ {0,1}`: binary indicator controlling whether the asset is held long or short
- `xj = pj - mj`: net position used in objective

### Objective Function

Minimize:
$$
\lambda \cdot x^T Q_{PCA} x - \mu^T x
$$

Where:
- \( \lambda \): risk aversion coefficient (user-supplied)
- \( Q_{PCA} \): covariance matrix approximated via PCA
- \( \mu \): vector of expected returns

### Constraints

| Constraint Type | Description |
|------------------|-------------|
| Position limits  | \( |x_j| \leq u \), and \( x_j = 0 \) or \( |x_j| \geq d \) |
| Mutual exclusivity | Only one of \( p_j, m_j \) can be positive |
| Long-side cardinality | \( L \leq \sum z_j \leq U \) |
| Short-side cardinality | \( SL \leq \sum (1 - z_j) \leq SU \) |
| Gross exposure | \( f \leq \sum p_j \leq g \); \( h \leq \sum m_j \leq i \) |

The model is solved using **Gurobi** to return the optimal portfolio weights \( x^* \).

---

## 📉 Monte Carlo Path Simulation

To evaluate robustness, I simulated 100 days of portfolio value evolution under stochastic returns.

### 📊 Method:
- Initial capital: \$1,000,000,000
- For asset *k*, daily return drawn from:  
  $$
  r_k \sim \mathcal{U}[\mu_k - \delta_k, \mu_k + \delta_k], \quad \delta_k = \min(\sigma_k, |\mu_k| / 2)
  $$
- Prices updated multiplicatively.
- Portfolio re-marked to market daily.

The result is a time series of simulated portfolio net value over 100 days.

---

## Project Structure

```bash
portfolio_optimization_project/
├── MIQP.ipynb: Main code
├── w5000.csv: Historical price data 
├── MIQP_README.md
