# CSC314 Operations Research: Starting Point

## What This Course Is
Operations Research (OR) is about making the best decision when you have limited resources. You write the situation as maths, then solve it with a method that is guaranteed to find the best answer.

Typical questions:
- A factory has limited machine hours and material. How many of each product should it make to earn the most?
- Goods sit in 3 warehouses and 4 shops need them. What shipping plan costs the least?
- Two players choose strategies against each other. What is the best way to play?

Almost the whole course is one idea, **Linear Programming (LP)**, followed by variations on it. If LP clicks, the rest is mostly extensions.

## The Core Idea: Linear Programming
An LP has three parts:
- **Decision variables**: What you choose (how many chairs, how many tables)
- **Objective function**: What you want to maximise or minimise (profit, cost)
- **Constraints**: The limits you must respect (hours, materials, demand)

"Linear" means every expression is just numbers times variables added together (3x + 5y), with no x², no xy, no x/y.

## A Full Example (Do This by Hand)
A workshop makes chairs (x) and tables (y). Profit is ₦3 per chair and ₦5 per table. Each chair needs 1 hour of carpentry and each table needs 2, with 8 hours available. Each chair needs 3 hours of finishing and each table needs 2, with 12 hours available.

```
maximise  z = 3x + 5y
subject to  x + 2y ≤ 8      (carpentry hours)
           3x + 2y ≤ 12     (finishing hours)
            x, y ≥ 0
```

### Picture It
- Each constraint draws a line on an x-y graph, and the allowed region is the area under both lines (the **feasible region**)
- The region is a polygon. The key fact of LP is that the best answer is always at a **corner** of that polygon

### Check the Corners
| Corner | x | y | z = 3x + 5y |
|---|---|---|---|
| Origin | 0 | 0 | 0 |
| On x-axis | 4 | 0 | 12 |
| On y-axis | 0 | 4 | 20 |
| Where the two lines cross | 2 | 3 | **21** |

The crossing point is found by solving x + 2y = 8 and 3x + 2y = 12 together, giving x = 2, y = 3. Best plan: 2 chairs and 3 tables for ₦21 profit, using all hours of both resources.

### Now See the Simplex Method
Checking every corner works for 2 variables, but real problems have thousands of corners. **Simplex** walks along edges instead:
- Start at (0, 0), z = 0
- Look for the variable that improves z fastest per unit (y, worth 5), increase it until a constraint stops you (y = 4, from carpentry), which lands at (0, 4), z = 20
- From there, increasing x still helps, increase it until finishing stops you, landing at (2, 3), z = 21
- No move improves z any further, so stop: that corner is optimal

Path: (0,0) → (0,4) → (2,3). Simplex is just this walk done with a table, with row operations doing the moving.

## Why the Later Topics Exist
Each one answers a "what if" about this same example.

- **Duality**: Every LP has a twin problem that prices the resources. In the example, one extra carpentry hour is worth ₦2.25 and one extra finishing hour ₦0.25 (these are **shadow prices**)
- **Dual Simplex**: A version of simplex for when a solution looks optimal but breaks a constraint. Useful when you change a limit or add a new constraint
- **Revised Simplex**: Same walk, done with matrix algebra so a computer does less work
- **Sensitivity analysis (the "from inverse" topic)**: How much can profits or hours change before the best plan changes?
- **Parametric Programming**: Same question, but tracking a smooth change (what happens as profit per table goes from ₦5 up to ₦9?)
- **Integer Programming**: What if you cannot make 2.5 chairs? Whole-number answers are much harder, so you need methods like **Branch and Bound**
- **Transportation Problem**: LP with a particular shape (suppliers to destinations), which has a faster specialised method than general simplex
- **Game Theory**: Two opponents, the table of payoffs. Surprisingly, a game can be solved as an LP

## Glossary
- **Feasible**: Obeys all constraints
- **Optimal**: Feasible and the best objective value
- **Corner point (extreme point)**: A vertex of the feasible region, where the optimum lives
- **Slack variable**: Leftover unused resource, added to turn ≤ into = (simplex needs equations)
- **Surplus variable**: Amount above a minimum, subtracted to turn ≥ into =
- **Artificial variable**: A temporary helper to get simplex started when no obvious starting corner exists
- **Basic variable**: A variable allowed to be nonzero in the current corner
- **Shadow price**: Extra profit from one more unit of a resource
- **Primal / Dual**: The original LP and its twin
- **Degenerate**: A corner where more constraints meet than needed, can make simplex stall
- **Unbounded**: The objective can grow forever (usually a modelling mistake)
- **Infeasible**: No solution satisfies all constraints

## Suggested Study Order
- **Week 1**: LP formulation and the graphical method. Practise turning word problems into maths until it feels natural, because this is where most marks are won and lost
- **Week 2 to 3**: Simplex by hand (maximisation first, then Big-M for ≥ and = constraints). Do many small examples, speed comes from repetition
- **Week 4**: Duality and dual simplex
- **Then**: Sensitivity, parametric, and revised simplex (these assume you are comfortable with the tableau)
- **Then**: Transportation (separate method, can be done any time after Week 1)
- **Then**: Integer programming and game theory

## Prerequisite Refresh
Revise these from MTH230 and CSC212:
- Solving 2 to 3 simultaneous equations
- Matrix multiplication
- The inverse of a 2×2 and 3×3 matrix
- Row operations (Gauss-Jordan elimination)

## Tools
- **By hand**: Required for exams, so always do first
- **To check answers**: Python
```python
from scipy.optimize import linprog

# linprog minimises, so negate the objective to maximise
res = linprog(c=[-3, -5], A_ub=[[1, 2], [3, 2]], b_ub=[8, 12], bounds=(0, None))
print(res.x, -res.fun)   # [2. 3.] 21.0
```

## First Steps
- Redo the chairs and tables example on paper without looking, including the graph
- Invent your own variation (change the hours or profits) and solve it again
- Then move on to the full reference version, **CSC314-Operations-Research-Primer.md**, which holds the formulas and procedures for every topic
