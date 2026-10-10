---
type: algorithm
course: cse317
status: active
order: 20
---

# Memory-Bounded Heuristic Search Algorithms

> 📖 **Reading Order:** Step 20 of 43 | **Module 4:** Informed (Heuristic) Search & A* Search  
> ◄ **Previous:** [[Optimality of A-Star Search]] | ► **Next:** [[Romania Travel Routing A-Star Search Example]]

---

## Starting Point and the Problem

As proven in [[A-Star Search Algorithm]], A* search expands all nodes with $f(n) < C^*$ and stores them in memory. In large search spaces, A* quickly exhausts available RAM within minutes. To solve problems with massive state spaces (e.g., 15-puzzle with $10^{13}$ states, or 24-puzzle), we require algorithms that preserve heuristic guidance while strictly bounding memory consumption.

---

## The Three Principal Memory-Bounded Algorithms

### 1. Iterative Deepening A* (IDA*)

**IDA*** adapts the concept of Iterative Deepening Search ([[Iterative Deepening Search Algorithm]]) to heuristic search:
- Instead of cutting off search by depth, IDA* cuts off search by the **evaluation function $f(n) = g(n) + h(n)$**.
- In each iteration, DFS explores all paths until $f(n)$ exceeds a threshold.
- The cutoff threshold for the *next* iteration is updated to the **minimum $f$-value that exceeded the threshold in the current iteration**.

```python
def IDA_STAR(problem, h):
    threshold = h(problem.INITIAL_STATE)
    while True:
        min_exceeded = float('inf')
        result, min_exceeded = CONTOUR_DFS(Node(problem.INITIAL_STATE), threshold, problem, h)
        if result == 'found':
            return result
        if min_exceeded == float('inf'):
            return FAILURE
        threshold = min_exceeded
```

- **Space Complexity:** $O(bd)$ (Strictly linear, like DFS!).
- **Completeness & Optimality:** Complete and optimal if $h$ is admissible.
- **Drawback:** In domains with continuous or real-valued step costs, each iteration may increase the threshold by only $\epsilon$, regenerating almost all nodes each time.

---

### 2. Recursive Best-First Search (RBFS)

**RBFS** mimics the operation of standard best-first search, but operates in linear space using recursion:
- Maintains an $f$-limit parameter representing the $f$-value of the **best alternative path** available from any ancestor.
- It explores the current best branch as long as its $f$-value remains $\le f\text{-limit}$.
- If all children exceed the limit, it unwinds the recursion, **backing up the best child's $f$-value to the parent node**, and switches to the alternative path.

```
                  [ Parent Node (backed-up f = 15) ]
                              /        \
           [ Current Path: f = 16 ]    [ Best Alternative: f = 15 ]
                  (Exceeds limit! Switch to alternative!)
```

- **Space Complexity:** $O(bd)$ (Linear space).
- **Drawback:** Prone to excessive "mind-changing" (thrashing): if alternative paths have similar $f$-values, RBFS repeatedly switches back and forth, regenerating states.

---

### 3. Simplified Memory-Bounded A* (SMA*)

While IDA* and RBFS use too little memory ($O(bd)$) and waste time regenerating nodes, **SMA*** utilizes **all available memory**:
- Runs standard A* until memory is completely full.
- When memory is exhausted and a new node must be added, SMA* **drops the worst leaf node** (the node with the highest $f$-value).
- Before dropping the node, SMA* **backs up its $f$-value to its parent**.
- If all other paths turn out worse than the dropped node, SMA* can regenerate it using the backed-up parent value.

- **Optimality:** Optimal if the available memory is large enough to store the shallowest optimal solution path.

---

## Comparison Matrix

| Algorithm | Memory Usage | Completeness | Optimality | Main Weakness |
|---|---|---|---|---|
| **A\*** | $O(b^d)$ (Exponential) | Yes | Yes | Runs out of memory quickly |
| **IDA\*** | $O(bd)$ (Linear) | Yes | Yes | Inefficient when step costs vary continuously |
| **RBFS** | $O(bd)$ (Linear) | Yes | Yes | Path thrashing / node regeneration |
| **SMA\*** | User-bounded ($M$ nodes) | Yes | Yes (if $M \ge d$) | High memory management overhead |

---

## Exam Relevance

- Explaining how the cutoff threshold is updated in IDA*.
- Comparing IDA*, RBFS, and SMA* on memory and node regeneration trade-offs.

---

## Related Concepts

- [[A-Star Search Algorithm]]
- [[Iterative Deepening Search Algorithm]]
- [[Optimality of A-Star Search]]

---

## Prerequisites

- [[A-Star Search Algorithm]]

---

## Sources

- **Lectures:** [[cse317/01 - Sources/Lectures/MMi/Chap4-InformedSearch.ppt|Chap4-InformedSearch.ppt]] (Slides 36–42)
- **Textbook:** [[cse317/01 - Sources/Lectures/MMi/Artificial Intelligence A Modern Approach 3rd Edition.pdf|Artificial Intelligence: A Modern Approach (3rd Edition)]], Chapter 3: Solving Problems by Searching (Section 3.5.3)

---

## Navigation

◄ **Previous:** [[Optimality of A-Star Search]] | ► **Next:** [[Romania Travel Routing A-Star Search Example]]
