# CSE 313 — Operating Systems

> **Credits:** 3.0 | **Contact Hours:** 3 hours/week  
> **Course Scope:** Operating system architecture, dual-mode protection, process management, CPU scheduling algorithms, inter-process communication, concurrency synchronization, and deadlock prevention, avoidance, and detection.

---

## 🗺️ Master Sequential Reading Roadmap (Steps 01 – 34)

Read these notes in strict chronological order to build cumulative mastery from bare-metal architecture to complex deadlock avoidance:

### Module 1: OS Architecture & Kernel Fundamentals
- [ ] **Step 01 (Concept):** [[Operating System Structures and Functions]] — Role of the kernel, system call interface, monolithic vs microkernel architecture.
- [ ] **Step 02 (Concept):** [[Dual-Mode Operation and System Calls]] — User vs Kernel mode, trap handlers, privileged instructions, and architectural protection.
- [ ] **Step 03 (Concept):** [[Computer Booting and Hardware Abstractions]] — BIOS/UEFI, MBR/GPT, bootstrap loader sequence, and hardware virtualization layers.

### Module 2: Processes, Multiprogramming & Threads
- [ ] **Step 04 (Concept):** [[Process Concepts and Memory Layout]] — Address space segmentation: Text, Data, BSS, Heap, and Stack.
- [ ] **Step 05 (Concept):** [[Process Lifecycle and State Transitions]] — 5-state lifecycle model (New, Ready, Running, Waiting, Terminated) and dispatching queues.
- [ ] **Step 06 (Concept):** [[Process Control Block and Context Switching]] — PCB anatomy, register save/restore, and hardware context switch costs.
- [ ] **Step 07 (Concept):** [[Process Creation and Termination Operations]] — POSIX `fork()`, `exec()`, `wait()`, `exit()`, process trees, and init/systemd.
- [ ] **Step 08 (Concept):** [[Threads and Multithreading Models]] — User-level vs Kernel-level threads, Many-to-One, One-to-One, and Many-to-Many models.
- [ ] **Step 09 (Formula):** [[CPU Multiprogramming Utilization Formula]] — Mathematical CPU utilization model: $U = 1 - p^n$.
- [ ] **Step 10 (Example):** [[Process Forking and Zombie Orphan Example]] — Tracing PID return values, memory isolation, zombie state creation, and orphan reparenting.
- [ ] **Step 11 (Problem):** [[Problem — Fork Execution Tree and Process Tracing]] — Tracing sequential forks, loops, and tree expansions (`Q-CSE313-001`).

### Module 3: CPU Scheduling
- [ ] **Step 12 (Concept):** [[CPU Scheduling Principles and Criteria]] — CPU/IO burst cycles, preemptive vs non-preemptive scheduling, throughput, turnaround, waiting, and response times.
- [ ] **Step 13 (Algorithm):** [[Batch Scheduling Algorithms]] — FCFS (convoy effect), Non-Preemptive SJF, and SRTF (preemptive SJF).
- [ ] **Step 14 (Algorithm):** [[Interactive Scheduling Algorithms]] — Round Robin, Priority Scheduling, Multilevel Queue, and MLFQ.
- [ ] **Step 15 (Formula):** [[Scheduling Metrics and Burst Estimation Formulas]] — Exponential smoothing $\tau_{n+1} = \alpha t_n + (1-\alpha)\tau_n$ and metrics formulas.
- [ ] **Step 16 (Example):** [[Comprehensive CPU Scheduling Simulation Example]] — Comparative 4-process simulation with ASCII Gantt charts.
- [ ] **Step 17 (Problem):** [[Problem — CPU Scheduling Algorithm Simulation and Gantt Chart]] — Comprehensive 5-process simulation across 5 scheduling algorithms (`Q-CSE313-002`).

### Module 4: Inter-Process Communication & Synchronization
- [ ] **Step 18 (Concept):** [[Race Conditions and Critical-Section Problem]] — Shared memory hazards, spooler example, and the 4 criteria for valid mutual exclusion.
- [ ] **Step 19 (Algorithm):** [[Peterson's Algorithm and Hardware Mutual Exclusion]] — Peterson's 2-process protocol, proofs, TSL/XCHG instructions, and priority inversion.
- [ ] **Step 20 (Concept):** [[Semaphores and Synchronization Primitives]] — Lost wakeup problem, Dijkstra's semaphore definition, atomic wait/signal, and POSIX mutexes.
- [ ] **Step 21 (Concept):** [[Monitors and Condition Variables]] — Encapsulated language monitors, condition queues, Hoare (signal-and-wait) vs Mesa (signal-and-continue) semantics.
- [ ] **Step 22 (Concept):** [[Message Passing and IPC Models]] — Direct vs indirect mailboxes, synchronous rendezvous vs asynchronous buffering, UNIX pipes and sockets.
- [ ] **Step 23 (Algorithm):** [[Classic Synchronization Solutions]] — Bounded buffer, Readers-Writers (starvation analysis), Dining Philosophers, and Sleeping Barber.
- [ ] **Step 24 (Example):** [[Producer-Consumer Semaphore Implementation Example]] — Chronological 6-step state trace, full POSIX C implementation, and semaphore inversion bug.
- [ ] **Step 25 (Problem):** [[Problem — Dining Philosophers Deadlock-Free Synchronization]] — Asymmetric ordering, room capacity semaphores, and Tanenbaum's state tracing (`Q-CSE313-003`).

### Module 5: Deadlocks
- [ ] **Step 26 (Concept):** [[Deadlock Fundamentals and Coffman Conditions]] — The 4 Coffman conditions, Ostrich algorithm, Deadlock vs Livelock vs Starvation.
- [ ] **Step 27 (Concept):** [[Resource Allocation Graphs and Deadlock Modeling]] — Holt's bipartite graph, cycle vs deadlock theorems (single vs multiple instances), and graph reduction.
- [ ] **Step 28 (Concept):** [[Deadlock Prevention and Avoidance Strategies]] — Attacking Coffman conditions, linear resource ordering, safe vs unsafe states, and resource trajectories.
- [ ] **Step 29 (Algorithm):** [[Banker's Algorithm]] — Dijkstra's multi-resource Banker's algorithm, safety verification, and speculative request evaluation.
- [ ] **Step 30 (Algorithm):** [[Deadlock Detection and Recovery Algorithms]] — DFS cycle detection with backtracking, matrix reduction, and recovery via preemption, rollback, and termination.
- [ ] **Step 31 (Example):** [[Banker's Algorithm Multi-Resource Step-by-Step Example]] — Step-by-step 4-process, 3-resource numerical simulation from course lecture notes.
- [ ] **Step 32 (Example):** [[Resource Allocation Graph Cycle Detection Example]] — Tracing DFS stack $L$ from nodes $D$ and $R$ from course simulation lecture notes.
- [ ] **Step 33 (Problem):** [[Problem — Banker's Algorithm Safe State and Request Granting]] — 5-process, 4-resource safety and immediate request evaluation (`Q-CSE313-004`).
- [ ] **Step 34 (Problem):** [[Problem — Resource Allocation Graph Reduction and Cycle Detection]] — Single-instance DFS cycle detection and multi-instance graph reduction (`Q-CSE313-005`).

---

## 📚 Course Sources

The notes are derived with strict traceability from the following primary course lectures:
- `SRC-CSE313-001`: `1. Introduction-week1-RRR-2026.pdf` (Introduction, OS Structures, Dual-Mode, Booting)
- `SRC-CSE313-002`: `2. ProcessAndThread-week2-RRR.pdf` (Processes, PCB, Context Switch, Threads, Multiprogramming)
- `SRC-CSE313-003`: `3. Scheduling-week-3-RRR.pdf` (CPU Scheduling Criteria, FCFS, SJF, SRTF, RR, Priority, MLFQ)
- `SRC-CSE313-004`: `4. IPC-week-4-5-RRR.pptx` (Race Conditions, Peterson's, Semaphores, Monitors, Classic IPC Problems)
- `SRC-CSE313-005`: `5. Deadlocks-week6-7-RRR.pdf` (Coffman Conditions, RAG, Banker's Algorithm, Detection & Recovery)
- `SRC-CSE313-006`: `Notes on algorithm simulation.pdf` (Banker's Algorithm & RAG Cycle Detection Simulation Steps)

---

## 🎯 Testing & Practice

- **Question Bank:** [[05 - Testing/Question Bank/Question Bank]]
  - `Q-CSE313-001`: [[Problem — Fork Execution Tree and Process Tracing]]
  - `Q-CSE313-002`: [[Problem — CPU Scheduling Algorithm Simulation and Gantt Chart]]
  - `Q-CSE313-003`: [[Problem — Dining Philosophers Deadlock-Free Synchronization]]
  - `Q-CSE313-004`: [[Problem — Banker's Algorithm Safe State and Request Granting]]
  - `Q-CSE313-005`: [[Problem — Resource Allocation Graph Reduction and Cycle Detection]]

---

## 🗺️ Visual Maps & Navigation

- **Topic Map:** [[03 - Maps/Topic Map]]
- **Dependency Map:** [[03 - Maps/Dependency Map]]
