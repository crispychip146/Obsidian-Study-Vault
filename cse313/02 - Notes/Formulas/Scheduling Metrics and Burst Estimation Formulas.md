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

## Building the idea

A Gantt chart records who executes and when. The metrics in [[CPU Scheduling Principles and Criteria]] are different readings of that same timeline. Subtract arrival from completion to measure the whole stay; subtract arrival from first execution to measure the initial delay.

For a CPU-only job, the whole stay consists of its CPU service and its ready-queue waiting. Removing service gives $WT=CT-AT-BT$. If the model includes blocking for I/O, removing CPU service alone also leaves blocked time, so it no longer isolates ready waiting.

Burst prediction answers a separate question: how should a scheduler guess the next burst? Let its old estimate be $\tau_n$ and the newest observation be $t_n$. The update can be written $\tau_{n+1}=\tau_n+\alpha(t_n-\tau_n)$. It moves the estimate partway toward what just happened. Expanding the recurrence explains the familiar weighted-sum form: each older observation is multiplied by another factor of $1-\alpha$, so its influence fades.

At $\alpha=0$, no observation changes the estimate. At $\alpha=1$, the next prediction is simply the last burst. These endpoints explain the parameter without treating a chosen intermediate value as a universal OS setting.

## Formula

### Core Scheduling Performance Metrics:
$$\text{Turnaround Time } (T_{\text{TAT}}) = T_{\text{completion}} - T_{\text{arrival}}$$

$$\text{Waiting Time } (T_{\text{wait}}) = T_{\text{TAT}} - T_{\text{burst}}$$

$$\text{Response Time } (T_{\text{resp}}) = T_{\text{first\_execution}} - T_{\text{arrival}}$$

$$\text{Throughput} = \frac{\text{Total Completed Processes}}{\text{Total Elapsed Time}}$$

### Exponential Smoothing Burst Estimation Formula:
$$\tau_{n+1} = \alpha t_n + (1 - \alpha) \tau_n$$

---

## Variables

| Symbol | Meaning |
|---|---|
| $T_{\text{completion}}$ | Timestamp when process finishes execution |
| $T_{\text{arrival}}$ | Timestamp when process enters the Ready Queue |
| $T_{\text{burst}}$ | Total CPU time required by the process |
| $t_n$ | Measured duration of the most recent ($n$-th) CPU burst |
| $\tau_n$ | Predicted duration of the $n$-th CPU burst |
| $\tau_{n+1}$ | Predicted duration of the upcoming ($n+1$-th) CPU burst |
| $\alpha$ | Smoothing factor ($0 \le \alpha \le 1$), typically $\alpha = 0.5$ |

---

## Conditions

- Metric calculations require discrete arrival and completion timestamps from a Gantt chart.
- Burst estimation assumes process burst behaviors exhibit temporal locality (recent past predicts near future).

---

## Intuition

### Choosing the Smoothing Factor $\alpha$

The value of $\alpha$ controls how rapidly the scheduler adapts to changing process behavior:

| Value of $\alpha$ | Mathematical Formula | Physical Meaning | Behavior |
|---|---|---|---|
| **$\alpha = 0$** | $\tau_{n+1} = \tau_n = \tau_0$ | Recent actual burst history is **completely ignored**; prediction remains constant forever. | Completely unresponsive; frozen guess. |
| **$\alpha = 1$** | $\tau_{n+1} = t_n$ | History is completely ignored; only the **single most recent burst** matters. | Highly volatile; over-reacts to transient spikes. |
| **$\alpha = 0.5$** | $\tau_{n+1} = 0.5 t_n + 0.5 \tau_n$ | Equal weight given to the latest observed burst and the accumulated historical trend. | **Standard OS choice**; balances stability with agility. |

---

## Derivation

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

## Example

### Worked Numerical Example of Exponential Smoothing

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

## Common Mistakes

- Calculating Waiting Time as $T_{\text{completion}} - T_{\text{arrival}}$ (which is Turnaround Time!). Waiting Time is strictly Turnaround Time minus Burst Time.
- Confusing Response Time (time until first CPU allocation) with Turnaround Time (time until final completion).

---

## What to carry forward

Check metrics against units and the chart: waiting must be nonnegative, first execution cannot precede arrival, and total executed service must match each burst. [[Comprehensive CPU Scheduling Simulation Example]] turns these bookkeeping identities into an algorithm comparison.

## Related notes

- [[CPU Scheduling Principles and Criteria]]
- [[Comprehensive CPU Scheduling Simulation Example]]

## Sources

- **Lectures:** `cse313/01 - Sources/Lectures/3. Scheduling-week-3-RRR.pdf` (Slides 13–24)
- **Textbook:** Andrew S. Tanenbaum & Herbert Bos, *Modern Operating Systems* (4th Edition), Chapter 2 (Section 2.4.2)
