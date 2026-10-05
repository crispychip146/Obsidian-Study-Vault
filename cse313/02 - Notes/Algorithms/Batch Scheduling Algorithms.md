---
type: algorithm
course: cse313
status: active
order: 13
---

# Batch Scheduling Algorithms

> 📖 **Reading Order:** Step 13 of 34 | **Module 3:** CPU Scheduling  
> ◄ **Previous:** [[CPU Scheduling Principles and Criteria]] | ► **Next:** [[Interactive Scheduling Algorithms]]

---

> [!IMPORTANT] **Exam practice references (Appeared in 2017 Q4a, 2017 Q4b, 2017 Q4c, 2019 Q1a, 2019 Q1b, 2020 Q2a, 2021 Q2b, 2021 Q2c, 2021 Q2d)**
>
> ### What Exam Questions Expect & How to Think:
> 1. **Batch Job Burst Range Ordering for Parameter $X$ (2019 Q1b, 2021 Q2d verbatim):**
>    - **The Setup:** 4 jobs arrive at the same time with burst lengths $9, 3, 5, X$. Determine SJF execution order for all possible ranges of $X$.
>    - **The decision rule:** Sort the known numbers ($3, 5, 9$). Then systematically position $X$ across the 4 partition intervals:
>      - If $X \le 3 \implies \mathbf{X \to 3 \to 5 \to 9}$
>      - If $3 < X \le 5 \implies \mathbf{3 \to X \to 5 \to 9}$
>      - If $5 < X \le 9 \implies \mathbf{3 \to 5 \to X \to 9}$
>      - If $X > 9 \implies \mathbf{3 \to 5 \to 9 \to X}$
> 2. **The FCFS Convoy Effect (2020 Q2a, 2021 Q2b verbatim):**
>    - Explain that when a long CPU-bound process holds the CPU, short I/O-bound processes queue behind it while I/O devices sit idle. Once the CPU-bound job finally waits for I/O, the I/O-bound jobs quickly finish their CPU bursts and crowd the I/O queue, leaving the CPU idle.
> 3. **SRTF vs Non-Preemptive SJF Advantage Proof (2021 Q2c):**
>    - Construct a concrete counterexample: $P_1(A=0, B=10)$ and $P_2(A=2, B=2)$. Under non-preemptive SJF, $P_1$ finishes at 10 and $P_2$ finishes at 12 (Avg TAT = 10.0 ms). Under SRTF, $P_2$ preempts $P_1$ at $t=2$ and finishes at 4, $P_1$ finishes at 12 (Avg TAT = 7.0 ms). Demonstrates superior average turnaround!
> 4. **Batch OS Scheduler Objectives (2017 Q4c):**
>    - Maximize Throughput (jobs/hr), Maximize CPU Utilization (keep CPU near 100%), Minimize Turnaround Time.

---

## Building the idea

Start with several ready jobs sharing one CPU. FCFS chooses by arrival order. SJF chooses the shortest next burst when the CPU becomes free. SRTF reconsiders the choice when new work arrives, comparing it with what remains of the current burst.

Why can short jobs first reduce total waiting? Consider two simultaneous jobs of lengths 8 and 2. Running 8 first makes the short job wait 8; running 2 first makes the long job wait 2. Both schedules do 10 units of work, but one creates less waiting for the other job. More generally, exchanging adjacent jobs $a>b$ reduces their combined waiting by $a-b$ while leaving later jobs' start times unchanged. Repeating such exchanges produces sorted order. That proves the SJF claim for simultaneous arrivals, known bursts, and negligible switching costs.

SRTF adds another observation: work already executed should not count against a job's current priority. Compare **remaining** times, not original bursts. At each arrival, update the running job's remaining work before deciding whether to preempt.

These policies apply the criteria in [[CPU Scheduling Principles and Criteria]], but shortest-job preference is not a fairness guarantee: continued short arrivals can postpone long jobs.

## Inputs

- Process ID list: $\{P_1, P_2, \dots, P_n\}$
- Arrival Times: $AT = [at_1, at_2, \dots, at_n]$
- Burst Times: $BT = [bt_1, bt_2, \dots, bt_n]$

---

## Outputs

- Gantt Chart (chronological timeline of CPU allocations).
- Completion Times $CT$, Turnaround Times $TAT = CT - AT$, Waiting Times $WT = TAT - BT$.
- Average Turnaround Time and Average Waiting Time.

---

## How It Works

### Overview

In batch operating systems (supercomputers, mainframe batch queues, background payroll/compilation jobs), there are no interactive users sitting at terminals waiting for immediate keyboard responses. The primary scheduling objectives are **maximizing throughput**, **maximizing CPU utilization**, and **minimizing average turnaround time**.

Three foundational algorithms govern batch scheduling:
1. **First-Come, First-Served (FCFS)**
2. **Shortest Job First (SJF)**
3. **Shortest Remaining Time First (SRTF)**

---

### Algorithmic Logic
- **Type:** Non-Preemptive.
- **Data Structure:** Standard First-In, First-Out (FIFO) queue.
- **Rule:** As processes become ready, they join the tail of the Ready Queue. The scheduler dispatches the process at the head of the queue. The running process holds the CPU until it voluntarily yields (for I/O) or terminates.

### Pseudocode:
```text
function FCFS_Schedule(ready_queue):
    while ready_queue is not empty:
        process = ready_queue.dequeue_front()
        context_switch_to(process)
        wait_until_process_yields_or_terminates(process)
```

### The Convoy Effect:
The fatal weakness of FCFS is the **Convoy Effect**:
- Imagine one massive compute-bound process $P_1$ with a burst time of $100\text{ ms}$, followed by five tiny I/O-bound processes $P_2 \dots P_6$ with burst times of $1\text{ ms}$ each.
- In FCFS, $P_1$ seizes the CPU for $100\text{ ms}$. All five I/O-bound processes sit idle in the Ready Queue, waiting $100\text{ ms}$ just to execute $1\text{ ms}$ of code!
- Meanwhile, the disk drives, network cards, and monitors sit completely idle.
- Average waiting time skyrockets. The short processes are "dragged like a convoy" behind the slow truck.

---

### Algorithmic Logic
- **Type:** Non-Preemptive.
- **Rule:** When the CPU becomes free, the scheduler inspects the Ready Queue and assigns the CPU to the process with the **smallest next CPU burst time**.
- If two processes have identical burst times, FCFS is used as a tie-breaker.

### Pseudocode:
```text
function SJF_Schedule(ready_queue):
    while ready_queue is not empty:
        # Find process with minimum CPU burst time
        shortest_process = find_min_burst(ready_queue)
        ready_queue.remove(shortest_process)
        context_switch_to(shortest_process)
        wait_until_process_yields_or_terminates(shortest_process)
```

### Optimality Theorem & Proof Sketch
**Theorem:** SJF is provably **optimal**—it generates the minimum possible average waiting time for any given set of stationary, simultaneous processes.

*Proof Sketch:*
Consider $n$ processes with burst times $t_1, t_2, \dots, t_n$.
Suppose they are scheduled in arbitrary order $1, 2, \dots, n$.
The waiting times are:
- $W_1 = 0$
- $W_2 = t_1$
- $W_3 = t_1 + t_2$
- $\dots$
- $W_n = t_1 + t_2 + \dots + t_{n-1}$

Summing the total waiting time:
$$W_{\text{total}} = (n-1)t_1 + (n-2)t_2 + (n-3)t_3 + \dots + 1 \cdot t_{n-1} + 0 \cdot t_n$$
Notice the multiplier for $t_1$ is $(n-1)$, while the multiplier for $t_n$ is $0$.
By the **rearrangement inequality**, the sum $\sum (n - i) t_i$ is mathematically minimized if and only if the coefficients are paired with the values in ascending order:
$$t_1 \le t_2 \le t_3 \le \dots \le t_n$$
Hence, executing the shortest jobs first minimizes average waiting time! $\blacksquare$

### Critical Drawback: Starvation & Unknowable Futures
1. **Unknowable Future:** In general systems, the OS cannot know how long a process will compute before it runs. Burst times must be predicted using historical exponential smoothing (see [[Scheduling Metrics and Burst Estimation Formulas]]).
2. **Starvation (Indefinite Blocking):** If a continuous stream of short processes enters the Ready Queue, a long process will sit waiting at the back of the queue and may **never execute**!

---

### Algorithmic Logic
- **Type:** Preemptive version of SJF.
- **Rule:** Whenever a new process arrives in the Ready Queue, the scheduler compares its required burst time against the **remaining CPU burst time** of the currently running process:
  $$\text{If } \text{Burst}_{\text{new}} < \text{RemainingTime}_{\text{current}} \implies \mathbf{Preempt\ Current\ Process!}$$
- The running process is forcefully suspended and returned to the Ready Queue. The CPU is dispatched to the newly arrived shorter process.

### Pseudocode:
```text
event On_Process_Arrival(new_process):
    ready_queue.insert(new_process)
    if running_process is None:
        ready_queue.remove(new_process)
        dispatch(new_process)
    elif new_process.burst_time < running_process.remaining_time:
        preempt(running_process)
        ready_queue.insert(running_process)
        ready_queue.remove(new_process)
        dispatch(new_process)

event On_Process_Termination_Or_Block(process):
    if ready_queue is not empty:
        next_proc = find_min_remaining_time(ready_queue)
        ready_queue.remove(next_proc)
        dispatch(next_proc)
```

---

### Comparative Performance Summary

| Feature | FCFS | Non-Preemptive SJF | Preemptive SRTF |
|---|---|---|---|
| **Preemption** | Non-preemptive | Non-preemptive | **Preemptive** |
| **Average Waiting Time** | High (susceptible to Convoy Effect) | Low (optimal among non-preemptive) | **Minimal (lowest possible)** |
| **Overhead** | Minimal (O(1) queue ops) | Low (sorting burst times) | Moderate (frequent context switches) |
| **Starvation Risk** | None (FIFO guarantees service) | **Yes** (long jobs starve) | **Yes** (long jobs starve) |
| **Implementation Complexity** | Trivial | Difficult (burst prediction) | Difficult (tracks remaining times) |

---

## Pseudocode

```c
// Implementation provided in lecture references
```

---

## Example

Four processes $P_1(BT=8), P_2(BT=4), P_3(BT=9), P_4(BT=5)$ arriving at $t=0$. Under FCFS, average wait is $10.25$ ms; under SJF (order $P_2, P_4, P_1, P_3$), average wait drops to $7.0$ ms!

---

## Complexity

### Time Complexity
$O(n \log n)$ to maintain priority queues of ready jobs.

### Space Complexity
$O(n)$ space for ready queue descriptors and timeline structures.

---

## Properties

- **Optimality of SJF:** Shortest Job First is provably optimal for minimizing average waiting time among non-preemptive scheduling algorithms.
- **Fairness Deficiency:** Both SJF and SRTF can cause indefinite starvation for long CPU bursts if short jobs arrive continuously.

---

## Limitations

- SJF and SRTF require knowing future CPU burst lengths in advance, which cannot be known with certainty on general-purpose OSes (requiring exponential smoothing estimation).

---

## Common Mistakes

1. **Equal Remaining Time During Preemption:**
   - If a newly arrived process has a burst time *equal* to the currently running process's remaining time, standard practice does **not** preempt. Preempting would waste a context switch with zero gain in waiting time.
2. **Preemption Thrashing:**
   - If processes arrive with infinitesimally smaller remaining times, the CPU can spend more time context switching than actually executing code.

---

## Exam Relevance

- **Next Step:** Interactive systems require time-sliced sharing where processes cannot monopolize the CPU (see [[Interactive Scheduling Algorithms]]).
- **Calculations:** See exact step-by-step Gantt charts and waiting time computations comparing FCFS, SJF, and SRTF (see [[Comprehensive CPU Scheduling Simulation Example]] and [[Problem — CPU Scheduling Algorithm Simulation and Gantt Chart]]).
- **Exam Testing:** Core exam material. Students are routinely required to draw Gantt charts and compute average turnaround and waiting times under FCFS, SJF, and SRTF.

---

## What to carry forward

The optimality statement needs its model; it is not a claim about every real workload. FCFS queue operations can be constant time, while shortest-job selection depends on the data structure. [[Scheduling Metrics and Burst Estimation Formulas]] explains how a scheduler can estimate an unknown future burst.

## Related notes

- [[CPU Scheduling Principles and Criteria]]
- [[Scheduling Metrics and Burst Estimation Formulas]]

## Sources

- **Lectures:** `cse313/01 - Sources/Lectures/3. Scheduling-week-3-RRR.pdf` (Slides 13–24)
- **Textbook:** Andrew S. Tanenbaum & Herbert Bos, *Modern Operating Systems* (4th Edition), Chapter 2 (Section 2.4.2: Scheduling in Batch Systems)
