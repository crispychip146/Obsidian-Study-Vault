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

## Problem

Consider a workload of 4 processes arriving at different times with varying CPU burst durations:

| Process | Arrival Time ($A_i$) | CPU Burst Time ($B_i$) | Priority (Lower = Higher) |
|---|---|---|---|
| **$P_1$** | $0\text{ ms}$ | $8\text{ ms}$ | 3 |
| **$P_2$** | $1\text{ ms}$ | $4\text{ ms}$ | 1 |
| **$P_3$** | $2\text{ ms}$ | $9\text{ ms}$ | 4 |
| **$P_4$** | $3\text{ ms}$ | $5\text{ ms}$ | 2 |

**Goal:**  
Simulate execution, construct execution timelines, and compute individual and average **Turnaround Time ($T_{\text{turn}}$)**, **Waiting Time ($T_{\text{wait}}$)**, and **Response Time ($T_{\text{resp}}$)** under four standard algorithms:
1. **First-Come, First-Served (FCFS)**
2. **Shortest Job First (SJF — Non-Preemptive)**
3. **Shortest Remaining Time First (SRTF — Preemptive)**
4. **Round Robin (RR with Time Quantum $q = 4\text{ ms}$)**

---

## Solution

Use the workload table to separate two kinds of information: arrival time determines **eligibility**, while burst or remaining time influences **selection**. At $t=0$, only $P_1$ is eligible. Therefore non-preemptive SJF cannot start $P_2$ merely because its burst is shorter.

FCFS keeps $P_1$ running and then follows arrivals. Non-preemptive SJF also keeps it running, but at $t=8$ chooses among all jobs that have arrived. SRTF differs at $t=1$: $P_1$ has 7 units left, while the arriving $P_2$ needs 4, so $P_2$ takes over. That single decision changes several completion times.

For Round Robin, write the queue after every slice. Arriving jobs wait at the tail; a partially served job rejoins when its quantum ends. A finished job disappears. The state trace explains the chart, and the chart explains the metrics using [[Scheduling Metrics and Burst Estimation Formulas]].

All these schedules perform the same 26 units of CPU service under the zero-overhead model. Differences in average waiting come from how that service is distributed among jobs, not from magically doing less work.

### Execution Trace & Gantt Chart:
- At $t = 0$: $P_1$ arrives and runs until completion ($t = 8$).
- At $t = 8$: $P_2$ runs until completion ($t = 8 + 4 = 12$).
- At $t = 12$: $P_3$ runs until completion ($t = 12 + 9 = 21$).
- At $t = 21$: $P_4$ runs until completion ($t = 21 + 5 = 26$).

| Interval (ms) | Running process |
|---|---|
| [0, 8) | $P_1$ |
| [8, 12) | $P_2$ |
| [12, 21) | $P_3$ |
| [21, 26) | $P_4$ |

### Metrics Calculation:
- **$P_1$:** Completion $C = 8 \implies T_{\text{turn}} = 8 - 0 = 8\text{ ms}, \quad T_{\text{wait}} = 8 - 8 = 0\text{ ms}, \quad T_{\text{resp}} = 0\text{ ms}$
- **$P_2$:** Completion $C = 12 \implies T_{\text{turn}} = 12 - 1 = 11\text{ ms}, \quad T_{\text{wait}} = 11 - 4 = 7\text{ ms}, \quad T_{\text{resp}} = 8 - 1 = 7\text{ ms}$
- **$P_3$:** Completion $C = 21 \implies T_{\text{turn}} = 21 - 2 = 19\text{ ms}, \quad T_{\text{wait}} = 19 - 9 = 10\text{ ms}, \quad T_{\text{resp}} = 12 - 2 = 10\text{ ms}$
- **$P_4$:** Completion $C = 26 \implies T_{\text{turn}} = 26 - 3 = 23\text{ ms}, \quad T_{\text{wait}} = 23 - 5 = 18\text{ ms}, \quad T_{\text{resp}} = 21 - 3 = 18\text{ ms}$

$$\text{Average Turnaround Time} = \frac{8 + 11 + 19 + 23}{4} = \frac{61}{4} = \mathbf{15.25\text{ ms}}$$
$$\text{Average Waiting Time} = \frac{0 + 7 + 10 + 18}{4} = \frac{35}{4} = \mathbf{8.75\text{ ms}}$$

---

### Execution Trace & Gantt Chart:
- At $t = 0$: Only $P_1$ has arrived. $P_1$ is dispatched and runs to completion (non-preemptive!) from $t = 0$ to $t = 8$.
- At $t = 8$: $P_2 (B=4)$, $P_3 (B=9)$, and $P_4 (B=5)$ are all in the Ready Queue.
  - Shortest is $P_2 (4\text{ ms}) \implies P_2$ runs from $t = 8$ to $t = 12$.
- At $t = 12$: Ready Queue contains $P_4 (5\text{ ms})$ and $P_3 (9\text{ ms})$.
  - Shortest is $P_4 \implies P_4$ runs from $t = 12$ to $t = 17$.
- At $t = 17$: $P_3$ runs from $t = 17$ to $t = 26$.

| Interval (ms) | Running process |
|---|---|
| [0, 8) | $P_1$ |
| [8, 12) | $P_2$ |
| [12, 17) | $P_4$ |
| [17, 26) | $P_3$ |

### Metrics Calculation:
- **$P_1$:** $C = 8 \implies T_{\text{turn}} = 8 - 0 = 8\text{ ms}, \quad T_{\text{wait}} = 8 - 8 = 0\text{ ms}, \quad T_{\text{resp}} = 0\text{ ms}$
- **$P_2$:** $C = 12 \implies T_{\text{turn}} = 12 - 1 = 11\text{ ms}, \quad T_{\text{wait}} = 11 - 4 = 7\text{ ms}, \quad T_{\text{resp}} = 8 - 1 = 7\text{ ms}$
- **$P_3$:** $C = 26 \implies T_{\text{turn}} = 26 - 2 = 24\text{ ms}, \quad T_{\text{wait}} = 24 - 9 = 15\text{ ms}, \quad T_{\text{resp}} = 17 - 2 = 15\text{ ms}$
- **$P_4$:** $C = 17 \implies T_{\text{turn}} = 17 - 3 = 14\text{ ms}, \quad T_{\text{wait}} = 14 - 5 = 9\text{ ms}, \quad T_{\text{resp}} = 12 - 3 = 9\text{ ms}$

$$\text{Average Turnaround Time} = \frac{8 + 11 + 24 + 14}{4} = \frac{57}{4} = \mathbf{14.25\text{ ms}}$$
$$\text{Average Waiting Time} = \frac{0 + 7 + 15 + 9}{4} = \frac{31}{4} = \mathbf{7.75\text{ ms}}$$

---

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

| Interval (ms) | Running process |
|---|---|
| [0, 1) | $P_1$ |
| [1, 5) | $P_2$ |
| [5, 10) | $P_4$ |
| [10, 17) | $P_1$ |
| [17, 26) | $P_3$ |

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

| Interval (ms) | Running process |
|---|---|
| [0, 4) | $P_1$ |
| [4, 8) | $P_2$ |
| [8, 12) | $P_3$ |
| [12, 16) | $P_4$ |
| [16, 20) | $P_1$ |
| [20, 24) | $P_3$ |
| [24, 25) | $P_4$ |
| [25, 26) | $P_3$ |

### Metrics Calculation:
- **$P_1$:** Completion $C = 20 \implies T_{\text{turn}} = 20 - 0 = 20\text{ ms}, \quad T_{\text{wait}} = 20 - 8 = 12\text{ ms}, \quad T_{\text{resp}} = 0 - 0 = 0\text{ ms}$
- **$P_2$:** Completion $C = 8 \implies T_{\text{turn}} = 8 - 1 = 7\text{ ms}, \quad T_{\text{wait}} = 7 - 4 = 3\text{ ms}, \quad T_{\text{resp}} = 4 - 1 = 3\text{ ms}$
- **$P_3$:** Completion $C = 26 \implies T_{\text{turn}} = 26 - 2 = 24\text{ ms}, \quad T_{\text{wait}} = 24 - 9 = 15\text{ ms}, \quad T_{\text{resp}} = 8 - 2 = 6\text{ ms}$
- **$P_4$:** Completion $C = 25 \implies T_{\text{turn}} = 25 - 3 = 22\text{ ms}, \quad T_{\text{wait}} = 22 - 5 = 17\text{ ms}, \quad T_{\text{resp}} = 12 - 3 = 9\text{ ms}$

$$\text{Average Turnaround Time} = \frac{20 + 7 + 24 + 22}{4} = \frac{73}{4} = \mathbf{18.25\text{ ms}}$$
$$\text{Average Waiting Time} = \frac{12 + 3 + 15 + 17}{4} = \frac{47}{4} = \mathbf{11.75\text{ ms}}$$
$$\text{Average Response Time} = \frac{0 + 3 + 6 + 9}{4} = \frac{18}{4} = \mathbf{4.50\text{ ms}}$$

---

## Result

| Metric | FCFS | SJF (Non-Preemptive) | SRTF (Preemptive) | Round Robin ($q=4$) |
|---|---|---|---|---|
| **Average Turnaround Time** | $15.25\text{ ms}$ | $14.25\text{ ms}$ | **$13.00\text{ ms}$ (Best)** | $18.25\text{ ms}$ |
| **Average Waiting Time** | $8.75\text{ ms}$ | $7.75\text{ ms}$ | **$6.50\text{ ms}$ (Best)** | $11.75\text{ ms}$ |
| **Average Response Time** | $8.75\text{ ms}$ | $7.75\text{ ms}$ | $4.25\text{ ms}$ | **$4.50\text{ ms}$ (Fair & Fast)** |
| **Preemption Overhead** | None | None | 1 Preemption ($P_1$) | 4 Preemptions |

### Deep Insight:
- **SRTF** wins decisively on **average waiting time ($6.50\text{ ms}$)** and **turnaround time ($13.00\text{ ms}$)** because it relentlessly schedules the shortest remaining chunks.
- **Round Robin** has a higher turnaround time ($18.25\text{ ms}$) because long jobs are interleaved and prolonged, while reducing mean first-response delay relative to FCFS in this workload to $4.50\text{ ms}$. The SRTF response average here is slightly lower at $4.25\text{ ms}$; responsiveness depends on the workload and policy.

---

## What to carry forward

Check each job's accumulated intervals against its burst before computing averages. SRTF and RR answer different goals: finishing short remaining work versus giving repeated turns. The comparison table describes this workload; it does not prove that one policy dominates on every workload.

## Related notes

- [[Scheduling Metrics and Burst Estimation Formulas]]

## Sources

- Andrew S. Tanenbaum & Herbert Bos, *Modern Operating Systems* (4th Edition)
- Silberschatz et al., *Operating System Concepts* (10th Edition)
