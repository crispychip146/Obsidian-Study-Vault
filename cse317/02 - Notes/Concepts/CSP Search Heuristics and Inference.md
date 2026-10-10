---
type: concept
course: cse317
status: active
order: 37
---

# CSP Search Heuristics and Inference

> 📖 **Reading Order:** Step 37 of 43 | **Module 7:** Constraint Satisfaction Problems (CSPs)  
> ◄ **Previous:** [[Backtracking Search for CSPs Algorithm]] | ► **Next:** [[Min-Conflicts Algorithm for CSPs]]

---

## Starting Point and the Problem

Plain backtracking search (choosing variables and values arbitrarily) is still fundamentally a blind search. On realistic problems with dozens or hundreds of variables, naive backtracking experiences combinatorial explosion. To make backtracking search blazingly fast, we enhance it with three domain-independent heuristic decisions:
1. Which variable should be assigned next? (**Variable Ordering**)
2. In what order should its values be tried? (**Value Ordering**)
3. Can we detect future failures before making assignments? (**Inference**)

---

## 1. Variable Ordering Heuristics

### A. Minimum Remaining Values (MRV) Heuristic
- **Also Known As:** *"Most Constrained Variable"* or *"Fail-First"* heuristic.
- **Rule:** Choose the unassigned variable with the **fewest legal values remaining** in its domain.
- **Intuition:** If variable $X_i$ has only $1$ value left in its domain, assigning it now leaves no ambiguity. If it is doomed to fail, it fails immediately, pruning the tree at depth $1$ rather than wasting time searching deep subtrees!

### B. Degree Heuristic
- **Also Known As:** *"Most Constraining Variable"* heuristic.
- **Rule:** Choose the unassigned variable that is involved in the **largest number of constraints with other unassigned variables**.
- **Role:** Typically used as a **tie-breaker** when MRV produces multiple variables with the same minimum domain size.
- **Intuition:** By selecting the variable with the highest degree, we impose constraints on the maximum number of neighbors, aggressively shrinking their domains.

---

## 2. Value Ordering Heuristic

### Least Constraining Value (LCV) Heuristic
- **Rule:** Given a chosen variable $X_i$, order its domain values such that the value that **rules out the fewest choices for neighboring unassigned variables** is tried first.
- **Intuition:** *"Fail-First"* applies to variables (we want to detect failure quickly), but **"Succeed-First"** applies to values! Since we only need ONE valid solution, we want to try the value most likely to lead to success without boxing our neighbors into a corner.

```
       VARIABLE ORDERING: Fail First! (MRV + Degree)
       VALUE ORDERING:    Succeed First! (Least Constraining Value)
```

---

## 3. Inference Interleaved with Search

Rather than running inference only at the beginning, we interleave inference at **every step of backtracking**:

### A. Forward Checking (FC)
- Whenever variable $X$ is assigned value $v$, inspect every unassigned neighbor $Y$ connected to $X$.
- Remove from $D_Y$ any value that conflicts with $v$.
- If any neighbor's domain becomes empty, **backtrack immediately**!
- *Limitation:* Forward checking only looks one step ahead. It does not detect when two unassigned neighbors conflict with each other!

### B. Maintaining Arc Consistency (MAC)
- The gold standard of CSP solving.
- Whenever variable $X_i$ is assigned a value, run **AC-3**, initializing the queue with all arcs $(X_k, X_i)$ for all unassigned neighbors $X_k$.
- Propagates domain reductions transitively across the entire network.
- Detects failures drastically earlier than forward checking.

---

## Exam Relevance

- Applying MRV, Degree Heuristic, and LCV to select variables and values on a given CSP trace.
- Contrasting Forward Checking with Maintaining Arc Consistency (MAC).

---

## Related Concepts

- [[Backtracking Search for CSPs Algorithm]]
- [[AC-3 Algorithm]]
- [[Australia Map Coloring CSP Example]]

---

## Prerequisites

- [[Backtracking Search for CSPs Algorithm]]
- [[AC-3 Algorithm]]

---

## Sources

- **Lectures:** [[cse317/01 - Sources/Lectures/MMi/CSP.pptx|CSP.pptx]] (Slides 26–34)
- **Textbook:** [[cse317/01 - Sources/Lectures/MMi/Artificial Intelligence A Modern Approach 3rd Edition.pdf|Artificial Intelligence: A Modern Approach (3rd Edition)]], Chapter 6: Constraint Satisfaction Problems (Sections 6.3.1–6.3.2)

---

## Navigation

◄ **Previous:** [[Backtracking Search for CSPs Algorithm]] | ► **Next:** [[Min-Conflicts Algorithm for CSPs]]
