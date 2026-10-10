---
type: algorithm
course: cse317
status: active
order: 14
---

# Bidirectional Search Algorithm

> 📖 **Reading Order:** Step 14 of 43 | **Module 3:** Problem Solving & Uninformed Search  
> ◄ **Previous:** [[Iterative Deepening Search Algorithm]] | ► **Next:** [[8-Puzzle and Vacuum World State Space Example]]

---

## Starting Point and the Problem

Standard search algorithms explore forward from the initial state toward the goal. For a problem with branching factor $b$ and goal depth $d$, forward search requires $O(b^d)$ time. As $d$ grows, $b^d$ explodes combinatorially. If we know the exact goal state in advance, an intuitive question arises: why not search simultaneously forward from the initial state and backward from the goal state?

---

## Developing the Idea

Suppose a path of length $d$ connects start state $S$ to goal state $G$.
- If we search forward from $S$ to depth $d/2$, we generate $O(b^{d/2})$ nodes.
- If we search backward from $G$ to depth $d/2$, we generate $O(b^{d/2})$ nodes.
- When the two frontiers intersect, a complete solution path is found!

```
       [ Start S ] ──► ──► ──► [ Frontier S ]
                                     ▲
                              Intersection!
                                     ▼
       [ Goal G ]  ◄── ◄── ◄── [ Frontier G ]
```

### The Complexity Reduction
$$\text{Total Time Complexity} = b^{d/2} + b^{d/2} = 2 b^{d/2} \ll b^d$$

For $b = 10$ and $d = 8$:
- Forward search generates: $10^8 = 100,000,000\text{ nodes}$.
- Bidirectional search generates: $2 \times 10^4 = 20,000\text{ nodes}$.
This represents a **5,000-fold speedup**!

---

## Algorithm Mechanics

1. Maintain two frontiers: `frontier_forward` starting at $s_0$, and `frontier_backward` starting at $s_{goal}$.
2. In each iteration, expand a node from one of the frontiers (typically the smaller frontier).
3. Check for intersection: If a newly generated node exists in the opposing search's frontier or explored set, a path connecting start and goal has been established.
4. Construct the complete solution by concatenating the path from start to intersection with the reversed path from intersection to goal.

---

## Practical Challenges of Bidirectional Search

While theoretically attractive, bidirectional search faces significant practical obstacles:

1. **Predecessor Generation:**
   Backward search requires computing predecessors (states that can lead to state $s$ via some action). In many domains, actions are not easily reversible (e.g., irreversible chemical reactions, cryptographic hashing).
2. **Multiple or Abstract Goals:**
   If the goal is an explicit single state (like a specific city), backward search is easy. But if the goal is an abstract condition (e.g., "Checkmate in chess" or "Clean room in vacuum world"), there are millions of potential goal states, making backward initialization impossible.
3. **Intersection Detection:**
   At least one frontier must be held entirely in a hash table in memory to check whether newly generated nodes intersect in $O(1)$ time. This requires $O(b^{d/2})$ space.

---

## Evaluation Properties

- **Completeness:** Complete if $b$ is finite and both frontiers use BFS.
- **Optimality:** Optimal for uniform step costs, provided intersection checking properly accounts for path costs.
- **Time Complexity:** $O(b^{d/2})$.
- **Space Complexity:** $O(b^{d/2})$ (at least one frontier must remain in memory).

---

## Exam Relevance

- Proving the $O(b^{d/2})$ time reduction.
- Identifying domains where bidirectional search cannot be easily applied (abstract goals, non-invertible operators).

---

## Related Concepts

- [[Breadth-First Search Algorithm]]
- [[Iterative Deepening Search Algorithm]]
- [[Uninformed Search Strategies]]

---

## Prerequisites

- [[Breadth-First Search Algorithm]]

---

## Navigation

◄ **Previous:** [[Iterative Deepening Search Algorithm]] | ► **Next:** [[8-Puzzle and Vacuum World State Space Example]]
