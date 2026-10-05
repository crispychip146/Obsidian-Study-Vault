---
type: algorithm
course: cse313
status: active
order: 14
---

# Interactive Scheduling Algorithms

> 📖 **Reading Order:** Step 14 of 34 | **Module 3:** CPU Scheduling  
> ◄ **Previous:** [[Batch Scheduling Algorithms]] | ► **Next:** [[Scheduling Metrics and Burst Estimation Formulas]]

---

> [!IMPORTANT] **Exam practice references (Appeared in 2017 Q4b, 2018 Q3b, 2018 Q4b, 2018 Q4c, 2019 Q1a, 2020 Q1a, 2020 Q4c, 2021 Q2a)**
>
> ### What Exam Questions Expect & How to Think:
> 1. **Priority Scheduling with Round Robin Tie-Breaking (2017 Q4b, 2019 Q1a, 2020 Q1a, 2021 Q2a):**
>    - **The Setup:** Processes have both numerical priority and arrival times. The scheduler always runs highest-priority processes first. Processes with identical priority share the CPU using Round Robin with time quantum $q$.
>    - **The decision rule:** Check whether priority convention states *higher number = higher priority* (e.g., 2020 Q1a) or *lower number = higher priority* (e.g., 2017 Q4b, 2019 Q1a). Read the prompt carefully!
> 2. **Dynamic / Tiered Quanta (2020 Q1a):**
>    - If the problem specifies *"The first 5 quanta use $q=20$, subsequent quanta use $q=30$,"* track elapsed time slices across the entire system. Once 5 slices (100 ms of CPU time) are consumed, switch your slice limit to 30 ms.
> 3. **Non-Preemptive Quantum Completion Clause (2021 Q2a):**
>    - When the question specifies: *"A running process completes its current quantum before any rescheduling occurs,"* if a higher-priority process arrives at $t=20$ during a $q=30$ slice running from $0 \to 30$, do NOT preempt at $t=20$! Let the running process finish until $t=30$, then switch!
> 4. **Round Robin with Scheduler Overhead $S$ (2018 Q3b):**
>    - When scheduler overhead is given ($S=1, q=2$), the CPU alternates: $[S=1] \to [P_1 \text{ for } 2] \to [S=1] \to [P_2 \text{ for } 2] \to \dots$. Turnaround time must include all intervening scheduler intervals!
> 5. **New Job Insertion at Head vs Tail of Ready Queue (2018 Q4c):**
>    - Newly arriving jobs must be inserted at the **end (tail)** of the ready queue. If placed at the head, continuous incoming bursts would preempt older waiting jobs, destroying cyclic fairness and causing starvation.
> 6. **MLFQ Demotion and Priority Boost (2018 Q4b):**
>    - CPU-bound jobs consume full quanta and get demoted ($Q_0 \to Q_1 \to Q_2$). Interactive jobs yield early and stay at $Q_0$. The Priority Boost period ($S=500\text{ ms}$) flushes all jobs back to $Q_0$ to prevent starvation and allow compute-bound jobs whose behavior turns interactive to reclaim low response latency.

---

## Building the idea

An interactive user wants evidence that the application is responding before it completes all its work. Round Robin addresses this by limiting a turn to a **quantum** $q$. The ready queue, rather than the original burst length, determines who runs next.

Follow a job with 10 units remaining and $q=3$. It runs 3, keeps 7, and rejoins the queue behind jobs already waiting. A job with only 2 units remaining finishes after 2; it does not consume a fictional extra unit. This is why a trace must track both queue order and remaining work.

A smaller quantum creates more frequent opportunities to run, but also more switching. A larger quantum reduces that overhead while making jobs wait longer between turns. With a fixed set of $n$ ready jobs and negligible overhead, the other $n-1$ jobs can each use at most $q$ before our job's next turn.

Priority scheduling instead ranks importance. MLFQ uses observed behavior to revise that ranking: sustained CPU use suggests a long burst, while periodic boosts give postponed jobs another opportunity. Read each rule as a policy decision; implementations differ, and a simplified MLFQ is not a universal description of desktop kernels.

## Inputs

- Ready Queue of runnable processes.
- Time quantum length $q$ (typically 10–100 ms).
- Priority values and feedback queue thresholds.

---

## Outputs

- Scheduled process ID dispatched to the CPU.
- Maximum response time bound $R \le (n - 1)q$ for $n$ processes.

---

## How It Works

### Overview

In interactive multi-user and desktop operating systems, human users expect near-instantaneous feedback to keyboard, mouse, and network events. Algorithms cannot allow long jobs to monopolize the CPU. The overriding design goals are **minimizing response time**, **preventing starvation**, and **maintaining fairness**.

The dominant interactive algorithms are:
1. **Round Robin (RR)**
2. **Priority Scheduling (with Aging)**
3. **Multilevel Feedback Queue (MLFQ)**
4. **Lottery Scheduling (Proportional Share)**

---

### Algorithmic Logic
- **Type:** Preemptive time-slicing.
- **Mechanism:** The scheduler maintains a FIFO Ready Queue. Each process is allocated a fixed small chunk of CPU time called a **Time Quantum** (or time slice), typically between $10\text{ ms}$ and $100\text{ ms}$.
- The CPU scheduler pops the process at the head of the queue, starts the hardware timer, and dispatches the process.
- **Branching Conditions:**
  1. **Process finishes or blocks before quantum expires:** The process voluntarily yields the CPU (e.g., for I/O), the timer is reset, and the next process is dispatched.
  2. **Quantum expires while process is computing:** The hardware timer generates an interrupt. The OS moves the running process to the **tail of the Ready Queue** and dispatches the new head of the queue.

```mermaid
flowchart LR
    Head["Ready Queue Head (P1)"] -->|Dispatch| CPU["CPU Execution (Time Quantum q)"]
    CPU -->|Blocks for I/O| Blocked["I/O Wait Queue"]
    CPU -->|Quantum Expires (Timer Interrupt)| Tail["Ready Queue Tail"]
    Tail --> Head
```

### The Quantum Sizing Dilemma ($q$)

Choosing the length of the time quantum $q$ involves a fundamental trade-off:

```
                      TIME QUANTUM SIZING TRADE-OFF
                      
      q Too Small (< 1 ms)                     q Too Large (> 500 ms)
<-------------------------------------------------------------------->
High Context Switch Overhead             Poor Interactive Response Time
CPU wastes time swapping registers       Degrades into FCFS (Convoy Effect)
```

- **If $q$ is extremely small (e.g., $1\text{ ms}$):**
  If context switch time is $0.2\text{ ms}$, then $\frac{0.2}{1.2} \approx 16.7\%$ of all CPU capability is burned on context switching!
- **If $q$ is extremely large (e.g., $1000\text{ ms}$):**
  Short interactive tasks must wait behind long tasks. The system feels sluggish and non-responsive; RR degenerates into [[Batch Scheduling Algorithms|FCFS]].
- **Golden Rule of Thumb:**  
  Set the time quantum $q$ such that **$80\%$ of all CPU bursts are shorter than $q$**, while keeping context switch overhead below $1\%$ of the quantum. Treat these percentages as a design heuristic to evaluate for a specified workload, rather than universal kernel settings.

---

### Algorithmic Logic
- Each process is assigned an integer **priority level**.
- The scheduler always allocates the CPU to the highest-priority ready process. (Convention: In UNIX, a smaller integer value represents a higher priority—e.g., priority 0 is higher than priority 20).
- Can be **preemptive** (new high-priority arrival immediately preempts a lower-priority running job) or **non-preemptive**.

### The Starvation Problem & The Aging Solution
- **Starvation (Indefinite Blocking):** In pure static priority scheduling, low-priority processes can sit in the Ready Queue forever if higher-priority processes keep arriving. (In 1973, when the IBM 7094 at MIT was decommissioned, a low-priority job submitted in 1967 was discovered sitting unexecuted in the queue!).
- **Aging Solution:** The OS dynamically increases the priority of processes that wait in the Ready Queue over time:
  $$\text{Priority}_{\text{new}} = \text{Priority}_{\text{base}} - \lfloor \alpha \cdot T_{\text{wait}} \rfloor$$
  Eventually, any starving process's priority climbs high enough to outrank all other jobs and seize the CPU.

### Priority Inversion & Priority Inheritance (The Mars Pathfinder Bug)
Consider three processes: High ($H$), Medium ($M$), Low ($L$):
1. $L$ acquires a shared mutex lock on a shared data bus.
2. $H$ becomes ready, preempts $L$, and attempts to acquire the lock. $H$ blocks waiting for $L$ to unlock it.
3. Suddenly, medium-priority process $M$ (which does not need the lock) becomes ready. Because $\text{Priority}(M) > \text{Priority}(L)$, $M$ preempts $L$!
4. **Disaster:** $H$ is waiting for $L$, but $L$ cannot run because $M$ is executing. A medium-priority process is indirectly blocking a high-priority process indefinitely!
- **Solution (Priority Inheritance Protocol):**  
  Whenever a high-priority process $H$ blocks on a resource held by low-priority process $L$, process $L$ temporarily **inherits the high priority of $H$** until it releases the lock, preventing medium processes from preempting it.

---

### 3. Multilevel Feedback Queue (MLFQ)

A **Multilevel Feedback Queue (MLFQ)** is a family of policies that adapts priority using observed CPU behavior. The classroom rules below define one model; real operating systems use varied and evolving scheduling policies.

### Why MLFQ?
SJF is optimal, but requires knowing the future. MLFQ **learns from process history** to approximate SJF dynamically without knowing burst lengths in advance!

```mermaid
flowchart TD
    NewJob["Newly Arrived Job"] --> Q0["Queue 0: Priority Highest | Quantum q = 8 ms (RR)"]
    Q0 -->|Uses Full Quantum without I/O| Q1["Queue 1: Priority Medium | Quantum q = 16 ms (RR)"]
    Q1 -->|Uses Full Quantum without I/O| Q2["Queue 2: Priority Low | FCFS / Quantum q = 32 ms"]
    Q0 -->|Blocks on I/O before q| Q0
    Q1 -->|Blocks on I/O before q| Q1
    
    Boost["Periodic Priority Boost (Every S seconds)<br/>Move ALL jobs back to Queue 0"] -.-> Q0
```

### The 5 Core MLFQ Rules:
1. **Rule 1:** If $\text{Priority}(A) > \text{Priority}(B)$, Process $A$ runs ($B$ does not).
2. **Rule 2:** If $\text{Priority}(A) == \text{Priority}(B)$, $A$ and $B$ run in Round Robin using the quantum of that queue.
3. **Rule 3:** When a job enters the system, it is placed at the **highest priority queue** (Queue 0).
4. **Rule 4 (Demotion):** If a job uses up its entire time quantum without voluntarily yielding, its priority is **reduced by 1 level** (demoted to the next lower queue with a larger quantum). If a job yields for I/O before its quantum expires, it stays at the same priority level.
5. **Rule 5 (Priority Boost):** After some time period $S$, move **all jobs in the system to Queue 0**. This improves opportunities for delayed jobs under suitable scheduling and workload assumptions; a full starvation bound needs those assumptions. It also handles changing job behavior.

---

### 4. Lottery Scheduling (Proportional Share)

- **Mechanism:** The OS allocates each process a set of discrete **lottery tickets**. Whenever a scheduling decision is made, the OS generates a pseudo-random number between $1$ and $T_{\text{total}}$. Whichever process holds the winning ticket gets the CPU!
- **Proportional Share Property:** If Process $A$ holds 75 tickets and Process $B$ holds 25 tickets, each equal-length lottery opportunity selects A with probability 0.75 and B with probability 0.25. Finite-run shares fluctuate; long-run proportions approach these values under the sampling assumptions.
- **Ticket Transfers:** A client can temporarily transfer its lottery tickets to a server process while waiting for an RPC, preventing server bottlenecks.

---

### Comparative Reference Table

| Algorithm | Primary Strengths | Primary Weaknesses | Best Suited For |
|---|---|---|---|
| **Round Robin (RR)** | Fair, starvation-free, predictable response times | High context switch overhead if $q$ is small; higher average turnaround time | General interactive systems |
| **Priority with Aging** | Differentiates critical system daemons from background jobs | Risk of priority inversion without inheritance | Real-time and server kernels |
| **MLFQ** | Automatic learning, approximates SJF, optimizes both response & turnaround time | Complex parameter tuning (number of queues, quanta, boost frequency) | General desktop and mobile OSes |
| **Lottery Scheduling** | Mathematically simple proportional sharing, flexible ticket delegation | Non-deterministic in short time horizons | Virtual machine hypervisors, cloud multi-tenancy |

---

## Pseudocode

```c
// Implementation provided in lecture references
```

---

## Example

Three processes $P_1(24\text{ ms}), P_2(3\text{ ms}), P_3(3\text{ ms})$ with quantum $q = 4\text{ ms}$. $P_1$ runs for 4 ms, then $P_2$ finishes in 3 ms, $P_3$ finishes in 3 ms, and $P_1$ finishes its remaining 20 ms in 5 slices.

---

## Complexity

### Time Complexity
$O(1)$ dispatch time using FIFO round-robin pointer rotation or multilevel array bitmasks.

### Space Complexity
$O(n)$ space for priority queue headers and ready lists.

---

## Properties

- **Bounded Response Guarantee:** For a fixed set of $n$ ready jobs, FIFO Round Robin with quantum $q$ and zero scheduling overhead bounds the intervening CPU time before the next turn by $(n-1)q$. Arrivals, priority rules, and overhead require their own accounting.
- **Quantum Sensitivity:** If $q \to \infty$, RR degenerates into FCFS; if $q \to 0$, context switch overhead dominates and system throughput drops toward zero.

---

## Limitations

- Priority inversion can occur when a high-priority process waits for a resource held by a low-priority process (resolved by Priority Inheritance).

---

## Exam Relevance

- **Next Step:** Mathematical formulas for calculating waiting times and predicting future burst times using exponential smoothing (see [[Scheduling Metrics and Burst Estimation Formulas]]).
- **Simulation:** Full step-by-step Gantt chart walkthroughs of Round Robin vs FCFS vs SJF (see [[Comprehensive CPU Scheduling Simulation Example]]).
- **Exam Testing:** High-frequency exam questions:
  - "Explain how Round Robin behaves when $q \to 0$ and $q \to \infty$."
  - "Define the Priority Inversion problem and describe how Priority Inheritance resolves it."
  - "State the 5 rules of the Multilevel Feedback Queue."

---

## What to carry forward

At a quantum boundary, arrivals and requeueing can occur at the same timestamp. State the exercise's tie rule before tracing. [[Comprehensive CPU Scheduling Simulation Example]] shows how responsiveness can improve even when average turnaround gets worse. Lottery shares are probabilistic expectations, not exact guarantees for every finite run.

## Related notes

- [[Comprehensive CPU Scheduling Simulation Example]]

## Sources

- **Lectures:** `cse313/01 - Sources/Lectures/3. Scheduling-week-3-RRR.pdf` (Slides 25–48)
- **Textbook:** Andrew S. Tanenbaum & Herbert Bos, *Modern Operating Systems* (4th Edition), Chapter 2 (Section 2.4.3: Scheduling in Interactive Systems)
