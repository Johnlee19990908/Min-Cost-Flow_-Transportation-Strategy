# Minimum Cost Procurement and Transportation Strategy
![Eight Image](https://github.com/Johnlee19990908/Supply-Chain-Network-Planning/blob/main/readme_photo/2.png)

## 1. Objective
The project aims to develop an optimal transportation network to manage the shipment of oil across various cities within a specified planning horizon. The goal is to minimize the total transportation cost while ensuring that the supply and demand constraints are met across all nodes (cities) and time periods (weeks).

## 2. Model Overview
We use a Transportation Optimization Model to represent the movement of oil between cities. The model incorporates multiple cities, transportation routes, and time periods, and seeks to find the optimal shipping schedule that minimizes the total cost. 

![Eight Image](https://github.com/Johnlee19990908/Supply-Chain-Network-Planning/blob/main/readme_photo/3.png)

The transportation problem is formulated as follows:

### Sets
- `CITIES`: Set of cities involved in the supply-demand network.
- `LINKS`: Set of directed links between cities, represented as pairs `(i, j)` where `i` and `j` are cities.

### Parameters
- `supply[i]`: Amount of supply available at city `i` (for `i` in `CITIES`).
- `demand[j]`: Amount of demand required at city `j` (for `j` in `CITIES`).
- `cost[i, j]`: Cost per unit of shipment from city `i` to city `j` (for `(i, j)` in `LINKS`).
- `capacity[i, j]`: Maximum shipment capacity on the link from city `i` to city `j` (for `(i, j)` in `LINKS`).

### Decision Variable
- `Ship[i, j]`: Number of packages to be shipped from city `i` to city `j`. This is constrained to be non-negative and below the link capacity.

### Objective Function &  Constraints

![Eight Image](https://github.com/Johnlee19990908/Supply-Chain-Network-Planning/blob/main/readme_photo/4.png)

## Solution and Result

After running file time_expand_network.ipynb, the model using an optimization solver (e.g., CBC 2.10.10), the optimal solution provides:
- The optimal shipping quantities across the network,
- The total transportation cost.

### Total Cost:
Total Cost: 67,100 units

### Shipping Plan:

#### Kansas (KAN) Shipments:
1. **Kansas Week 1**:
    - Sent 100 units to Norfork Week 2
2. **Kansas Week 2**:
    - Sent 100 units to Norfork Week 3
3. **Kansas Week 3**:
    - Sent 50 units to the Dummy node (DUM) (surplus supply)
4. **Kansas Week 4**:
    - Sent 50 units to the Dummy node (DUM) (surplus supply)
#### Houston (HOU) Shipments:
1. **Houston Week 2**:
    - Sent 50 units to Haiti Week 3
    - Sent 50 units to Panama Week 3
2. **Houston Week 3**:
    - Sent 50 units to Haiti Week 4
    - Sent 150 units to Panama Week 4
3. **Houston Week 4**:
    - Sent 50 units to Haiti Week 5
    - Sent 50 units to Panama Week 5
4. **Houston Week 5**:
    - Sent 50 units to Haiti Week 6
    - Sent 50 units to Panama Week 6
#### Norfork (NOR) Shipments:
1. **Norfork Week 2**:
    - Sent 100 units to Haiti Week 4
2. **Norfork Week 3**:
    - Sent 100 units to Haiti Week 5
#### Chicago (CHI) Shipments:
1. **Chicago Week 1**:
    - Sent 200 units to Houston Week 3
2. **Chicago Week 2**:
    - Sent 200 units to Houston Week 4
3. **Chicago Week 3**:
    - Sent 200 units to Houston Week 5
#### Panama (PAN) Shipments:
1. **Panama Week 4**:
    - Sent 50 units to Panama Week 5 (Inventory holding)

### By following this shipping plan, the optimal distribution of oil is achieved while minimizing the transportation costs across the entire network.

