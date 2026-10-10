---
type: concept
course: cse317
status: active
order: 31
---

# Evaluation Functions and Cutting Off Search

> 📖 **Reading Order:** Step 31 of 43 | **Module 6:** Adversarial Search & Game Playing  
> ◄ **Previous:** [[Alpha-Beta Pruning Algorithm]] | ► **Next:** [[Minimax and Alpha-Beta Game Tree Pruning Example]]

---

## Starting Point and the Problem

Even with the optimal $O(b^{m/2})$ speedup of Alpha-Beta pruning, searching to the terminal depth of complex games remains impossible within human turn-time limits (e.g., Chess with $m \approx 80$). Shannon (1950) proposed the solution implemented by all competitive game-playing engines: **cut off the search at a fixed depth $d$ and apply a heuristic evaluation function** to estimate the win probability of non-terminal states.

---

## Developing the Idea

We modify the standard Minimax / Alpha-Beta algorithm in two fundamental ways:
1. Replace `TERMINAL-TEST(s)` with `CUTOFF-TEST(s, depth)`.
2. Replace `UTILITY(s)` with `EVAL(s)` (a heuristic evaluation function).

```
   Full Game Tree (Impractical)              Depth-Limited Search with Cutoff
       [ Root ]                                     [ Root ]
          │                                            │
          │ Search depth 80                            │ Search depth 6 (Cutoff!)
          ▼                                            ▼
     [ Terminal: +1 / -1 ]                         [ Eval(s): +2.4 ]
```

---

## Designing Heuristic Evaluation Functions

An evaluation function $\text{Eval}(s)$ maps an arbitrary board state to a real number estimating the state's desirability for MAX:
- By convention: positive values favor MAX; negative values favor MIN; zero indicates neutral/draw.
- Terminal states should match actual utility signs: $\text{Eval}(\text{Win}) = +\infty, \text{Eval}(\text{Loss}) = -\infty$.

### Weighted Linear Evaluation Functions
The most common evaluation function formulation is a **weighted linear combination of features**:
$$\text{Eval}(s) = w_1 f_1(s) + w_2 f_2(s) + \dots + w_n f_n(s) = \sum_{i=1}^n w_i f_i(s)$$

*Example (Chess):*
- Features $f_i$: Material count differences ($\Delta \text{Pawns}, \Delta \text{Knights}, \Delta \text{Bishops}, \Delta \text{Rooks}, \Delta \text{Queens}$), piece mobility, center control, king pawn shield safety.
- Weights $w_i$: Traditional piece values:
  $$w_{\text{pawn}} = 1, \quad w_{\text{knight}} = 3, \quad w_{\text{bishop}} = 3.25, \quad w_{\text{rook}} = 5, \quad w_{\text{queen}} = 9$$

---

## Critical Failure Modes and Advanced Techniques

### 1. The Horizon Effect
The **Horizon Effect** occurs when an inevitable negative event (e.g., losing a queen) is pushed beyond the search horizon by making delaying moves (e.g., sacrificing pawns with inconsequential checks). The engine thinks it "saved" the queen because the capture lies past depth $d$, whereas in reality it simply suffered additional material damage.

### 2. Quiescence Search
Evaluating a state in the middle of a violent exchange (e.g., right after our queen captures an opponent knight, but before the opponent's pawn recaptures our queen) produces a disastrously misleading $\text{Eval}$ score.
- **Solution:** Apply the evaluation function only to **quiescent (quiet) states** where no active captures or checks are pending.
- If a state is non-quiescent, continue searching deeper along capture branches until quiet states are reached (**Quiescence Search**).

---

## Exam Relevance

- Formulating a weighted linear evaluation function for chess or checkers.
- Explaining the Horizon Effect and how Quiescence Search resolves it.

---

## Related Concepts

- [[Minimax Algorithm]]
- [[Alpha-Beta Pruning Algorithm]]

---

## Prerequisites

- [[Alpha-Beta Pruning Algorithm]]

---

## Navigation

◄ **Previous:** [[Alpha-Beta Pruning Algorithm]] | ► **Next:** [[Minimax and Alpha-Beta Game Tree Pruning Example]]
