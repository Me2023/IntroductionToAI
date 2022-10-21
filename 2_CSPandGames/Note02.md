# Constraint Satisfaction Problems (CSPs)

## Examples

### N-queens identification problem

### Constraint Graphs: Map Coloring
Constraint satisfaction problems are often represented as constraint graphs, where nodes represent variables and edges represent constraints between them.
- Types of Constraints
    - Unary Constraints
    - Binary Constraints
    - Higher-order Constraints

## Solving Constraint Satisfaction Problems
### Backtracking Search
Constraint satisfaction problems are traditionally solved using a search algorithm known as **backtracking search**. Backtracking search is an optimization on *depth first search* used specifically for the problem of constraint satisfaction, with improvements coming from two main principles:
1. Fix an ordering for variables, and select values for variables in this order.

2. When selecting values for a variable, only select values that don’t conflict with any previously assigned values. If no such values exist, backtrack and return to the previous variable, changing its value.

The pseudocode for how recursive backtracking works is presented below:

```python
def backtrackingSearch(csp):
    return recursiveBacktracking({}, csp)

def recursiveBacktracking(assignment, csp):
    if assignment.isComplete():
        return assignment
    var = selectUnassignedVariable(variables[csp], assignment, csp)
    for value in orderDomainValues(var, assignment, csp):
        if assignment.isConsistent(value): # given constriants[csp]
            

```