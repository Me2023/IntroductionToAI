# Uninformed Search

```python
def treeSearch(problem, frontier):
    frontier = insert(makeNode(initialState[problem]), frontier)
    while not isEmpty(frontier):
        node = pop(frontier)
        if problem.isGoal(node.state):
            return node
        for child-node in expand(problem, node):
            add child-node to frontier
    return failure
```