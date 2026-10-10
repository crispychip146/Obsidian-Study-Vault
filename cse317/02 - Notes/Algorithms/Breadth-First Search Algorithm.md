---
type: algorithm
course: cse317
status: active
order: 10
---

# Breadth-First Search Algorithm

> 📖 **Reading Order:** Step 10 of 43 | **Module 3:** Problem Solving & Uninformed Search  
> ◄ **Previous:** [[Uninformed Search Strategies]] | ► **Next:** [[Uniform-Cost Search Algorithm]]

---

## Starting Point and the Problem

When searching a state space with no heuristic guidance, we need a systematic way to guarantee discovering a solution if one exists. If an algorithm dives deeply down a single infinite branch (like naive DFS), it may never return, even if a shallow goal exists just one step from the start. **Breadth-First Search (BFS)** solves this by exploring all states level by level, guaranteeing that the shallowest goal is expanded first.

---

## Developing the Idea

Imagine dropping a pebble into a pond: ripples expand outward concentrically. Similarly, BFS expands concentric frontiers of depth $0$, then depth $1$, then depth $2$, and so on.
To implement this level-by-level exploration, we store the frontier in a **First-In, First-Out (FIFO) queue**. Every newly generated child is placed at the back of the queue, ensuring all nodes at depth $k$ are expanded before any node at depth $k+1$.

```
                       [ Depth 0: Root ]
                              │
               ┌──────────────┴──────────────┐
               ▼                             ▼
       [ Depth 1: Node A ]           [ Depth 1: Node B ]
               │                             │
         ┌─────┴─────┐                 ┌─────┴─────┐
         ▼           ▼                 ▼           ▼
       [ D2: C ]   [ D2: D ]         [ D2: E ]   [ D2: Goal ]
```

---

## Algorithm Specification

```python
def BREADTH_FIRST_SEARCH(problem):
    # Create the root node
    node = Node(state=problem.INITIAL_STATE, path_cost=0)
    
    # Early Goal Test: check root immediately
    if problem.GOAL_TEST(node.state):
        return SOLUTION(node)
    
    # Frontier is a FIFO queue
    frontier = FIFOQueue([node])
    explored = set()  # Closed list for Graph Search

    while not frontier.is_empty():
        node = frontier.pop()  # Removes the shallowest node
        explored.add(node.state)

        for action in problem.ACTIONS(node.state):
            child = CHILD_NODE(problem, node, action)
            
            if child.state not in explored and child.state not in frontier:
                # Early Goal Test on generation
                if problem.GOAL_TEST(child.state):
                    return SOLUTION(child)
                frontier.push(child)
                
    return FAILURE
```

---

## How It Works: Step-by-Step Mechanism

1. **Root Initialization:** Check if the initial state is a goal. If so, return immediately. Otherwise, insert it into the FIFO queue.
2. **Expansion:** Pop the oldest node from the front of the queue.
3. **Explored Set Check:** Add the node's state to `explored` to prevent cycles.
4. **Successor Generation & Early Goal Test:**
   - Generate all valid successor child nodes.
   - For each child not already in `explored` or `frontier`, perform the **goal test immediately upon generation**.
   - If a child is a goal, terminate and return the solution.
   - If not, append the child to the back of the FIFO queue.

---

## Technical Details

### Why Goal Test on Generation (Early Goal Test)?

In BFS, we apply the goal test when a node is **generated**, rather than when it is selected for expansion.
- *Proof of Correctness:* Because all nodes at depth $d-1$ are expanded before nodes at depth $d$, the first time a goal node is generated at depth $d$, we are guaranteed that no goal exists at depth $< d$.
- *Optimization Benefit:* Performing the goal test on generation saves expanding an entire additional layer of nodes (up to a factor of $b$ speedup).
- *Contrast:* In Uniform-Cost Search (UCS), this early test is illegal because the first generated path to a goal may not be the cheapest.

### Complexity Analysis

- **Time Complexity:**
  At depth $0$, $1$ node. At depth $1$, $b$ nodes. At depth $d$, $b^d$ nodes.
  $$\text{Total generated} = 1 + b + b^2 + \dots + b^d = \frac{b^{d+1} - 1}{b - 1} = O(b^d)$$
- **Space Complexity:**
  All nodes at depth $d$ remain in the frontier simultaneously:
  $$\text{Space} = O(b^d)$$
  This exponential memory requirement makes pure BFS impractical for deep search spaces.

---

## Important Properties and Why They Hold

- **Completeness:** If the branching factor $b$ is finite and a solution exists at depth $d$, BFS will systematically examine all nodes at depths $0, 1, \dots, d-1$ and must find the goal at depth $d$.
- **Optimality:** BFS is optimal if and only if **all step costs are equal (uniform)**. In that case, path cost $g(n) = c \cdot \text{depth}(n)$, so the shallowest goal is guaranteed to be the lowest-cost goal. If step costs vary, BFS is NOT optimal.

---

## Common Mistakes

- Forgetting the explored set, resulting in infinite loops in graphs with reversible actions.
- Assuming BFS is optimal for road networks with varying edge distances.

---

## Exam Relevance

- Explaining why BFS can safely apply the goal test on node generation rather than expansion.
- Computing time and space complexity with numeric branching factors.
- Proving optimality conditions for uniform step costs.

---

## Related Concepts

- [[Uninformed Search Strategies]]
- [[Uniform-Cost Search Algorithm]]
- [[Iterative Deepening Search Algorithm]]

---

## Prerequisites

- [[Problem-Solving Agents and State Space Formulation]]
- [[Uninformed Search Strategies]]

---

## Sources

- **Lectures:** [[cse317/01 - Sources/Lectures/MMi/Chap3-ProbSol.ppt|Chap3-ProbSol.ppt]], [[cse317/01 - Sources/Lectures/MMi/UnInformedSearch.ppt|UnInformedSearch.ppt]] (Slides 6–10)
- **Textbook:** [[cse317/01 - Sources/Lectures/MMi/Artificial Intelligence A Modern Approach 3rd Edition.pdf|Artificial Intelligence: A Modern Approach (3rd Edition)]], Chapter 3: Solving Problems by Searching (Section 3.4.1)

---

## Navigation

◄ **Previous:** [[Uninformed Search Strategies]] | ► **Next:** [[Uniform-Cost Search Algorithm]]
