# Unit V: Languages and Programming Techniques for AI

## METAHEURISTICS OVERVIEW

### Genetic Algorithm (GA)
- Evolution-inspired population-based search
- Selection, Crossover, Mutation operators
- Applications: TSP, scheduling, optimization

### Simulated Annealing (SA)
- Temperature-based acceptance criterion
- Accept worse solutions with probability e^(-ΔE/T)
- Good for escaping local optima

### Particle Swarm Optimization (PSO)
- Swarm intelligence from bird flocking
- Velocity = inertia + cognitive + social components
- Continuous optimization problems

### Ant Colony Optimization (ACO)
- Pheromone-guided path finding
- Reinforcement of good paths
- Excellent for routing problems

## PROLOG PROGRAMMING

### Facts and Rules
```prolog
parent(tom, bob).
grandparent(X,Z) :- parent(X,Y), parent(Y,Z).
```

### Unification and Backtracking
- Pattern matching with variable substitution
- Multiple solutions through backtracking
- Automatic search tree exploration

### Production Systems
- Forward chaining: Facts → Conclusions
- Backward chaining: Goals ← Facts
- Medical diagnosis, expert systems

## LISP PROGRAMMING

### S-Expressions
```lisp
(function arg1 arg2)
(+ (* 2 3) (- 5 1))
```

### List Processing
- car, cdr, cons for list manipulation
- Recursion as primary iteration
- Higher-order functions: mapcar, reduce

### Functional Programming
- Lambda functions for anonymous computations
- Function composition
- Symbolic processing capabilities

## SEARCH ALGORITHMS

- DFS and BFS implementation in LISP
- Recursive list processing
- Backtracking through recursion

---

**Complete comprehensive notes for all Unit V topics with examples, algorithms, and exam preparation materials included in repository.**
