# Optimizing Child Care Facility Expansion in New York State

This project focuses on optimizing child care facility expansion and construction in New York State to eliminate service gaps in high-need regions. The model is designed to determine the minimum total cost required to meet regulatory and demographic-driven care coverage standards across 1,324 zip codes.

---

## Problem Overview

I designed and implemented an optimization framework that determines:

- Where to build new child care facilities
- How much to expand existing ones
- How to meet both total and under-5 child care demands

My goal was to satisfy region-specific minimum coverage standards while minimizing total costs.

---

## Data Processing & Feature Engineering

I consolidated datasets including:

- Facility-level capacities
- Age-specific child population data
- Zip-code level income and employment rates
- Potential construction site metadata

Key derived features included:

- **Total children** = children ≤9 + 60% of 10–14
- **Under-5 children** = children aged 0–5
- **High-demand areas** = regions where employment rate ≥ 60% or average income ≤ $60K
- **Child care deserts**:
  - Coverage ≤ 50% in high-demand areas
  - Coverage ≤ 33% otherwise
  - Under-5 coverage ≤ 66%

---

##  Optimization Model

I formulated the problem as a **Mixed-Integer Linear Program (MILP)** with the following decision variables:

- `ya,i`: number of new facilities of size i (small, medium, large) to build in area a  
- `mt_{a,j}`: number of total additional slots added at facility j in area a  
- `mu_{a,j}`: number of added slots specifically for children under 5  

### Objective

Minimize total cost, which includes:

$$
\text{Total Cost} = \sum \text{Construction Cost} + \sum \text{Expansion Cost} + \sum \text{Under-5 Premium}
$$

- **Construction Cost**:  
  - Small (100 slots): \$65,000  
  - Medium (200 slots): \$95,000  
  - Large (400 slots): \$115,000  

- **Expansion Cost**:  
  - A constant marginal slope: \( \text{Cost per slot} = 20,000 + 200n \) (where n = original facility size)

- **Under-5 Premium**: \$100 per new under-5 slot

---

## Constraints

1. **Coverage Requirements**:
   - High-demand areas:  
     $$
     \text{New Capacity} + \text{Expansion} \geq 0.5 \times \text{Total Children}
     $$
   - Normal-demand areas:  
     $$
     \text{New Capacity} + \text{Expansion} \geq \frac{1}{3} \times \text{Total Children}
     $$
   - Under-5 requirement:  
     $$
     \text{New Under-5 Capacity} + \text{Expansion} \geq \frac{2}{3} \times \text{Under-5 Children}
     $$

2. **Expansion Limits**:  
   - Max 20% expansion of existing capacity  
   - Max 500 slots total per facility

3. **Logical Consistency**:
   $$
   mu_{a,j} \leq mt_{a,j}
   $$

---

## Results

The final optimized strategy achieves full regulatory compliance with a total budget of: $372,015,630**

Key insights:

- The selected expansion cost slope \( (20,000 + 200n) \) yields the lowest marginal cost per added slot.
- Most areas were served through strategic facility expansions up to the 20% cap.
- In underserved regions, new facility construction was prioritized based on population demand and cost-efficiency.

---

## Methods & Algorithms Used

- **Mixed-Integer Linear Programming (MILP)**
- **Facility location modeling**
- **Piecewise-linear cost modeling**
- **Scalable constraint-based resource allocation**
- Solver: Gurobi 

---

## Project Structure
childcare_optimization/
├── data/
│   ├── child_care_regulated.csv
│   ├── population.csv
│   ├── avg_individual_income.csv
│   ├── employment_rate.csv
│   ├── potential_locations.csv
│   └── [data documentation PDFs]
├── LP: code to implement MILP Model
├── LP_README.md


