---
type: algorithm
course: cse317
status: active
order: 11
---

# Uniform-Cost Search Algorithm

> 📖 **Reading Order:** Step 11 of 43 | **Module 3:** Problem Solving & Uninformed Search  
> ◄ **Previous:** [[Breadth-First Search Algorithm]] | ► **Next:** [[Depth-First Search and Depth-Limited Search Algorithm]]

---

## Starting Point and the Problem

Breadth-First Search finds the solution with the fewest number of steps. However, in most real-world problems—such as routing a delivery vehicle or optimizing network packets—different actions incur different costs (fuel, distance, time, monetary expense). A path with 3 short steps might cost $3 \times 1 = 3$, while a direct 1-step path costs $10$. BFS would mistakenly select the expensive 1-step path. We require an algorithm that finds the path with the minimum total cost $g(n)$, regardless of step count.

---

## Developing the Idea

Instead of expanding the shallowest node, **Uniform-Cost Search (UCS)** expands the node with the **lowest path cost $g(n)$**.
To achieve this, the frontier is implemented as a **Priority Queue** ordered by path cost $g(n)$.
UCS is essentially Dijkstra's algorithm generalized to search on infinite or dynamically generated state spaces.

```
                  [ Priority Queue Frontier: Ordered by g(n) ]
                                       │
                         Pop node with minimum g(n)
                                       │
                                       ▼
                         ┌───────────────────────────┐
                         │   Is node a Goal State?   │
                         └─────────────┬─────────────┘
                                       │
                      ┌────────────────┴────────────────┐
                   YES│                               NO│
                      ▼                                 ▼
               [ Return Optimal ]              [ Generate Children ]
               [    Solution    ]              [ Insert with g(child) ]
```

---

## Algorithm Specification

```python
def UNIFORM_COST_SEARCH(problem):
    # Root node with path cost g = 0
    node = Node(state=problem.INITIAL_STATE, path_cost=0)
    
    # Priority Queue ordered by path_cost g(n)
    frontier = PriorityQueue(key=lambda n: n.path_cost)
    frontier.push(node)
    explored = set()

    while not frontier.is_empty():
        # Crucial: Pop the node with MINIMUM g(n)
        node = frontier.pop()
        
        # LATE GOAL TEST: Goal test applied ONLY on expansion!
        if problem.GOAL_TEST(node.state):
            return SOLUTION(node)
            
        explored.add(node.state)

        for action in problem.ACTIONS(node.state):
            child = CHILD_NODE(problem, node, action)
            
            if child.state not in explored and child.state not in frontier:
                frontier.push(child)
            elif child.state in frontier:
                # If a cheaper path to an existing frontier state is found:
                if child.path_cost < frontier[child.state].path_cost:
                    frontier.decrease_key(child.state, child)
                    
    return FAILURE
```

---

## How It Works: The Critical Differences from BFS

UCS introduces two non-negotiable design requirements:

### 1. Goal Test on Expansion, NOT on Generation (Late Goal Test)
- In BFS, we test for a goal the instant a child is generated.
- In UCS, we **MUST wait until the goal node is popped from the priority queue (expanded)** before returning.
- *Reason:* A goal state might be generated early via an expensive path. If we returned it immediately, we would return a suboptimal solution. By waiting until it is popped, the priority queue guarantees that all paths with cost $< g(\text{goal})$ have already been explored!

### 2. Path Cost Reduction (Decrease-Key)
- If a newly generated child leads to a state already sitting in the frontier, we must compare their path costs. If the new path is cheaper, the node in the frontier must be updated with the lower cost.

---

## Proof of Optimality

**Theorem:** Uniform-Cost Search is guaranteed to return the optimal solution $C^*$.

*Proof by Contradiction:*
1. Suppose UCS terminates by returning a suboptimal goal node $G_2$ with cost $g(G_2) > C^*$.
2. Let $G$ be an optimal goal node with $g(G) = C^*$.
3. Since a solution to $G$ exists, there must be some unexpanded node $n$ on the optimal path to $G$ currently sitting in the frontier.
4. Because step costs are strictly positive ($c \ge \epsilon > 0$), path costs are non-decreasing along any path:
   $$g(n) \le g(G) = C^*$$
5. Since $G_2$ is suboptimal:
   $$C^* < g(G_2) \implies g(n) < g(G_2)$$
6. Since the priority queue always pops the node with the minimum $g$-value, node $n$ must be popped and expanded before $G_2$ can ever be popped.
7. This holds for every node along the optimal path up to and including $G$ itself.
8. Therefore, $G$ must be popped before $G_2$, contradicting the assumption that $G_2$ was returned. $\blacksquare$

---

## Complexity Analysis

- **Completeness:** Complete provided every step cost is strictly positive: $c(s, a, s') \ge \epsilon > 0$. If zero-cost or negative loops existed, UCS could get trapped in an infinite loop.
- **Time and Space Complexity:**
  Let $C^*$ be the cost of the optimal solution. The algorithm expands all nodes with cost $\le C^*$.
  In the worst case, the effective depth is $\lfloor C^* / \epsilon \rfloor$.
  $$\text{Time} = \text{Space} = O\left(b^{1 + \lfloor C^* / \epsilon \rfloor}\right)$$
  When all step costs are equal ($\epsilon = c$), this reduces exactly to $O(b^d)$ like BFS.

---

## Common Mistakes

- Applying the goal test upon node generation instead of node expansion.
- Allowing step costs of zero or negative numbers without Bellman-Ford protections.

---

## Exam Relevance

- Tracing UCS on a step-cost graph step by step with explicit priority queue contents.
- Providing the rigorous proof of optimality.
- Explaining why $c(s, a, s') \ge \epsilon > 0$ is required for completeness.

---

## Related Concepts

- [[Breadth-First Search Algorithm]]
- [[A-Star Search Algorithm]]
- [[Uninformed Search Strategies]]

---

## Prerequisites

- [[Uninformed Search Strategies]]
- [[Breadth-First Search Algorithm]]

---

## Navigation

◄ **Previous:** [[Breadth-First Search Algorithm]] | ► **Next:** [[Depth-First Search and Depth-Limited Search Algorithm]]
