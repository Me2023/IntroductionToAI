## Question 1 (3 points): Finding a Fixed Food Dot using Depth First Search
### How to Test `SearchAgent`
https://inst.eecs.berkeley.edu/~cs188/su22/project1/#question-1-3-points-finding-a-fixed-food-dot-using-depth-first-search
```
python3 pacman.py -l tinyMaze -p SearchAgent -a fn=tinyMazeSearch
```
The command above tells the `SearchAgent` to use `tinyMazeSearch` as its search algorithm, which is implemented in search.py. Pacman should navigate the maze successfully.

```
python3 pacman.py -l tinyMaze -p SearchAgent
python3 pacman.py -l mediumMaze -p SearchAgent
python3 pacman.py -l bigMaze -z .5 -p SearchAgent
```
```
python3 autograder.py -q q1
```
### What to do in this Question
Implement the **depth-first search (DFS) algorithm** in the `depthFirstSearch` function in search.py. To make your algorithm complete, write the **graph search version of DFS**, which avoids expanding any already visited states.

### a Solution
https://github.com/filR/edX-CS188.1x-Artificial-Intelligence/blob/master/Project%201%20-%20Search%20in%20Pacman/search.py
```python
def generic_search(problem, fringe, add_to_fringe_fn):
    closed = set()
    start = (problem.getStartState(), 0, [])  # (node, cost, path)
    add_to_fringe_fn(fringe, start, 0)

    while not fringe.isEmpty():
        (node, cost, path) = fringe.pop()

        if problem.isGoalState(node):
            return path

        if not node in closed:
            closed.add(node)

            for child_node, child_action, child_cost in problem.getSuccessors(node):
                new_cost = cost + child_cost
                new_path = path + [child_action]
                new_state = (child_node, new_cost, new_path)
                add_to_fringe_fn(fringe, new_state, new_cost)

def depthFirstSearch(problem):
    fringe = util.Stack()
    def add_to_fringe_fn(fringe, state, cost):
        fringe.push(state)

    return generic_search(problem, fringe, add_to_fringe_fn)

```
### My Trial
