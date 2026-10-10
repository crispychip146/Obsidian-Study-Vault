---
type: algorithm
course: cse317
status: active
order: 12
---

# Depth-First Search and Depth-Limited Search Algorithm

> 📖 **Reading Order:** Step 12 of 43 | **Module 3:** Problem Solving & Uninformed Search  
> ◄ **Previous:** [[Uniform-Cost Search Algorithm]] | ► **Next:** [[Iterative Deepening Search Algorithm]]

---

## Starting Point and the Problem

As demonstrated in [[Uninformed Search Strategies]], Breadth-First Search and Uniform-Cost Search are crippled by exponential memory requirements ($O(b^d)$). On realistic problems, memory runs out within minutes. We need an alternative search strategy whose memory footprint grows **linearly** with depth, allowing deep exploration without running out of RAM. This is achieved by **Depth-First Search (DFS)**.

---

## Developing the Idea

DFS explores as deeply as possible along each branch before backtracking.
Instead of concentric ripples, DFS follows a single path to its terminal leaf. If that leaf is not a goal, it backtracks to the most recent ancestor with unexplored siblings.
To implement this, the frontier is structured as a **Last-In, First-Out (LIFO) queue (Stack)**, or executed via **recursion**.

```
                           [ Node 1: Root ]
                            /            \
                [ Node 2 ]                  [ Node 5 ]
                 /      \                    /      \
            [ Node 3 ]  [ Node 4 ]      [ Node 6 ]  [ Node 7 ]
```
*Traversal Order:* $1 \to 2 \to 3 \to (\text{backtrack}) \to 4 \to (\text{backtrack}) \to 5 \to 6 \to \dots$

---

## Algorithm Specifications

### 1. Depth-First Search (Recursive Implementation)

```python
def DEPTH_FIRST_SEARCH(problem):
    return RECURSIVE_DFS(Node(problem.INITIAL_STATE), problem, set())

def RECURSIVE_DFS(node, problem, explored):
    if problem.GOAL_TEST(node.state):
        return SOLUTION(node)
    
    explored.add(node.state)
    
    for action in problem.ACTIONS(node.state):
        child = CHILD_NODE(problem, node, action)
        if child.state not in explored:
            result = RECURSIVE_DFS(child, problem, explored)
            if result is not FAILURE:
                return result
                
    return FAILURE
```

---

### 2. Depth-Limited Search (DLS)

Standard DFS can get trapped down an infinitely deep branch (e.g., in continuous spaces or infinite trees). **Depth-Limited Search (DLS)** prevents this by imposing a predetermined depth cutoff $l$.

```python
def DEPTH_LIMITED_SEARCH(problem, limit):
    return RECURSIVE_DLS(Node(problem.INITIAL_STATE), problem, limit)

def RECURSIVE_DLS(node, problem, limit):
    if problem.GOAL_TEST(node.state):
        return SOLUTION(node)
    elif limit == 0:
        return 'cutoff'  # Reached depth limit without goal
    
    cutoff_occurred = False
    for action in problem.ACTIONS(node.state):
        child = CHILD_NODE(problem, node, action)
        result = RECURSIVE_DLS(child, problem, limit - 1)
        
        if result == 'cutoff':
            cutoff_occurred = True
        elif result is not FAILURE:
            return result
            
    return 'cutoff' if cutoff_occurred else FAILURE
```

---

## Complexity Analysis

Let $b$ be the branching factor, $m$ be the maximum depth of the state space, and $l$ be the depth limit.

### Space Complexity: The Linear Advantage
At any point during DFS, the memory needs to store only:
1. The path from the root to the current node ($m$ nodes).
2. The remaining unexpanded siblings along that path ($b-1$ siblings per level).
$$\text{Space Complexity} = O(bm)$$
For DLS with limit $l$:
$$\text{Space Complexity} = O(bl)$$

*Comparison with BFS:* For $b=10$ and depth $d=10$:
- BFS requires $\approx 10^{10}$ nodes ($\approx 10\text{ Terabytes}$).
- DFS requires only $10 \times 10 = 100$ nodes ($\approx 100\text{ Kilobytes}$).

### Time Complexity
In the worst case, DFS generates all nodes in the state space up to maximum depth $m$:
$$\text{Time Complexity} = O(b^m)$$
If $m \gg d$ (e.g., a shallow goal at $d=3$, but the graph has branches of depth $m=100$), DFS can waste enormous time exploring deep dead ends.

---

## Evaluation Properties

- **Completeness:**
  - Tree Search DFS: **Incomplete**. Fails if infinite paths or cycles exist.
  - Graph Search DFS: Complete in finite state spaces; incomplete in infinite state spaces.
  - DLS: Incomplete if the shallowest goal lies beyond the depth limit ($d > l$).
- **Optimality:**
  - **Non-optimal**. DFS returns the *first* goal it finds, which may be a deep, expensive path, even if a shallow, cheap goal exists on another branch.

---

## Common Mistakes

- Using Tree-search DFS on graphs with reversible actions, creating infinite recursive loops until stack overflow.
- Believing DLS returns failure when a limit cutoff occurs. A cutoff indicates the goal might still exist at depth $> l$; failure indicates the entire sub-tree was exhausted with no goal.

---

## Exam Relevance

- Distinguishing between failure and cutoff in DLS.
- Calculating the exact memory footprint of DFS vs. BFS.
- Explaining why DFS is non-optimal.

---

## Related Concepts

- [[Uninformed Search Strategies]]
- [[Breadth-First Search Algorithm]]
- [[Iterative Deepening Search Algorithm]]

---

## Prerequisites

- [[Uninformed Search Strategies]]

---

## Navigation

◄ **Previous:** [[Uniform-Cost Search Algorithm]] | ► **Next:** [[Iterative Deepening Search Algorithm]]
