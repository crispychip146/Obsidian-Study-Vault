# CSE313 — Question Bank

This file catalogs all practice, exam, tutorial, and lecture problems for **CSE313: Operating Systems**, tracking metadata, concepts tested, difficulty, and solution availability.

---

## Question Master Table

| ID | Title | Source | Topic | Question Type | Difficulty | Solution Status | Note Link |
|---|---|---|---|---|---|---|---|
| **Q-CSE313-001** | Fork Execution Tree and Process Tracing | `2. ProcessAndThread-week2-RRR.pdf` (Slides 20–32) | Process Management / POSIX `fork()` | Code Tracing / Tree Modeling | Medium | Solved | [[Problem — Fork Execution Tree and Process Tracing]] |
| **Q-CSE313-002** | CPU Scheduling Simulation and Gantt Chart | `3. Scheduling-week-3-RRR.pdf` (Slides 12–50) | CPU Scheduling Algorithms | Numerical Simulation / Gantt Chart | Medium-Hard | Solved | [[Problem — CPU Scheduling Algorithm Simulation and Gantt Chart]] |
| **Q-CSE313-003** | Dining Philosophers Deadlock-Free Synchronization | `4. IPC-week-4-5-RRR.pptx` (Slides 44–48) | Process Synchronization / Concurrency | Mathematical Proof / State Tracing | Hard | Solved | [[Problem — Dining Philosophers Deadlock-Free Synchronization]] |
| **Q-CSE313-004** | Banker's Algorithm Safe State and Request Granting | `5. Deadlocks-week6-7-RRR.pdf` & `Notes on algorithm simulation.pdf` | Deadlock Avoidance / Banker's Algorithm | Matrix & Vector Simulation | Medium-Hard | Solved | [[Problem — Banker's Algorithm Safe State and Request Granting]] |
| **Q-CSE313-005** | Resource Allocation Graph Reduction and Cycle Detection | `5. Deadlocks-week6-7-RRR.pdf` & `Notes on algorithm simulation.pdf` | Deadlock Detection / RAG Reduction | Graph Traversal / DFS Cycle Tracing | Medium | Solved | [[Problem — Resource Allocation Graph Reduction and Cycle Detection]] |

---

## Detailed Question Summaries

### Q-CSE313-001: Fork Execution Tree and Process Tracing
- **Problem Summary:** Traces execution across 4 distinct POSIX C code snippets: sequential forks, loop-based forking with PID condition checks, child-only branching, and shared memory isolation verification.
- **Concepts Tested:** [[Process Creation and Termination Operations]], [[Process Concepts and Memory Layout]], [[Process Lifecycle and State Transitions]]
- **Key Insight:** `fork()` creates an exact duplicate virtual address space; modifications to variables in child processes never alter parent variables. $k$ sequential forks produce $2^k$ total processes.
- **Detailed Note:** [[Problem — Fork Execution Tree and Process Tracing]]

---

### Q-CSE313-002: CPU Scheduling Simulation and Gantt Chart
- **Problem Summary:** Given a workload of 5 processes with arrival times, burst times, and priorities, construct execution Gantt charts and compute Turnaround, Waiting, and Response times under FCFS, SJF, SRTF, Preemptive Priority, and Round Robin ($q=3\text{ ms}$). Analyzes quantum sizing trade-offs.
- **Concepts Tested:** [[CPU Scheduling Principles and Criteria]], [[Batch Scheduling Algorithms]], [[Interactive Scheduling Algorithms]], [[Scheduling Metrics and Burst Estimation Formulas]]
- **Key Insight:** SRTF achieves optimal average turnaround time ($8.80\text{ ms}$) and waiting time ($4.40\text{ ms}$); Round Robin provides fair responsiveness ($4.40\text{ ms}$ response time) without starvation.
- **Detailed Note:** [[Problem — CPU Scheduling Algorithm Simulation and Gantt Chart]]

---

### Q-CSE313-003: Dining Philosophers Deadlock-Free Synchronization
- **Problem Summary:** Evaluates 3 distinct deadlock-free strategies for the 5-philosopher dining problem: asymmetric odd/even chopstick ordering, room capacity limiting semaphore ($room=4$), and Tanenbaum's state-based two-fork atomic acquisition. Traces chronological state progression and proves deadlock freedom.
- **Concepts Tested:** [[Race Conditions and Critical-Section Problem]], [[Semaphores and Synchronization Primitives]], [[Classic Synchronization Solutions]]
- **Key Insight:** Asymmetry breaks the circular wait condition. The room semaphore leverages the Pigeonhole Principle ($4 < 5$). Tanenbaum's state model ensures a philosopher only eats if neither neighbor is eating.
- **Detailed Note:** [[Problem — Dining Philosophers Deadlock-Free Synchronization]]

---

### Q-CSE313-004: Banker's Algorithm Safe State and Request Granting
- **Problem Summary:** In a 5-process, 4-resource system ($A, B, C, D$), computes the Available vector and Need matrix. Verifies that the initial state is safe with sequence $\langle P_0, P_2, P_1, P_3, P_4 \rangle$. Evaluates dynamic requests: immediately grants $Request_1 = (1, 1, 0, 0)$ after speculative safety verification, but blocks $Request_4 = (1, 2, 0, 1)$ due to insufficient available units of Resource $B$.
- **Concepts Tested:** [[Deadlock Fundamentals and Coffman Conditions]], [[Deadlock Prevention and Avoidance Strategies]], [[Banker's Algorithm]]
- **Key Insight:** Speculative allocation must pass the safety check before resources are permanently granted to guarantee the OS never enters an unsafe state.
- **Detailed Note:** [[Problem — Banker's Algorithm Safe State and Request Granting]]

---

### Q-CSE313-005: Resource Allocation Graph Reduction and Cycle Detection
- **Problem Summary:** Solves two distinct deadlock detection problems: traces the course DFS cycle detection algorithm with backtracking on a single-instance graph from nodes $P_1$ and $R_4$, detecting cycle $P_1 \to R_1 \to P_2 \to R_2 \to P_3 \to R_3 \to P_1$. Simulates step-by-step graph reduction on a multi-instance graph, proving that despite containing a directed cycle, the system is completely reducible and deadlock-free.
- **Concepts Tested:** [[Resource Allocation Graphs and Deadlock Modeling]], [[Deadlock Prevention and Avoidance Strategies]], [[Deadlock Detection and Recovery Algorithms]]
- **Key Insight:** In single-instance systems, cycle $\iff$ deadlock. In multi-instance systems, a cycle is necessary but not sufficient for deadlock.
- **Detailed Note:** [[Problem — Resource Allocation Graph Reduction and Cycle Detection]]
