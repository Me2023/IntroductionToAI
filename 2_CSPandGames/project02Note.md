# Project 2: Multi-Agent Search
https://inst.eecs.berkeley.edu/~cs188/su22/project2/
```
python3 pacman.py
python3 pacman.py -p ReflexAgent
python3 pacman.py -p ReflexAgent -l testClassic
```
- play a game of classic Pacman by using the arrow keys to move
- by ReflexAgent
- by ReflexAgent on simple layouts

## Question 1: Reflex Agent
`ReflexAgent` in multiAgents.py
### How to test our agent
```
python3 pacman.py -p ReflexAgent -l testClassic
python3 pacman.py --frameTime 0 -p ReflexAgent -k 1
python3 pacman.py --frameTime 0 -p ReflexAgent -k 2
```