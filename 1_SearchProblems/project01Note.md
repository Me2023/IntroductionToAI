## Question 1: Finding a Fixed Food Dot using Depth First Search
### How to Test `Search`
https://inst.eecs.berkeley.edu/~cs188/su22/project1/#question-1-3-points-finding-a-fixed-food-dot-using-depth-first-search
```
python3 pacman.py -l tinyMaze -p SearchAgent -a fn=tinyMazeSearch
```
The command above tells the `SearchAgent` to use `tinyMazeSearch` as its search algorithm, which is implemented in search.py. Pacman should navigate the maze successfully.

```
python3 pacman.py -l tinyMaze -p SearchAgent -a fn=depthFirstSearch
python3 pacman.py -l mediumMaze -p SearchAgent -a fn=depthFirstSearch
python3 pacman.py -l bigMaze -z .5 -p SearchAgent -a fn=depthFirstSearch
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
At first, I didn't figure out what a `state` stood for, and regarded `node` as `state`, which led to the incorrect code below: 


```python
frontier.push(problem.getStartState())
```
```python
node = frontier.pop()
```
```python
for childNode in problem.getSuccessors(node):
    frontier.push(childNode)
```


Just notice that a state is a tuple consisting of `node`, `cost` and `path`.
```python
state = (node, cost, path)  # tuple
```

Corrected:
```python
start = (problem.getStartState(), 0, [])    # (node, cost, path)
frontier.push(start)
```
```python
(node, cost, path) = frontier.pop()
```
```python
for childNode, childAction, childCost in problem.getSuccessors(node):
    newPath = path + [childAction]
    newCost = cost + childCost
    newState = (childNode, newCost, newPath)
    frontier.push(newState)
```
Final answer:
```python
reached = set()             # An empty set. "closed" is also ok
frontier = util.Stack()     # "fringe" is also ok
start = (problem.getStartState(), 0, [])    # (node, cost, path)
frontier.push(start)
while not frontier.isEmpty():
    (node, cost, path) = frontier.pop()
    if problem.isGoalState(node):
        return path
    if node not in reached:
        reached.add(node)
        for childNode, childAction, childCost in problem.getSuccessors(node):
            newPath = path + [childAction]
            newCost = cost + childCost
            newState = (childNode, newCost, newPath)
            frontier.push(newState)
```
What is `path`?
> In graph theory, a path in a graph is a finite or infinite sequence of edges which joins a sequence of vertices.

## Question 2: Breadth First Search
`breadthFirstSearch` in search.py
```
python3 pacman.py -l mediumMaze -p SearchAgent -a fn=bfs
python3 pacman.py -l bigMaze -p SearchAgent -a fn=bfs -z .5
```
```python
frontier = util.Queue()
```

## Question 3: Varying the Cost Function
`uniformCostSearch` in search.py
```
python3 pacman.py -l mediumMaze -p SearchAgent -a fn=ucs
python3 pacman.py -l mediumDottedMaze -p StayEastSearchAgent -a fn=ucs
python3 pacman.py -l mediumScaryMaze -p StayWestSearchAgent -a fn=ucs
```
```python
frontier = util.PriorityQueue()
frontier.push(start, 0)
frontier.push(newState, newCost)
```

## Question 4: A* search
### Arguments and Test
Implement A* graph search in the empty function `aStarSearch` in search.py. A* takes a `heuristic` function as an argument. 

Heuristics take two arguments: a `state` in the search problem (the main argument), and the `problem` itself (for reference information). 

The `nullHeuristic` heuristic function in search.py is a trivial example.

```python
def nullHeuristic(state, problem=None):
    """
    A heuristic function estimates the cost from the current state to the nearest
    goal in the provided SearchProblem.  This heuristic is trivial.
    """
    return 0
```
Test in terminal:
```
python3 pacman.py -l bigMaze -z .5 -p SearchAgent -a fn=astar,heuristic=manhattanHeuristic
```

### Final Answer
```python
totalCost = 0 + heuristic(startState[0], problem)
frontier.push(startState, totalCost)

newTotalCost = newCost + heuristic(newState[0], problem)
frontier.push(newState, newTotalCost)
```

*Note:* not `heuristic(startState, problem)` but `heuristic(startState[0], problem)`

---
The real power of A* will only be apparent with a more challenging search problem. Now, it’s time to formulate a *new problem* and *design a heuristic* for it.

## Question 5: Finding All the Corners
Implement the `CornersProblem` search problem in searchAgents.py. 

### How to Test our Problem
```
python3 pacman.py -l tinyCorners -p SearchAgent -a fn=bfs,prob=CornersProblem
python3 pacman.py -l mediumCorners -p SearchAgent -a fn=bfs,prob=CornersProblem
```
### My Trial
```python
currentPosition, corner1, corner2, corner3, corner4 = state
x, y = currentPosition
dx, dy = Actions.directionToVector(action)
nextx, nexty = int(x + dx), int(y + dy)
hitsWall = self.walls[nextx][nexty]
if not hitsWall:
    nextState = ((nextx, nexty), corner1, corner2, corner3, corner4)
    cost = 1
    if (nextx, nexty) in self.corners:      # touch a corner
        corIndex = 1
        for corPos in self.corners:         # which corner    
            if (nextx, nexty) == corPos:
                nextState[corIndex] == True
                break
            else:
                corIndex += 1
    successors.append( ( nextState, action, cost) )
```
- Problems:
    - `nextState[corIndex] == True` -> 'tuple' object does not support item assignment
    - `if...for` is not necessary

### Final Answer
```python
def getSuccessors(self, state: Any):
    successors = []
    for action in [Directions.NORTH, Directions.SOUTH, Directions.EAST, Directions.WEST]:
        currentPosition, corner1, corner2, corner3, corner4 = state
        x, y = currentPosition
        dx, dy = Actions.directionToVector(action)
        nextx, nexty = int(x + dx), int(y + dy)
        hitsWall = self.walls[nextx][nexty]
        if not hitsWall:
            nextPosition = (nextx, nexty)
            cost = 1
            nextState = (nextPosition, 
                            True if nextPosition == self.corners[0] else corner1,
                            True if nextPosition == self.corners[1] else corner2, 
                            True if nextPosition == self.corners[2] else corner3,
                            True if nextPosition == self.corners[3] else corner4)
            successors.append( ( nextState, action, cost) )
    self._expanded += 1
    return successors
```


## Question 6: Corners Problem: Heuristic
`cornersHeuristic` in searchAgents.py
### How to Test our Heuristic for the `CornersProblem`
```
python3 pacman.py -l mediumCorners -p AStarCornersAgent -z 0.5
```
AStarCornersAgent is a shortcut, or:
```
python3 pacman.py -l mediumCorners -p SearchAgent -a fn=aStarSearch,prob=CornersProblem,heuristic=cornersHeuristic
```

### Admissibility vs. Consistency
> ***Admissibility vs. Consistency:*** 
> 
> Remember, heuristics are just functions that take search states and return numbers that estimate the cost to a nearest goal. More effective heuristics will return values closer to the actual goal costs. 
> 
> - To be admissible, the heuristic values must be *lower bounds* on the actual shortest path cost to the nearest goal (and non-negative). 
> - To be consistent, it must additionally hold that if an action has cost c, then taking that action can only cause a *drop in heuristic of at most c*.

### My First Trial
```python
def cornersHeuristic(state: Any, problem: CornersProblem):
    corners = problem.corners # These are the corner coordinates
    walls = problem.walls # These are the walls of the maze, as a Grid (game.py)
    position, corner1, corner2, corner3, corner4 = state
    cornersState = (corner1, corner2, corner3, corner4)
    i = 0
    distance = 0
    for cornerPos in corners:
        if not cornersState[i]:         # not been hit
            distance += abs(position[0] - cornerPos[0]) + abs(position[1] - cornerPos[1])
        i += 1
    return distance
```
- Problems:
    - not takes `walls` into account -> relaxed search problem
    - can't work

### Final Answer
```python
def cornersHeuristic(state: Any, problem: CornersProblem):
    corners = problem.corners # These are the corner coordinates
    walls = problem.walls # These are the walls of the maze, as a Grid (game.py)
    position, corner1, corner2, corner3, corner4 = state
    cornersState = (corner1, corner2, corner3, corner4)
    i = 0
    distance = [0, 0, 0, 0]
    for cornerPos in corners:
        if not cornersState[i]:         # not been hit
            distance[i] = abs(position[0] - cornerPos[0]) + abs(position[1] - cornerPos[1])
        i += 1
    return max(distance)
```

---

## Question 7: Eating All The Dots
Fill in `foodHeuristic` in searchAgents.py with a consistent heuristic for the `FoodSearchProblem`. 

### Preview
A* with a null heuristic (equivalent to uniform-cost search) should quickly find an optimal solution to testSearch with no code change (total cost of 7).
```
python3 pacman.py -l testSearch -p AStarFoodSearchAgent
```
*Note:* AStarFoodSearchAgent is a shortcut for
```
-p SearchAgent -a fn=astar,prob=FoodSearchProblem,heuristic=foodHeuristic
```

### How to Test our heuristic for `FoodSearchProblem`
```
python3 pacman.py -l trickySearch -p AStarFoodSearchAgent
```

### about Arguments
- The state is a tuple ( pacmanPosition, foodGrid ) 
    - foodGrid is a `Grid` (see game.py) of either True or False. You can call `foodGrid.asList()` to get a list of food coordinates instead.

### My First Trial
I thought it is the same as the heuristic for `CornersProblem`, except for the number of "corners".
```python
def foodHeuristic(state: Tuple[Tuple, List[List]], problem: FoodSearchProblem):
    position, foodGrid = state
    foodPos = foodGrid.asList()
    i = 0
    distance = [0] * foodPos.len()
    for food in foodPos:
        distance[i] = abs(position[0] - food[0]) + abs(position[1] - food[1])
        i += 1
    return max(distance)
```
- can't work: when there is no food, distance is an empty list and can't be used by `max`
- `distance = [0] * (foodPos.len() + 1)`

### Final Answer
```python
def foodHeuristic(state: Tuple[Tuple, List[List]], problem: FoodSearchProblem):
    position, foodGrid = state
    "*** YOUR CODE HERE ***"
    foodPos = foodGrid.asList()
    distance = [0]
    for food in foodPos:
        distance.append(abs(position[0] - food[0]) + abs(position[1] - food[1]))
    return max(distance)
```

```python
def foodHeuristic(state: Tuple[Tuple, List[List]], problem: FoodSearchProblem):
    position, foodGrid = state
    foodPos = foodGrid.asList()
    i = 0
    distance = 0
    for food in foodPos:
        temp = abs(position[0] - food[0]) + abs(position[1] - food[1])
        if temp > distance:
            distance = temp
    return distance
```
Search nodes expanded: 9551


### another Solution
https://github.com/molson194/Artificial-Intelligence-Berkeley-CS188/blob/master/Project-1/searchAgents.py
```python
def foodHeuristic(state, problem):
    position, foodGrid = state
    x, y = position
    foodList = list(foodGrid.asList())
    maxX = 0
    maxY = 0
    minX = 0
    minY = 0

    for item in foodList:
        foodX, foodY = item
        xDistance = foodX - x
        yDistance = foodY - y
        if xDistance > maxX:
            maxX = xDistance
        elif xDistance < minX:
            minX = xDistance
        if yDistance > maxY:
            maxY = yDistance
        elif yDistance < minY:
            minY = yDistance
    return maxX - minX + maxY - minY
```
Search nodes expanded: 8600

## Question 8: Suboptimal Search
You’ll write an agent that always greedily eats the closest dot. `ClosestDotSearchAgent` is implemented for you in searchAgents.py, but it’s missing a key function that finds a path to the closest dot.

Implement the function `findPathToClosestDot` in searchAgents.py. 

*Hint:* The quickest way to complete `findPathToClosestDot` is to fill in the AnyFoodSearchProblem, which is missing its goal test. Then, solve that problem with an appropriate search function. The solution should be very short!

### How to Test
```
python3 pacman.py -l bigSearch -p ClosestDotSearchAgent -z .5
```