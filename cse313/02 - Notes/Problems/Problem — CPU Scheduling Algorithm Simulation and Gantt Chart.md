---
type: problem
course: cse313
status: active
order: 17
---

# Problem — CPU Scheduling Algorithm Simulation and Gantt Chart

> 📖 **Reading Order:** Step 17 of 34 | **Module 3:** CPU Scheduling  
> ◄ **Previous:** [[Comprehensive CPU Scheduling Simulation Example]] | ► **Next:** [[Race Conditions and Critical-Section Problem]]

---

## Problem Statement

A multiprogrammed operating system has a single CPU core and five processes arriving in the ready queue. The arrival times, CPU burst times, and process priorities are listed below:

| Process | Arrival Time ($A_i$) | CPU Burst Time ($B_i$) | Priority (Lower number = Higher priority) |
|---|---|---|---|
| **$P_1$** | 0 ms | 10 ms | 3 |
| **$P_2$** | 1 ms | 4 ms | 1 |
| **$P_3$** | 2 ms | 5 ms | 4 |
| **$P_4$** | 3 ms | 2 ms | 2 |
| **$P_5$** | 4 ms | 1 ms | 5 |

### Tasks:
1. Draw the execution Gantt chart for:
   - **(a)** First-Come, First-Served (**FCFS**)
   - **(b)** Shortest Job First (**SJF**, Non-preemptive)
   - **(c)** Shortest Remaining Time First (**SRTF**, Preemptive SJF)
   - **(d)** Preemptive Priority Scheduling
   - **(e)** Round Robin (**RR**) with Time Quantum $q = 3\text{ ms}$ (assume newly arriving processes at time $t$ enter the ready queue *before* a process whose quantum just expired at time $t$).
2. For each scheduling algorithm, construct a calculation table showing:
   - Completion Time ($C_i$)
   - Turnaround Time ($T_{\text{turn}, i} = C_i - A_i$)
   - Waiting Time ($T_{\text{wait}, i} = T_{\text{turn}, i} - B_i$)
   - Response Time ($T_{\text{resp}, i} = \text{First Run Time} - A_i$)
3. Compute the average turnaround time ($\bar{T}_{\text{turn}}$) and average waiting time ($\bar{T}_{\text{wait}}$) for each algorithm.
4. Explain how changing the Round Robin time quantum from $q = 3\text{ ms}$ to $q = 1\text{ ms}$ affects system throughput, turnaround time, and context-switch overhead.

---

## Detailed Step-by-Step Solutions

### Part 1: First-Come, First-Served (FCFS)

#### Execution Trace:
- $t = 0$: $P_1$ arrives and executes until completion ($t = 10$).
- $t = 10$: $P_2$ executes until completion ($t = 10 + 4 = 14$).
- $t = 14$: $P_3$ executes until completion ($t = 14 + 5 = 19$).
- $t = 19$: $P_4$ executes until completion ($t = 19 + 2 = 21$).
- $t = 21$: $P_5$ executes until completion ($t = 21 + 1 = 22$).

#### Gantt Chart (FCFS):
```
|        P1        |    P2    |     P3     |   P4   | P5 |
0                 10         14           19       21   22
```

#### Metrics Table:

| Process | $A_i$ | $B_i$ | Completion ($C_i$) | Turnaround ($T_{\text{turn}}$) | Waiting ($T_{\text{wait}}$) | Response ($T_{\text{resp}}$) |
|---|---|---|---|---|---|---|
| $P_1$ | 0 | 10 | 10 | $10 - 0 = 10$ | $10 - 10 = 0$ | $0 - 0 = 0$ |
| $P_2$ | 1 | 4 | 14 | $14 - 1 = 13$ | $13 - 4 = 9$ | $10 - 1 = 9$ |
| $P_3$ | 2 | 5 | 19 | $19 - 2 = 17$ | $17 - 5 = 12$ | $14 - 2 = 12$ |
| $P_4$ | 3 | 2 | 21 | $21 - 3 = 18$ | $18 - 2 = 16$ | $19 - 3 = 16$ |
| $P_5$ | 4 | 1 | 22 | $22 - 4 = 18$ | $18 - 1 = 17$ | $21 - 4 = 17$ |

- **Average Turnaround Time ($\bar{T}_{\text{turn}}$):** $\frac{10 + 13 + 17 + 18 + 18}{5} = \frac{76}{5} = \mathbf{15.20\text{ ms}}$
- **Average Waiting Time ($\bar{T}_{\text{wait}}$):** $\frac{0 + 9 + 12 + 16 + 17}{5} = \frac{54}{5} = \mathbf{10.80\text{ ms}}$
- **Average Response Time ($\bar{T}_{\text{resp}}$):** $\frac{0 + 9 + 12 + 16 + 17}{5} = \mathbf{10.80\text{ ms}}$

---

### Part 2: Non-Preemptive Shortest Job First (SJF)

#### Execution Trace:
- $t = 0$: Only $P_1$ is in ready queue. It runs until completion at $t = 10$.
- $t = 10$: All processes $P_2, P_3, P_4, P_5$ have arrived.
  - $B_2 = 4$, $B_3 = 5$, $B_4 = 2$, $B_5 = 1$.
  - Shortest is $P_5$ ($B = 1$). $P_5$ runs from $t = 10$ to $t = 11$.
- $t = 11$: Remaining ready: $P_2 (4)$, $P_3 (5)$, $P_4 (2)$.
  - Shortest is $P_4$ ($B = 2$). $P_4$ runs from $t = 11$ to $t = 13$.
- $t = 13$: Remaining ready: $P_2 (4)$, $P_3 (5)$.
  - Shortest is $P_2$ ($B = 4$). $P_2$ runs from $t = 13$ to $t = 17$.
- $t = 17$: Remaining ready: $P_3 (5)$.
  - $P_3$ runs from $t = 17$ to $t = 22$.

#### Gantt Chart (SJF):
```
|        P1        | P5 |   P4   |    P2    |     P3     |
0                 10   11       13        17           22
```

#### Metrics Table:

| Process | $A_i$ | $B_i$ | Completion ($C_i$) | Turnaround ($T_{\text{turn}}$) | Waiting ($T_{\text{wait}}$) | Response ($T_{\text{resp}}$) |
|---|---|---|---|---|---|---|
| $P_1$ | 0 | 10 | 10 | $10 - 0 = 10$ | $10 - 10 = 0$ | $0 - 0 = 0$ |
| $P_2$ | 1 | 4 | 17 | $17 - 1 = 16$ | $16 - 4 = 12$ | $13 - 1 = 12$ |
| $P_3$ | 2 | 5 | 22 | $22 - 2 = 20$ | $20 - 5 = 15$ | $17 - 2 = 15$ |
| $P_4$ | 3 | 2 | 13 | $13 - 3 = 10$ | $10 - 2 = 8$ | $11 - 3 = 8$ |
| $P_5$ | 4 | 1 | 11 | $11 - 4 = 7$ | $7 - 1 = 6$ | $10 - 4 = 6$ |

- **Average Turnaround Time ($\bar{T}_{\text{turn}}$):** $\frac{10 + 16 + 20 + 10 + 7}{5} = \frac{63}{5} = \mathbf{12.60\text{ ms}}$
- **Average Waiting Time ($\bar{T}_{\text{wait}}$):** $\frac{0 + 12 + 15 + 8 + 6}{5} = \frac{41}{5} = \mathbf{8.20\text{ ms}}$

---

### Part 3: Shortest Remaining Time First (SRTF)

#### Execution Trace:
- $t = 0$: $P_1$ arrives, remaining $= 10$. $P_1$ runs.
- $t = 1$: $P_2$ arrives ($B_2 = 4$). $P_1$ has remaining $= 9$. Since $4 < 9$, $P_1$ is **preempted**! $P_2$ starts running.
- $t = 2$: $P_3$ arrives ($B_3 = 5$). $P_2$ has remaining $= 3$. Since $3 < 5$, $P_2$ continues.
- $t = 3$: $P_4$ arrives ($B_4 = 2$). $P_2$ has remaining $= 2$. Equal remaining times ($2 = 2$), $P_2$ continues.
- $t = 4$: $P_5$ arrives ($B_5 = 1$). $P_2$ has remaining $= 1$. Tie ($1 = 1$), $P_2$ continues.
- $t = 5$: $P_2$ completes ($C_2 = 5$).
  - Remaining pool: $P_1 (9)$, $P_3 (5)$, $P_4 (2)$, $P_5 (1)$.
  - Shortest is $P_5$ ($1$). $P_5$ runs until $t = 6$.
- $t = 6$: $P_5$ completes ($C_5 = 6$).
  - Remaining pool: $P_1 (9)$, $P_3 (5)$, $P_4 (2)$.
  - Shortest is $P_4$ ($2$). $P_4$ runs until $t = 8$.
- $t = 8$: $P_4$ completes ($C_4 = 8$).
  - Remaining pool: $P_1 (9)$, $P_3 (5)$.
  - Shortest is $P_3$ ($5$). $P_3$ runs until $t = 13$.
- $t = 13$: $P_3$ completes ($C_3 = 13$).
  - Remaining: $P_1 (9)$. $P_1$ runs until $t = 22$.
- $t = 22$: $P_1$ completes ($C_1 = 22$).

#### Gantt Chart (SRTF):
```
| P1 |    P2    | P5 |   P4   |     P3     |        P1        |
0    1          5    6        8           13                 22
```

#### Metrics Table:

| Process | $A_i$ | $B_i$ | Completion ($C_i$) | Turnaround ($T_{\text{turn}}$) | Waiting ($T_{\text{wait}}$) | First Run | Response ($T_{\text{resp}}$) |
|---|---|---|---|---|---|---|---|
| $P_1$ | 0 | 10 | 22 | $22 - 0 = 22$ | $22 - 10 = 12$ | 0 | $0 - 0 = 0$ |
| $P_2$ | 1 | 4 | 5 | $5 - 1 = 4$ | $4 - 4 = 0$ | 1 | $1 - 1 = 0$ |
| $P_3$ | 2 | 5 | 13 | $13 - 2 = 11$ | $11 - 5 = 6$ | 8 | $8 - 2 = 6$ |
| $P_4$ | 3 | 2 | 8 | $8 - 3 = 5$ | $5 - 2 = 3$ | 6 | $6 - 3 = 3$ |
| $P_5$ | 4 | 1 | 6 | $6 - 4 = 2$ | $2 - 1 = 1$ | 5 | $5 - 4 = 1$ |

- **Average Turnaround Time ($\bar{T}_{\text{turn}}$):** $\frac{22 + 4 + 11 + 5 + 2}{5} = \frac{44}{5} = \mathbf{8.80\text{ ms}}$
- **Average Waiting Time ($\bar{T}_{\text{wait}}$):** $\frac{12 + 0 + 6 + 3 + 1}{5} = \frac{22}{5} = \mathbf{4.40\text{ ms}}$
- **Average Response Time ($\bar{T}_{\text{resp}}$):** $\frac{0 + 0 + 6 + 3 + 1}{5} = \frac{10}{5} = \mathbf{2.00\text{ ms}}$

---

### Part 4: Preemptive Priority Scheduling (Lower Number = Higher Priority)

#### Execution Trace:
- $t = 0$: $P_1$ (priority 3) starts running.
- $t = 1$: $P_2$ (priority 1) arrives. Since $1 < 3$, $P_2$ has higher priority. $P_1$ preempted ($P_1$ remaining = 9). $P_2$ runs.
- $t = 2$: $P_3$ (priority 4) arrives. $P_2$ priority (1) > 4 $\implies P_2$ continues.
- $t = 3$: $P_4$ (priority 2) arrives. $P_2$ priority (1) > 2 $\implies P_2$ continues.
- $t = 4$: $P_5$ (priority 5) arrives. $P_2$ priority (1) > 5 $\implies P_2$ continues.
- $t = 5$: $P_2$ completes ($C_2 = 5$).
  - Ready pool priorities: $P_4$ (2), $P_1$ (3), $P_3$ (4), $P_5$ (5).
  - Highest priority is $P_4$ (priority 2, burst 2). $P_4$ runs from $t = 5$ to $t = 7$.
- $t = 7$: $P_4$ completes ($C_4 = 7$).
  - Ready pool priorities: $P_1$ (3, remaining 9), $P_3$ (4, burst 5), $P_5$ (5, burst 1).
  - Highest priority is $P_1$ (priority 3). $P_1$ runs from $t = 7$ to $t = 16$.
- $t = 16$: $P_1$ completes ($C_1 = 16$).
  - Highest priority is $P_3$ (priority 4, burst 5). $P_3$ runs from $t = 16$ to $t = 21$.
- $t = 21$: $P_3$ completes ($C_3 = 21$).
  - Highest priority is $P_5$ (priority 5, burst 1). $P_5$ runs from $t = 21$ to $t = 22$.
- $t = 22$: $P_5$ completes ($C_5 = 22$).

#### Gantt Chart (Priority):
```
| P1 |    P2    |   P4   |        P1        |     P3     | P5 |
0    1          5        7                 16           21   22
```

#### Metrics Table:

| Process | $A_i$ | $B_i$ | Priority | Completion ($C_i$) | Turnaround ($T_{\text{turn}}$) | Waiting ($T_{\text{wait}}$) | Response ($T_{\text{resp}}$) |
|---|---|---|---|---|---|---|---|
| $P_1$ | 0 | 10 | 3 | 16 | $16 - 0 = 16$ | $16 - 10 = 6$ | 0 |
| $P_2$ | 1 | 4 | 1 | 5 | $5 - 1 = 4$ | $4 - 4 = 0$ | $1 - 1 = 0$ |
| $P_3$ | 2 | 5 | 4 | 21 | $21 - 2 = 19$ | $19 - 5 = 14$ | $16 - 2 = 14$ |
| $P_4$ | 3 | 2 | 2 | 7 | $7 - 3 = 4$ | $4 - 2 = 2$ | $5 - 3 = 2$ |
| $P_5$ | 4 | 1 | 5 | 22 | $22 - 4 = 18$ | $18 - 1 = 17$ | $21 - 4 = 17$ |

- **Average Turnaround Time ($\bar{T}_{\text{turn}}$):** $\frac{16 + 4 + 19 + 4 + 18}{5} = \frac{61}{5} = \mathbf{12.20\text{ ms}}$
- **Average Waiting Time ($\bar{T}_{\text{wait}}$):** $\frac{6 + 0 + 14 + 2 + 17}{5} = \frac{39}{5} = \mathbf{7.80\text{ ms}}$

---

### Part 5: Round Robin (RR, $q = 3\text{ ms}$)

#### Step-by-Step Queue Tracing:
- $t = 0$: $P_1$ arrives. Queue: $[P_1]$.
  - $P_1$ runs for quantum 3 ms (until $t = 3$). Remaining: $10 - 3 = 7$.
  - Arrivals during $[0, 3]$: $P_2$ at 1, $P_3$ at 2, $P_4$ at 3.
  - Queue after $t = 3$: $[P_2, P_3, P_4, P_1]$ (incoming $P_4$ queued before expired $P_1$).
- $t = 3$: $P_2$ runs for quantum 3 ms (until $t = 6$). Remaining: $4 - 3 = 1$.
  - Arrivals during $[3, 6]$: $P_5$ at 4.
  - Queue after $t = 6$: $[P_3, P_4, P_1, P_5, P_2]$.
- $t = 6$: $P_3$ runs for quantum 3 ms (until $t = 9$). Remaining: $5 - 3 = 2$.
  - Queue after $t = 9$: $[P_4, P_1, P_5, P_2, P_3]$.
- $t = 9$: $P_4$ runs. Its burst is only $2\text{ ms} \le 3$.
  - Runs from $t = 9$ to $t = 11$. $P_4$ completes! ($C_4 = 11$).
  - Queue after $t = 11$: $[P_1, P_5, P_2, P_3]$.
- $t = 11$: $P_1$ runs for quantum 3 ms (until $t = 14$). Remaining: $7 - 3 = 4$.
  - Queue after $t = 14$: $[P_5, P_2, P_3, P_1]$.
- $t = 14$: $P_5$ runs. Its burst is $1\text{ ms} \le 3$.
  - Runs from $t = 14$ to $t = 15$. $P_5$ completes! ($C_5 = 15$).
  - Queue after $t = 15$: $[P_2, P_3, P_1]$.
- $t = 15$: $P_2$ runs. Its remaining burst is $1\text{ ms} \le 3$.
  - Runs from $t = 15$ to $t = 16$. $P_2$ completes! ($C_2 = 16$).
  - Queue after $t = 16$: $[P_3, P_1]$.
- $t = 16$: $P_3$ runs. Its remaining burst is $2\text{ ms} \le 3$.
  - Runs from $t = 16$ to $t = 18$. $P_3$ completes! ($C_3 = 18$).
  - Queue after $t = 18$: $[P_1]$.
- $t = 18$: $P_1$ runs. Remaining burst is 4 ms.
  - Runs quantum 3 ms (until $t = 21$). Remaining: 1 ms.
  - Runs final 1 ms (until $t = 22$). $P_1$ completes! ($C_1 = 22$).

#### Gantt Chart (RR, $q = 3$):
```
|  P1  |  P2  |  P3  |  P4  |  P1  | P5 | P2 |  P3  |     P1     |
0      3      6      9     11     14   15   16     18           22
```

#### Metrics Table:

| Process | $A_i$ | $B_i$ | Completion ($C_i$) | Turnaround ($T_{\text{turn}}$) | Waiting ($T_{\text{wait}}$) | First Run | Response ($T_{\text{resp}}$) |
|---|---|---|---|---|---|---|---|
| $P_1$ | 0 | 10 | 22 | $22 - 0 = 22$ | $22 - 10 = 12$ | 0 | $0 - 0 = 0$ |
| $P_2$ | 1 | 4 | 16 | $16 - 1 = 15$ | $15 - 4 = 11$ | 3 | $3 - 1 = 2$ |
| $P_3$ | 2 | 5 | 18 | $18 - 2 = 16$ | $16 - 5 = 11$ | 6 | $6 - 2 = 4$ |
| $P_4$ | 3 | 2 | 11 | $11 - 3 = 8$ | $8 - 2 = 6$ | 9 | $9 - 3 = 6$ |
| $P_5$ | 4 | 1 | 15 | $15 - 4 = 11$ | $11 - 1 = 10$ | 14 | $14 - 4 = 10$ |

- **Average Turnaround Time ($\bar{T}_{\text{turn}}$):** $\frac{22 + 15 + 16 + 8 + 11}{5} = \frac{72}{5} = \mathbf{14.40\text{ ms}}$
- **Average Waiting Time ($\bar{T}_{\text{wait}}$):** $\frac{12 + 11 + 11 + 6 + 10}{5} = \frac{50}{5} = \mathbf{10.00\text{ ms}}$
- **Average Response Time ($\bar{T}_{\text{resp}}$):** $\frac{0 + 2 + 4 + 6 + 10}{5} = \frac{22}{5} = \mathbf{4.40\text{ ms}}$

---

### Part 6: Algorithm Comparison Summary

| Algorithm | $\bar{T}_{\text{turn}}$ (ms) | $\bar{T}_{\text{wait}}$ (ms) | $\bar{T}_{\text{resp}}$ (ms) | Preemptive? | Starvation Possible? |
|---|---|---|---|---|---|
| **FCFS** | 15.20 | 10.80 | 10.80 | No | No |
| **SJF (Non-preemptive)** | 12.60 | 8.20 | 8.20 | No | Yes |
| **SRTF (Preemptive SJF)** | **8.80** | **4.40** | **2.00** | Yes | Yes |
| **Preemptive Priority** | 12.20 | 7.80 | 6.60 | Yes | Yes (low priority) |
| **Round Robin ($q = 3$)** | 14.40 | 10.00 | 4.40 | Yes | No |

**Key Takeaways:**
1. **SRTF achieves optimal average waiting and turnaround times** ($\bar{T}_{\text{wait}} = 4.40\text{ ms}$, $\bar{T}_{\text{turn}} = 8.80\text{ ms}$), cutting FCFS delays by more than half.
2. **Round Robin provides fair responsiveness** for interactive tasks ($\bar{T}_{\text{resp}} = 4.40\text{ ms}$ vs $10.80\text{ ms}$ in FCFS), ensuring every ready process gets CPU attention without starvation.
3. **Impact of Quantum Size Reduction ($q = 3\text{ ms} \to q = 1\text{ ms}$):**
   - *Advantage:* Response time improves even further; short tasks finish rapidly.
   - *Disadvantage:* Context switch overhead escalates dramatically. If each context switch takes $s = 0.1\text{ ms}$, 22 switches waste $2.2\text{ ms}$ of pure CPU time, reducing overall CPU efficiency ($\frac{\text{CPU Burst}}{\text{CPU Burst} + \text{Overhead}}$). Turnaround time degrades when quantum becomes too small relative to context switch cost.

---

## Source Traceability & Metadata
- **Source Material:** `3. Scheduling-week-3-RRR.pdf` (Slides 12–50: Criteria, FCFS, SJF, SRTF, Priority, Round Robin).
- **Question ID:** `Q-CSE313-002`
- **Related Notes:**
  - Concept: [[CPU Scheduling Principles and Criteria]]
  - Algorithms: [[Batch Scheduling Algorithms]], [[Interactive Scheduling Algorithms]]
  - Formula: [[Scheduling Metrics and Burst Estimation Formulas]]
  - Example: [[Comprehensive CPU Scheduling Simulation Example]]
