# Data-Driven MILP Optimization of a Biofuel Supply Chain Network
Authors - Atharva Shailesh Anchalwar, Ahmad Abu Shamat, Ethan Chi 

This repository contains the code and data for a **data-driven mixed-integer linear programming (MILP) model** that designs an economically efficient and realistic **bioethanol supply chain** in Texas. The work is an adaptation and substantial extension of the **IISE Logistics and Supply Chain Division Case Competition 2024**, with added decision variables, refined parameters, county-level demand modeling, and extensive scenario analysis.

The model simultaneously decides:

- Which **hubs** and **biorefineries** to open (and at what capacity tier),
- How much **biomass** to move from suppliers → hubs → biorefineries,
- How much **bioethanol** to ship from biorefineries → demand counties,
- How many **trucks** and **trains** are required, and
- How much **electricity** to co-generate at plants,

in order to **maximize profit** while respecting supply, capacity, demand, and transportation constraints.

---

## Problem Overview

A bioethanol company is planning a supply chain consisting of:

- **Suppliers (254 Texas counties)** providing biomass,
- **Candidate hubs (33 locations)** for preprocessing and consolidation,
- **Candidate biorefineries (167 locations)** for converting biomass to bioethanol and electricity,
- **Demand counties (254)** consuming bioethanol.

Key modeling assumptions:

- Biomass is transported **by truck** from suppliers to hubs.
- Preprocessed biomass is transported **by train** from hubs to biorefineries.
- Finished bioethanol is transported **by truck** from biorefineries to demand counties.
- Hubs and biorefineries have **tiered capacity options** (small/medium/large).
- Electricity co-generation at plants provides a **secondary revenue stream**.
- Unmet demand is allowed but penalized via a **penalty cost** per liter.

The objective is to **maximize total profit**:

> revenue from bioethanol + revenue from electricity  
> − investment costs − transportation & loading costs − penalty for unmet demand.

---

## Mathematical Model

The core of the project is a **mixed-integer linear program**.

### Sets & Indices

- `i ∈ {1,…,254}` – Suppliers (counties)
- `j ∈ {1,…,33}` – Hubs
- `k ∈ {1,…,167}` – Biorefineries
- `c ∈ {1,…,254}` – Demand counties

### Main Parameters (high level)

- `S_i` – Biomass supply at supplier `i`
- `d_c` – Bioethanol demand at county `c`
- `Q_j` – Preprocessing capacity of hub `j`
- `R_k` – Conversion capacity of biorefinery `k`
- `Y_k` – Biomass-to-bioethanol yield at biorefinery `k`
- `Y_e` – Biomass-to-electricity yield at biorefinery
- `C_ij`, `T_jk`, `G_kc` – Transportation costs (truck & rail)
- `L_j`, `M_k` – Fixed investment cost of opening hub `j` / plant `k`
- `D_penalty` – Penalty cost for unmet demand
- `r_b`, `r_e` – Selling prices for bioethanol and electricity
- `T_R`, `CK_1`, `CK_2` – Capacities of trains and truck types

### Decision Variables (high level)

- `x_ij` – Biomass flow from supplier `i` to hub `j`
- `y_jk` – Biomass flow from hub `j` to biorefinery `k`
- `z_kc` – Bioethanol flow from plant `k` to county `c`
- `z_j ∈ {0,1}` – 1 if hub `j` is opened
- `w_k ∈ {0,1}` – 1 if biorefinery `k` is opened
- `n_jk` – Number of trains between hub `j` and plant `k`
- `m_ij` – Number of trucks between supplier `i` and hub `j`
- `p_kc` – Number of trucks between plant `k` and county `c`
- `d_{c,unmet}` – Unmet demand at county `c`
- `e_k` – Electricity generated at biorefinery `k`

### Objective

Maximize:

- **Revenue** from:
  - Bioethanol sales
  - Electricity sales

minus

- **Investment costs** (hubs & plants),
- **Transportation & loading costs** (truck + rail),
- **Penalties** for unmet demand.

Subject to:

- Supply limits,
- Demand satisfaction (with unmet demand variable),
- Hub and plant capacity limits,
- Flow conservation at hubs and plants,
- Vehicle capacity constraints (trucks & trains),
- Non-negativity and integrality of variables.

---

## Data & Sources

The model integrates multiple data sources to approximate realistic conditions:

- **Case data**: candidate hub/plant locations, investment costs, capacities, and initial distance/cost matrices.
- **County-level demand**: constructed from population data for all Texas counties.
- **Distances and transport costs**: road distances from plants to counties via the Google Maps Distance Matrix API; costs estimated by linear regression on provided cost matrices.
- **Fuel prices by county**: used as a factor in road transportation costs.
- **Land price index**: adjusts investment costs for hubs and plants.
- **Electricity prices and generation capacity**: based on ERCOT and real-world biofuel plant installations.
- **Vehicle characteristics**: capacities and loading cost estimates for DOT-406/407 trucks and DOT-117 railcars.
