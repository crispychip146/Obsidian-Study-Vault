---
type: concept
course: cse317
status: active
order: 8
---

# Problem-Solving Agents and State Space Formulation

> 📖 **Reading Order:** Step 08 of 43 | **Module 3:** Problem Solving & Uninformed Search  
> ◄ **Previous:** [[Agentic AI and Autonomous Systems]] | ► **Next:** [[Uninformed Search Strategies]]

---

## Starting Point and the Problem

Reflex agents act based solely on immediate percepts or internal state mappings. However, when an agent must solve a complex task—such as navigating a road network from Arad to Bucharest or arranging tiles in an 8-puzzle—no single immediate reflex action is guaranteed to lead to the goal. The agent cannot simply react; it must consider a sequence of actions that form a path to a desirable state. To do this, an agent must formulate a **goal**, formulate a well-defined **problem**, and perform **search** before executing any physical action.

---

## Developing the Idea

A problem-solving agent follows a distinct four-phase lifecycle:
1. **Goal Formulation:** Decide what world states are desirable based on the current situation and performance measure. Goals organize behavior by limiting the objectives the agent tries to achieve.
2. **Problem Formulation:** Decide what actions and states to consider, abstracting away unnecessary real-world details.
3. **Search:** Computationally simulate sequences of actions in an internal model of the world until a path reaching a goal state is discovered.
4. **Execution:** Carry out the actions in the discovered solution path one by one in the real physical environment.

```
                  ┌──────────────────────┐
                  │   Perceive Percept   │
                  └──────────┬───────────┘
                             │
                             ▼
                  ┌──────────────────────┐
                  │   Formulate Goal     │
                  └──────────┬───────────┘
                             │
                             ▼
                  ┌──────────────────────┐
                  │  Formulate Problem   │
                  │  (State-Space Model) │
                  └──────────┬───────────┘
                             │
                             ▼
                  ┌──────────────────────┐
                  │    Offline Search    │
                  │ (Find Solution Path) │
                  └──────────┬───────────┘
                             │
                             ▼
                  ┌──────────────────────┐
                  │   Execute Solution   │
                  │    (Act in World)    │
                  └──────────────────────┘
```

---

## Definition

A well-defined **Search Problem** is formally defined by five components:

1. **Initial State ($s_0$):** The state the agent starts in.
2. **Actions ($A(s)$):** A description of the legal actions available to the agent when in state $s$.
3. **Transition Model ($\text{Result}(s, a)$):** A formal function returning the state $s'$ that results from executing action $a$ in state $s$.
4. **Goal Test ($G(s)$):** A function or explicit set of states that determines whether a given state $s$ is a goal state.
5. **Path Cost ($c(s, a, s')$):** A function that assigns a numeric cost to taking action $a$ in state $s$ to reach $s'$. The step cost is denoted $c(s, a, s') \ge 0$. The path cost $g(n)$ is the sum of step costs along the path.

A **Solution** is an action sequence that leads from the initial state to a goal state. An **Optimal Solution** has the lowest path cost among all solutions.

---

## State Space vs. Search Tree vs. Search Graph

It is vital to distinguish between the abstract state space and the data structures used by search algorithms:

```
        STATE SPACE (Graph)                         SEARCH TREE (Data Structure)
         ┌───┐       ┌───┐                                      ┌───┐
         │ A │◄─────►│ B │                                      │ A │ (Root)
         └───┘       └───┘                                     ┌┴───┴┐
           ▲           ▲                                       ▼     ▼
           │           │                                     ┌───┐ ┌───┐
           ▼           ▼                                     │ B │ │ C │
         ┌───┐       ┌───┐                                   └───┘ └───┘
         │ C │◄─────►│ D │                                  ┌──┴──┐
         └───┘       └───┘                                  ▼     ▼
     (Finite: 4 distinct states)                          ┌───┐ ┌───┐
                                                          │ A │ │ D │  (Infinite paths if
                                                          └───┘ └───┘   loops not checked!)
```

1. **State Space:** The set of all valid configurations of the world. It is typically a directed graph where nodes are world states and directed edges are legal actions.
2. **Search Tree:** A mathematical tree generated dynamically during the search process. The root is the initial state; branches are actions; nodes represent paths through the state space.
   - *Node Structure:* A search node $n$ is a concrete data structure containing:
     - `n.STATE`: The state in the state space to which the node corresponds.
     - `n.PARENT`: The node in the search tree that generated this node.
     - `n.ACTION`: The action applied to the parent to generate this node.
     - `n.PATH-COST` ($g(n)$): The total cost of the path from the root to $n$.
     - `n.DEPTH`: The number of steps along the path from the root.
3. **Redundant Paths and Loopy Paths:**
   - In any state space with reversible actions (e.g., $A \to B \to A$), a naive tree search will generate an infinite search tree.
   - **Tree Search:** Does not remember previously explored states; vulnerable to infinite loops.
   - **Graph Search:** Maintains an **Explored Set** (or *Closed List*) of all states expanded so far. If a newly generated state has already been explored or is in the frontier, the redundant path is discarded.

---

## How It Works: The General Graph Search Infrastructure

```
function GRAPH-SEARCH(problem):
    node = a node with STATE = problem.INITIAL-STATE, PATH-COST = 0
    frontier = a queue containing node (e.g., FIFO, LIFO, or Priority Queue)
    explored = an empty set

    loop:
        if IS-EMPTY(frontier) then return failure
        node = POP(frontier)
        if problem.GOAL-TEST(node.STATE) returns true then
            return SOLUTION(node)
        add node.STATE to explored
        for each action in problem.ACTIONS(node.STATE):
            child = CHILD-NODE(problem, node, action)
            if child.STATE is not in explored or frontier then
                INSERT(child, frontier)
            else if child.STATE is in frontier with higher PATH-COST then
                replace that frontier node with child
```

---

## Common Mistakes

- Confusing a **State** with a **Node**:
  - A *State* represents a physical configuration of the world (e.g., "In Bucharest").
  - A *Node* is a data structure within the search tree containing a state, parent pointer, action, path cost $g(n)$, and depth. Two distinct nodes can contain the exact same state (representing different paths to that state).
- Confusing **Tree Search** with **Graph Search**: Tree search uses less memory because it does not store the explored set, but it can run forever on loopy state spaces.

---

## Exam Relevance

- Formally defining the 5-tuple of a search problem for a given real-world puzzle.
- Explaining the components of a search node data structure.
- Explaining the difference between tree search and graph search.

---

## Related Concepts

- [[Uninformed Search Strategies]]
- [[Breadth-First Search Algorithm]]
- [[Uniform-Cost Search Algorithm]]
- [[8-Puzzle and Vacuum World State Space Example]]

---

## Prerequisites

- [[Intelligent Agents and Rationality]]

---

## Navigation

◄ **Previous:** [[Agentic AI and Autonomous Systems]] | ► **Next:** [[Uninformed Search Strategies]]
