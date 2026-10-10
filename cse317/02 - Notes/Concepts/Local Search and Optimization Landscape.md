---
type: concept
course: cse317
status: active
order: 22
---

# Local Search and Optimization Landscape

> 📖 **Reading Order:** Step 22 of 43 | **Module 5:** Local Search, Optimization & Genetic Algorithms  
> ◄ **Previous:** [[Romania Travel Routing A-Star Search Example]] | ► **Next:** [[Hill-Climbing Search Algorithm]]

---

## Starting Point and the Problem

In systematic search algorithms (such as BFS, UCS, and A*), the path to the goal is the primary output—for example, the sequence of driving turns from Arad to Bucharest. The algorithms systematically maintain frontiers of paths in memory. However, in many critical engineering and AI problems—such as integrated circuit (VLSI) layout, factory production scheduling, network routing, or the 8-Queens problem—the path used to reach the solution is completely irrelevant. Only the **final configuration** of the system matters. In such domains, systematically storing paths in memory is wasteful. **Local Search Algorithms** operate using a complete-state formulation, modifying a single current state (or a small set of states) without retaining the search history.

---

## Developing the Idea

Local search algorithms evaluate states using an **Objective Function** (or *Fitness Function*, to be maximized) or a **Cost / Heuristic Function** (to be minimized). The state space can be visualized as an elevation landscape:

```
            Elevation / Objective Function Value
                              ▲
                              │
                    Global    │
                   Maximum    │
                      ▲       │
                     / \      │
                    /   \     │      Local
                   /     \    │     Maximum
                  /       \   │        ▲
                 /         \  │       / \
                /           \_│______/   \      Plateau / Shoulder
               /              │           \___________
              /               │                       \
    ─────────┴────────────────┼────────────────────────┴────────► State Space
                              │
```

### Landscape Topography Features

1. **Global Maximum:** The absolute highest peak in the entire state space (the optimal solution).
2. **Local Maximum:** A peak that is higher than all its immediate neighbors, but lower than the global maximum. A simple greedy hill-climber will get trapped here indefinitely.
3. **Plateau (Flat Local Maximum):** A completely flat region of the landscape where all neighboring states have the exact same elevation. The search cannot determine which direction leads uphill.
4. **Shoulder:** A flat plateau that eventually resumes ascending.
5. **Ridge:** A narrow, sloping crest where the slope along the ridge is gentle, but the sides drop off sharply. Standard steepest-ascent search oscillates violently across the crest, making painfully slow progress.

---

## Complete-State Formulation

Unlike incremental formulations where a state starts empty and pieces are added one by one, local search uses a **complete-state formulation**:
- Every state in the search space is a complete assignment of values to all components (e.g., all 8 queens are placed on the board from the start).
- Actions modify existing components (e.g., slide queen in column 3 from row 2 to row 6).
- Search terminates when an optimal configuration (zero constraint violations or maximum objective value) is achieved.

---

## Key Advantages of Local Search

1. **Negligible Memory Consumption:** Typically stores only the current state and a few parameters ($O(1)$ space).
2. **Usability in Infinite or Continuous Spaces:** Can navigate continuous multidimensional optimization surfaces using gradients.
3. **Suitability for Pure Optimization:** Excellent for finding reasonable solutions in massive NP-hard combinatorial spaces where systematic search is computationally impossible.

---

## Common Mistakes

- Confusing an incremental formulation (where path matters) with a complete-state formulation.
- Assuming a local maximum can be detected purely by inspecting a single state (requires generating all immediate neighbors to confirm that none is strictly better).

---

## Exam Relevance

- Drawing and labeling the features of a state-space landscape (global max, local max, plateau, shoulder, ridge).
- Explaining why ridges cause severe difficulties for hill-climbing search.

---

## Related Concepts

- [[Hill-Climbing Search Algorithm]]
- [[Simulated Annealing Algorithm]]
- [[Local Beam Search Algorithm]]
- [[Genetic Algorithm]]

---

## Prerequisites

- [[Problem-Solving Agents and State Space Formulation]]

---

## Navigation

◄ **Previous:** [[Romania Travel Routing A-Star Search Example]] | ► **Next:** [[Hill-Climbing Search Algorithm]]
