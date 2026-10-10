---
type: problem
course: cse317
status: active
order: 41
---

# Problem — Admissible and Consistent Heuristic Verification

> 📖 **Reading Order:** Step 41 of 43 | **Module 8:** Exam Problems & Rigorous Proofs  
> ◄ **Previous:** [[Problem — Search Strategy Completeness and Complexity Analysis]] | ► **Next:** [[Problem — Alpha-Beta Pruning Trace and Node Evaluation]]

---

## Problem

Consider a state space search graph where all step costs are non-negative ($c(n, a, n') \ge 0$). Let $h^*(n)$ denote the true optimal cost from node $n$ to the goal.

1. Let $h_1(n)$ and $h_2(n)$ be two admissible heuristics. Define $h_{\max}(n) = \max\{h_1(n), h_2(n)\}$ and $h_{\text{avg}}(n) = \frac{h_1(n) + h_2(n)}{2}$.
   - Prove whether $h_{\max}(n)$ is admissible.
   - Prove whether $h_{\text{avg}}(n)$ is admissible.
   - Which heuristic is mathematically guaranteed to expand fewer or equal nodes in A* Tree Search? Justify using heuristic dominance.
2. Provide a concrete 3-node counterexample demonstrating that an admissible heuristic is NOT necessarily consistent.

---

## Given

- Step costs $c(n, a, n') \ge 0$.
- $h_1(n) \le h^*(n)$ and $h_2(n) \le h^*(n)$ for all $n$.
- $h_{\max}(n) = \max\{h_1(n), h_2(n)\}$.
- $h_{\text{avg}}(n) = \frac{h_1(n) + h_2(n)}{2}$.

---

## Required

1. Proofs of admissibility for $h_{\max}$ and $h_{\text{avg}}$, and dominance comparison.
2. Concrete graph showing an admissible heuristic that violates consistency.

---

## Concepts Tested

- [[Heuristic Functions and Properties]]
- [[Optimality of A-Star Search]]
- [[A-Star Search Algorithm]]

---

## Prerequisites

- [[Heuristic Functions and Properties]]

---

## Question Type

- Proof
- Counterexample
- Analysis

---

## Solution

### Understanding the Situation

We are testing mathematical understanding of the two foundational properties of heuristics in A* search: admissibility ($h(n) \le h^*(n)$) and consistency ($h(n) \le c(n, a, n') + h(n')$).

---

### Step-by-Step Proof

#### Part 1: Admissibility of Composite Heuristics

1. **Proof that $h_{\max}(n)$ is admissible:**
   - By assumption, $h_1(n) \le h^*(n)$ and $h_2(n) \le h^*(n)$ for all $n$.
   - Since both values are $\le h^*(n)$, their maximum cannot exceed $h^*(n)$:
     $$h_{\max}(n) = \max\{h_1(n), h_2(n)\} \le h^*(n)$$
   - Therefore, $h_{\max}(n)$ is **admissible**. $\blacksquare$

2. **Proof that $h_{\text{avg}}(n)$ is admissible:**
   - Summing the two admissibility inequalities:
     $$h_1(n) + h_2(n) \le 2 h^*(n)$$
   - Dividing by 2:
     $$h_{\text{avg}}(n) = \frac{h_1(n) + h_2(n)}{2} \le h^*(n)$$
   - Therefore, $h_{\text{avg}}(n)$ is **admissible**. $\blacksquare$

3. **Dominance Comparison:**
   - For any two real numbers $a, b$:
     $$\max\{a, b\} \ge \frac{a + b}{2}$$
   - Thus, for every node $n$:
     $$h_{\max}(n) \ge h_{\text{avg}}(n)$$
   - By definition of heuristic dominance, **$h_{\max}$ dominates $h_{\text{avg}}$**.
   - **Conclusion:** A* using $h_{\max}$ is mathematically guaranteed to expand a subset of (fewer or equal) nodes compared to A* using $h_{\text{avg}}$. Hence, one should **always use $\max\{h_1, h_2\}$, never the average**!

---

#### Part 2: Counterexample (Admissible but Inconsistent)

Consider a directed graph with 3 nodes: Start $A$, intermediate node $B$, and Goal node $C$:

```
                    c(A, B) = 1
              [ A ] ──────────► [ B ]
                │                 │
     c(A, C) = 4│                 │ c(B, C) = 1
                ▼                 ▼
              [ C ] ◄─────────────┘
             (Goal)
```

- **Step Costs:**
  - $c(A, B) = 1$
  - $c(B, C) = 1$
  - $c(A, C) = 4$
- **True Optimal Costs to Goal $C$:**
  - $h^*(B) = 1$ (via edge $B \to C$)
  - $h^*(A) = 1 + 1 = 2$ (via path $A \to B \to C$)
  - $h^*(C) = 0$

Now, define the heuristic $h$:
- $h(C) = 0$
- $h(B) = 0$
- $h(A) = 2$

**Verification of Admissibility:**
- $h(A) = 2 \le h^*(A) = 2$ (Valid!)
- $h(B) = 0 \le h^*(B) = 1$ (Valid!)
- $h(C) = 0 \le h^*(C) = 0$ (Valid!)
$\implies h$ is strictly **admissible** (never overestimates).

**Verification of Consistency on Arc $A \to B$:**
The consistency condition requires:
$$h(A) \le c(A, B) + h(B)$$
Substitute values:
$$2 \le 1 + 0 \implies 2 \le 1 \quad \textbf{(FALSE!)}$$
The consistency triangle inequality is violated by $1$ unit!

**Result:**
$h$ is admissible, but **INCONSISTENT**.

---

## Checking and Interpreting the Result

In this counterexample, $f(A) = g(A) + h(A) = 0 + 2 = 2$, but for successor $B$, $f(B) = g(B) + h(B) = 1 + 0 = 1$. Notice that $f$ dropped from $2$ to $1$! Inconsistent heuristics cause $f$-values to decrease along paths, destroying monotonic contour expansion.

---

## Common Mistakes

- Averaging heuristics instead of taking the maximum (averaging dilutes information and loses dominance).
- Confusing the consistency condition $h(A) \le c(A, B) + h(B)$ with $h(A) \ge h(B)$.

---

## Related Notes

- [[Heuristic Functions and Properties]]
- [[Optimality of A-Star Search]]
- [[A-Star Search Algorithm]]

---

## Navigation

◄ **Previous:** [[Problem — Search Strategy Completeness and Complexity Analysis]] | ► **Next:** [[Problem — Alpha-Beta Pruning Trace and Node Evaluation]]
