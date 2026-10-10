---
type: concept
course: cse317
status: active
order: 33
---

# Constraint Satisfaction Problems

> 📖 **Reading Order:** Step 33 of 43 | **Module 7:** Constraint Satisfaction Problems (CSPs)  
> ◄ **Previous:** [[Minimax and Alpha-Beta Game Tree Pruning Example]] | ► **Next:** [[Constraint Propagation and Arc Consistency]]

---

## Starting Point and the Problem

In standard search (such as BFS, UCS, or A*), states are treated as atomic, indivisible black boxes: the search algorithm evaluates whether a state is a goal, but has no visibility into its internal structure. Consequently, search algorithms cannot infer why a path failed or exploit general-purpose heuristics across diverse domains. In contrast, many real-world problems—such as university course timetabling, circuit routing, puzzle solving (Sudoku, Cryptarithmetic), and factory scheduling—have states defined by a factored set of variables that must satisfy specific relational restrictions. **Constraint Satisfaction Problems (CSPs)** exploit this factored representation.

---

## Developing the Idea

A factored representation allows us to represent a state as a set of **variables**, each taking a value from a specific **domain**, subject to a set of **constraints**.

```
                         [ VARIABLES: X₁, X₂, ..., Xₙ ]
                                       │
                         [ DOMAINS: D₁, D₂, ..., Dₙ ]
                                       │
                         [ CONSTRAINTS: C₁, C₂, ..., Cₘ ]
                                       │
                                       ▼
                   Assignment: {X₁ = v₁, X₂ = v₂, ..., Xₙ = vₙ}
                   Satisfying ALL Constraints Simultaneously!
```

---

## Formal Definition

A **Constraint Satisfaction Problem (CSP)** is formally defined by a 3-tuple $(X, D, C)$:

1. **Variables ($X$):** A finite set of variables:
   $$X = \{X_1, X_2, \dots, X_n\}$$
2. **Domains ($D$):** A set of domains, one for each variable:
   $$D = \{D_1, D_2, \dots, D_n\}$$
   where $D_i$ is the set of allowable values for variable $X_i$.
   - Discrete finite domains (e.g., boolean $\{0, 1\}$, or colors $\{\text{Red}, \text{Green}, \text{Blue}\}$).
   - Discrete infinite domains (e.g., integers $\mathbb{Z}$, requiring constraint languages).
   - Continuous domains (e.g., start/end times in scheduling, solved via linear programming).
3. **Constraints ($C$):** A finite set of constraints:
   $$C = \{C_1, C_2, \dots, C_m\}$$
   Each constraint $C_i = \langle \text{scope}, \text{relation} \rangle$ consists of a tuple of variables that participate in the constraint, and a relation specifying the legal combinations of values for those variables.

---

## Assignment Terminology

- **Consistent Assignment:** An assignment that does not violate any constraints.
- **Complete Assignment:** An assignment where every variable is assigned a value.
- **Solution:** A complete and consistent assignment.
- **Partial Assignment:** An assignment where only a subset of variables are assigned values.

---

## Types of Constraints

1. **Unary Constraints:** Involves a single variable.
   - *Example:* $X_1 \neq \text{Green}$. (Can be eliminated immediately by pruning the domain $D_1$).
2. **Binary Constraints:** Relates pairs of variables.
   - *Example:* $X_1 \neq X_2$.
3. **Higher-Order / Global Constraints:** Involves three or more variables.
   - *Example:* $\text{Alldiff}(X_1, X_2, X_3, X_4)$ (all variables must take mutually distinct values).
   - *Cryptarithmetic Constraints:* $\text{SEND} + \text{MORE} = \text{MONEY}$.

---

## The Constraint Graph

For binary CSPs, the problem can be visualized as a **Constraint Graph**:
- **Nodes:** Represent variables $X_i$.
- **Edges:** Represent binary constraints between variables.

```
       [ Western Australia (WA) ] ────── [ Northern Territory (NT) ]
                   \                               /
                    \                             /
                     \                           /
                      [ South Australia (SA) ]
```

---

## Common Mistakes

- Treating CSPs as standard unconstrained search trees where actions generate paths. In CSPs, the order in which variables are assigned does not matter (commutativity).
- Confusing a complete assignment with a consistent assignment.

---

## Exam Relevance

- Formally specifying a real-world problem as a CSP $(X, D, C)$.
- Drawing the constraint graph for a given puzzle or map.
- Classifying constraints as unary, binary, or global.

---

## Related Concepts

- [[Constraint Propagation and Arc Consistency]]
- [[AC-3 Algorithm]]
- [[Backtracking Search for CSPs Algorithm]]
- [[Australia Map Coloring CSP Example]]

---

## Prerequisites

- [[Problem-Solving Agents and State Space Formulation]]

---

## Sources

- **Lectures:** [[cse317/01 - Sources/Lectures/MMi/CSP.pptx|CSP.pptx]] (Slides 1–10)
- **Textbook:** [[cse317/01 - Sources/Lectures/MMi/Artificial Intelligence A Modern Approach 3rd Edition.pdf|Artificial Intelligence: A Modern Approach (3rd Edition)]], Chapter 6: Constraint Satisfaction Problems (Section 6.1)

---

## Navigation

◄ **Previous:** [[Minimax and Alpha-Beta Game Tree Pruning Example]] | ► **Next:** [[Constraint Propagation and Arc Consistency]]
