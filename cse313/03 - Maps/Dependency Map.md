# CSE313 — Dependency Map

This map visualizes the conceptual prerequisites, algorithmic foundations, and learning pathways across all five operating systems modules.

---

## 1. High-Level Modular Learning Flow

```mermaid
flowchart TD
    M1["Module 1: OS Architecture & Kernel Fundamentals<br/>(Steps 01 - 03)"]
    M2["Module 2: Processes, Multiprogramming & Threads<br/>(Steps 04 - 11)"]
    M3["Module 3: CPU Scheduling<br/>(Steps 12 - 17)"]
    M4["Module 4: IPC & Synchronization<br/>(Steps 18 - 25)"]
    M5["Module 5: Deadlocks<br/>(Steps 26 - 34)"]

    M1 --> M2
    M2 --> M3
    M2 --> M4
    M3 --> M4
    M4 --> M5
```

---

## 2. Detailed Topic Dependency Graph

```mermaid
flowchart TD
    %% Module 1
    OS["Operating System Structures & Functions"] --> DM["Dual-Mode Operation & System Calls"]
    DM --> BOOT["Computer Booting & Hardware Abstractions"]

    %% Module 2
    DM --> PROC["Process Concepts & Memory Layout"]
    PROC --> LIFE["Process Lifecycle & State Transitions"]
    LIFE --> PCB["Process Control Block & Context Switching"]
    PCB --> OPS["Process Creation & Termination (fork/exec)"]
    OPS --> THR["Threads & Multithreading Models"]
    LIFE --> FORM_UTIL["CPU Multiprogramming Formula"]
    OPS --> EX_FORK["Process Forking & Zombie Example"]
    EX_FORK --> PROB_FORK["Problem — Fork Tree Tracing (Q-001)"]

    %% Module 3
    LIFE --> SCHED_CRIT["CPU Scheduling Principles & Criteria"]
    SCHED_CRIT --> BATCH["Batch Scheduling (FCFS, SJF, SRTF)"]
    SCHED_CRIT --> INTER["Interactive Scheduling (RR, Priority, MLFQ)"]
    BATCH --> FORM_BURST["Scheduling Metrics & Burst Formulas"]
    INTER --> FORM_BURST
    FORM_BURST --> EX_SCHED["CPU Scheduling Simulation Example"]
    EX_SCHED --> PROB_SCHED["Problem — CPU Scheduling & Gantt (Q-002)"]

    %% Module 4
    PCB --> RACE["Race Conditions & Critical Section Problem"]
    THR --> RACE
    RACE --> PETERSON["Peterson's Algorithm & TSL/XCHG"]
    RACE --> SEM["Semaphores & Synchronization Primitives"]
    SEM --> MON["Monitors & Condition Variables"]
    SEM --> MSG["Message Passing & IPC Models"]
    SEM --> CLASSIC["Classic Synchronization Solutions"]
    CLASSIC --> EX_PROD["Producer-Consumer Semaphore Example"]
    CLASSIC --> PROB_PHIL["Problem — Dining Philosophers (Q-003)"]

    %% Module 5
    RACE --> DEAD_FUND["Deadlock Fundamentals & Coffman Conditions"]
    CLASSIC --> DEAD_FUND
    DEAD_FUND --> RAG["Resource Allocation Graphs & Modeling"]
    DEAD_FUND --> PREV["Deadlock Prevention & Avoidance"]
    PREV --> BANKER["Banker's Algorithm"]
    RAG --> DETECT["Deadlock Detection & Recovery"]
    BANKER --> EX_BANK["Banker's Algorithm Simulation Example"]
    DETECT --> EX_RAG["RAG Cycle Detection DFS Example"]
    EX_BANK --> PROB_BANK["Problem — Banker's Algorithm (Q-004)"]
    EX_RAG --> PROB_RAG["Problem — RAG Reduction & Cycles (Q-005)"]
```

---

## 3. Prerequisite Matrix

| Knowledge Note | Direct Prerequisites | Unlocks / Enables |
|---|---|---|
| [[Dual-Mode Operation and System Calls]] | [[Operating System Structures and Functions]] | [[Process Concepts and Memory Layout]], [[Process Control Block and Context Switching]] |
| [[Process Lifecycle and State Transitions]] | [[Process Concepts and Memory Layout]] | [[CPU Scheduling Principles and Criteria]], [[Process Control Block and Context Switching]] |
| [[Threads and Multithreading Models]] | [[Process Concepts and Memory Layout]] | [[Race Conditions and Critical-Section Problem]] |
| [[CPU Scheduling Principles and Criteria]] | [[Process Lifecycle and State Transitions]] | [[Batch Scheduling Algorithms]], [[Interactive Scheduling Algorithms]] |
| [[Race Conditions and Critical-Section Problem]] | [[Threads and Multithreading Models]], [[Process Control Block and Context Switching]] | [[Peterson's Algorithm and Hardware Mutual Exclusion]], [[Semaphores and Synchronization Primitives]] |
| [[Semaphores and Synchronization Primitives]] | [[Race Conditions and Critical-Section Problem]] | [[Monitors and Condition Variables]], [[Classic Synchronization Solutions]] |
| [[Deadlock Fundamentals and Coffman Conditions]] | [[Semaphores and Synchronization Primitives]], [[Classic Synchronization Solutions]] | [[Resource Allocation Graphs and Deadlock Modeling]], [[Deadlock Prevention and Avoidance Strategies]] |
| [[Banker's Algorithm]] | [[Deadlock Prevention and Avoidance Strategies]] | [[Banker's Algorithm Multi-Resource Step-by-Step Example]], [[Problem — Banker's Algorithm Safe State and Request Granting]] |
| [[Deadlock Detection and Recovery Algorithms]] | [[Resource Allocation Graphs and Deadlock Modeling]] | [[Resource Allocation Graph Cycle Detection Example]], [[Problem — Resource Allocation Graph Reduction and Cycle Detection]] |
