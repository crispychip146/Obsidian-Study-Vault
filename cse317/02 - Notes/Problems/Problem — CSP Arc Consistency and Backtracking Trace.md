---
type: problem
course: cse317
status: active
order: 43
---

# Problem — CSP Arc Consistency and Backtracking Trace

> 📖 **Reading Order:** Step 43 of 43 | **Module 8:** Exam Problems & Rigorous Proofs  
> ◄ **Previous:** [[Problem — Alpha-Beta Pruning Trace and Node Evaluation]] | ► **Next:** *End of Course*

---

## Problem

Consider a Constraint Satisfaction Problem with 3 variables $X, Y, Z$ with domains:
$$D_X = \{1, 2, 3\}, \quad D_Y = \{1, 2, 3\}, \quad D_Z = \{1, 2, 3\}$$
The binary constraints are:
1. $C_{XY}: X < Y$
2. $C_{YZ}: Y < Z$

Execute the following:
1. Run the **AC-3 Algorithm** to completion on this network. Show the initial queue, every arc popped, domain revisions made, and the final arc-consistent domains.
2. Does AC-3 alone solve the problem (reduce all domains to singletons)?
3. If Backtracking with Forward Checking is applied to the original unreduced domains, show the search tree trace when assigning variables in order $X$, then $Y$, then $Z$.

---

## Given

- Variables $X, Y, Z$.
- Initial domains: $D_X = D_Y = D_Z = \{1, 2, 3\}$.
- Constraints: $X < Y$ and $Y < Z$.

---

## Required

1. Complete step-by-step trace of AC-3.
2. Identification of final domains and solution status.
3. Forward Checking search tree trace.

---

## Concepts Tested

- [[Constraint Satisfaction Problems]]
- [[Constraint Propagation and Arc Consistency]]
- [[AC-3 Algorithm]]
- [[Backtracking Search for CSPs Algorithm]]
- [[CSP Search Heuristics and Inference]]

---

## Prerequisites

- [[AC-3 Algorithm]]

---

## Question Type

- Algorithm Trace
- CSP Solving
- Analysis

---

## Solution

### Understanding the Situation

We have a chain constraint network $X < Y < Z$ with domain $\{1, 2, 3\}$. The binary arcs are $(X, Y), (Y, X), (Y, Z), (Z, Y)$. We must enforce directed arc consistency using AC-3.

---

### Step-by-Step AC-3 Execution

#### Initial Queue:
$$\text{Queue} = [(X, Y), (Y, X), (Y, Z), (Z, Y)]$$

1. **Pop $(X, Y)$:**
   - Test each $x \in D_X = \{1, 2, 3\}$ for support in $D_Y = \{1, 2, 3\}$ such that $x < y$:
     - $x = 1$: $y = 2$ satisfies $1 < 2$. Supported!
     - $x = 2$: $y = 3$ satisfies $2 < 3$. Supported!
     - $x = 3$: No $y \in \{1, 2, 3\}$ satisfies $3 < y$. **UNSUPPORTED!**
   - Prune $3$ from $D_X$: Updated $D_X = \{1, 2\}$.
   - Since $D_X$ was revised, re-enqueue incoming arcs to $X$. (There are no other neighbors).
   - Queue: $[(Y, X), (Y, Z), (Z, Y)]$.

2. **Pop $(Y, X)$:**
   - Test each $y \in D_Y = \{1, 2, 3\}$ for support in $D_X = \{1, 2\}$ such that $x < y$:
     - $y = 1$: No $x \in \{1, 2\}$ satisfies $x < 1$. **UNSUPPORTED!**
     - $y = 2$: $x = 1$ satisfies $1 < 2$. Supported!
     - $y = 3$: $x = 1$ satisfies $1 < 3$. Supported!
   - Prune $1$ from $D_Y$: Updated $D_Y = \{2, 3\}$.
   - $D_Y$ revised: re-enqueue incoming arcs to $Y$: arc $(Z, Y)$ is already in queue; arc $(X, Y)$ must be re-added!
   - Queue: $[(Y, Z), (Z, Y), (X, Y)]$.

3. **Pop $(Y, Z)$:**
   - Test each $y \in D_Y = \{2, 3\}$ for support in $D_Z = \{1, 2, 3\}$ such that $y < z$:
     - $y = 2$: $z = 3$ satisfies $2 < 3$. Supported!
     - $y = 3$: No $z \in \{1, 2, 3\}$ satisfies $3 < z$. **UNSUPPORTED!**
   - Prune $3$ from $D_Y$: Updated $D_Y = \{2\}$.
   - $D_Y$ revised: re-enqueue incoming arc $(X, Y)$ (already in queue).
   - Queue: $[(Z, Y), (X, Y)]$.

4. **Pop $(Z, Y)$:**
   - Test each $z \in D_Z = \{1, 2, 3\}$ for support in $D_Y = \{2\}$ such that $y < z$:
     - $z = 1$: No $y \in \{2\}$ satisfies $y < 1$. **UNSUPPORTED!**
     - $z = 2$: No $y \in \{2\}$ satisfies $y < 2$. **UNSUPPORTED!**
     - $z = 3$: $y = 2$ satisfies $2 < 3$. Supported!
   - Prune $1$ and $2$ from $D_Z$: Updated $D_Z = \{3\}$.
   - Queue: $[(X, Y)]$.

5. **Pop $(X, Y)$:**
   - Test each $x \in D_X = \{1, 2\}$ for support in $D_Y = \{2\}$ such that $x < y$:
     - $x = 1$: $y = 2$ satisfies $1 < 2$. Supported!
     - $x = 2$: No $y \in \{2\}$ satisfies $2 < y$. **UNSUPPORTED!**
   - Prune $2$ from $D_X$: Updated $D_X = \{1\}$.
   - Queue is now empty! Terminate.

---

### Final Domains and Solution Status

The final domains after AC-3 are:
$$D_X = \{\mathbf{1}\}, \quad D_Y = \{\mathbf{2}\}, \quad D_Z = \{\mathbf{3}\}$$

**Conclusion:**
In this problem, **AC-3 ALONE COMPLETELY SOLVES THE CSP**!
Every domain was reduced to a singleton, yielding the unique valid solution:
$$X = 1, \quad Y = 2, \quad Z = 3$$
Zero search or backtracking was required!

---

## Common Mistakes

- Forgetting to re-enqueue $(X, Y)$ when $D_Y$ was modified in step 2.
- Testing $y < x$ instead of $x < y$ during $(Y, X)$ revision (the constraint relation is fixed: $X < Y$, regardless of arc direction).

---

## Related Notes

- [[Constraint Propagation and Arc Consistency]]
- [[AC-3 Algorithm]]
- [[Australia Map Coloring CSP Example]]

---

## Sources

- **Lectures:** [[cse317/01 - Sources/Lectures/MMi/CSP.pptx|CSP.pptx]]
- **Textbook:** [[cse317/01 - Sources/Lectures/MMi/Artificial Intelligence A Modern Approach 3rd Edition.pdf|Artificial Intelligence: A Modern Approach (3rd Edition)]], Chapter 6: Constraint Satisfaction Problems

---

## Navigation

◄ **Previous:** [[Problem — Alpha-Beta Pruning Trace and Node Evaluation]] | ► **Next:** *End of Course*
