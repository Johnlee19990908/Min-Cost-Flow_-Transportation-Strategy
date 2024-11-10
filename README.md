# Minimum Cost Procurement and Transportation Strategy

## 1. Objective
The project aims to develop an optimal transportation network to manage the shipment of oil across various cities within a specified planning horizon. The goal is to minimize the total transportation cost while ensuring that the supply and demand constraints are met across all nodes (cities) and time periods (weeks).

## 2. Model Overview
We use a Transportation Optimization Model to represent the movement of oil between cities. The model incorporates multiple cities, transportation routes, and time periods, and seeks to find the optimal shipping schedule that minimizes the total cost. 

### Key Elements:
- **Cities (Nodes)**: Represent source, transit, and destination points (e.g., CHI1, KAN1, HOU1).
- **Links**: Directed arcs representing feasible transportation routes between cities, with associated shipping costs.
- **Supply and Demand**: The supply at each source node (e.g., CHI1) must be shipped to the destination nodes (e.g., HOU1, NOR2) to meet the demand.
- **Time Horizon**: The problem is modeled for a 6-week planning horizon, where the shipping schedule is optimized for each week.

## 3. Problem Setup
The decision variables are represented by the shipment quantity from one city to another during each time period. The model is subject to several constraints, including:
- **Flow Balance**: At each city, the incoming and outgoing shipments must balance the demand and supply.
- **Capacity Constraints**: The shipping quantity between any two cities cannot exceed the maximum route capacity.
- **Supply-Demand Imbalance**: The total supply does not always equal the total demand. To handle this, a dummy node is introduced.

## 4. Dummy Node Explanation
To ensure feasibility when supply exceeds demand, a dummy node is introduced to absorb the excess supply. In this case, since the supply is 100 units higher than the demand, the dummy node will serve as a sink for this surplus supply. The dummy node is linked to the source nodes, where it "absorbs" the excess oil, ensuring that the flow balance is maintained and the system is feasible despite the imbalance between supply and demand.

## 5. Model Formulation

The transportation problem is formulated as follows:

### Decision Variables:
- `Ship[(i, j, t)]`: Represents the quantity of oil shipped from city `i` to city `j` in week `t`.



## Objective Function

Minimize the total transportation cost across all routes:

$$
\text{Total\_Cost} = \sum_{(i,j) \in \text{LINKS}} \sum_{t=1}^{6} \text{cost}_{i,j} \times \text{Ship}_{i,j,t}
$$

Where:

- \( \text{cost}_{i,j} \) is the transportation cost between cities \(i\) and \(j\),
- \( \text{Ship}_{i,j,t} \) is the quantity of oil shipped from city \(i\) to city \(j\) during week \(t\).

### Constraints:

#### 1. Flow Balance:
For each node \(i\), the sum of shipments leaving node \(i\) must equal the sum of shipments entering node \(i\), adjusted for supply and demand:

$$
\sum_{j} \text{Ship}_{i,j,t} - \sum_{j} \text{Ship}_{j,i,t} = \text{Demand}_{i,t} - \text{Supply}_{i,t}
$$

Where:
- \( \text{Demand}_{i,t} \) is the demand for oil at city \(i\) in week \(t\),
- \( \text{Supply}_{i,t} \) is the supply of oil at city \(i\) in week \(t\).

#### 2. Capacity Constraints:
Each route has a maximum shipping capacity, ensuring that the quantity shipped between any two cities does not exceed the route's capacity:

$$
\text{Ship}_{i,j,t} \leq \text{Capacity}_{i,j}, \quad \forall (i,j) \in \text{LINKS}, t = 1,2,\ldots,6
$$

Where:
- \( \text{Capacity}_{i,j} \) is the maximum shipping capacity for the route from city \(i\) to city \(j\).

#### 3. Dummy Node Constraints:
The dummy node serves as a "sink" for surplus supply or as a "source" for missing demand. This constraint ensures that the dummy node only receives or sends oil when there is an imbalance in supply and demand:

$$
\sum_{i} \text{Ship}_{i,DUM,t} - \sum_{j} \text{Ship}_{DUM,j,t} = \text{Surplus/Demand}_{DUM,t}
$$

Where:
- \( \text{Surplus/Demand}_{DUM,t} \) represents the surplus supply (if positive) or demand (if negative) for the dummy node in week \(t\).


## Solution and Result

After running the model using an optimization solver (e.g., CBC 2.10.10), the optimal solution provides:
- The optimal shipping quantities across the network,
- The total transportation cost.

### Total Cost:
The total transportation cost is calculated as the sum of the costs across all routes and time periods, which in this case is:

\[
\text{Total\_Cost} = 67,100 \text{ units}
\]

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

By following this shipping plan, the optimal distribution of oil is achieved while minimizing the transportation costs across the entire network.

