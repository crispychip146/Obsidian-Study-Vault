---
type: algorithm
course: cse317
status: active
order: 36
---

# Backtracking Search for CSPs Algorithm

> 📖 **Reading Order:** Step 36 of 43 | **Module 7:** Constraint Satisfaction Problems (CSPs)  
> ◄ **Previous:** [[AC-3 Algorithm]] | ► **Next:** [[CSP Search Heuristics and Inference]]

---

## Starting Point and the Problem

While constraint propagation (like AC-3) prunes large numbers of inconsistent values, it does not always reduce all domains to singletons. In many CSPs, after AC-3 runs, domains still contain multiple values. To find a concrete solution, the agent must perform search. If we used standard BFS, the branching factor at the top level would be $n \cdot d$, at the next level $(n-1) \cdot d$, generating $n! \cdot d^n$ leaves. However, in CSPs, **variable assignment is commutative**: assigning $X_1 = \text{Red}$ then $X_2 = \text{Blue}$ reaches the exact same state as assigning $X_2 = \text{Blue}$ then $X_1 = \text{Red}$. We only need to consider assigning **one single variable at each depth of the tree**! This depth-first search strategy is called **Backtracking Search**.

---

## Developing the Idea

Backtracking search fixes the order of variable consideration:
- At depth $0$, pick variable $X_1$ and assign a value.
- At depth $1$, pick variable $X_2$ and assign a value.
- If an assignment violates any constraint, **immediately backtrack** (unassign the value and try the next value in the domain).

```
                      [ Root: No assignments ]
                                 │
                   Assign X₁ = Red (Depth 1)
                                 │
                   Assign X₂ = Red (Conflict! Backtrack!)
                                 │
                   Assign X₂ = Blue (Depth 2: Valid!)
                                 │
                   Assign X₃ = Green ...
```

---

## Algorithm Specification

```python
def BACKTRACKING_SEARCH(csp):
    return BACKTRACK({}, csp)

def BACKTRACK(assignment, csp):
    # Base Case: If all variables assigned, solution found!
    if len(assignment) == len(csp.VARIABLES):
        return assignment
        
    # Select an unassigned variable (using MRV / Degree Heuristic)
    var = SELECT_UNASSIGNED_VARIABLE(assignment, csp)
    
    # Iterate through domain values (using Least Constraining Value)
    for value in ORDER_DOMAIN_VALUES(var, assignment, csp):
        if csp.IS_CONSISTENT(var, value, assignment):
            assignment[var] = value
            
            # Optional: Inference (Forward Checking or MAC)
            inferences = INFERENCE(csp, var, value)
            if inferences is not FAILURE:
                result = BACKTRACK(assignment, csp)
                if result is not FAILURE:
                    return result
                    
            # Backtrack: Remove assignment and restore domains
            del assignment[var]
            csp.RESTORE_DOMAINS(inferences)
            
    return FAILURE
```

---

## Why Backtracking is Powerful for CSPs

1. **Pruning Exponential Subtrees Early:** By checking constraints immediately after each single assignment, entire exponential subtrees of invalid combinations are pruned at the shallowest possible depth.
2. **Minimal Memory:** Operates in $O(n)$ linear space (storing only the current assignment path of length $\le n$).

---

## Exam Relevance

- Explaining why commutativity reduces the state space from $n! d^n$ to $d^n$.
- Writing the recursive backtracking algorithm.

---

## Related Concepts

- [[CSP Search Heuristics and Inference]]
- [[AC-3 Algorithm]]
- [[Australia Map Coloring CSP Example]]

---

## Prerequisites

- [[Constraint Satisfaction Problems]]
- [[Depth-First Search and Depth-Limited Search Algorithm]]

---

## Sources

- **Lectures:** [[cse317/01 - Sources/Lectures/MMi/CSP.pptx|CSP.pptx]] (Slides 21–25)
- **Textbook:** [[cse317/01 - Sources/Lectures/MMi/Artificial Intelligence A Modern Approach 3rd Edition.pdf|Artificial Intelligence: A Modern Approach (3rd Edition)]], Chapter 6: Constraint Satisfaction Problems (Section 6.3)

---

## Navigation

◄ **Previous:** [[AC-3 Algorithm]] | ► **Next:** [[CSP Search Heuristics and Inference]]
