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
            assignment.add(var = value)
            result = recursiveBacktracking(assignment, csp)
            if result != failure:
                return result
            assignment.remove(var = value)
    return failure
```

#### Filtering
- forward checking
    - Arc Consistency Algorithm #3 (AC-3 algorithm)
    ```python
    def AC_3(csp):
        # inputs: csp, a binary CSP with variables{X1, X2, ..., Xn}
        # local variables: queue, a queue of arcs, initially all the arcs in csp
        while not queue.empty():
            queue.pop((Xi, Xj))
            if removeInconsistentValues(Xi, Xj):
                # Because we pruned a value from the domain of Xi, we need to enqueue all arcs with Xi at the head
                for Xk in neighbors(Xi):
                    queue.push((Xk, Xi))
                    
    def removeInconsistentValues(Xi, xj):
        removed = false
        for x in domain[Xi]:
            if no value y in domain[Xj] allows (x, y) to satisfy the constraint Xi <-> Xj:
                domain[Xi].delete(x)
                removed = true
        return removed
    ```
    The AC-3 algorithm has a worst case time complexity of O(ed3), where e is the number of arcs (directed edges) and d is the size of the largest domain.

    - k-consistency
    As an interesting parting note about consistency, arc consistency is a subset of a more generalized notion of consistency known as k-consistency, which when enforced guarantees that for any set of k nodes in the CSP, a consistent assignment to any subset of k−1 nodes guarantees that the kth node will have at least one consistent value.

    arc consistency is equivalent to 2-consistency.

#### Ordering
In practice, it’s often much more effective to compute the next variable and corresponding value "on the fly" with two broad principles, minimum remaining values and least constraining value.

- Minimum Remaining Values (MRV)
chooses whichever unassigned variable has the fewest valid remaining values (the most constrained variable)

- Least Constraining Value (LCV)
select the value that prunes the fewest values from the domains of the remaining unassigned values

#### Structure
- tree-structured CSP algorithm
    - cutset conditioning


### Local Search
In fact, local search appears to run in almost constant time and have a high probability of success not only for N-queens with arbitrarily large N, but also for any randomly generated CSP! 

However, despite these advantages, local search is both incomplete and suboptimal and so won’t necessarily converge to an optimal solution.

- Hill-Climbing Search (steepest-ascent)
    ```python
    def hillClimbing(problem):
        current = makeNode(problem.initialState())
        while 1:
            neighbor = current.highestValuedSuccessor()
            if neighbor.value <= current.value:
                return current.state
            current = neighbor
    ```

    - Simulated Annealing Search
        Simulated annealing aims to combine random walk (randomly moves to nearby states) and hill-climbing to obtain a complete and efficient search algorithm.

        If on the other hand it leads to smaller objectives then the move is accepted with *some probability*. This probability is determined by the *temperature parameter*, which initially is high (more “bad" moves allowed) and gets decreased according to some schedule. 
        > If temperature is decreased slowly enough then the simulated annealing algorithm will reach the global maximum with probability approaching 1.

        ```python
        def simulatedAnnealing(problem, schedule):
            current = problem.initialState()
            for t in range(1, inf):
                T = schedule(t)
                if T == 0:
                    return current
                nextOne = current.randomSuccessor()
                dE = nextOne.value - current.value
                if dE > 0:
                    current = nextOne
                else:
                    current = nextOne only with probability exp(dE/T)
        ```

    - Local Beam Search
        The key difference between the two is that local beam search keeps track of k states (threads) at each iteration.

        - Genetic Algorithms
        ```python
        def geneticAlgorithm(population, fitnessFn):
            # inputs:
            # - population, a set of individuals
            # - fitnessFn, a function that measures the fitness of an individual
            while fitEnoughOrTimeEnough(population):
                newPopulation = set()
                for i in range(1, size(population)):
                    x = randomSelection(population, fitnessFn)
                    y = randomSelection(population, fitnessFn)
                    child = reproduce(x, y)
                    if smallRandomProbability():
                        child = mutate(child)
                    newPopulation.add(child)
                population = newPopulation
            return bestIndividual(population, fitnessFn)

        def reproduce(x, y):
            # inputs: x, y, parent individuals
            n = length(x)
            c = random(1, n)
            return append(substring(x, 1, c), substring(y, c+1, n))
        ```
## Summary
It’s important to remember that constraint satisfaction problems in general do not have an efficient algorithm which solves them in polynomial time with respect to the number of variables involved.

However, by using various heuristics, we can often find solutions in an acceptable amount of time.