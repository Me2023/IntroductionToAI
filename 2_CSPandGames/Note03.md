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

terminal utilities and state value