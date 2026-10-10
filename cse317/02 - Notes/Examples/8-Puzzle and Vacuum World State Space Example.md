---
type: example
course: cse317
status: active
order: 15
---

# 8-Puzzle and Vacuum World State Space Example

> 📖 **Reading Order:** Step 15 of 43 | **Module 3:** Problem Solving & Uninformed Search  
> ◄ **Previous:** [[Bidirectional Search Algorithm]] | ► **Next:** [[Heuristic Functions and Properties]]

---

## Starting Point and the Problem

To understand how abstract state space concepts translate into concrete implementations, we examine two classic benchmark AI toy problems: the **Vacuum Cleaner World** and the **8-Puzzle**. We formulate their 5-tuple state space definitions and calculate their exact state space sizes.

---

## Example 1: The Vacuum Cleaner World

```
                 State 1                      State 2
           ┌─────────┬─────────┐        ┌─────────┬─────────┐
           │[Agent] D│    D    │        │    D    │[Agent] D│
           └─────────┴─────────┘        └─────────┴─────────┘
                 State 3                      State 4
           ┌─────────┬─────────┐        ┌─────────┬─────────┐
           │[Agent]  │    D    │        │         │[Agent] D│
           └─────────┴─────────┘        └─────────┴─────────┘
                 State 5                      State 6
           ┌─────────┬─────────┐        ┌─────────┬─────────┐
           │[Agent] D│         │        │    D    │[Agent]  │
           └─────────┴─────────┘        └─────────┴─────────┘
                 State 7                      State 8
           ┌─────────┬─────────┐        ┌─────────┬─────────┐
           │[Agent]  │         │        │         │[Agent]  │
           └─────────┴─────────┘        └─────────┴─────────┘
```

### Problem Formulation
1. **States:** Determined by two factors:
   - Agent location: Square A or Square B (2 possibilities).
   - Dirt status of Square A: Clean or Dirty (2 possibilities).
   - Dirt status of Square B: Clean or Dirty (2 possibilities).
   $$\text{Total States} = 2 \times 2 \times 2 = 8\text{ discrete states}$$
2. **Initial State:** Any of the 8 states (e.g., State 1: Agent in A, both dirty).
3. **Actions:** `Left`, `Right`, `Suck`.
4. **Transition Model:**
   - `Suck` cleans the current square.
   - `Left` moves agent to A (no-op if already in A).
   - `Right` moves agent to B (no-op if already in B).
5. **Goal Test:** Both squares are Clean (States 7 and 8).
6. **Path Cost:** Each action costs $1$.

---

## Example 2: The 8-Puzzle

The 8-puzzle consists of a $3 \times 3$ board with 8 numbered sliding tiles and one blank space. A tile adjacent to the blank space can slide into the blank.

```
       INITIAL STATE                   GOAL STATE
     ┌───┬───┬───┐                   ┌───┬───┬───┐
     │ 7 │ 2 │ 4 │                   │ 1 │ 2 │ 3 │
     ├───┼───┼───┤                   ├───┼───┼───┤
     │ 5 │   │ 6 │                   │ 4 │ 5 │ 6 │
     ├───┼───┼───┤                   ├───┼───┼───┤
     │ 8 │ 3 │ 1 │                   │ 7 │ 8 │   │
     └───┴───┴───┘                   └───┴───┴───┘
```

### Problem Formulation
1. **States:** A configuration of the numbers $1$ to $8$ and the blank space across the $9$ board cells.
   - *Total Permutations:* $9! = 362,880$.
   - *Reachable State Space:* Parity constraints divide the state space into two disjoint graph components. Exactly half of the permutations are reachable from any initial configuration:
     $$\text{Reachable States} = \frac{9!}{2} = 181,440$$
2. **Initial State:** Any specified reachable board configuration.
3. **Actions:** Moving the blank `Left`, `Right`, `Up`, or `Down` (between 2 and 4 legal actions depending on blank position).
4. **Transition Model:** Swaps the blank with the tile in the designated direction.
5. **Goal Test:** Current board matches the goal configuration.
6. **Path Cost:** Each step costs $1$.

---

## Exam Relevance

- Formulating state representations and action sets for $N$-puzzle generalizations (e.g., 15-puzzle: $\frac{16!}{2} \approx 10^{13}$ reachable states).
- Explaining state space parity constraints.

---

## Related Concepts

- [[Problem-Solving Agents and State Space Formulation]]
- [[Heuristic Functions and Properties]]

---

## Sources

- **Lectures:** [[cse317/01 - Sources/Lectures/MMi/Chap3-ProbSol.ppt|Chap3-ProbSol.ppt]] (Slides 8–14)
- **Textbook:** [[cse317/01 - Sources/Lectures/MMi/Artificial Intelligence A Modern Approach 3rd Edition.pdf|Artificial Intelligence: A Modern Approach (3rd Edition)]], Chapter 3: Solving Problems by Searching (Section 3.2)

---

## Navigation

◄ **Previous:** [[Bidirectional Search Algorithm]] | ► **Next:** [[Heuristic Functions and Properties]]
