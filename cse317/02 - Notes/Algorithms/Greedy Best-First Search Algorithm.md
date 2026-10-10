---
type: algorithm
course: cse317
status: active
order: 17
---

# Greedy Best-First Search Algorithm

> 📖 **Reading Order:** Step 17 of 43 | **Module 4:** Informed (Heuristic) Search & A* Search  
> ◄ **Previous:** [[Heuristic Functions and Properties]] | ► **Next:** [[A-Star Search Algorithm]]

---

## Starting Point and the Problem

When searching a large state space, we want to reach the goal as quickly as possible. An intuitive human approach is to always take the action that appears to get us closest to the destination right now. **Greedy Best-First Search** formalizes this pure heuristic greed.

---

## Developing the Idea

Greedy Best-First Search evaluates nodes using only the heuristic function:
$$f(n) = h(n)$$
At each step, it expands the node on the frontier that is estimated to be closest to the goal.

```
       [ Frontier Priority Queue: Ordered strictly by h(n) ]
                                 │
                   Pop node with smallest h(n)
                                 │
                                 ▼
                     [ Expand toward goal ]
```

---

## Algorithm Specification

```python
def GREEDY_BEST_FIRST_SEARCH(problem, h):
    node = Node(problem.INITIAL_STATE)
    # Priority Queue ordered strictly by h(node.state)
    frontier = PriorityQueue(key=lambda n: h(n.state))
    frontier.push(node)
    explored = set()

    while not frontier.is_empty():
        node = frontier.pop()
        
        if problem.GOAL_TEST(node.state):
            return SOLUTION(node)
            
        explored.add(node.state)
        
        for action in problem.ACTIONS(node.state):
            child = CHILD_NODE(problem, node, action)
            if child.state not in explored and child.state not in frontier:
                frontier.push(child)
                
    return FAILURE
```

---

## Limitations and Failure Modes

Despite its intuitive appeal, Greedy Best-First Search suffers from severe theoretical weaknesses:

1. **Non-Optimality:**
   Greedy search ignores the accumulated path cost $g(n)$. If a direct path toward the goal has low $h(n)$ but incurs an enormous step cost, greedy search blindly follows it.
2. **Susceptibility to Dead Ends and Detours:**
   Like DFS, greedy search can rush down a false path that appears close to the goal as the crow flies, only to hit an impassable obstacle (e.g., a mountain range or river).
3. **Incompleteness:**
   In tree search, greedy search can get trapped in infinite loops in state spaces with cycles. In graph search with finite states, it is complete.

---

## Complexity Analysis

Let $b$ be the branching factor and $m$ be the maximum depth of the state space:
- **Time Complexity:** $O(b^m)$ in the worst case (can explore an entire erroneous subtree).
- **Space Complexity:** $O(b^m)$ (stores all generated nodes in the frontier).

However, with a high-quality heuristic, the practical execution time is often drastically faster than uninformed algorithms.

---

## Common Mistakes

- Confusing Greedy Best-First Search with A* Search ($f(n) = h(n)$ vs. $f(n) = g(n) + h(n)$).
- Assuming Greedy BFS finds the shortest path.

---

## Exam Relevance

- Tracing Greedy BFS on road maps (e.g., Arad to Bucharest showing why it chooses Fagaras instead of Rimnicu Vilcea).
- Contrasting the evaluation function of Greedy BFS with A*.

---

## Related Concepts

- [[Heuristic Functions and Properties]]
- [[A-Star Search Algorithm]]
- [[Uniform-Cost Search Algorithm]]

---

## Prerequisites

- [[Heuristic Functions and Properties]]

---

## Sources

- **Lectures:** [[cse317/01 - Sources/Lectures/MMi/Chap4-InformedSearch.ppt|Chap4-InformedSearch.ppt]] (Slides 13–18)
- **Textbook:** [[cse317/01 - Sources/Lectures/MMi/Artificial Intelligence A Modern Approach 3rd Edition.pdf|Artificial Intelligence: A Modern Approach (3rd Edition)]], Chapter 3: Solving Problems by Searching (Section 3.5.1)

---

## Navigation

◄ **Previous:** [[Heuristic Functions and Properties]] | ► **Next:** [[A-Star Search Algorithm]]
