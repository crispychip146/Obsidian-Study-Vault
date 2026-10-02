# CSE313 — Topic Map

This map organizes all topics covered in CSE313 (Operating Systems), linking syllabus domains to corresponding conceptual notes, algorithms, formulas, examples, and practice problems.

---

## 1. Operating System Architecture & Kernel Fundamentals

### 1.1 Core Architecture & Protection
- **Core Concepts:**
  - [[Operating System Structures and Functions]] (Kernel role, monolithic vs microkernel architecture, virtual machines)
  - [[Dual-Mode Operation and System Calls]] (User vs Kernel mode, trap handlers, privileged instructions, memory protection)
  - [[Computer Booting and Hardware Abstractions]] (BIOS/UEFI, MBR/GPT, bootstrap sequence, hardware abstraction layers)

---

## 2. Processes, Multiprogramming & Multithreading

### 2.1 Process Execution Model
- **Core Concepts:**
  - [[Process Concepts and Memory Layout]] (Address space: Text, Data, BSS, Heap, Stack)
  - [[Process Lifecycle and State Transitions]] (New, Ready, Running, Waiting, Terminated, dispatch queues)
  - [[Process Control Block and Context Switching]] (PCB structure, CPU register save/restore, switch overhead)
  - [[Process Creation and Termination Operations]] (POSIX `fork()`, `exec()`, `wait()`, `exit()`, process trees)
  - [[Threads and Multithreading Models]] (User vs Kernel threads, Many-to-One, One-to-One, Many-to-Many models)
- **Mathematical Formula:**
  - [[CPU Multiprogramming Utilization Formula]] ($U = 1 - p^n$)
- **Worked Examples:**
  - [[Process Forking and Zombie Orphan Example]] (Memory isolation, zombie termination, orphan reparenting to init)
- **Practice Problems:**
  - [[Problem — Fork Execution Tree and Process Tracing]] (`Q-CSE313-001`: loop and recursive tree expansions)

---

## 3. CPU Scheduling

### 3.1 Scheduling Criteria & Algorithms
- **Core Concept:**
  - [[CPU Scheduling Principles and Criteria]] (CPU/IO burst cycles, preemptive vs non-preemptive scheduling, turnaround, waiting, response times)
- **Algorithms:**
  - [[Batch Scheduling Algorithms]] (FCFS, Non-Preemptive SJF, SRTF)
  - [[Interactive Scheduling Algorithms]] (Round Robin, Priority Scheduling, Multilevel Queue, MLFQ)
- **Mathematical Formula:**
  - [[Scheduling Metrics and Burst Estimation Formulas]] (Exponential smoothing burst prediction: $\tau_{n+1} = \alpha t_n + (1-\alpha)\tau_n$, waiting time formulas)
- **Worked Examples:**
  - [[Comprehensive CPU Scheduling Simulation Example]] (Detailed 4-process comparative Gantt chart trace)
- **Practice Problems:**
  - [[Problem — CPU Scheduling Algorithm Simulation and Gantt Chart]] (`Q-CSE313-002`: 5-process simulation across 5 scheduling disciplines)

---

## 4. Inter-Process Communication & Synchronization

### 4.1 Race Conditions, Hardware Primitives & Software Protocols
- **Core Concepts:**
  - [[Race Conditions and Critical-Section Problem]] (Shared memory hazards, spooler example, 4 criteria for valid mutual exclusion)
  - [[Semaphores and Synchronization Primitives]] (Lost wakeup problem, atomic `wait`/`signal`, counting vs binary semaphores)
  - [[Monitors and Condition Variables]] (Compiler-enforced exclusion, Hoare signal-and-wait vs Mesa signal-and-continue)
  - [[Message Passing and IPC Models]] (Direct vs indirect mailboxes, rendezvous, buffering, UNIX pipes and sockets)
- **Algorithms:**
  - [[Peterson's Algorithm and Hardware Mutual Exclusion]] (Peterson's 2-process protocol, TSL/XCHG hardware atomic instructions, priority inversion)
  - [[Classic Synchronization Solutions]] (Producer-Consumer bounded buffer, Readers-Writers with reader preference, Dining Philosophers, Sleeping Barber)
- **Worked Examples:**
  - [[Producer-Consumer Semaphore Implementation Example]] (Step-by-step state matrix trace and POSIX C implementation)
- **Practice Problems:**
  - [[Problem — Dining Philosophers Deadlock-Free Synchronization]] (`Q-CSE313-003`: Asymmetric ordering, room capacity limiter, and state-based solutions)

---

## 5. Deadlocks

### 5.1 Deadlock Conditions, Avoidance & Detection
- **Core Concepts:**
  - [[Deadlock Fundamentals and Coffman Conditions]] (The 4 Coffman conditions, Ostrich algorithm, Deadlock vs Livelock vs Starvation)
  - [[Resource Allocation Graphs and Deadlock Modeling]] (Holt's bipartite graph, cycle vs deadlock theorems for single/multi-instance systems)
  - [[Deadlock Prevention and Avoidance Strategies]] (Attacking Coffman conditions, linear resource ordering, safe vs unsafe states, resource trajectories)
- **Algorithms:**
  - [[Banker's Algorithm]] (Dijkstra's multi-resource Banker's algorithm, safety verification, and speculative request testing)
  - [[Deadlock Detection and Recovery Algorithms]] (DFS cycle detection with backtracking, matrix reduction, recovery via preemption, rollback, and process abort)
- **Worked Examples:**
  - [[Banker's Algorithm Multi-Resource Step-by-Step Example]] (Exact 4-process, 3-resource simulation from course notes)
  - [[Resource Allocation Graph Cycle Detection Example]] (Exact DFS stack $L$ traversal from nodes $D$ and $R$ from course notes)
- **Practice Problems:**
  - [[Problem — Banker's Algorithm Safe State and Request Granting]] (`Q-CSE313-004`: 5-process, 4-resource safety check and dynamic request granting)
  - [[Problem — Resource Allocation Graph Reduction and Cycle Detection]] (`Q-CSE313-005`: Single-instance DFS cycle detection and multi-instance graph reduction)
