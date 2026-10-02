---
type: example
course: cse313
status: active
order: 16
---

# Comprehensive CPU Scheduling Simulation Example

> 📖 **Reading Order:** Step 16 of 34 | **Module 3:** CPU Scheduling  
> ◄ **Previous:** [[Scheduling Metrics and Burst Estimation Formulas]] | ► **Next:** [[Problem — CPU Scheduling Algorithm Simulation and Gantt Chart]]

---

## Problem Context & Setup

Consider a workload of 4 processes arriving at different times with varying CPU burst durations:

| Process | Arrival Time ($A_i$) | CPU Burst Time ($B_i$) | Priority (Lower = Higher) |
|---|---|---|---|
| **$P_1$** | $0\text{ ms}$ | $8\text{ ms}$ | 3 |
| **$P_2$** | $1\text{ ms}$ | $4\text{ ms}$ | 1 |
| **$P_3$** | $2\text{ ms}$ | $9\text{ ms}$ | 4 |
| **$P_4$** | $3\text{ ms}$ | $5\text{ ms}$ | 2 |

**Goal:**  
Simulate execution, construct ASCII Gantt charts, and compute individual and average **Turnaround Time ($T_{\text{turn}}$)**, **Waiting Time ($T_{\text{wait}}$)**, and **Response Time ($T_{\text{resp}}$)** under four standard algorithms:
1. **First-Come, First-Served (FCFS)**
2. **Shortest Job First (SJF — Non-Preemptive)**
3. **Shortest Remaining Time First (SRTF — Preemptive)**
4. **Round Robin (RR with Time Quantum $q = 4\text{ ms}$)**

---

## 1. First-Come, First-Served (FCFS)

### Execution Trace & Gantt Chart:
- At $t = 0$: $P_1$ arrives and runs until completion ($t = 8$).
- At $t = 8$: $P_2$ runs until completion ($t = 8 + 4 = 12$).
- At $t = 12$: $P_3$ runs until completion ($t = 12 + 9 = 21$).
- At $t = 21$: $P_4$ runs until completion ($t = 21 + 5 = 26$).

```
Gantt Chart (FCFS):
|    P1    |   P2   |     P3     |   P4   |
0          8        12           21       26
```

### Metrics Calculation:
- **$P_1$:** Completion $C = 8 \implies T_{\text{turn}} = 8 - 0 = 8\text{ ms}, \quad T_{\text{wait}} = 8 - 8 = 0\text{ ms}, \quad T_{\text{resp}} = 0\text{ ms}$
- **$P_2$:** Completion $C = 12 \implies T_{\text{turn}} = 12 - 1 = 11\text{ ms}, \quad T_{\text{wait}} = 11 - 4 = 7\text{ ms}, \quad T_{\text{resp}} = 8 - 1 = 7\text{ ms}$
- **$P_3$:** Completion $C = 21 \implies T_{\text{turn}} = 21 - 2 = 19\text{ ms}, \quad T_{\text{wait}} = 19 - 9 = 10\text{ ms}, \quad T_{\text{resp}} = 12 - 2 = 10\text{ ms}$
- **$P_4$:** Completion $C = 26 \implies T_{\text{turn}} = 26 - 3 = 23\text{ ms}, \quad T_{\text{wait}} = 23 - 5 = 18\text{ ms}, \quad T_{\text{resp}} = 21 - 3 = 18\text{ ms}$

$$\text{Average Turnaround Time} = \frac{8 + 11 + 19 + 23}{4} = \frac{61}{4} = \mathbf{15.25\text{ ms}}$$
$$\text{Average Waiting Time} = \frac{0 + 7 + 10 + 18}{4} = \frac{35}{4} = \mathbf{8.75\text{ ms}}$$

---

## 2. Shortest Job First (SJF — Non-Preemptive)

### Execution Trace & Gantt Chart:
- At $t = 0$: Only $P_1$ has arrived. $P_1$ is dispatched and runs to completion (non-preemptive!) from $t = 0$ to $t = 8$.
- At $t = 8$: $P_2 (B=4)$, $P_3 (B=9)$, and $P_4 (B=5)$ are all in the Ready Queue.
  - Shortest is $P_2 (4\text{ ms}) \implies P_2$ runs from $t = 8$ to $t = 12$.
- At $t = 12$: Ready Queue contains $P_4 (5\text{ ms})$ and $P_3 (9\text{ ms})$.
  - Shortest is $P_4 \implies P_4$ runs from $t = 12$ to $t = 17$.
- At $t = 17$: $P_3$ runs from $t = 17$ to $t = 26$.

```
Gantt Chart (SJF Non-Preemptive):
|    P1    |   P2   |   P4   |     P3     |
0          8        12       17           26
```

### Metrics Calculation:
- **$P_1$:** $C = 8 \implies T_{\text{turn}} = 8 - 0 = 8\text{ ms}, \quad T_{\text{wait}} = 8 - 8 = 0\text{ ms}, \quad T_{\text{resp}} = 0\text{ ms}$
- **$P_2$:** $C = 12 \implies T_{\text{turn}} = 12 - 1 = 11\text{ ms}, \quad T_{\text{wait}} = 11 - 4 = 7\text{ ms}, \quad T_{\text{resp}} = 8 - 1 = 7\text{ ms}$
- **$P_3$:** $C = 26 \implies T_{\text{turn}} = 26 - 2 = 24\text{ ms}, \quad T_{\text{wait}} = 24 - 9 = 15\text{ ms}, \quad T_{\text{resp}} = 17 - 2 = 15\text{ ms}$
- **$P_4$:** $C = 17 \implies T_{\text{turn}} = 17 - 3 = 14\text{ ms}, \quad T_{\text{wait}} = 14 - 5 = 9\text{ ms}, \quad T_{\text{resp}} = 12 - 3 = 9\text{ ms}$

$$\text{Average Turnaround Time} = \frac{8 + 11 + 24 + 14}{4} = \frac{57}{4} = \mathbf{14.25\text{ ms}}$$
$$\text{Average Waiting Time} = \frac{0 + 7 + 15 + 9}{4} = \frac{31}{4} = \mathbf{7.75\text{ ms}}$$

---

## 3. Shortest Remaining Time First (SRTF — Preemptive)

### Execution Trace & Gantt Chart:
- At $t = 0$: $P_1$ starts (remaining: 8).
- At $t = 1$: $P_2$ arrives with burst $4$. $P_1$ has $7\text{ ms}$ remaining.
  Since $4 < 7$, **$P_1$ is PREEMPTED!** $P_2$ starts running (remaining: 4).
- At $t = 2$: $P_3$ arrives ($B=9$). $P_2$ has $3\text{ ms}$ remaining $\implies P_2$ continues.
- At $t = 3$: $P_4$ arrives ($B=5$). $P_2$ has $2\text{ ms}$ remaining $\implies P_2$ continues.
- At $t = 5$: $P_2$ completes!
  Ready queue contains: $P_4 (5\text{ ms})$, $P_1 (7\text{ ms})$, $P_3 (9\text{ ms})$.
  Shortest remaining is $P_4 \implies P_4$ runs from $t = 5$ to $t = 10$.
- At $t = 10$: $P_4$ completes!
  Ready queue contains: $P_1 (7\text{ ms})$, $P_3 (9\text{ ms})$.
  Shortest remaining is $P_1 \implies P_1$ runs from $t = 10$ to $t = 17$.
- At $t = 17$: $P_1$ completes!
  $P_3$ runs from $t = 17$ to $t = 26$.

```
Gantt Chart (SRTF Preemptive):
| P1 |   P2   |   P4   |    P1    |     P3     |
0    1        5        10         17           26
```

### Metrics Calculation:
- **$P_1$:** Completion $C = 17 \implies T_{\text{turn}} = 17 - 0 = 17\text{ ms}$.
  $T_{\text{wait}} = 17 - 8 = 9\text{ ms}$.
  First scheduled at $t = 0 \implies T_{\text{resp}} = 0 - 0 = 0\text{ ms}$.
- **$P_2$:** Completion $C = 5 \implies T_{\text{turn}} = 5 - 1 = 4\text{ ms}$.
  $T_{\text{wait}} = 4 - 4 = 0\text{ ms}$.
  First scheduled at $t = 1 \implies T_{\text{resp}} = 1 - 1 = 0\text{ ms}$.
- **$P_3$:** Completion $C = 26 \implies T_{\text{turn}} = 26 - 2 = 24\text{ ms}$.
  $T_{\text{wait}} = 24 - 9 = 15\text{ ms}$.
  First scheduled at $t = 17 \implies T_{\text{resp}} = 17 - 2 = 15\text{ ms}$.
- **$P_4$:** Completion $C = 10 \implies T_{\text{turn}} = 10 - 3 = 7\text{ ms}$.
  $T_{\text{wait}} = 7 - 5 = 2\text{ ms}$.
  First scheduled at $t = 5 \implies T_{\text{resp}} = 5 - 3 = 2\text{ ms}$.

$$\text{Average Turnaround Time} = \frac{17 + 4 + 24 + 7}{4} = \frac{52}{4} = \mathbf{13.00\text{ ms}}$$
$$\text{Average Waiting Time} = \frac{9 + 0 + 15 + 2}{4} = \frac{26}{4} = \mathbf{6.50\text{ ms}}$$

---

## 4. Round Robin (RR with Quantum $q = 4\text{ ms}$)

### Execution Trace & Gantt Chart:
- At $t = 0$: Ready queue = $[P_1]$. $P_1$ runs for full quantum $q=4$ (remaining: $8 - 4 = 4$).
  During this interval: $P_2$ arrived at $t=1$, $P_3$ at $t=2$, $P_4$ at $t=3$.
- At $t = 4$: $P_1$ preempted and moved to queue tail. Ready queue = $[P_2, P_3, P_4, P_1]$.
- At $t = 4$: $P_2$ runs for $4\text{ ms}$. Its burst is $4$, so it **completes at $t = 8$**!
  Ready queue = $[P_3, P_4, P_1]$.
- At $t = 8$: $P_3$ runs for $q=4$ (remaining: $9 - 4 = 5$).
  At $t = 12$: $P_3$ preempted. Ready queue = $[P_4, P_1, P_3]$.
- At $t = 12$: $P_4$ runs for $q=4$ (remaining: $5 - 4 = 1$).
  At $t = 16$: $P_4$ preempted. Ready queue = $[P_1, P_3, P_4]$.
- At $t = 16$: $P_1$ runs for remaining $4\text{ ms}$, **completes at $t = 20$**!
  Ready queue = $[P_3, P_4]$.
- At $t = 20$: $P_3$ runs for $q=4$ (remaining: $5 - 4 = 1$).
  At $t = 24$: $P_3$ preempted. Ready queue = $[P_4, P_3]$.
- At $t = 24$: $P_4$ runs for its remaining $1\text{ ms}$, **completes at $t = 25$**!
  Ready queue = $[P_3]$.
- At $t = 25$: $P_3$ runs for its remaining $1\text{ ms}$, **completes at $t = 26$**!

```
Gantt Chart (Round Robin, q = 4):
|   P1   |   P2   |   P3   |   P4   |   P1   |   P3   | P4 | P3 |
0        4        8        12       16       20       24   25   26
```

### Metrics Calculation:
- **$P_1$:** Completion $C = 20 \implies T_{\text{turn}} = 20 - 0 = 20\text{ ms}, \quad T_{\text{wait}} = 20 - 8 = 12\text{ ms}, \quad T_{\text{resp}} = 0 - 0 = 0\text{ ms}$
- **$P_2$:** Completion $C = 8 \implies T_{\text{turn}} = 8 - 1 = 7\text{ ms}, \quad T_{\text{wait}} = 7 - 4 = 3\text{ ms}, \quad T_{\text{resp}} = 4 - 1 = 3\text{ ms}$
- **$P_3$:** Completion $C = 26 \implies T_{\text{turn}} = 26 - 2 = 24\text{ ms}, \quad T_{\text{wait}} = 24 - 9 = 15\text{ ms}, \quad T_{\text{resp}} = 8 - 2 = 6\text{ ms}$
- **$P_4$:** Completion $C = 25 \implies T_{\text{turn}} = 25 - 3 = 22\text{ ms}, \quad T_{\text{wait}} = 22 - 5 = 17\text{ ms}, \quad T_{\text{resp}} = 12 - 3 = 9\text{ ms}$

$$\text{Average Turnaround Time} = \frac{20 + 7 + 24 + 22}{4} = \frac{73}{4} = \mathbf{18.25\text{ ms}}$$
$$\text{Average Waiting Time} = \frac{12 + 3 + 15 + 17}{4} = \frac{47}{4} = \mathbf{11.75\text{ ms}}$$
$$\text{Average Response Time} = \frac{0 + 3 + 6 + 9}{4} = \frac{18}{4} = \mathbf{4.50\text{ ms}}$$

---

## Master Comparison Summary

| Metric | FCFS | SJF (Non-Preemptive) | SRTF (Preemptive) | Round Robin ($q=4$) |
|---|---|---|---|---|
| **Average Turnaround Time** | $15.25\text{ ms}$ | $14.25\text{ ms}$ | **$13.00\text{ ms}$ (Best)** | $18.25\text{ ms}$ |
| **Average Waiting Time** | $8.75\text{ ms}$ | $7.75\text{ ms}$ | **$6.50\text{ ms}$ (Best)** | $11.75\text{ ms}$ |
| **Average Response Time** | $8.75\text{ ms}$ | $7.75\text{ ms}$ | $4.25\text{ ms}$ | **$4.50\text{ ms}$ (Fair & Fast)** |
| **Preemption Overhead** | None | None | 1 Preemption ($P_1$) | 4 Preemptions |

### Deep Insight:
- **SRTF** wins decisively on **average waiting time ($6.50\text{ ms}$)** and **turnaround time ($13.00\text{ ms}$)** because it relentlessly schedules the shortest remaining chunks.
- **Round Robin** has a higher turnaround time ($18.25\text{ ms}$) because long jobs are interleaved and prolonged, but it guarantees that **every process gets its first response quickly** (average response time drops to $4.50\text{ ms}$), providing the smooth responsiveness human users require!

---

## Related Notes

- [[Batch Scheduling Algorithms]] — Formal specifications of FCFS, SJF, and SRTF.
- [[Interactive Scheduling Algorithms]] — Mechanics of Round Robin and quantum sizing.
- [[Problem — CPU Scheduling Algorithm Simulation and Gantt Chart]] — Practice exam problem.

---

## Sources & Traceability

- **Lectures:** `cse313/01 - Sources/Lectures/3. Scheduling-week-3-RRR.pdf` (Slides 13–48)
- **Textbook:** Andrew S. Tanenbaum & Herbert Bos, *Modern Operating Systems* (4th Edition), Chapter 2 (Section 2.4)
