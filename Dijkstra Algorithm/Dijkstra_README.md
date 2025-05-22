# Project 2: NYISO Grid Tour Optimization

This project analyzes node-to-node distances in the New York Independent System Operator (NYISO) electric grid using network algorithms. I explore shortest paths, construct minimum-cost inspection tours, and compute an Eulerian circuit to optimize routing.

---

## Question 1: Shortest Path Distribution

I selected **1 million random node pairs** and used **Dijkstra’s algorithm** to compute the shortest path between them. Edge weights were defined as **geographical distances** using the Haversine formula. Disconnected node pairs were excluded.

### Findings:
- The distribution is **right-skewed**, with most paths < 50 km.
- Peaks at ~200 km, 400 km, and 600 km suggest **modular clusters**.
- Gaps in frequency reflect irregular network structure and connectivity density.

---

## Question 2: MST + Random Node Pairing

I constructed a **Minimum Spanning Tree (MST)** to connect all nodes with the least total distance, then added shortest paths between **odd-degree nodes** to enable an Eulerian circuit.

### Steps:
1. Compute MST:
   - Total MST weight:  
     $$
     \sum_{e \in MST} W(e) = 7407.12 \text{ km}
     $$
2. Identify 580 odd-degree nodes.
3. Randomly pair odd nodes:
   - Pairing cost:  
     $$
     \sum_{(u, v) \in P} d(u, v) = 76036.64 \text{ km}
     $$
4. Total inspection tour cost:
   $$
   \text{Total Cost} = \sum_{e \in MST} W(e) + \sum_{(u, v) \in P} d(u, v)
   $$
   $$
   = 7407.12 + 76036.64 = 83443.76 \text{ km}
   $$

---

## Question 3: Random Pairing Optimization (1000 Trials)

I repeated **1000 random pairings** of odd-degree nodes and computed the corresponding total tour costs.

### 🔁 Result:
- **Best pairing**:  
  $$
  \text{Best Tour Cost} = 70033.55 \text{ km}
  $$
- The experiment showed significant improvement over the initial pairing
- This suggests potential for further optimization (though not guaranteed to be globally optimal)

---

## Question 4: Eulerian Circuit Construction via Splicing

I constructed an **Eulerian circuit** based on the best pairing using a **splicing algorithm**.

### 🧩 Method:
- Started with a multigraph of the MST edges.
- Added shortest paths between paired odd-degree nodes.
- Used `networkx.eulerian_circuit` to compute the tour.
- Calculated tour cost by summing edge weights:
  - For repeated edges, only the first encountered weight was used.

### ✅ Final Result:
- Eulerian tour cost:  
  $$
  70033.55 \text{ km}
  $$
- This matched the optimal pairing from Question 3, confirming the correctness of the splicing process

---

## File Structure

```text
Dijkstra Algorithm/
├── Network Instpection + Dijkstra
├── "nysiobuses.csv": contains information about the buses (nodes) of the New York state power grid.  The first column is the "bus number."
├── "nyisobranches.csv": indicates for each branch the numbers for the two buses connected by that branch.
└── README.md
