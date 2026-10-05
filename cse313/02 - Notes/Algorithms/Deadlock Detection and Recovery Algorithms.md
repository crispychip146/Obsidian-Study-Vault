---
type: algorithm
course: cse313
status: active
order: 30
---

# Deadlock Detection and Recovery Algorithms

> 📖 **Reading Order:** Step 30 of 34 | **Module 5:** Deadlocks  
> ◄ **Previous:** [[Banker's Algorithm]] | ► **Next:** [[Banker's Algorithm Multi-Resource Step-by-Step Example]]

---

> [!IMPORTANT] **Exam practice references (Appeared in 2017 Q3c, 2019 Q3b, 2020 Q3a, 2020 Q3c)**
>
> ### Practice tasks and reasoning:
> 1. **Executing DFS Cycle Detection on Resource Allocation Graphs (2017 Q3c, 2020 Q3a):**
>    - **The Setup:** Given a list of directed edges between Processes ($A, B, C, D$) and Resources ($1, 2, \dots, 6$). Run the cycle-detection algorithm starting from a specified node.
>    - **The tracing method:**
>      - Maintain a visited/active path stack. Follow outgoing edges directed from process to requested resource ($P \to R$) and from allocated resource to holder ($R \to P$).
>      - **2017 Q3(c) Case (Start at B):** $B \to 1 \to A$. $A$ has no outgoing edges (out-degree 0). Backtrack. Output: **"No cycle found from node B."**
>      - **2020 Q3(a) Case (Start at C):** $C \to 5 \to D \to 6 \to A \to 1 \to B \to 4 \to C$. A back-edge to ancestor node $C$ is encountered! Output: **"Cycle detected: $C \to 5 \to D \to 6 \to A \to 1 \to B \to 4 \to C$. System is in Deadlock!"**
> 2. **Can a Process Be Deadlocked Without Being in a Cycle? (2019 Q3b & 2020 Q3c verbatim):**
>    - **The Answer:** **YES.**
>    - **The Proof & Example:** Suppose processes $P_1$ and $P_2$ form a circular deadlock over single-instance resources $R_1$ and $R_2$ ($P_1 \to R_2 \to P_2 \to R_1 \to P_1$). If a third process $P_3$ now requests resource $R_1$, $P_3$ will block indefinitely waiting for $R_1$. Since $P_1$ will never release $R_1$, $P_3$ is permanently deadlocked, despite having no directed path leading back to $P_3$.

---

## Building the idea

Detection asks whether the **current** outstanding requests can resolve, rather than whether every process could still receive its declared maximum. This separates it from [[Banker's Algorithm]]. In a request-matrix test, use Request, Allocation, and Available, not Max-based Need.

For one-instance resources, follow the wait-for graph with DFS. Mark nodes on the current recursion path. An edge to a node still on that path closes a directed cycle. An edge to a previously finished node does not: it may simply merge two paths. A visited set alone cannot distinguish these cases.

For multiple instances, search for a process whose present request fits the hypothetical Work vector. Assume it finishes and returns its allocation, then repeat. Processes left unable to finish are examined for deadlock under the detection model. This is the same resource-release logic used in safety testing, applied to a different request meaning.

Recovery then changes the real system: abort a process, preempt a reclaimable resource, or roll back work where supported. Detection identifies the trap; it does not automatically choose a recovery action without costs or consistency consequences.

## Inputs

- For Single-Instance: Wait-For Graph $G = (V, E)$.
- For Multiple-Instance: $A$ (Available vector), $CA$ (Allocation matrix), $Q$ (Request matrix).

---

## Outputs

- Set of deadlocked processes: $D = \{P_{d1}, P_{d2}, \dots\}$ (or empty set if deadlock-free).
- Recovery action plan (target process to abort or rollback).

---

## How It Works

### 4. Invocation Timing Strategies

Running the detection algorithm is computationally expensive. Operating systems schedule detection based on three policies:

| Policy | Trigger Event | Advantage | Disadvantage |
|---|---|---|---|
| **Continuous (Per-Request)** | Invoked every time a resource request cannot be granted immediately. | Deadlocks are detected the exact instant they form; fewer processes involved. | Enormous CPU overhead; degrades throughput. |
| **Periodic Interval** | Invoked every $K$ minutes (e.g., every 30 minutes). | Predictable CPU overhead. | Deadlocked processes remain frozen until the next timer tick. |
| **Utilization-Based** | Invoked when overall CPU utilization drops below a threshold (e.g., $< 20\%$). | Blocked resource waits can reduce runnable work, though other tasks may keep the CPU busy and some waiting protocols spin. | Indirect indicator; low utilization could just mean a quiet workload. |

---

### 5. Recovery from Deadlock

Once a deadlock is detected, the OS must break the circular wait using one of three recovery mechanisms:

### 1. Recovery Through Preemption
- The OS forcibly reclaims an allocated resource from one process and hands it to another.
- *Feasibility:* Highly restricted. Feasible for CPU registers or memory; impossible for active database write locks or hardware burning tasks without causing data corruption.

### 2. Recovery Through Rollback
- The OS periodically creates a **checkpoint** (saving process memory image, registers, file pointers) to disk.
- When deadlock occurs, the OS rolls back a participating process to a checkpoint saved *prior* to its resource acquisition, freeing its resources for other deadlocked tasks.

### 3. Recovery Through Process Termination
- **Option A (Total Abort):** Kill all deadlocked processes. Guaranteed to break deadlock, but causes massive computational waste.
- **Option B (Incremental Abort):** Abort one process at a time and re-run the detection algorithm until the deadlock cycle dissolves.
  - **Victim Selection Metrics:**
    1. Priority of the process (terminate lower-priority processes first).
    2. Computational time spent vs remaining time to finish.
    3. Number and type of resources held (terminate the process holding the most contested resources).
    4. Interactive vs Batch (kill batch jobs before disrupting interactive users).

---

## Pseudocode

### 1. Algorithmic Overview & Motivation

In systems where neither static prevention nor dynamic avoidance is enforced, the OS permits processes to request and acquire resources freely. However, to prevent permanent system freezes, the operating system must:
1. **Detect** whether a deadlock has occurred.
2. **Recover** from the deadlock by breaking the circular wait.

---

## Example

Matrix detection: Available $A = [0, 0, 0]$. If $P_1$ requests $[0, 0, 0]$, it finishes and releases $[0, 1, 0]$. This unlocks $P_2$, which finishes. If no process can be unlocked, remaining unmarked processes are deadlocked.

---

## Complexity

### Time Complexity
$O(V+E)$ for conventional adjacency-list DFS cycle detection in a WFG; $O(m \times n^2)$ for multi-instance matrix reduction.

### Space Complexity
$O(V + E)$ or $O(m \times n)$ memory.

---

## Properties

- **Snapshot requirement:** Analyze a consistent graph/matrix snapshot. A DFS cycle establishes cyclic waiting for single-instance resources; identifying every indefinitely blocked dependent requires more than stopping at the first cycle.
- **Victim Selection Cost:** Recovery must balance process priority, computation time already spent, resources held, and number of rollbacks.

---

## Limitations

- Checkpointing and rollbacks incur significant disk I/O overhead.
- Terminating processes can corrupt shared databases or leave temporary files inconsistent.

---

## What to carry forward

Keep algorithm state separate from system state. DFS colors and Work vectors are reasoning tools; recovery mutates the actual allocations or executions. [[Resource Allocation Graph Cycle Detection Example]] shows path tracking, while [[Problem — Resource Allocation Graph Reduction and Cycle Detection]] contrasts the single- and multi-instance conclusions.

## Related notes

- [[Banker's Algorithm]]
- [[Resource Allocation Graph Cycle Detection Example]]
- [[Problem — Resource Allocation Graph Reduction and Cycle Detection]]

## Detection algorithms

When each resource class has only one instance, the Resource Allocation Graph can be analyzed using a Depth-First Search (DFS) cycle-detection algorithm with backtracking.

### Formal Algorithm (Tanenbaum Specification):
For each node $N$ in the graph:
1. Initialize a search list $L = \emptyset$. Clear all edge marks.
2. Designate current node $CN = N$. Add $CN$ to list $L$.
3. Check if $CN$ appears more than once in $L$:
   - If **yes**: A cycle is detected! A potential deadlock exists. Output the cycle and terminate.
4. From $CN$, check for any **unmarked outgoing edges**:
   - If an unmarked outgoing edge $(CN, X)$ exists:
     - Mark the edge.
     - Set $CN = X$.
     - Add $X$ to $L$.
     - Go to Step 3.
   - If **no unmarked outgoing edges** exist from $CN$:
     - If $CN == N$ (the initial starting node): Stop. No cycle exists from node $N$.
     - Else (Dead-End Encountered): **Backtrack** to the previous node in $L$, remove $CN$ from $L$, set $CN$ to the predecessor node, and go to Step 4.

---

When resource types have multiple instances, cycles in the graph do not guarantee a deadlock. We must evaluate whether currently pending requests can be fulfilled using an algorithm closely related to the Banker's safety check.

### Data Structures:
- $E = (e_1, e_2, \dots, e_m)$: Total Existing Resource vector.
- $A = (a_1, a_2, \dots, a_m)$: Current Available Resource vector ($A = E - \sum C_i$).
- $C$: Current Allocation Matrix ($n \times m$), where $C_{i,j}$ is instances of $R_j$ held by $P_i$.
- $R$: Current Pending Request Matrix ($n \times m$), where $R_{i,j}$ is instances of $R_j$ currently requested by $P_i$.

```
Algorithmic Steps:
1. Initialize Work vector W = A.
   Mark all processes Pi whose allocation row C_i == 0 (they hold no resources).

2. Look for an unmarked process row Pi such that:
         R_i <= W
   (Process Pi's current pending request can be completely met by available resources).

3. If such a process Pi is found:
         W = W + C_i    // Pi finishes, releasing its allocated resources
         Mark process Pi
         Go to Step 2.

4. If no such process can be found:
         Algorithm terminates.

Conclusion:
   If all processes are marked -> System is NOT deadlocked.
   Any UNMARKED processes are CURRENTLY DEADLOCKED!
```

---

## Sources

- **Source Material:** `5. Deadlocks-week6-7-RRR.pdf` (Slides 16–24: Deadlock Detection with One and Multiple Resources, Recovery Methods) and `Notes on algorithm simulation.pdf`.
- **Previous Topic:** [[Banker's Algorithm]] (Step 29).
- **Next Topic:** [[Banker's Algorithm Multi-Resource Step-by-Step Example]] (Step 31).
