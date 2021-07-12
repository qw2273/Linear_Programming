# Notes taken from online class [Supply Chain Analytics in Python](https://campus.datacamp.com/courses/supply-chain-analytics-in-python/)


### What is Linear Programing(LP)
LP is a modeling tool for Optimization

### What is Optimization
Optimization use mathematical model whose requirements are linear relationships

### Basic Components in LP
1. Decision variables: variables decide the output
2. Objective Function: math expression that use variables to express goal
3. Constraint: limitations on decision variables

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
_Status of Solution_

![Screen Shot 2021-07-11 at 10 47 57 PM](https://user-images.githubusercontent.com/47950186/125223427-bf29e680-e299-11eb-9cf6-a45f4bb33a99.png)


#### Multiple decision variables & Complex problem 
__LpVariable.dicts() & lpSum()__


### Logical Constraints
![Screen Shot 2021-07-11 at 2 00 09 PM](https://user-images.githubusercontent.com/47950186/125205400-0a1f0c00-e250-11eb-830d-7e3697e46c1f.png)


#### _Code example_ 
```
# Initialize, Define Decision Vars., Object. Fun., and Constraints
model = LpProblem("Capacitated Plant Location Model", LpMinimize)

loc = ['USA', 'Germany', 'Japan', 'Brazil', 'India']
size = ['Low_Cap','High_Cap']
fix_cost = {'High_Cap': {'Brazil': 4730,'Germany': 7270,  'India': 3080, 'Japan': 9100,'USA': 9500},
            'Low_Cap': {'Brazil': 3230,  'Germany': 4980, 'India': 2110, 'Japan': 6230, 'USA': 6500}}
fix_cost = pd.DataFrame.from_dict(fix_cost)
var_cost = {'Brazil': {'Brazil': 8, 'Germany': 14, 'India': 23, 'Japan': 21, 'USA': 12},
          'Germany': {'Brazil': 14, 'Germany': 6, 'India': 13, 'Japan': 14, 'USA': 13},
          'India': {'Brazil': 21, 'Germany': 13, 'India': 8, 'Japan': 9, 'USA': 17},
          'Japan': {'Brazil': 21, 'Germany': 14, 'India': 10, 'Japan': 3, 'USA': 20},
          'USA': {'Brazil': 12, 'Germany': 13, 'India': 22, 'Japan': 20, 'USA': 6}}
var_cost = pd.DataFrame.from_dict(var_cost)
demand = {'Dmd': {'Brazil': 145.4,  'Germany': 84.1, 'India': 156.4,'Japan': 1676.8,  'USA': 2719.6}}
demand  = pd.DataFrame.from_dict(demand)

production = LpVariable.dicts("production_", [(i,j) for i in loc for j in loc],lowBound=0, upBound=None, cat='Integer')
plant_flag = LpVariable.dicts("plant_", [(i,s) for s in size for i in loc], cat='Binary')

model += (lpSum([fix_cost.loc[i,s] * plant_flag[(i,s)] for s in size for i in loc])
          + lpSum([var_cost.loc[i,j] * production[(i,j)] for i in loc for j in loc]))

for j in loc:
    model += lpSum([production[(i, j)] for i in loc]) == demand.loc[j,'Dmd']
for i in loc:
    model += lpSum([production[(i, j)] for j in loc]) <= lpSum([cap.loc[i,s]*plant_flag[(i,s)] for s in size])

# Define logical constraint
# if the high capacity plant in USA is open, then a low capacity plant in Germany is also opened
model += plant_flag[('USA', 'High_Cap')] - plant_flag[('Germany', 'Low_Cap')] <= 0

# Solve Model
model.solve()
print(LpStatus[model.status])

for v in model.variables():
    print (v.name , "=", v.varValue)

print("Objective", value(model.objective))    

------ print results in dataframe format ---------
df_list = [] 
for j in loc: 
    df_list.append( [production[i, j ].varValue for i in loc]) 
production_optimal_results = pd.DataFrame(df_list, columns = loc, index = loc )

df_list = [] 
for j in loc: 
    df_list.append( [plant_flag[j, s ].varValue for s in size ]) 
plant_optimal_results = pd.DataFrame(df_list, columns = size, index = loc )


# save LP
writeLP(yourfilename) 
f = open(yourfilename)
print(f.read())
f.close()

```



### Sanity checking the solution
![Screen Shot 2021-07-11 at 11 25 58 PM](https://user-images.githubusercontent.com/47950186/125226446-1aaaa300-e29f-11eb-9f7f-9715c308e23d.png)

#### Status <> Optimal is a Red Flag
![Screen Shot 2021-07-11 at 11 28 01 PM](https://user-images.githubusercontent.com/47950186/125226636-72490e80-e29f-11eb-929c-141d767d6a61.png)

![Screen Shot 2021-07-11 at 11 32 55 PM](https://user-images.githubusercontent.com/47950186/125227016-1632ba00-e2a0-11eb-8289-da2b9e44d505.png)

