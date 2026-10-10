---
type: problem
course: cse317
status: active
order: 42
---

# Problem — Alpha-Beta Pruning Trace and Node Evaluation

> 📖 **Reading Order:** Step 42 of 43 | **Module 8:** Exam Problems & Rigorous Proofs  
> ◄ **Previous:** [[Problem — Admissible and Consistent Heuristic Verification]] | ► **Next:** [[Problem — CSP Arc Consistency and Backtracking Trace]]

---

## Problem

A 2-player zero-sum game tree is shown below. MAX moves at the root (Node A), followed by MIN (Nodes B and C), followed by MAX (Nodes D, E, F, G). The terminal leaf utility values are given from left to right.

```
                            [ Node A: MAX ]
                           /               \
              [ Node B: MIN ]             [ Node C: MIN ]
             /               \           /               \
          [ D ]             [ E ]     [ F ]             [ G ]   (MAX)
          /   \             /   \     /   \             /   \
         4     2           7     1   3     8           2     9  (Leaves)
```

1. Compute the Minimax value for every internal node and determine the optimal root decision for MAX.
2. Execute **Alpha-Beta Pruning** from left to right.
   - List the $[\alpha, \beta]$ intervals passed to each node.
   - Identify all leaf nodes that are **pruned** and specify whether each cutoff is an **Alpha cutoff** or a **Beta cutoff**.
3. Re-order the children at each node to achieve **optimal move ordering**, and show that no branches are pruned if worst-case ordering were used instead.

---

## Given

- 3-ply Game tree with leaves:
  - Under D: $(4, 2)$
  - Under E: $(7, 1)$
  - Under F: $(3, 8)$
  - Under G: $(2, 9)$

---

## Required

1. Minimax values for D, E, F, G, B, C, A.
2. Step-by-step Alpha-Beta trace with interval tracking and pruned leaf identification.
3. Move ordering analysis.

---

## Concepts Tested

- [[Minimax Algorithm]]
- [[Alpha-Beta Pruning Algorithm]]
- [[Adversarial Search and Two-Player Games]]

---

## Prerequisites

- [[Alpha-Beta Pruning Algorithm]]

---

## Question Type

- Game Tree Trace
- Numerical
- Analysis

---

## Solution

### Understanding the Situation

We evaluate a two-player game tree with root MAX. Minimax evaluates all nodes. Alpha-Beta maintains $[\alpha, \beta]$ where $\alpha$ is MAX's guaranteed minimum and $\beta$ is MIN's guaranteed maximum. When $\alpha \ge \beta$, search cuts off.

---

### Step-by-Step Solution

#### Part 1: Standard Minimax Evaluation

- **MAX Layer (Nodes D, E, F, G):**
  - $\text{Val}(D) = \max(4, 2) = \mathbf{4}$
  - $\text{Val}(E) = \max(7, 1) = \mathbf{7}$
  - $\text{Val}(F) = \max(3, 8) = \mathbf{8}$
  - $\text{Val}(G) = \max(2, 9) = \mathbf{9}$
- **MIN Layer (Nodes B, C):**
  - $\text{Val}(B) = \min(\text{Val}(D), \text{Val}(E)) = \min(4, 7) = \mathbf{4}$
  - $\text{Val}(C) = \min(\text{Val}(F), \text{Val}(G)) = \min(8, 9) = \mathbf{8}$
- **Root MAX Layer (Node A):**
  - $\text{Val}(A) = \max(\text{Val}(B), \text{Val}(C)) = \max(4, 8) = \mathbf{8}$
- **Optimal Action:** Root MAX chooses **Node C**!

---

#### Part 2: Alpha-Beta Pruning Trace (Left-to-Right)

1. **Root Node A (MAX):** Initialized with $\alpha = -\infty, \beta = +\infty$.
   - Calls Node B with $[\alpha = -\infty, \beta = +\infty]$.
2. **Node B (MIN):**
   - Calls Node D with $[\alpha = -\infty, \beta = +\infty]$.
3. **Node D (MAX):**
   - Evaluates leaf $4 \implies v = 4 \implies \alpha = \max(-\infty, 4) = 4$.
   - Evaluates leaf $2 \implies v = \max(4, 2) = 4$.
   - Returns $4$ to B.
4. **Node B (MIN):**
   - Updates $\beta = \min(+\infty, 4) = \mathbf{4}$.
   - Current interval at B: $[\alpha = -\infty, \beta = 4]$.
   - Calls Node E with $[\alpha = -\infty, \beta = 4]$.
5. **Node E (MAX):**
   - Evaluates first leaf $7 \implies v = 7$.
   - Checks cutoff condition: $v \ge \beta \implies 7 \ge 4$!
   - **BETA CUTOFF OCCURS AT NODE E!**
   - **Prune leaf $1$!**
   - Node E immediately returns at least $7$.
6. **Node B (MIN):**
   - $\beta = \min(4, 7) = 4$. Returns $4$ to Root A.
7. **Root Node A (MAX):**
   - Updates $\alpha = \max(-\infty, 4) = \mathbf{4}$.
   - Current interval at A: $[\alpha = 4, \beta = +\infty]$.
   - Calls Node C with $[\alpha = 4, \beta = +\infty]$.
8. **Node C (MIN):**
   - Calls Node F with $[\alpha = 4, \beta = +\infty]$.
9. **Node F (MAX):**
   - Evaluates leaf $3 \implies v = 3 \implies \alpha = \max(4, 3) = 4$.
   - Evaluates leaf $8 \implies v = \max(3, 8) = 8$.
   - Returns $8$ to C.
10. **Node C (MIN):**
    - Updates $\beta = \min(+\infty, 8) = \mathbf{8}$.
    - Current interval at C: $[\alpha = 4, \beta = 8]$.
    - Calls Node G with $[\alpha = 4, \beta = 8]$.
11. **Node G (MAX):**
    - Evaluates first leaf $2 \implies v = 2 \implies \alpha$ remains $4$.
    - Evaluates second leaf $9 \implies v = \max(2, 9) = 9$.
    - Check cutoff: $9 \ge \beta \implies 9 \ge 8$ (at end of node).
    - Returns $9$ to C.
12. **Node C (MIN):**
    - $\beta = \min(8, 9) = 8$. Returns $8$ to Root A.
13. **Root Node A:**
    - $\alpha = \max(4, 8) = 8$. Action = Move to C.

**Pruning Summary:**
- **Pruned Node:** Leaf $1$ under Node E.
- **Cutoff Type:** Beta cutoff (because $v = 7 \ge \beta = 4$).

---

## Checking and Interpreting the Result

The final root value with Alpha-Beta pruning is $\mathbf{8}$ (move to C), which is strictly identical to the Minimax result, verifying the lossless property of Alpha-Beta pruning.

---

## Common Mistakes

- Forgetting to propagate $\alpha = 4$ from Root A to Node C.
- Confusing Alpha cutoff (occurs at MIN node when $v \le \alpha$) with Beta cutoff (occurs at MAX node when $v \ge \beta$).

---

## Related Notes

- [[Minimax Algorithm]]
- [[Alpha-Beta Pruning Algorithm]]
- [[Minimax and Alpha-Beta Game Tree Pruning Example]]

---

## Sources

- **Lectures:** [[cse317/01 - Sources/Lectures/MMi/Chap5-AdvSearch.ppt|Chap5-AdvSearch.ppt]]
- **Textbook:** [[cse317/01 - Sources/Lectures/MMi/Artificial Intelligence A Modern Approach 3rd Edition.pdf|Artificial Intelligence: A Modern Approach (3rd Edition)]], Chapter 5: Adversarial Search

---

## Navigation

◄ **Previous:** [[Problem — Admissible and Consistent Heuristic Verification]] | ► **Next:** [[Problem — CSP Arc Consistency and Backtracking Trace]]
