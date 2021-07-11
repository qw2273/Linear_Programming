### What is Linear Programing(LP)
LP is a modeling tool for Optimization
### What is Optimization
Optimization use mathematical model whose requirements are linear relationships
### Basic Components in LP
1. Decision variables: what you can control
2. Objective Function: math expression that use variables to express goal
3. Constraint: math expression that describe the limits of a solution
### Type of modeling optimization
![Screen Shot 2021-07-10 at 7 11 51 PM](https://user-images.githubusercontent.com/47950186/125178375-5f0c4500-e1b2-11eb-8835-07eac65d99c5.png)
### Modeling framework for LP and IP problems written in Python : PuLP
### Common modeling process for PuLP
#### 1. Intialize Model: __LpProblem()__
> model = LpProblem(name = "Name of your problem", sense = LpMinimize/LpMaxmize) 
> 
#### 2. Define decision variables: __LpVariable()__
> LpVariable(name,indexs,  lowBound=None, upBound=None, cat='Continuous', e=None ) 
> 
* name = name of decision variables
* indexs = A list of strings of the keys to the dictionary of LPvariables
* lowBound = lower bound, None means negative infinity
* upBound = upper bound, None means positive infinity
* cat = type of varialbes,  Continuous'/'Integer'/'Binary'
* e = Used for column based modeling (???) 
#### 3. Define objective function
#### 4. Define constraints
#### 5. Solve Model
#### _Code example_ 
![Screen Shot 2021-07-10 at 7 49 17 PM](https://user-images.githubusercontent.com/47950186/125178956-9cbf9c80-e1b7-11eb-80e2-96a11bfdf097.png)

#### Multiple decision variables & Complex problem 
__LpVariable.dicts() & lpSum()__

```
# Initialize Model
model = LpProblem("Minimize Transportation Costs", LpMinimize)

# Define decision variable 
warehouse = ['New York', 'Atlanta']
customers = ['East', 'South', 'Midwest', 'West']
regional_demand = [1800, 1200, 1100, 1000]
costs = {('Atlanta', 'East'): 232,
         ('Atlanta', 'Midwest'): 230,
         ('Atlanta', 'South'): 212,
         ('Atlanta', 'West'): 280,
         ('New York', 'East'): 211,
         ('New York', 'Midwest'): 240,
         ('New York', 'South'): 232,
         ('New York', 'West'): 300}
transport = LpVariable.dicts("route", [(w,c) for w in warehouse for c in customers], lowBound=0, cat='Integer')

# Define Objective 
# lpSum: sum of linear expressions
model += lpSum([cost[(w,c)]*transport[(w,c)] for w in warehouse for c in customers])

# For each customer, sum warehouse shipments and set equal to customer demand
for c in customers:
    model += lpSum([var_dict[(w, c)] for w in warehouse]) == demand[c]

```

