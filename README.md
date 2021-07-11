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
> LpVariable(name, lowBound=None, upBound=None, cat='Continuous'/'Integer', e=None ) 
> 
* name = name of decision variables
* lowBound = lower bound, None means negative infinity
* upBound = upper bound, None means positive infinity
* cat = type of varialbes,  Continuous'/'Integer'/'Binary'
* e = Used for column based modeling (???) 
#### 3. Define objective function
#### 4. Define constraints
#### 5. Solve Model
#### _Code example_ 
![Screen Shot 2021-07-10 at 7 49 17 PM](https://user-images.githubusercontent.com/47950186/125178956-9cbf9c80-e1b7-11eb-80e2-96a11bfdf097.png)

#### Multiple decision variables - lpSum
```
> cake_types = ["A", "B", "C", "D", "E", "F"]
> profit_by_cake = {"A":20, "B":40, "C":33, "D":14, "E":6, "F":60}
> var_dict = {"A":A, "B":B, "C":C, "D":D, "E":E, "F":F}
> model += lpSum([profit_by_cake[type] * var_dict[type] for type in cake_types])
```
