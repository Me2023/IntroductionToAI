# Project1 Intro

https://inst.eecs.berkeley.edu/~cs188/su22/project1/

# Uninformed Search


```python
def treeSearch(problem, frontier):
    frontier = insert(makeNode(initialState[problem]), frontier)
    while not isEmpty(frontier):
        node = pop(frontier)
        if problem.isGoal(node.state):
            return node
        for childNode in expand(problem, node):
            add childNode to frontier
    return failure
```
```python
    def EXPAND(problem, node) yields nodes:
        s = node.STATE
        for each action in problem.ACTIONS(s):
            s′ = problem.RESULT(s, action)
            yield NODE(STATE=s′, PARENT=node, ACTION=action)
```

```python
def graphSearch(problem, frontier):
    reached = set()
    frontier = insert(makeNode(initialState[problem]), frontier)
    while not isEmpty(frontier):
        node = pop(frontier)
        if problem.isGoal(node.state):
            return node
        if node.state is not in reached:
            add node.state in reached
            for childNode in expand(problem, node):
                frontier = insert(childNode, frontier)
    return failure
```

## Depth-First Search (DFS)
Removing the deepest node and replacing it on the frontier with its children necessarily means the children are now the new deepest nodes.

To implement DFS. we require a structure that always gives the most recently added objects highest priority.

A *last-in, first-out (LIFO) stack* does exactly this.

## Breath-First Search (BFS)
If we want to visit shallower nodes before deeper nodes, we must visit
nodes in their order of insertion. 

Hence, we desire a structure that outputs the oldest enqueued object to represent our frontier. 

For this, BFS uses a *first-in, first-out (FIFO) queue*, which does exactly this.

## Uniform Cost Search (UCS)
Uniform cost search (UCS), our last strategy, is a strategy for exploration that always selects the lowest cost frontier node from the start node for expansion.

To represent the frontier for UCS, the choice is usually a *heap-based priority
queue*, where the priority for a given enqueued node v is the path cost from the start node to v, or the backward cost of v. 

Intuitively, a priority queue constructed in this manner simply reshuffles itself to maintain the desired ordering by path cost as we remove the current minimum cost path and replace it with its children.

# Informed Search
## Heuristics
## Greedy Search
## A* Search
A* search is a strategy for exploration that always selects the frontier node with the *lowest estimated total cost* for expansion, where total cost is the entire cost from the start node to the goal node.

Just like greedy search and UCS, A* search also uses a *priority queue* to represent its frontier. 

Again, the only difference is the method of priority selection. A* combines the *total backward cost* (sum of edge weights in the path to the state) used by UCS with the *estimated forward cost* (heuristic value) used by greedy search by adding these two values, effectively yielding an *estimated total cost* from start to goal.