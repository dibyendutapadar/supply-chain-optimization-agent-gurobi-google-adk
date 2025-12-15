# Python Model Class Reference - Gurobi Optimizer Reference Manual


---------------------------------------------------------
---

---

*class* Model[#](#Model "Link to this definition")

Gurobi model object. Common methods include [`optimize`](#Model.optimize "Model.optimize"), [`printStats`](#Model.printStats "Model.printStats"), [`printAttr`](#Model.printAttr "Model.printAttr"), and [`write`](#Model.write "Model.write"). Common model-building methods include [`addVar`](#Model.addVar "Model.addVar"), [`addVars`](#Model.addVars "Model.addVars"), [`addMVar`](#Model.addMVar "Model.addMVar"), [`addConstr`](#Model.addConstr "Model.addConstr"), and [`addConstrs`](#Model.addConstrs "Model.addConstrs").

Model(*name=''*, *env=None*)
[#](#Model.Model "Link to this definition")

Model constructor.

Parameters:

* **name** – Name of new model (ASCII only; spaces discouraged, non-ASCII characters not allowed, and LP format may fail).

* **env** – (optional) Gurobi environment.

Returns:

New model object with no variables or constraints.

Example:

```
# Default environment
model1 = gp.Model("NewModel1")
```

---

## addVar

addVar(*lb=0.0*, *ub=float('inf')*, *obj=0.0*, *vtype=GRB.CONTINUOUS*, *name=''*, *column=None*)

Add a single decision variable.

Parameters:

* **lb** – Lower bound.
* **ub** – Upper bound.
* **obj** – Objective coefficient.
* **vtype** – Variable type (`GRB.CONTINUOUS`, `GRB.BINARY`, `GRB.INTEGER`, `GRB.SEMICONT`, `GRB.SEMIINT`).
* **name** – Variable name (ASCII only; spaces discouraged).
* **column** – Column object specifying constraint participation.

Returns:

New variable object.

Example:

```
x = model.addVar()
y = model.addVar(vtype=GRB.INTEGER, obj=1.0, name="y")
z = model.addVar(0.0, 1.0, 1.0, GRB.BINARY, "z")
```

---

## addVars

addVars(**indices*, *lb=0.0*, *ub=float('inf')*, *obj=0.0*, *vtype=GRB.CONTINUOUS*, *name=''*)

Add multiple decision variables. Returns a [`tupledict`](about:blank/tupledict.html#tupledict "tupledict") indexed by the provided indices.

Indices can be:

* Integers, creating multi-dimensional arrays.
* Lists of immutable objects, creating the Cartesian product.
* A list of tuples, enabling sparse indexing.

Index elements must be scalar types (`int`, `float`, `string`, etc.).

Named arguments (`lb`, `ub`, `obj`, `vtype`) may be scalars, dicts keyed by indices, or lists (when a single index list is used). A scalar `name` is automatically subscripted by indices.

Parameters:

* **indices** – Indices for accessing variables.
* **lb** – Lower bound(s).
* **ub** – Upper bound(s).
* **obj** – Objective coefficient(s).
* **vtype** – Variable type(s).
* **name** – Base name(s) (ASCII only; spaces discouraged).

Returns:

New `tupledict` of variables.

Example:

```
x = model.addVars(3, 4, 5, vtype=GRB.BINARY)
l = tuplelist([(1, 2), (1, 3), (2, 3)])
y = model.addVars(l, ub=[1, 2, 3])
z = model.addVars(3, name=["a", "b", "c"])
```

---

## addConstr

addConstr(*constr*, *name=''*)

Add a single constraint from a [`TempConstr`](about:blank/tempconstr.html#TempConstr "TempConstr"). Supports linear, matrix, quadratic, and general constraints.

Parameters:

* **constr** – `TempConstr` expression.
* **name** – Constraint name (ASCII only; spaces discouraged).

Returns:

A constraint object (`Constr`, `MConstr`, `QConstr`, or `MQConstr`).

Example:

```
model.addConstr(x + y <= 2.0, "c1")
model.addConstr(x*x + y*y <= 4.0, "qc0")
model.addConstr(x + y + z == [1, 2], "rgc0")
model.addConstr(A @ t >= b)
model.addConstr(z == and_(x, y, w), "gc0")
model.addConstr((w == 1) >> (x + y <= 1), "ic0")
```

Warning

> [!info] Only one comparison operator is allowed per constraint.

---

## addConstrs

addConstrs(*generator*, *name=''*)

Add multiple constraints using a Python generator expression. Returns a [`tupledict`](about:blank/tupledict.html#tupledict "tupledict") indexed by generator values.

Each generator iteration produces one constraint. Generated names are automatically subscripted when `name` is provided. Index values must be scalar types.

Supports linear, quadratic, and general constraints.

Parameters:

* **generator** – Generator expression producing constraints.
* **name** – Base name for generated constraints (ASCII only; spaces discouraged).

Returns:

Dictionary of constraint objects.

Example:

```
model.addConstrs(x.sum(i, '*') <= capacity[i] for i in range(5))
model.addConstrs(x[i] + x[j] <= 1 for i in range(5) for j in range(5))
model.addConstrs(z[i] == max_(x[i], y[i]) for i in range(5))
```

Warning

> [!info] Only one comparison operator is allowed per constraint.

---

## addMVar

addMVar(*shape*, *lb=0.0*, *ub=float('inf')*, *obj=0.0*, *vtype=GRB.CONTINUOUS*, *name=''*)

Add an [`MVar`](about:blank/mvar.html#MVar "MVar"), a NumPy-like ndarray of Gurobi variables with arbitrary dimensions. Supports NumPy indexing, slicing, and matrix expressions for linear or quadratic objectives and constraints.

Parameters:

* **shape** – Int or tuple of ints defining dimensions.
* **lb** – Lower bound(s).
* **ub** – Upper bound(s).
* **obj** – Objective coefficient(s).
* **vtype** – Variable type(s).
* **name** – Base name or ndarray of names (ASCII only; spaces discouraged).

Argument values may be scalars, lists, or ndarrays broadcastable to `shape`.

Returns:

New `MVar` object.

Example:

```
x = model.addMVar((4, 2), vtype=GRB.BINARY)
y = model.addMVar((3,), lb=[-1, -2, -1])
```

---

## optimize

optimize(*callback=None*, *wheres=None*)

Optimize the model using the appropriate algorithm for its type. Populates solution attributes and processes all pending model modifications. Optimization may terminate early due to parameter limits.

Parameters:

* **callback** – Optional callback function `(model, where)`.
* **wheres** – Optional list of callback `where` codes to enable.

Note

Callbacks may be invoked at `POLLING` even if not explicitly enabled.

Example:

```
model.optimize()

model.optimize(callback)

model.optimize(callback, wheres=[GRB.Callback.MIPSOL])
```

---
## getConstrByName

getConstrByName(_name_)
[#](#Model.getConstrByName "Link to this definition")

Retrieve a linear constraint from its name. If multiple linear constraints have the same name, this method chooses one arbitrarily.

Parameters:

**name** – Name of desired constraint.

Returns:

Constraint with the specified name.

Example:

```
c0 = model.getConstrByName("c0")

```



---
---

# FEW SHOT CODE Example

## Example 1 : 
Minimizing cost of transportation between distribution and production, where production has can produce constraint and distribution has distribution constraint.


```python
import gurobipy as gp
from gurobipy import GRB

# Sets
production = ['Baltimore', 'Cleveland', 'Little_Rock', 'Birmingham', 'Charleston']
distribution = ['Columbia', 'Indianapolis', 'Lexington', 'Nashville', 'Richmond', 'St._Louis']

# Parameters
transportation_cost = {
    'Baltimore': {'Columbia': 4.5, 'Indianapolis': 5.09, 'Lexington': 4.33, 'Nashville': 5.96, 'Richmond': 1.96, 'St._Louis': 7.3},
    'Cleveland': {'Columbia': 2.43, 'Indianapolis': 2.37, 'Lexington': 2.54, 'Nashville': 4.13, 'Richmond': 3.2, 'St._Louis': 4.88},
    'Little_Rock': {'Columbia': 6.42, 'Indianapolis': 4.83, 'Lexington': 3.39, 'Nashville': 4.4, 'Richmond': 7.44, 'St._Louis': 2.92},
    'Birmingham': {'Columbia': 3.33, 'Indianapolis': 4.33, 'Lexington': 3.38, 'Nashville': 1.53, 'Richmond': 5.95, 'St._Louis': 4.01},
    'Charleston': {'Columbia': 3.02, 'Indianapolis': 2.61, 'Lexington': 1.61, 'Nashville': 4.44, 'Richmond': 2.36, 'St._Louis': 4.6}
}

demand = {
    'Columbia': 89,
    'Indianapolis': 95,
    'Lexington': 121,
    'Nashville': 101,
    'Richmond': 116,
    'St._Louis': 181
}

can_produce = {
    'Baltimore': 180,
    'Cleveland': 200,
    'Little_Rock': 140,
    'Birmingham': 80,
    'Charleston': 180
}

must_produce = {
    'Baltimore': 135,
    'Cleveland': 150,
    'Little_Rock': 105,
    'Birmingham': 60,
    'Charleston': 135
}

min_production_level = 0.75

# Create a new model
m = gp.Model('supply_chain_optimization')

# Decision Variables
x = m.addVars(production, distribution, vtype=GRB.CONTINUOUS, name="x")



# Constraints
# Meet demand at each distribution center
m.addConstrs((gp.quicksum(x[p, d] for p in production) >= demand[d] for d in distribution), name="meet_demand")

# Production capacity constraints
m.addConstrs((gp.quicksum(x[p, d] for d in distribution) <= can_produce[p] for p in production), name="can_produce_cap")

# Minimum production level constraint
m.addConstrs((gp.quicksum(x[p, d] for d in distribution) >= min(min_production_level * can_produce[p], must_produce[p]) for p in production), name="min_production")

# Objective: Minimize the total shipping cost
m.setObjective(gp.quicksum(transportation_cost[p][d] * x[p, d] for p in production for d in distribution), GRB.MINIMIZE)

# Optimize the model
m.optimize()

# initialize used production
used = {p: 0.0 for p in can_produce}

for v in m.getVars():
    # adjust this parsing to your variable naming
    for p in can_produce:
        if v.varName.startswith(f"x[{p},"):
            used[p] += v.x
            

    
result = {
    "ObjectiveValue": m.ObjVal,

    "decision": {
        v.VarName: v.X
        for v in m.getVars()
    },

    "production_metrics": {
        p: {
            "Capacity": can_produce[p],

            "Remaining": m.getConstrByName(
                f"can_produce_cap[{p}]"
            ).Slack,

            "Utilization": (
                (can_produce[p]
                 - m.getConstrByName(f"can_produce_cap[{p}]").Slack)
                / can_produce[p]
                if can_produce[p] > 0 else None
            )
        }
        for p in production
    }
}

```



## Example 2
A company operates a three-tier supply chain with factories, wholesalers, and retailers, where retailer demand must be fully met either through direct factory shipments or via wholesalers. Each transportation route has a per-unit cost, factories are limited by production capacity, and wholesalers are constrained by how much they can move onward. The objective is to minimize total transportation cost while satisfying all demand and capacity constraints using a linear programming formulation.


```python
import gurobipy as gp
from gurobipy import GRB

# Define parameters
factories = ['FA', 'FB']
wholesalers = ['WA', 'WB']
retailers = ['RA', 'RB', 'RC', 'RD', 'RE', 'RF', 'RG']

factory_to_wholesaler_cost = {
    'FA': {'WA': 6, 'WB': 8},
    'FB': {'WA': 7, 'WB': 5}
}

factory_to_retailer_cost = {
    'FA': {
        'RA': 10, 'RB': 12, 'RC': 11, 'RD': 9,
        'RE': 13, 'RF': 14, 'RG': 12
    },
    'FB': {
        'RA': 11, 'RB': 9, 'RC': 10, 'RD': 12,
        'RE': 8, 'RF': 13, 'RG': 11
    }
}

wholesaler_to_retailer_cost = {
    'WA': {
        'RA': 4, 'RB': 5, 'RC': 6, 'RD': 5,
        'RE': 6, 'RF': 7, 'RG': 5
    },
    'WB': {
        'RA': 5, 'RB': 4, 'RC': 5, 'RD': 6,
        'RE': 4, 'RF': 6, 'RG': 5
    }
}

retailer_demand = {
    'RA': 40, 'RB': 35, 'RC': 50,
    'RD': 45, 'RE': 30, 'RF': 25, 'RG': 20
}

wholesaler_capacity = {
    'WA': 120,
    'WB': 100
}

factory_capacity = {
    'FA': 140,
    'FB': 120
}

# Create a Gurobi Model
model = gp.Model("SupplyChainOptimization")

# Decision variables
x_fw = model.addVars(
    factories, wholesalers,
    lb=0, vtype=GRB.CONTINUOUS, name="x_fw"
)

x_fr = model.addVars(
    factories, retailers,
    lb=0, vtype=GRB.CONTINUOUS, name="x_fr"
)

y_wr = model.addVars(
    wholesalers, retailers,
    lb=0, vtype=GRB.CONTINUOUS, name="y_wr"
)

# Constraints
# Demand constraint
for j in retailers:
    model.addConstr(
        gp.quicksum(x_fr[i, j] for i in factories)
        + gp.quicksum(y_wr[k, j] for k in wholesalers)
        == retailer_demand[j],
        "Demand_{}".format(j)
    )

# Factory capacity constraint
for i in factories:
    model.addConstr(
        gp.quicksum(x_fr[i, j] for j in retailers)
        + gp.quicksum(x_fw[i, k] for k in wholesalers)
        <= factory_capacity[i],
        "FactoryCapacity_{}".format(i)
    )

# Wholesaler capacity constraint
for k in wholesalers:
    model.addConstr(
        gp.quicksum(y_wr[k, j] for j in retailers)
        <= wholesaler_capacity[k],
        "WholesalerCapacity_{}".format(k)
    )

# Objective Function
model.setObjective(
    gp.quicksum(
        factory_to_wholesaler_cost[i][k] * x_fw[i, k]
        for i in factories for k in wholesalers
    )
    + gp.quicksum(
        factory_to_retailer_cost[i][j] * x_fr[i, j]
        for i in factories for j in retailers
    )
    + gp.quicksum(
        wholesaler_to_retailer_cost[k][j] * y_wr[k, j]
        for k in wholesalers for j in retailers
    ),
    GRB.MINIMIZE
)

# Optimize the model
model.optimize()

# Result storage
result = {
    "ObjectiveValue": model.ObjVal,

    "decision": {
        v.VarName: v.X
        for v in model.getVars()
    },

    "metrics": {
        "factory_capacity": {
            i: {
                "Capacity": factory_capacity[i],
                "Remaining": model.getConstrByName(
                    f"FactoryCapacity_{i}"
                ).Slack,
                "Utilization": (
                    (factory_capacity[i]
                     - model.getConstrByName(f"FactoryCapacity_{i}").Slack)
                    / factory_capacity[i]
                    if factory_capacity[i] > 0 else None
                )
            }
            for i in factories
        },

        "wholesaler_capacity": {
            k: {
                "Capacity": wholesaler_capacity[k],
                "Remaining": model.getConstrByName(
                    f"WholesalerCapacity_{k}"
                ).Slack,
                "Utilization": (
                    (wholesaler_capacity[k]
                     - model.getConstrByName(f"WholesalerCapacity_{k}").Slack)
                    / wholesaler_capacity[k]
                    if wholesaler_capacity[k] > 0 else None
                )
            }
            for k in wholesalers
        }
    }
}

```