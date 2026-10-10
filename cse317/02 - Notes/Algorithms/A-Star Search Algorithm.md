---
type: algorithm
course: cse317
status: active
order: 18
---

# A-Star Search Algorithm

> 📖 **Reading Order:** Step 18 of 43 | **Module 4:** Informed (Heuristic) Search & A* Search  
> ◄ **Previous:** [[Greedy Best-First Search Algorithm]] | ► **Next:** [[Optimality of A-Star Search]]

---

## Starting Point and the Problem

We have observed two contrasting search strategies:
- **Uniform-Cost Search:** Minimizes cost incurred so far ($g(n)$). Optimal and complete, but inefficient because it expands equally in all directions without orienting toward the goal.
- **Greedy Best-First Search:** Minimizes estimated cost to goal ($h(n)$). Fast and goal-directed, but neither optimal nor complete.

We desire a search algorithm that combines the **optimality guarantees of Uniform-Cost Search** with the **goal-directed efficiency of Greedy Best-First Search**. That algorithm is **A* Search** (Hart, Nilsson, and Raphael, 1968).

---

## Developing the Idea

A* evaluates nodes by combining the cost already spent with the estimated cost to complete the path:
$$f(n) = g(n) + h(n)$$

```
                           f(n) = g(n) + h(n)
                                 │
           ┌─────────────────────┴─────────────────────┐
           ▼                                           ▼
          g(n)                                        h(n)
   Cost incurred so far                      Estimated remaining cost
   from start to node n                       from node n to goal
```

- $f(n)$ represents the **estimated total cost of the cheapest solution passing through node $n$**.
- If $h(n)$ is admissible ($h(n) \le h^*(n)$), $f(n)$ never overestimates the true cost of a solution through $n$.

---

## Algorithm Specification (Graph Search A*)

```python
def A_STAR_SEARCH(problem, h):
    # Initial node: g = 0, f = 0 + h(start)
    node = Node(state=problem.INITIAL_STATE, path_cost=0)
    
    # Priority Queue ordered by f(n) = g(n) + h(n)
    frontier = PriorityQueue(key=lambda n: n.path_cost + h(n.state))
    frontier.push(node)
    
    # Explored set (closed list) mapping state -> best g(state) seen
    explored = {}

    while not frontier.is_empty():
        # Pop node with MINIMUM f(n)
        node = frontier.pop()
        
        # LATE GOAL TEST: Apply goal test ONLY when popped!
        if problem.GOAL_TEST(node.state):
            return SOLUTION(node)
            
        explored[node.state] = node.path_cost

        for action in problem.ACTIONS(node.state):
            child = CHILD_NODE(problem, node, action)
            child_g = child.path_cost
            
            # If already explored with a cheaper or equal g-cost, skip
            if child.state in explored and child_g >= explored[child.state]:
                continue
                
            # If in frontier with higher g-cost, update it; otherwise insert
            if child.state not in frontier:
                frontier.push(child)
            elif child_g < frontier[child.state].path_cost:
                frontier.decrease_key(child.state, child)
                
    return FAILURE
```

---

## How It Works: Step-by-Step Mechanics

1. **Priority Queue Ordering:** The frontier is prioritized by lowest $f(n) = g(n) + h(n)$. Ties are typically broken by preferring nodes with higher $g(n)$ (closer to the goal).
2. **Late Goal Test:** Like UCS, the goal test is executed **only when a node is selected for expansion (popped from frontier)**. Testing on generation would destroy optimality because a cheaper path to that goal could still be queued.
3. **Pruning Suboptimal Cycles:** In graph search, if a new path reaches a state that was already visited, but with a strictly lower $g$-cost, the cheaper path must be updated.

---

## Theoretical Properties

1. **Completeness:** A* is complete on graphs with finite branching factor $b$ and step costs bounded below by $\epsilon > 0$.
2. **Optimality:**
   - Under **Tree Search**: Optimal if $h(n)$ is **admissible**.
   - Under **Graph Search**: Optimal if $h(n)$ is **consistent (monotonic)**.
3. **Optimal Efficiency:** No other optimal search algorithm expanding nodes in order of $f$ can expand fewer nodes than A* (for a given consistent heuristic).
4. **Complexity:**
   - **Time:** $O(b^{\epsilon \cdot d})$ where $\epsilon = \frac{h^* - h}{h^*}$ is the relative error of the heuristic. If $h$ is perfect ($h = h^*$), A* steps directly to the goal in $O(d)$ time.
   - **Space:** $O(b^d)$. Like BFS, A* stores all generated nodes in memory. **Memory exhaustion is A*'s primary real-world limitation**.

---

## Common Mistakes

- Testing for a goal node upon generation instead of expansion.
- Using an inadmissible heuristic and expecting optimal solutions.
- Confusing tree-search A* (needs admissibility) with graph-search A* (needs consistency).

---

## Exam Relevance

- Step-by-step trace of A* showing $g, h, f$ values and queue updates.
- Explaining the necessity of the late goal test.
- Explaining why memory, not time, is the bottleneck of A*.

---

## Related Concepts

- [[Heuristic Functions and Properties]]
- [[Optimality of A-Star Search]]
- [[Memory-Bounded Heuristic Search Algorithms]]
- [[Romania Travel Routing A-Star Search Example]]

---

## Prerequisites

- [[Uniform-Cost Search Algorithm]]
- [[Heuristic Functions and Properties]]

---

## Navigation

◄ **Previous:** [[Greedy Best-First Search Algorithm]] | ► **Next:** [[Optimality of A-Star Search]]
