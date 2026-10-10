---
type: concept
course: cse317
status: active
order: 19
---

# Optimality of A-Star Search

> 📖 **Reading Order:** Step 19 of 43 | **Module 4:** Informed (Heuristic) Search & A* Search  
> ◄ **Previous:** [[A-Star Search Algorithm]] | ► **Next:** [[Memory-Bounded Heuristic Search Algorithms]]

---

## Starting Point and the Problem

A* search is celebrated throughout artificial intelligence because it guarantees discovering the cheapest possible path to a goal while aggressively pruning suboptimal branches. However, this optimality guarantee is not accidental: it depends on strict mathematical properties of the heuristic function $h(n)$. We must rigorously prove why A* is optimal under Tree Search (with an admissible heuristic) and under Graph Search (with a consistent heuristic).

---

## Proof 1: Optimality of A* Tree Search (Admissible Heuristic)

**Theorem:** A* Tree Search is optimal if $h(n)$ is admissible ($h(n) \le h^*(n)$).

*Proof by Contradiction:*
1. Let $C^*$ be the cost of the optimal solution.
2. Let $G$ be an optimal goal node (so $g(G) = C^*$, and since it is a goal, $h(G) = 0$, giving $f(G) = g(G) + 0 = C^*$).
3. Suppose for contradiction that A* terminates by popping a suboptimal goal node $G_2$ from the frontier, where:
   $$g(G_2) > C^* \implies f(G_2) = g(G_2) + 0 > C^*$$
4. Consider an unexpanded node $n$ that lies on the optimal path from the start to $G$. Since a path to $G$ exists, such a node $n$ must be present on the frontier.
5. By definition:
   $$f(n) = g(n) + h(n)$$
6. Because $h$ is admissible, $h(n)$ does not overestimate the true cost to reach $G$ from $n$:
   $$h(n) \le h^*(n)$$
7. Therefore:
   $$f(n) = g(n) + h(n) \le g(n) + h^*(n) = C^*$$
8. Combining inequalities:
   $$f(n) \le C^* < f(G_2)$$
9. Since the priority queue always selects the node with the minimum $f$-value, node $n$ must be popped and expanded before $G_2$ could ever be popped.
10. This holds for every node along the optimal path, eventually expanding optimal goal $G$ itself before $G_2$.
11. This contradicts the hypothesis that $G_2$ was popped.
12. Therefore, A* Tree Search can never return a suboptimal goal. $\blacksquare$

---

## Proof 2: Optimality of A* Graph Search (Consistent Heuristic)

Under Graph Search, duplicate states are discarded. If A* were to reach a state via an expensive path first and discard a later cheaper path, optimality would fail. Consistency guarantees this never happens.

**Lemma 1 ($f$ is non-decreasing along paths):**
If $h(n)$ is consistent ($h(n) \le c(n, a, n') + h(n')$), then $f(n') \ge f(n)$ for any successor $n'$.
*Proof:*
$$f(n') = g(n') + h(n') = g(n) + c(n, a, n') + h(n')$$
Since $c(n, a, n') + h(n') \ge h(n)$:
$$f(n') \ge g(n) + h(n) = f(n) \quad \blacksquare$$

**Lemma 2 (Optimal Expansion Property):**
Whenever A* selects a node $n$ for expansion, the optimal path to that state has already been found: $g(n) = g^*(n)$.
*Proof by Contradiction:*
1. Suppose node $n$ is selected for expansion, but $g(n) > g^*(n)$ (a cheaper unexpanded path exists).
2. Let $n'$ be the first unexpanded node on the true optimal path to $n$.
3. Since $f$ is non-decreasing along optimal paths:
   $$f(n') \le f^*(n) < f(n)$$
4. But if $f(n') < f(n)$, the priority queue would have selected $n'$ for expansion before $n$!
5. This is a contradiction. Therefore, $g(n) = g^*(n)$. $\blacksquare$

**Main Theorem:**
Since every state is expanded with its optimal cost $g^*(n)$, the first time a goal state is expanded, its path cost is guaranteed optimal. Furthermore, no state ever needs to be re-opened once placed in the explored set! $\blacksquare$

---

## $f$-Contours and Pruning Geometry

A* search can be visualized as expanding concentric **$f$-contours**:
- For any value $k$, let the $k$-contour be the set of states with $f(s) \le k$.
- A* systematically expands all nodes with $f(n) < C^*$.
- It may expand some nodes with $f(n) = C^*$ before reaching the goal.
- It **never expands any node with $f(n) > C^*$**! All such branches are completely pruned.

```
       Start ──► [ f <= 200 ] ──► [ f <= 300 ] ──► [ f <= 400 ] ──► Goal (C* = 418)
                                                        │
                                            PRUNED: [ f > 418 ]
```

---

## Common Mistakes

- Assuming admissibility alone guarantees graph-search optimality without re-opening nodes. (Without consistency, a closed node might need to be returned to the frontier if a cheaper path is found later).
- Believing A* expands zero suboptimal nodes (it expands nodes with $f(n) \le C^*$).

---

## Exam Relevance

- Reproducing Proof 1 (Tree Search with admissible $h$).
- Reproducing Proof 2 (Graph Search with consistent $h$).
- Explaining the geometric concept of $f$-contours.

---

## Related Concepts

- [[Heuristic Functions and Properties]]
- [[A-Star Search Algorithm]]
- [[Memory-Bounded Heuristic Search Algorithms]]

---

## Prerequisites

- [[A-Star Search Algorithm]]
- [[Heuristic Functions and Properties]]

---

## Navigation

◄ **Previous:** [[A-Star Search Algorithm]] | ► **Next:** [[Memory-Bounded Heuristic Search Algorithms]]
