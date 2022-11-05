# Adversarial Search Problems (Games)
As opposed to normal search, which returned a comprehensive plan, adversarial search returns a strategy, or policy, which simply recommends the best possible move given some configuration of our agent(s) and their adversaries.

The standard game formulation consists of the following definitions:
- Initial state, s0
- Players, Players(s) denote whose turn is
- Actions, Actions(s) available actions for the player
- Transition model Result(s,a)
- Terminal test, Terminal−test(s)
- Terminal values, Utility(s, player)

## Deterministic Zero-sum Games
The first class of games we’ll cover are deterministic zero-sum games, games where actions are deterministic and our gain is directly equivalent to our opponent’s loss and vice versa. 

The easiest way to think about such games is as being defined by *a single variable value*, which one team or agent tries to *maximize* and the opposing team or agent tries to *minimize*, effectively putting them in direct competition. 

### Minimax
The first zero-sum-game algorithm we will consider is minimax, which runs under the motivating assumption that the opponent we face behaves optimally, and will always perform the move that is worst for us.

- terminal utilities and state value
The *value* of a *terminal state*, called a *terminal utility*, is always some deterministic known value and an inherent game property.

the *value* of a *non-terminal state* is defined as the maximum/minimum of the values of its children (or successors)

    - agent-controlled states: max(successors)
    - opponent-controlled states: min(successors)
    - terminal states: known

```python
def value(state):
    if state.isTerminal():
        return state.getUtility()
    elif agent.isMaxAgent():
        return maxValue(state)
    elif agent.isMinAgent():
        return minValue(state)

def maxValue(state):
    v = -inf
    for successor in state.getSuccessors():
        v = max(v, value(successor))
    return v

def minValue(state):
    v = +inf
    for successor in state.getSuccessors():
        v = min(v, value(successor))
    return v
```

#### Alpha-Beta Pruning
Minimax seems just about perfect - it’s simple, it’s optimal, and it’s intuitive. Yet, its execution is very
similar to depth-first search and it’s time complexity is identical, a dismal $O(b^m)$.

Implementing such pruning can reduce our runtime to as good as $O(b^(m/2))$, effectively doubling our "solvable" depth.

$\alpha$: MAX's best option on path to root

$\beta$: MIN's best option on path to root

```python
def maxValue(state, alpha, beta):
    v = -inf
    for successor in state.getSuccessors():
        v = max(v, value(successor, alpha, beta))
        if v >= beta:
            return v
        alpha = max(alpha, v)
    return v

def minValue(state, alpha, beta):
    v = +inf
    for successor in state.getSuccessors():
        v = min(v, value(successor, alpha, beta))
        if v <= alpha:
            return v
        beta = min(alpha, v)
    return v
```

#### Evaluation Functions
Evaluation functions, functions that take in a state and output an estimate of the true minimax value of that node.

These functions serve a very similar purpose in games as *heuristics* do in *standard search problems*.

The most common design for an evaluation function is a linear combination of features.

$$Eval(s) = w_1f_1(s)+w_2f_2(s)+...+w_nf_n(s)$$


### Expectimax
Because minimax believes it is responding to an *optimal opponent*, it’s often overly pessimistic in situations where optimal responses to an agent’s actions are not guaranteed. 

Such situations include scenarios with inherent randomness such as card or dice games or unpredictable opponents that move randomly or suboptimally. We’ll talk about scenarios with *inherent randomness* much more in detail when we discuss *Markov decision processes* in the second half of the course.

This randomness can be represented through a generalization of minimax known as expectimax. Expectimax introduces chance nodes into the game tree, which instead of considering the worst case scenario as minimizer nodes do, considers the average case.

#### Mixed Layer Types

### Monte Carlo Tree Search (MCTS)
The **MCTS UCT algorithm** uses the **UCB criterion** in tree search problems.

Note that because UCT inherently explores more promising children a higher number of times, as $N\to∞$, UCT approaches the behavior of a minimax agent.

## General Games
Not all games are zero-sum. Indeed, different agents may have have distinct tasks in a game that don’t directly involve strictly competing with one another. Such games can be set up with trees characterized by *multi-agent utilities*.

Each agent then attempts to maximize their own utility at each node they control, ignoring the utilities of other agents.