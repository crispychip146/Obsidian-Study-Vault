---
type: formula
course: cse313
status: active
order: 15
---

# Scheduling Metrics and Burst Estimation Formulas

> 📖 **Reading Order:** Step 15 of 34 | **Module 3:** CPU Scheduling  
> ◄ **Previous:** [[Interactive Scheduling Algorithms]] | ► **Next:** [[Comprehensive CPU Scheduling Simulation Example]]

---

## Mathematical Statements

### 1. Fundamental Scheduling Performance Metrics

Let a process $P_i$ have:
- Arrival time: $A_i$ (the moment it enters the Ready Queue).
- CPU burst time: $B_i$ (the total CPU execution time required).
- First dispatch time: $F_i$ (the moment the CPU is first allocated to it).
- Completion time: $C_i$ (the moment it finishes its final instruction and exits).

```
Timeline:
----+----------------------+--------------------+-------------------> Time
    |                      |                    |
Arrival (A_i)       First Run (F_i)      Completion (C_i)
    |<-- Response Time --->|
    |<---------------- Turnaround Time -------->|
```

The standard performance metrics are defined as:

1. **Turnaround Time ($T_{\text{turn}}$):**
   The total elapsed wall-clock time from submission to completion:
   $$T_{\text{turn}, i} = C_i - A_i$$
   The **Average Turnaround Time** across $N$ processes is:
   $$\bar{T}_{\text{turn}} = \frac{1}{N} \sum_{i=1}^N T_{\text{turn}, i} = \frac{1}{N} \sum_{i=1}^N (C_i - A_i)$$

2. **Waiting Time ($T_{\text{wait}}$):**
   The total cumulative time a process spends waiting in the Ready Queue:
   $$T_{\text{wait}, i} = T_{\text{turn}, i} - B_i = (C_i - A_i) - B_i$$
   The **Average Waiting Time** across $N$ processes is:
   $$\bar{T}_{\text{wait}} = \frac{1}{N} \sum_{i=1}^N T_{\text{wait}, i}$$

3. **Response Time ($T_{\text{resp}}$):**
   The time from arrival until the process first gets scheduled on the CPU:
   $$T_{\text{resp}, i} = F_i - A_i$$
   *(Note: In non-preemptive algorithms like FCFS and SJF, $T_{\text{resp}} = T_{\text{wait}}$. In preemptive algorithms like Round Robin and SRTF, $T_{\text{resp}} \le T_{\text{wait}}$)*.

4. **Normalized Turnaround Time (Penalty Ratio / Relative Delay):**
   The ratio of turnaround time to actual service time:
   $$R_{\text{norm}, i} = \frac{T_{\text{turn}, i}}{B_i} \ge 1.0$$
   A penalty ratio of $1.0$ indicates zero waiting time (ideal). A ratio of $10.0$ means the process took 10 times longer to complete than its actual computation required due to queueing delays.

5. **System Throughput:**
   $$\text{Throughput} = \frac{N}{\max(C_1, \dots, C_N) - \min(A_1, \dots, A_N)}$$

---

### 2. Exponential Smoothing for CPU Burst Estimation

Because [[Batch Scheduling Algorithms|Shortest Job First (SJF)]] requires knowing the future duration of the next CPU burst $t_{n+1}$, the operating system estimates it using an **Exponentially Weighted Moving Average (EWMA)** of past bursts:

$$\tau_{n+1} = \alpha \, t_n + (1 - \alpha) \, \tau_n$$

where:
- $t_n$ is the **actual, observed duration** of the $n$-th CPU burst.
- $\tau_n$ was the **predicted duration** for the $n$-th CPU burst.
- $\tau_{n+1}$ is the **predicted duration** for the upcoming $(n+1)$-th CPU burst.
- $\alpha \in [0, 1]$ is the **smoothing factor** (weighting constant).
- $\tau_0$ is the initial baseline default estimate (e.g., $10\text{ ms}$).

---

## Derivation: Why Is It Called "Exponential" Smoothing?

To see why this formula is called *exponential*, expand the recurrence relation backwards:

$$\tau_{n+1} = \alpha t_n + (1 - \alpha) \tau_n$$
Substitute $\tau_n = \alpha t_{n-1} + (1 - \alpha) \tau_{n-1}$:
$$\tau_{n+1} = \alpha t_n + (1 - \alpha)\left[ \alpha t_{n-1} + (1 - \alpha) \tau_{n-1} \right]$$
$$= \alpha t_n + \alpha(1 - \alpha) t_{n-1} + (1 - \alpha)^2 \tau_{n-1}$$

Continuing this substitution down to the initial prior $\tau_0$:
$$\tau_{n+1} = \alpha t_n + \alpha(1 - \alpha) t_{n-1} + \alpha(1 - \alpha)^2 t_{n-2} + \dots + \alpha(1 - \alpha)^j t_{n-j} + \dots + (1 - \alpha)^{n+1} \tau_0$$

### Key Insight:
Because $(1 - \alpha) < 1$, the coefficient weights $\alpha(1 - \alpha)^j$ decay **exponentially** with the age of the observation $j$:
- Recent burst $t_n$ has weight $\alpha$.
- Prior burst $t_{n-1}$ has weight $\alpha(1 - \alpha)$.
- Burst from 5 steps ago has weight $\alpha(1 - \alpha)^5 \ll \alpha$.

Past history is remembered, but its influence fades away exponentially! $\blacksquare$

---

## Choosing the Smoothing Factor $\alpha$

The value of $\alpha$ controls how rapidly the scheduler adapts to changing process behavior:

| Value of $\alpha$ | Mathematical Formula | Physical Meaning | Behavior |
|---|---|---|---|
| **$\alpha = 0$** | $\tau_{n+1} = \tau_n = \tau_0$ | Recent actual burst history is **completely ignored**; prediction remains constant forever. | Completely unresponsive; frozen guess. |
| **$\alpha = 1$** | $\tau_{n+1} = t_n$ | History is completely ignored; only the **single most recent burst** matters. | Highly volatile; over-reacts to transient spikes. |
| **$\alpha = 0.5$** | $\tau_{n+1} = 0.5 t_n + 0.5 \tau_n$ | Equal weight given to the latest observed burst and the accumulated historical trend. | **Standard OS choice**; balances stability with agility. |

---

## Worked Numerical Example of Exponential Smoothing

Suppose $\tau_0 = 10\text{ ms}$, $\alpha = 0.5$, and a process executes with actual observed bursts:
$$t_0 = 6\text{ ms}, \quad t_1 = 4\text{ ms}, \quad t_2 = 16\text{ ms}$$

1. **Calculate $\tau_1$:**
   $$\tau_1 = 0.5(t_0) + 0.5(\tau_0) = 0.5(6) + 0.5(10) = 3 + 5 = 8\text{ ms}$$
2. **Calculate $\tau_2$:**
   $$\tau_2 = 0.5(t_1) + 0.5(\tau_1) = 0.5(4) + 0.5(8) = 2 + 4 = 6\text{ ms}$$
3. **Calculate $\tau_3$:**
   $$\tau_3 = 0.5(t_2) + 0.5(\tau_2) = 0.5(16) + 0.5(6) = 8 + 3 = 11\text{ ms}$$

Notice how the prediction smoothly adjusts from $10 \to 8 \to 6$, and then climbs to $11$ when the burst surges to $16$.

---

## Related Notes

- [[Batch Scheduling Algorithms]] — Where $\tau_{n+1}$ is utilized for Shortest Job First sorting.
- [[Comprehensive CPU Scheduling Simulation Example]] — Step-by-step Gantt charts using these turnaround and waiting time formulas.

---

## Sources & Traceability

- **Lectures:** `cse313/01 - Sources/Lectures/3. Scheduling-week-3-RRR.pdf` (Slides 13–24)
- **Textbook:** Andrew S. Tanenbaum & Herbert Bos, *Modern Operating Systems* (4th Edition), Chapter 2 (Section 2.4.2)
