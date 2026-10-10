---
type: concept
course: cse317
status: active
order: 34
---

# Constraint Propagation and Arc Consistency

> 📖 **Reading Order:** Step 34 of 43 | **Module 7:** Constraint Satisfaction Problems (CSPs)  
> ◄ **Previous:** [[Constraint Satisfaction Problems]] | ► **Next:** [[AC-3 Algorithm]]

---

## Starting Point and the Problem

In standard search, an agent can only discover that a branch fails by searching down the tree until a dead end is reached. In a CSP, however, variables are interconnected by explicit mathematical constraints. If assigning a value to variable $X_i$ leaves variable $X_j$ with no legal values, we should deduce that failure immediately without searching! Using constraints to systematically prune unviable values from variable domains is called **Constraint Propagation**.

---

## Developing the Idea: Consistency Types

Constraint propagation enforces local consistency at multiple granularities:

```
    Node Consistency ──► Arc Consistency ──► Path Consistency ──► k-Consistency
     (1 Variable)         (2 Variables)        (3 Variables)        (k Variables)
```

---

## 1. Node Consistency (NC)

A single variable $X_i$ is **node-consistent** if every value in its domain $D_i$ satisfies all unary constraints on $X_i$.
- *Example:* If $D_1 = \{1, 2, 3, 4, 5\}$ and constraint is $X_1 \le 3$, enforcing node consistency reduces $D_1$ to $\{1, 2, 3\}$.

---

## 2. Arc Consistency (AC)

Arc consistency evaluates **directed binary relationships** between pairs of variables.

### Definition
A variable $X_i$ is **arc-consistent with respect to another variable $X_j$** (along the directed arc $X_i \to X_j$) if:
$$\forall x \in D_i, \quad \exists y \in D_j \text{ such that } (x, y) \text{ satisfies the binary constraint between } X_i \text{ and } X_j$$

In plain English: *Every value in $X_i$'s domain must have at least one compatible partner in $X_j$'s domain.*
If a value $x \in D_i$ has NO valid partner in $D_j$, then $x$ can NEVER be part of any valid solution, and **$x$ must be deleted from $D_i$**!

```
     Domain of Xi: {Red, Blue}                Domain of Xj: {Blue}
     Binary Constraint: Xi ≠ Xj

     Check Xi → Xj:
     - If Xi = Red:  Can Xj choose a valid color? Yes, Xj = Blue (Red ≠ Blue). Valid!
     - If Xi = Blue: Can Xj choose a valid color? NO! Xj has only Blue, violating Xi ≠ Xj!
     ==> PRUNE 'Blue' from Domain of Xi! Updated D_i = {Red}.
```

### Critical Property: Arc Consistency is Directed!
Notice that in the example above:
- $X_i \to X_j$ was NOT arc consistent (Blue had to be pruned from $D_i$).
- However, $X_j \to X_i$ WAS arc consistent (for $X_j = \text{Blue}$, there existed $X_i = \text{Red}$).
Enforcing arc consistency across an entire network requires checking **both directed directions** ($X_i \to X_j$ and $X_j \to X_i$) for every constraint edge.

---

## 3. Path Consistency & $k$-Consistency

- **Path Consistency (PC):** Evaluates triples of variables $\{X_i, X_j, X_k\}$. A pair $\{X_i, X_j\}$ is path-consistent with respect to $X_k$ if for every consistent assignment $(x_i, x_j)$, there exists an assignment $x_k \in D_k$ that satisfies both constraints $(X_i, X_k)$ and $(X_k, X_j)$.
- **$k$-Consistency:** For any consistent assignment to $k-1$ variables, there always exists a consistent value for any $k$-th variable.
  - $1$-consistency is Node Consistency.
  - $2$-consistency is Arc Consistency.
  - $3$-consistency is Path Consistency.
- **Strong $k$-Consistency:** A graph is strongly $k$-consistent if it is $1$-consistent, $2$-consistent, $\dots$, and $k$-consistent simultaneously. If a CSP with $n$ variables is strongly $n$-consistent, it can be solved in **backtrack-free $O(n)$ time**!

---

## Exam Relevance

- Formally defining arc consistency mathematically.
- Demonstrating the directed nature of arc consistency on a small variable network.
- Explaining how enforcing arc consistency prunes domains.

---

## Related Concepts

- [[Constraint Satisfaction Problems]]
- [[AC-3 Algorithm]]
- [[CSP Search Heuristics and Inference]]

---

## Prerequisites

- [[Constraint Satisfaction Problems]]

---

## Sources

- **Lectures:** [[cse317/01 - Sources/Lectures/MMi/CSP.pptx|CSP.pptx]] (Slides 11–18)
- **Textbook:** [[cse317/01 - Sources/Lectures/MMi/Artificial Intelligence A Modern Approach 3rd Edition.pdf|Artificial Intelligence: A Modern Approach (3rd Edition)]], Chapter 6: Constraint Satisfaction Problems (Section 6.2)

---

## Navigation

◄ **Previous:** [[Constraint Satisfaction Problems]] | ► **Next:** [[AC-3 Algorithm]]
