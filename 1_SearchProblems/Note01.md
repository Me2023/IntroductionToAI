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

## Depth-First Search
Removing the deepest node and replacing it on the frontier with its children necessarily means the children are now the new deepest nodes.

To implement DFS. we require a structure that always gives the most recently added objects highest priority.

A *last-in, first-out (LIFO) stack* does exactly this.

