# 🚇 Project 1: Minimum Spanning Tree (MST) Analysis of an Underground Network

This project explores how Minimum Spanning Trees can be used to analyze and optimize an underground transportation network. The analysis is conducted using geodesic and real-world distances, as well as simulations to evaluate robustness under uncertainty.

---

## 1. MST Based on Geodesic (Straight-Line) Distances

We first compute the MST using straight-line (geodesic) distances derived from latitude and longitude coordinates of each station.

### Method:
- Distances are computed using the **Haversine formula**, which accounts for Earth’s curvature.
- Connections with missing geo-data are removed (`NaN` values).
- A graph is constructed with stations as nodes and Haversine distances as edge weights.
- **Kruskal’s** and **Prim’s algorithms** are both applied to validate the MST result.

### Result:
Both algorithms yield the same MST length, confirming the accuracy of the implementation.

---

## 2. MST Based on Actual Connection Lengths

Next, we repeat the MST computation using the **actual length values** provided in the dataset for each connection.

### Method:
- The graph structure remains the same.
- Instead of geodesic distances, edge weights are set to the actual measured lengths.

### Result:
MST lengths using actual and geodesic distances are nearly identical, indicating that the overall network structure is not sensitive to how distance is measured.

---

## 3. Robustness Test via Simulation and Value-at-Risk (VaR)

To assess the stability of the MST under uncertain connection lengths, we perform a Monte Carlo simulation:

### Method:
- Conduct **10,000 simulations**, adding Gaussian noise (μ = 0, σ = 0.3) to each connection length.
- For each simulation, recalculate the MST length.
- Plot the distribution of MST lengths and calculate **Value-at-Risk (VaR)** at the 5% level.

### Findings:
- Distribution of MST lengths is approximately **normal**, centered around 250–255 miles.
- The **5% VaR threshold** is ~244 miles, meaning 95% of cases remain above this threshold.
- This shows the MST structure is relatively robust, even under randomized perturbations.

---

## 4. Delete-and-Repair Sensitivity Analysis

We further assess MST resilience by removing and repairing each edge:

### Method:
- Construct the MST with Kruskal’s algorithm.
- Iteratively **remove one edge** at a time.
- Recompute MST on the modified graph and record new total length.
- Visualize impact in a scatter plot.

### Findings:
- Most repaired MST lengths range from **257–258 miles**, suggesting high robustness.
- Occasional drops imply some initially suboptimal edges were replaced with better ones.
- Peaks indicate critical edges—removal leads to costlier alternate paths.

---

## Files in This Folder

- `MST.ipynb`: Code of MST Analysis of an Underground Network
- `stations.csv`, `connections.csv`, `lines.csv`: Dataset files

---

## Summary

This project demonstrates how MST algorithms can be applied to transportation networks to:
- Optimize design
- Evaluate robustness
- Identify critical infrastructure
