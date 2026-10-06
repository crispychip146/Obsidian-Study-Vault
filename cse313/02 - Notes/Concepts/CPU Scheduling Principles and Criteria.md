---
type: concept
course: cse313
status: active
order: 12
---
# CPU Scheduling Principles and Criteria

> 📖 **Reading Order:** Step 12 of 68 | **Module 3:** CPU Scheduling  
> ◄ **Previous:** [[Problem — Fork Execution Tree and Process Tracing]] | ► **Next:** [[Batch Scheduling Algorithms]]

---
> [!IMPORTANT] 🎯 **Exam Frequency & Intelligence (Appeared in 2017 Q4a, 2020 Q2a)**
> **Frequency:** ⭐⭐⭐⭐ **High Recurrence (Tested with Burst Diagram Analysis)**
>
> ### What Exam Questions Expect & How to Master Them:
> 1. **Differentiating Compute-Bound vs I/O-Bound Processes with Diagrams (2017 Q4a & 2020 Q2a):**
>    - **Compute-Bound (CPU-Bound):** Spends the vast majority of time executing arithmetic/logic instructions. Exhibits very long CPU bursts punctuated by brief, infrequent I/O requests (e.g., scientific computing, video encoding, matrix multiplication).
>    - **I/O-Bound:** Spends the vast majority of its lifecycle waiting for I/O operations (user typing, disk reads, network sockets). Characterized by frequent, very short CPU bursts followed by long I/O wait periods.
>    - **The Diagram Expected by Examiners:**
>      ```
>      Compute-Bound:
>      |================ Long CPU Burst ================|==| I/O |================ CPU ================|
>
>      I/O-Bound:
>      |==| CPU |======== Long I/O Wait ========|==| CPU |======== Long I/O Wait ========|==| CPU |
>      ```

---
## Starting Point and the Problem

In a multiprogrammed operating system, multiple runnable processes populate the Ready Queue simultaneously, all competing for execution time on the available CPU cores.

We want an algorithmic policy to decide which process receives the CPU next, how long it runs, and when it should be preempted, in order to maximize overall system productivity and user satisfaction. The central obstacle is that different scheduling goals conflict directly: minimizing response time for interactive users hurts batch job throughput, while minimizing context-switch overhead hurts fairness.

---
## Developing the Idea

The CPU scheduling subsystem resolves this conflict by leveraging the fundamental empirical property of computing workloads: the **CPU–I/O Burst Cycle**.

Processes alternate between bursts of intensive CPU computation and waiting for I/O:
- **I/O-Bound processes** have many very short CPU bursts separated by long I/O waits (e.g. text editors, browsers).
- **Compute-Bound processes** have few, very long CPU bursts and rare I/O waits (e.g. scientific simulations, video encoders).

By designing schedulers that track burst characteristics, the OS can prioritize I/O-bound jobs to keep peripheral devices busy while interleaving compute-bound jobs during idle periods.

---
## Definition

In a multiprogramming operating system, multiple processes reside simultaneously in the Ready state competing for execution time. **CPU Scheduling** is the core operating system mechanism that selects one process from the Ready Queue and allocates a physical CPU core to it.

The component of the operating system that performs this selection is the **Scheduler**, and the algorithm it executes is the **Scheduling Algorithm**.

---
## How It Works

### The CPU–I/O Burst Cycle

Process execution consists of an alternating cycle of **CPU execution (bursts)** and **I/O wait (bursts)**:
- A process computes on the CPU for some duration (CPU burst).
- It then initiates an I/O operation (disk read, network packet, keystroke) and blocks (I/O burst).
- Upon I/O completion, it re-enters the Ready Queue for another CPU burst.
- Eventually, the final CPU burst ends with a system call to terminate.

```mermaid
flowchart LR
    A["CPU Burst<br/>(Arithmetic, Logic, Code)"] --> B["I/O Burst<br/>(Disk, Network, Keyboard)"]
    B --> C["CPU Burst"]
    C --> D["I/O Burst"]
    D --> E["Final CPU Burst & Exit"]
```

### Compute-Bound vs. I/O-Bound Processes:
1. **Compute-Bound (CPU-Bound) Processes:**
   - Spend almost all their time doing intense calculations.
   - Characterized by **long, infrequent CPU bursts** and very short, infrequent I/O requests.
   - *Examples:* Scientific simulations, video rendering, cryptographic hashing, machine learning model training.
2. **I/O-Bound Processes:**
   - Spend almost all their time waiting for external I/O.
   - Characterized by **short, frequent CPU bursts** interspersed with frequent I/O requests.
   - *Examples:* Text editors, database queries, web servers, file copiers.

*Key Scheduler Goal:* Prioritize I/O-bound processes to keep I/O devices fully utilized while keeping CPU latency low.

---
## Example

Scheduling decisions at 4 critical points:
1. Process switches from Running to Waiting state (e.g. `read()` system call) $	o$ Non-preemptive scheduling.
2. Process switches from Running to Ready state (e.g. timer interrupt ticks) $	o$ Preemptive scheduling.
3. Process switches from Waiting to Ready state (e.g. I/O completion interrupt) $	o$ Preemptive scheduling choice.
4. Process terminates $	o$ Non-preemptive scheduling.

---
## Technical Details

See related modules for microarchitectural implementation details.

---
## Important Properties and Why They Hold

- **Preemption vs. Overhead Invariant:** Preemption guarantees bounded response times for interactive applications, but increases total CPU overhead due to frequent context switches and cache thrashing.
- **Turnaround vs. Waiting Equivalence:** Turnaround Time ($T_{TAT} = T_{	ext{completion}} - T_{	ext{arrival}}$) is always strictly equal to Waiting Time plus Burst Time: $T_{TAT} = T_{wait} + T_{burst}$.
- **Workload Trade-Off Invariant:** No single scheduling algorithm can simultaneously optimize all criteria (Throughput, Turnaround, Waiting Time, Response Time, and CPU Utilization).

---
## Common Mistakes

1. **Confusing Waiting Time with Turnaround Time:**
   - Turnaround time includes the process's own CPU burst time. Waiting time is *strictly* the time spent waiting in the Ready Queue doing nothing.
2. **Optimizing One Metric at the Expense of Another:**
   - Maximizing throughput (by running short jobs first via SJF) often harms fairness and can cause long jobs to starve indefinitely.
   - Providing instantaneous response times (via tiny Round Robin time quanta) incurs severe context switch overhead, degrading overall throughput.

---
## Exam Relevance

- **Next Step:** How batch systems optimize turnaround time using non-preemptive and shortest-burst strategies (see [[Batch Scheduling Algorithms]]).
- **Interactive Systems:** How time-sharing interactive systems slice CPU time into quanta (see [[Interactive Scheduling Algorithms]]).
- **Formulas:** Detailed mathematical definitions and exponential smoothing burst estimation (see [[Scheduling Metrics and Burst Estimation Formulas]]).
- **Exam Testing:** Frequently tested by asking students to define the 5 scheduling criteria and categorize an algorithm as preemptive vs non-preemptive.

---
## Related Concepts

- [[Batch Scheduling Algorithms]]
- [[Interactive Scheduling Algorithms]]
- [[Scheduling Metrics and Burst Estimation Formulas]]

---
## Prerequisites

- [[Process Lifecycle and State Transitions]]
- [[Process Control Block and Context Switching]]

---
## Problems

- [[Problem — CPU Scheduling Algorithm Simulation and Gantt Chart]]

---
## Sources

- **Lectures:** `cse313/01 - Sources/Lectures/3. Scheduling-week-3-RRR.pdf` (Slides 1–14)
- **Textbook:** Andrew S. Tanenbaum & Herbert Bos, *Modern Operating Systems* (4th Edition), Chapter 2 (Section 2.4: Scheduling)
