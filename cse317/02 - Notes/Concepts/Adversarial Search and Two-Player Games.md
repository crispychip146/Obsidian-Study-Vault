---
type: concept
course: cse317
status: active
order: 28
---

# Adversarial Search and Two-Player Games

> 📖 **Reading Order:** Step 28 of 43 | **Module 6:** Adversarial Search & Game Playing  
> ◄ **Previous:** [[8-Queens Problem Local Search and Genetic Algorithm Example]] | ► **Next:** [[Minimax Algorithm]]

---

## Starting Point and the Problem

In standard search problems (such as navigation or 8-puzzle), the agent is the sole actor in a passive world. The agent computes a path to a goal and executes it without interference. However, in competitive multi-agent environments—such as Chess, Checkers, Go, or competitive economic bidding—another autonomous entity with conflicting goals actively works to defeat the agent. The future state does not depend solely on our choices, but also on the counter-moves of an opponent. This is the domain of **Adversarial Search** (or *Game Theory*).

---

## Developing the Idea

In classical AI, we study **deterministic, turn-taking, two-player, zero-sum games of perfect information**:
1. **Deterministic:** No dice or chance cards; transitions are exact.
2. **Turn-Taking:** Players alternate moves strictly (MAX, then MIN, then MAX...).
3. **Two-Player:** Exactly two entities compete: **MAX** (our agent, who wants to maximize the final payoff) and **MIN** (the opponent, who wants to minimize the final payoff).
4. **Zero-Sum:** Utility is purely adversarial. Any gain for MAX is an identical loss for MIN:
   $$U_{\text{MAX}} + U_{\text{MIN}} = 0 \quad (\text{or } U_{\text{MAX}} = -U_{\text{MIN}})$$
   There is no room for cooperation or mutual benefit.
5. **Perfect Information:** Fully observable; both players see the complete board state at all times (unlike Poker).

---

## Formal Game Formulation

A game is formally defined as a 6-tuple:

1. **Initial State ($S_0$):** Specifies the setup of the board and which player moves first.
2. **Player Function ($\text{Player}(s)$):** Defines which player has the move in state $s$ (MAX or MIN).
3. **Actions ($\text{Actions}(s)$):** Returns the set of legal moves in state $s$.
4. **Transition Model ($\text{Result}(s, a)$):** Returns the state that results from taking action $a$ in state $s$.
5. **Terminal Test ($\text{Terminal-Test}(s)$):** Returns True if the game has ended (e.g., checkmate, win, loss, or draw), and False otherwise. States where the game has ended are called **Terminal States**.
6. **Utility Function ($\text{Utility}(s, p)$):** (Also called the *Objective* or *Payoff Function*). Defines the final numeric value for player $p$ in terminal state $s$.
   - In Chess: $+1$ (win), $0$ (draw), $-1$ (loss).
   - In Backgammon or Othello: the difference in final piece counts.

---

## The Game Tree

The state space forms a **Game Tree**:
- Root node is $S_0$.
- Levels alternate between **MAX layers** (where our agent chooses moves) and **MIN layers** (where the opponent chooses moves).
- Leaves are terminal states annotated with their utility values.

```
       MAX Level (Our Turn)               [ Root State: MAX ]
                                         /                   \
       MIN Level (Opponent)      [ State A: MIN ]     [ State B: MIN ]
                                   /        \             /        \
       MAX Level                [ s1 ]    [ s2 ]       [ s3 ]    [ s4 ]
```

---

## Common Mistakes

- Assuming the opponent will make blunders or play suboptimally. A rational adversarial strategy must assume the opponent is **competent and plays optimally to minimize our score**.
- Treating non-zero-sum cooperative games as zero-sum (can cause mutually destructive behaviors).

---

## Exam Relevance

- Formally specifying the 6-tuple game definition.
- Explaining the zero-sum property mathematically.
- Calculating the size of game trees (e.g., Chess branching factor $b \approx 35$, depth $m \approx 80 \implies 35^{80}$ nodes).

---

## Related Concepts

- [[Minimax Algorithm]]
- [[Alpha-Beta Pruning Algorithm]]
- [[Evaluation Functions and Cutting Off Search]]

---

## Prerequisites

- [[Problem-Solving Agents and State Space Formulation]]

---

## Navigation

◄ **Previous:** [[8-Queens Problem Local Search and Genetic Algorithm Example]] | ► **Next:** [[Minimax Algorithm]]
