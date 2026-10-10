---
type: example
course: cse317
status: active
order: 32
---

# Minimax and Alpha-Beta Game Tree Pruning Example

> 📖 **Reading Order:** Step 32 of 43 | **Module 6:** Adversarial Search & Game Playing  
> ◄ **Previous:** [[Evaluation Functions and Cutting Off Search]] | ► **Next:** [[Constraint Satisfaction Problems]]

---

## Starting Point and the Problem

To solidify understanding of Minimax and Alpha-Beta pruning, we execute a complete, step-by-step trace on a concrete 3-ply game tree with numeric terminal values, explicitly showing all $[\alpha, \beta]$ updates and pruned branches.

---

## Problem Game Tree

```
                            [ Node A: MAX ]
                           /               \
              [ Node B: MIN ]             [ Node C: MIN ]
             /       |       \           /       |       \
          [ D ]    [ E ]    [ F ]     [ G ]    [ H ]    [ I ]   (MAX)
          /   \    /   \    /   \     /   \    /   \    /   \
         3     5  6     9  1     2   0     4  7     8  5     3  (Leaves)
```

---

## Walkthrough: Alpha-Beta Pruning Trace

We initialize at the root: $\alpha = -\infty, \beta = +\infty$.

### Subtree under Node B:
1. **Explore Node D (MAX):**
   - Evaluates child $3 \implies v = 3 \implies \alpha = \max(-\infty, 3) = 3$.
   - Evaluates child $5 \implies v = \max(3, 5) = 5$.
   - Node D returns **$5$**.
   - At Node B (MIN): $\beta = \min(+\infty, 5) = \mathbf{5}$. Interval at B: $[\alpha=-\infty, \beta=5]$.
2. **Explore Node E (MAX):**
   - Passes $[\alpha=-\infty, \beta=5]$.
   - Evaluates first child $6 \implies v = 6$.
   - Check cutoff condition at E: $v \ge \beta \implies 6 \ge 5$!
   - **BETA CUTOFF OCCURS!**
   - **Prune second child ($9$)!**
   - Node E returns at least $6$.
   - At Node B (MIN): $\beta = \min(5, 6) = 5$.
3. **Explore Node F (MAX):**
   - Passes $[\alpha=-\infty, \beta=5]$.
   - Evaluates child $1 \implies v = 1$.
   - Evaluates child $2 \implies v = \max(1, 2) = 2$.
   - Node F returns **$2$**.
   - At Node B (MIN): $\beta = \min(5, 2) = \mathbf{2}$.
4. **Node B returns to Root A (MAX):**
   - Value of B = $2$.
   - At Root A: $\alpha = \max(-\infty, 2) = \mathbf{2}$.
   - Root A interval: $[\alpha = 2, \beta = +\infty]$.

---

### Subtree under Node C:
Root A passes $[\alpha = 2, \beta = +\infty]$ to Node C (MIN).

5. **Explore Node G (MAX):**
   - Passes $[\alpha = 2, \beta = +\infty]$.
   - Evaluates child $0 \implies v = 0$.
   - Evaluates child $4 \implies v = \max(0, 4) = 4$.
   - Node G returns **$4$**.
   - At Node C (MIN): $\beta = \min(+\infty, 4) = \mathbf{4}$. Interval at C: $[\alpha = 2, \beta = 4]$.
6. **Explore Node H (MAX):**
   - Passes $[\alpha = 2, \beta = 4]$.
   - Evaluates first child $7 \implies v = 7$.
   - Check cutoff condition at H: $v \ge \beta \implies 7 \ge 4$!
   - **BETA CUTOFF OCCURS!**
   - **Prune second child ($8$)!**
   - Node H returns at least $7$.
   - At Node C (MIN): $\beta = \min(4, 7) = 4$.
7. **Explore Node I (MAX):**
   - Passes $[\alpha = 2, \beta = 4]$.
   - Evaluates first child $5 \implies v = 5$.
   - Check cutoff condition at I: $v \ge \beta \implies 5 \ge 4$!
   - **BETA CUTOFF OCCURS!**
   - **Prune second child ($3$)!**
   - Node I returns at least $5$.
   - Node C returns **$4$**.

---

### Root Decision:
At Root A:
$$\text{Value}(A) = \max(\text{Value}(B), \text{Value}(C)) = \max(2, 4) = \mathbf{4}$$
- Optimal Action for MAX: **Move to Node C**!
- Pruned Nodes: Leaves $9$, $8$, and $3$ were completely skipped without evaluation!

---

## Exam Relevance

- Drawing game trees and indicating pruned edges with dashed lines or scissor marks.
- Documenting exact $\alpha, \beta$ bounds at each node during execution.

---

## Related Concepts

- [[Minimax Algorithm]]
- [[Alpha-Beta Pruning Algorithm]]

---

## Navigation

◄ **Previous:** [[Evaluation Functions and Cutting Off Search]] | ► **Next:** [[Constraint Satisfaction Problems]]
