---
type: algorithm
course: cse317
status: active
order: 38
---

# Min-Conflicts Algorithm for CSPs

> 📖 **Reading Order:** Step 38 of 43 | **Module 7:** Constraint Satisfaction Problems (CSPs)  
> ◄ **Previous:** [[CSP Search Heuristics and Inference]] | ► **Next:** [[Australia Map Coloring CSP Example]]

---

## Starting Point and the Problem

Backtracking search explores partial assignments systematically. However, for massive CSPs (such as the million-queens problem or large-scale satellite scheduling), backtracking search is too slow. As introduced in [[Local Search and Optimization Landscape]], local search algorithms operate on **complete assignments**, repairing conflicts iteratively. For CSPs, this is achieved by the **Min-Conflicts Algorithm** (Minton et al., 1992).

---

## Developing the Idea

1. Start with a complete assignment of values to all variables (generated randomly or with a greedy heuristic).
2. Many constraints will likely be violated (conflicts).
3. At each step:
   - Randomly pick a variable $X$ that is currently in conflict (violating at least one constraint).
   - Change $X$'s value to the value in its domain that **minimizes the total number of violated constraints**!
4. Repeat until all constraints are satisfied ($0$ conflicts) or a max-step cutoff is reached.

```
       [ Random Complete Assignment ]
                     │
                     ▼
       [ Are there conflicts? ] ──NO──► [ RETURN SOLUTION ]
                     │ YES
                     ▼
       [ Pick random conflicted variable X ]
                     │
                     ▼
       [ Assign X = value with MINIMUM constraint violations ]
                     │
                     ▼
       [ Loop back ]
```

---

## Algorithm Specification

```python
import random

def MIN_CONFLICTS(csp, max_steps=100000):
    # Step 1: Generate initial complete assignment
    current = INITIAL_COMPLETE_ASSIGNMENT(csp)
    
    for step in range(max_steps):
        # If no constraints are violated, return solution!
        if csp.TOTAL_CONFLICTS(current) == 0:
            return current
            
        # Select randomly from among variables that violate constraints
        conflicted_vars = [var for var in csp.VARIABLES 
                           if csp.CONFLICTS(var, current[var], current) > 0]
        var = random.choice(conflicted_vars)
        
        # Choose value that minimizes violated constraints
        best_value = min(csp.DOMAINS[var], 
                         key=lambda val: csp.CONFLICTS(var, val, current))
        current[var] = best_value
        
    return FAILURE
```

---

## Performance and Surprising Effectiveness

The Min-Conflicts heuristic exhibits extraordinary performance on dense, uniform constraint networks:
- On the **$N$-Queens Problem**, Min-Conflicts solves instances with $N = 10,000,000$ queens in approximately **$O(1)$ steps on average** (independent of $N$!), provided the initial assignment is generated with a reasonable greedy heuristic.
- *Reason:* Solutions are distributed densely throughout the state space, and steepest descent on conflicts quickly navigates plateaus.

---

## Common Mistakes

- Forgetting to randomize tie-breaking when multiple values yield the same minimum conflict count (causes infinite cycling).
- Applying Min-Conflicts to problems with very few, sparse solutions (e.g., hard Sudoku), where local search gets hopelessly trapped in local minima.

---

## Exam Relevance

- Tracing Min-Conflicts on 4-queens or 8-queens step by step.
- Explaining how Min-Conflicts chooses the variable and value.

---

## Related Concepts

- [[Local Search and Optimization Landscape]]
- [[Hill-Climbing Search Algorithm]]
- [[Constraint Satisfaction Problems]]

---

## Prerequisites

- [[Constraint Satisfaction Problems]]
- [[Hill-Climbing Search Algorithm]]

---

## Navigation

◄ **Previous:** [[CSP Search Heuristics and Inference]] | ► **Next:** [[Australia Map Coloring CSP Example]]
