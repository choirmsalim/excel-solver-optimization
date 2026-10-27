# Excel Optimization Hackathon — Nutrition Planning & Vehicle Routing

**DQLab Hackathon | Operations Research**

## Overview

This project uses Microsoft Excel and Solver to model two constrained optimization problems:

- **Nutrition Budget (Linear Programming):** build a food plan that meets protein and fiber targets within a budget and ingredient quantity limits.
- **Vehicle Routing Problem (VRP):** assign customer locations to truck routes while respecting vehicle capacity and minimizing total travel distance.

The project demonstrates how spreadsheet formulas, decision variables, objective functions, and constraints can turn raw inputs into practical planning models.

## Dataset

### 1. Nutrition Budget

Workbook: `excelhack03-perencanaan-gizi-dqlab.xlsx`

The `Bahan dan Menu` sheet contains five food ingredients—eggs, tempeh, chicken, spinach, and oats—with their unit prices, protein and fiber values, and minimum/maximum quantities. The model also contains budget and nutrition targets.

| Input / metric | Value in saved workbook |
|---|---:|
| Maximum budget | Rp2,000,000 |
| Protein target | 2,000 g |
| Fiber target | 500 g |
| Ingredients | 5 |

### 2. Vehicle Routing Problem

Workbook: `excelhack03-perencanaan-routing-dqlab.xlsx`

The workbook provides a depot, **50 customer locations**, customer demand values, and a **51 × 51 distance matrix** covering the depot and all customers.

| Input | Value |
|---|---:|
| Depot ID | 0 |
| Customer locations | 50 |
| Available trucks | 10 |
| Capacity per truck | 28 demand units |

*Dataset source: the supplied DQLab hackathon workbooks. No external dataset is added.*

## Tools & Methods

- **Microsoft Excel** — data organization, formula-based calculations, and result summaries.
- **Excel Solver Add-in** — objective and constraint optimization.
- **Simplex LP** — linear optimization for the nutrition model.
- **Evolutionary Algorithm** — search method for the combinatorial VRP.

### Spreadsheet formulas

| Formula | Role in the project |
|---|---|
| `INDEX` | Retrieve customer demand and distances from lookup tables / the distance matrix. |
| `IF/IFS` | Handle route separators, depot mapping, and empty route positions. |
| `COUNTIF` | Identify route separators and calculate route numbering. |
| `COUNTIFS` | Count route positions while applying multiple criteria. |
| `SUMPRODUCT` | Aggregate nutrition totals and truck demand. |
| `SUM` / `SUMIF` | Calculate totals and route-level summaries. |


## Workflow

1. **Prepare inputs** — review ingredient costs, nutrition values, customer demand, distance matrix, truck count, and capacity.
2. **Define decision variables** — food quantities for the nutrition model; encoded customer order and route separators for the VRP.
3. **Build formula logic** — calculate food cost, protein, fiber, route distance, and demand per truck.
4. **Configure Solver** — select the objective, variable cells, constraints, and appropriate solving method.
5. **Check feasibility** — confirm that budget and nutrition targets are met, every customer is served once, and no truck exceeds capacity.
6. **Review results** — inspect the food plan and the route summary after Solver completes.

## Results

The following figures are the **saved workbook snapshot**.

### Nutrition Budget

| Food | Planned units | Cost |
|---|---:|---:|
| Eggs | 26 | Rp780,000 |
| Tempeh | 40 | Rp600,000 |
| Chicken | 2 | Rp80,000 |
| Spinach | 5 | Rp50,000 |
| Oats | 16 | Rp400,000 |
| **Total** |  | **Rp1,910,000** |

- **Budget used:** 95.5% of the Rp2,000,000 limit
- **Budget remaining:** Rp90,000
- **Protein recorded:** 2,021 g against a 2,000 g target
- **Fiber recorded:** 500 g against a 500 g target

### Vehicle Routing

- **Total distance recorded:** 1,153.24 km
- **Trucks used:** 10
- **Total customer demand served:** 259 units
- **Capacity check:** each saved route is at or below 28 demand units

The VRP uses a heuristic search method; the saved distance is a feasible recorded result, not a proof of global optimality.

## How to Run

### Requirements

- Microsoft Excel desktop
- Excel **Solver Add-in** enabled
- Both `.xlsx` workbooks from this project

To enable Solver, go to **File → Options → Add-ins → Excel Add-ins → Go**, then check **Solver Add-in**.

### A. Run the Nutrition Budget model

1. Open `excelhack03-perencanaan-gizi-dqlab.xlsx` and select the `Bahan dan Menu` sheet.
2. Open **Data → Solver**.
3. Configure the model:
   - **Set Objective:** `H12` (total cost), select **Min**.
   - **By Changing Variable Cells:** `G7:G11` (planned ingredient quantities).
   - **Constraints:** `H12 <= B2`; `E2 >= B3`; `E3 >= B4`; `G7:G11 >= E7:E11`; and `G7:G11 <= F7:F11`.
   - **Select a Solving Method:** **Simplex LP**.
4. Click **Solve**, retain the solution if the constraints are satisfied, and review the updated cost and nutrition totals.

### B. Run the Vehicle Routing model

1. Open `excelhack03-perencanaan-routing-dqlab.xlsx`.
2. Open **Data → Solver**.
3. Configure the model:
   - **Set Objective:** `Solution!L13` (total route distance), select **Min**.
   - **By Changing Variable Cells:** `Helper!B3:B62` (encoded route sequence).
   - **Select a Solving Method:** **Evolutionary**.
   - Preserve the workbook's route encoding: customer IDs are `1–50`; values `51–60` are route separators / depot markers. For a fresh Solver setup, constrain `Helper!B3:B62` to integers from `1` to `60` and apply the **dif (alldifferent)** constraint to `Helper!B3:B62` so each ID is used once.
   - Add the capacity constraint `Helper!L3:L12 <= 28`. Keep each route within the available `Solution` layout (up to 8 customer stops per truck).
4. Click **Solve**. Review the `Solution` sheet and verify that every customer is included once, each route starts/ends at the depot as intended, and each truck's demand is no greater than 28 units.

> **Reproducibility note:** Solver outcomes can change with the initial solution, constraints, stopping settings, and Excel version. Always validate the final result against the model's constraints instead of relying only on the objective value.

## Reference

- [Microsoft Support: Define and solve a problem by using Solver](https://support.microsoft.com/en-us/excel/define-and-solve-a-problem-by-using-solver)

## Project Scope

This project focuses on spreadsheet-based optimization and model validation. It does not include external mapping, live traffic data, or a benchmark proving that the VRP result is globally optimal.
