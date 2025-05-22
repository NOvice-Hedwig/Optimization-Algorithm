# Inventory-Constrained Trading Optimization using Dynamic Programming and Forecasted Demand

This project implements an inventory-constrained trading strategy using **Dynamic Programming (DP)** to maximize expected revenue over a finite horizon. The model accounts for predicted price fluctuations and incorporates real-time inventory updates to guide optimal trade decisions.

---

## Problem Setup

The goal is to dynamically sell a fixed number of shares within a given time horizon, adjusting the trade size at each time step based on price forecasts and inventory constraints.

---

## Inputs and Assumptions

- Initial inventory: fixed number of units to be sold (e.g., 100 shares)
- Forecasted price series over time horizon \( T \)
- Action space: trade volumes allowed per step (e.g., 0 to 5 units)
- Constraints:
  - Can’t sell more than available inventory
  - Must sell all inventory by final time step

---

## Algorithm: Finite-Horizon Dynamic Programming

At each time step \( t \) and inventory level \( q \), the value function is recursively defined:

$$
V(t, q) = \max_{a \in A(q)} \left[ p_t \cdot a + V(t+1, q-a) \right]
$$

Where:
- \( p_t \): predicted price at time \( t \)
- \( a \): action (number of units to sell)
- \( A(q) \): feasible actions given current inventory \( q \)
- \( V(t+1, q-a) \): future expected value after executing action \( a \)

### Features:
- Backward recursion from terminal time
- Value function table \( V[t][q] \) stores optimal revenue
- Policy table stores optimal action at each state

---

## Simulation: Out-of-Sample Performance

After computing the optimal policy, I simulate trading execution using new (out-of-sample) price paths:

- Execute trade size from pre-computed policy
- Track cumulative revenue
- Plot inventory and revenue trajectories over time

---

## Outputs

- Optimal trade schedule
- Final total revenue under forecasted prices
- Revenue trajectory under simulated prices
- Inventory depletion over time

---

## 🛠️ Tools and Libraries

- Python: NumPy, Matplotlib
- Jupyter Notebook
- No external solvers (pure DP logic)

---

## Project Structure

```bash
DP Algorithm/
├── Dynamic Programming.ipynb: Main code: 
└── DP_README.md
