---
type: algorithm
course: cse317
status: active
order: 25
---

# Local Beam Search Algorithm

> 📖 **Reading Order:** Step 25 of 43 | **Module 5:** Local Search, Optimization & Genetic Algorithms  
> ◄ **Previous:** [[Simulated Annealing Algorithm]] | ► **Next:** [[Genetic Algorithm]]

---

## Starting Point and the Problem

Single-state local search algorithms (hill-climbing, simulated annealing) track only one state at a time. While random-restart hill-climbing runs multiple searches, those runs are completely independent: search #5 learns nothing from search #1. If one search path discovers a highly promising ridge, that information is not shared with the other searches. We need an algorithm that maintains multiple candidate solutions and shares information among them. That algorithm is **Local Beam Search**.

---

## Developing the Idea

Local Beam Search maintains **$k$ distinct states** simultaneously:
1. Start with $k$ randomly generated states.
2. At each step, generate all successors of all $k$ states.
3. If any successor is a goal, terminate.
4. Otherwise, select the **$k$ best successors from the entire combined pool** of successors, and repeat.

```
       [ State 1 ] ──► Generates successors: {s₁₁, s₁₂, s₁₃} ┐
       [ State 2 ] ──► Generates successors: {s₂₁, s₂₂, s₂₃} ┼──► Pool of all successors
       [ State 3 ] ──► Generates successors: {s₃₁, s₃₂, s₃₃} ┘            │
                                                                          ▼
                                                          Select TOP k best successors!
                                                                          │
                                                      ┌───────────────────┴───────────────────┐
                                                      ▼                                       ▼
                                              [ New State 1 ]                         [ New State 2 ] ...
```

---

## Local Beam Search vs. $k$ Parallel Random Restarts

A common misconception is that Local Beam Search is merely $k$ parallel hill-climbers running at the same time. **This is false!**
- In $k$ independent random restarts, each search operates in isolation. If 9 searches get stuck and 1 is on a good path, the 9 stuck searches continue wasting computation.
- In **Local Beam Search**, states **share information**:
  - The pool of successors is combined.
  - If one state discovers a fantastic region of the search space, **multiple beams from the $k$ slots will converge onto successors of that promising state**!
  - Poorly performing states are immediately abandoned, and resources are dynamically reallocated to the fruitful branches.

---

## Stochastic Beam Search

Standard local beam search can suffer from a lack of diversity: if one peak is steep, all $k$ beams quickly cluster around the same local maximum.
**Stochastic Beam Search** introduces natural selection:
- Instead of deterministically choosing the top $k$ successors, it chooses $k$ successors probabilistically, with probability proportional to their objective value / fitness:
  $$P(s_i) = \frac{\text{Value}(s_i)}{\sum_j \text{Value}(s_j)}$$
- This directly bridges local search with **Genetic Algorithms**!

---

## Complexity Analysis

- **Space Complexity:** $O(k \cdot S)$ where $S$ is state representation size (constant bounded memory).
- **Time Complexity:** Proportional to $k \cdot b$ per step.

---

## Exam Relevance

- Contrasting Local Beam Search with $k$ independent random-restart hill-climbing runs.
- Explaining how beams cluster and how Stochastic Beam Search restores diversity.

---

## Related Concepts

- [[Hill-Climbing Search Algorithm]]
- [[Simulated Annealing Algorithm]]
- [[Genetic Algorithm]]

---

## Prerequisites

- [[Hill-Climbing Search Algorithm]]

---

## Navigation

◄ **Previous:** [[Simulated Annealing Algorithm]] | ► **Next:** [[Genetic Algorithm]]
