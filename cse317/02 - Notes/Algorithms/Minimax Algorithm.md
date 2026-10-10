---
type: algorithm
course: cse317
status: active
order: 29
---

# Minimax Algorithm

> 📖 **Reading Order:** Step 29 of 43 | **Module 6:** Adversarial Search & Game Playing  
> ◄ **Previous:** [[Adversarial Search and Two-Player Games]] | ► **Next:** [[Alpha-Beta Pruning Algorithm]]

---

## Starting Point and the Problem

Given a game tree with terminal payoffs, how should MAX choose an action at the root? MAX cannot simply pick the branch with the highest possible terminal value, because MIN controls the intervening turns and will actively choose actions that block high payoffs. MAX must determine the **optimal strategy against an optimal opponent**. This optimal decision is computed by the **Minimax Algorithm** (John von Neumann, 1928).

---

## Developing the Idea

The minimax value of a node is the utility (for MAX) of being in that state, assuming that *both* players play optimally from that state until the end of the game:
- At a **Terminal Node**, the value is simply the terminal utility.
- At a **MAX Node**, MAX chooses the action that yields the **maximum** value among all legal successors.
- At a **MIN Node**, MIN chooses the action that yields the **minimum** value among all legal successors.

```
                     MAX: Chooses Max(v₁, v₂)
                               ▲
                      ┌────────┴────────┐
                      │                 │
              MIN: Min(3, 12, 8)     MIN: Min(2, 4, 6)
                      │                 │
                  v₁ = 3            v₂ = 2
                      ▲                 ▲
            ┌─────┬───┴───┐         ┌───┴───┬─────┐
            3    12       8         2       4     6  (Terminal Utilities)
```

In this tree:
- Left MIN node gets $\min(3, 12, 8) = 3$.
- Right MIN node gets $\min(2, 4, 6) = 2$.
- Root MAX node chooses $\max(3, 2) = \mathbf{3}$ (Left Move).

---

## Algorithm Specification

```python
def MINIMAX_DECISION(state, game):
    # Determine the action that achieves the maximum Minimax value
    best_action = None
    best_value = -float('inf')
    
    for action in game.ACTIONS(state):
        value = MIN_VALUE(game.RESULT(state, action), game)
        if value > best_value:
            best_value = value
            best_action = action
            
    return best_action

def MAX_VALUE(state, game):
    if game.TERMINAL_TEST(state):
        return game.UTILITY(state)
        
    v = -float('inf')
    for action in game.ACTIONS(state):
        v = max(v, MIN_VALUE(game.RESULT(state, action), game))
    return v

def MIN_VALUE(state, game):
    if game.TERMINAL_TEST(state):
        return game.UTILITY(state)
        
    v = float('inf')
    for action in game.ACTIONS(state):
        v = min(v, MAX_VALUE(game.RESULT(state, action), game))
    return v
```

---

## Complexity Analysis

Let $b$ be the legal branching factor and $m$ be the maximum depth of the game tree:
- **Time Complexity:** $O(b^m)$.
  The algorithm must explore every single node in the entire game tree down to terminal leaves.
- **Space Complexity:** $O(bm)$ (linear memory!).
  Because Minimax performs a depth-first search of the game tree, it only needs to store the current branch and unexpanded siblings in memory.

### The Combinatorial Barrier
For Chess ($b \approx 35, m \approx 80$), $b^m \approx 35^{80} \approx 10^{123}$.
Evaluating $10^{123}$ nodes is physically impossible. This combinatorial barrier demands two critical optimizations:
1. Pruning irrelevant branches without losing optimality: [[Alpha-Beta Pruning Algorithm]].
2. Cutting off search early and using heuristic evaluations: [[Evaluation Functions and Cutting Off Search]].

---

## Optimality Property

**Theorem:** The Minimax algorithm yields the optimal strategy against an optimal opponent.

*What if the opponent plays suboptimally?*
If the opponent makes a mistake, Minimax will achieve a payoff *greater than or equal to* the minimax value! It will never perform worse than its theoretical guarantee.

---

## Common Mistakes

- Alternating min/max incorrectly (e.g., executing max at a MIN layer).
- Forgetting that minimax values propagate **from the bottom (terminal leaves) upward to the root**.

---

## Exam Relevance

- Calculating minimax values for given game trees by hand.
- Formulating the recursive mathematical equations for $\text{Minimax}(s)$.
- Stating the time and space complexity bounds.

---

## Related Concepts

- [[Adversarial Search and Two-Player Games]]
- [[Alpha-Beta Pruning Algorithm]]
- [[Evaluation Functions and Cutting Off Search]]

---

## Prerequisites

- [[Adversarial Search and Two-Player Games]]
- [[Depth-First Search and Depth-Limited Search Algorithm]]

---

## Sources

- **Lectures:** [[cse317/01 - Sources/Lectures/MMi/Chap5-AdvSearch.ppt|Chap5-AdvSearch.ppt]] (Slides 13–24)
- **Textbook:** [[cse317/01 - Sources/Lectures/MMi/Artificial Intelligence A Modern Approach 3rd Edition.pdf|Artificial Intelligence: A Modern Approach (3rd Edition)]], Chapter 5: Adversarial Search (Section 5.2)

---

## Navigation

◄ **Previous:** [[Adversarial Search and Two-Player Games]] | ► **Next:** [[Alpha-Beta Pruning Algorithm]]
