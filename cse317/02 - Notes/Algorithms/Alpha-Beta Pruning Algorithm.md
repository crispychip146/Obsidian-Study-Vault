---
type: algorithm
course: cse317
status: active
order: 30
---

# Alpha-Beta Pruning Algorithm

> 📖 **Reading Order:** Step 30 of 43 | **Module 6:** Adversarial Search & Game Playing  
> ◄ **Previous:** [[Minimax Algorithm]] | ► **Next:** [[Evaluation Functions and Cutting Off Search]]

---

## Starting Point and the Problem

As shown in [[Minimax Algorithm]], pure Minimax requires $O(b^m)$ time, making complete search intractable for real games like Chess ($35^{80}$). However, large portions of the game tree are completely irrelevant to the final decision. If we have already discovered a move that guarantees us a score of $+5$, and exploring the first child of an alternative move reveals that the opponent can force our score down to $-10$, we can **immediately stop evaluating that alternative move**! Exploring the remaining children cannot change our choice. Pruning these dead branches is achieved by **Alpha-Beta Pruning**.

---

## Developing the Idea

Alpha-Beta pruning maintains two numeric parameters along the search path:
- $\alpha$: The value of the **best (highest-value) choice found so far along the path for MAX**. (Initialized to $-\infty$).
- $\beta$: The value of the **best (lowest-value) choice found so far along the path for MIN**. (Initialized to $+\infty$).

```
                      [ MAX Node ]  (Guaranteed at least α)
                           │
                           ▼
                      [ MIN Node ]  (Can be forced down to at most β)
```

### The Pruning Condition
As search progresses down the tree:
- MAX updates $\alpha$ whenever a child yields a value $> \alpha$.
- MIN updates $\beta$ whenever a child yields a value $< \beta$.

If at any point:
$$\alpha \ge \beta$$
**PRUNE!** The current node's remaining children are immediately discarded.
- *Why?* If $\alpha \ge \beta$, MAX already has an alternative path that guarantees at least $\alpha$, while MIN can force the current sub-branch down to $\beta \le \alpha$. A rational MAX will never choose to enter this sub-branch, so exploring its remaining children is a total waste of computation.

---

## Algorithm Specification

```python
def ALPHA_BETA_SEARCH(state, game):
    v, best_action = -float('inf'), None
    alpha = -float('inf')
    beta = float('inf')
    
    for action in game.ACTIONS(state):
        child_val = MIN_VALUE(game.RESULT(state, action), alpha, beta, game)
        if child_val > v:
            v = child_val
            best_action = action
        alpha = max(alpha, v)
        
    return best_action

def MAX_VALUE(state, alpha, beta, game):
    if game.TERMINAL_TEST(state):
        return game.UTILITY(state)
        
    v = -float('inf')
    for action in game.ACTIONS(state):
        v = max(v, MIN_VALUE(game.RESULT(state, action), alpha, beta, game))
        if v >= beta:
            return v  # PRUNE! Beta cutoff (MIN ancestor will avoid this branch)
        alpha = max(alpha, v)
    return v

def MIN_VALUE(state, alpha, beta, game):
    if game.TERMINAL_TEST(state):
        return game.UTILITY(state)
        
    v = float('inf')
    for action in game.ACTIONS(state):
        v = min(v, MAX_VALUE(game.RESULT(state, action), alpha, beta, game))
        if v <= alpha:
            return v  # PRUNE! Alpha cutoff (MAX ancestor will avoid this branch)
        beta = min(beta, v)
    return v
```

---

## Mathematical Equivalence to Minimax

**Theorem:** Alpha-Beta Pruning computes the **exact same root decision** as pure Minimax.

Pruning discards subtrees that are mathematically provable to have no influence on the root choice. It is a lossless optimization: zero accuracy is sacrificed.

---

## Complexity and Effectiveness

The efficiency of Alpha-Beta pruning depends entirely on **move ordering** (the order in which children are expanded):

1. **Worst-Case Move Ordering:**
   If the worst moves are evaluated first, no pruning occurs:
   $$\text{Time Complexity} = O(b^m)$$
2. **Best-Case (Optimal) Move Ordering:**
   If the best moves are evaluated first at every step:
   $$\text{Time Complexity} = O\left(b^{m/2}\right) = O\left((\sqrt{b})^m\right)$$

### The Power of Doubling Search Depth
With optimal move ordering, Alpha-Beta pruning reduces the effective branching factor from $b$ to $\sqrt{b}$.
- This means Alpha-Beta search can search **twice as deep** as Minimax in the exact same amount of time!
- For Chess ($b \approx 35$), effective branching drops to $\sqrt{35} \approx 6$.

---

## Common Mistakes

- Forgetting to pass the updated $\alpha$ and $\beta$ values down to recursive child calls.
- Pruning when $\alpha > \beta$ vs $\alpha \ge \beta$ (equality $\alpha = \beta$ is sufficient to prune because a rational player already has an equally good or better alternative).

---

## Exam Relevance

- Tracing Alpha-Beta pruning by hand on 3-ply and 4-ply game trees, showing exact $[\alpha, \beta]$ intervals and labeling pruned edges.
- Explaining the difference between an alpha cutoff and a beta cutoff.
- Deriving the best-case time complexity $O(b^{m/2})$.

---

## Related Concepts

- [[Minimax Algorithm]]
- [[Evaluation Functions and Cutting Off Search]]
- [[Minimax and Alpha-Beta Game Tree Pruning Example]]

---

## Prerequisites

- [[Minimax Algorithm]]

---

## Navigation

◄ **Previous:** [[Minimax Algorithm]] | ► **Next:** [[Evaluation Functions and Cutting Off Search]]
