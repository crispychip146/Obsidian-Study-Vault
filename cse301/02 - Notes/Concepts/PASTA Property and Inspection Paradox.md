---
type: concept
course: cse301
status: active
order: 84
---

# PASTA Property and Inspection Paradox

> 📖 **Reading Order:** Step 84 of 92 | **Module 11:** Queuing Theory  
> ◄ **Previous:** [[Little's Law]] | ► **Next:** [[M-M-1 Queue]]

---

## Building the idea

A time observer and an arriving customer need not see the same system. PASTA says that suitable external Poisson arrivals see time-average state probabilities. The Poisson arrival mechanism must not anticipate the system's future or preferentially arrive in selected states.

Thus, in a stationary finite-capacity queue, the probability an arrival finds the system full equals the stationary full-state probability. This connects a time-average state distribution to customer loss.

The inspection paradox is a different sampling effect. Observing an ongoing interval at a random time favors longer intervals because they occupy more of the timeline. Consequently the interval you encounter can be longer on average than one sampled uniformly from completed intervals. For a renewal process with suitable finite moments, mean residual life is $E[S^2]/(2E[S])$. Exponential intervals are special because memorylessness preserves their residual distribution.

## Definition

In queueing systems, the state of the system can look vastly different depending on **how** and **when** it is observed. We track three distinct steady-state probability distributions:

1. **Time-Average Probability ($P_n$):**
   The proportion of continuous clock time that the system contains exactly $n$ customers:
   $$P_n = \lim_{t \to \infty} \frac{1}{t} \int_0^t \mathbf{1}_{\{X(u) = n\}} du$$
2. **Arrival-Seen Probability ($a_n$):**
   The long-run proportion of arriving customers who find exactly $n$ customers in the system upon their arrival:
   $$a_n = \lim_{t \to \infty} \frac{\text{number of arrivals in } [0, t] \text{ that find } n \text{ customers}}{\text{total arrivals in } [0, t]}$$
3. **Departure-Seen Probability ($d_n$):**
   The long-run proportion of departing customers who leave behind exactly $n$ customers in the system upon their departure:
   $$d_n = \lim_{t \to \infty} \frac{\text{number of departures in } [0, t] \text{ that leave } n \text{ customers}}{\text{total departures in } [0, t]}$$

---

## How It Works

### Proposition 1: Arrivals and Departures See the Same Rates ($a_n = d_n$)

> **Theorem:** 
> For any queueing system where customers enter and leave the system **one at a time** (no batch arrivals or batch departures), the steady-state arrival-seen distribution equals the departure-seen distribution:
> $$a_n = d_n \quad \text{for all } n \ge 0$$

### Proof Intuition
- An arriving customer finds $n$ customers in the system if and only if the system state jumps from $n \to n + 1$ (an **up-crossing** of state $n$).
- A departing customer leaves $n$ customers behind if and only if the system state drops from $n + 1 \to n$ (a **down-crossing** of state $n$).

Because the system changes state in single-unit increments, the state cannot transition from $n$ to $n+1$ a second time without first transitioning back from $n+1$ to $n$. Therefore, between any two up-crossings from $n \to n+1$, there must be a down-crossing from $n+1 \to n$. Over any time interval $[0, t]$:
$$\lvert \text{Up-crossings}(n \to n+1) - \text{Down-crossings}(n+1 \to n) \rvert \le 1$$

Dividing both counts by the total number of transitions and taking $t \to \infty$:
$$\lim_{t \to \infty} \frac{\text{Up-crossings}}{A(t)} = \lim_{t \to \infty} \frac{\text{Down-crossings}}{D(t)} \implies a_n = d_n \quad \blacksquare$$

---
### Proposition 2: PASTA (Poisson Arrivals See Time Averages)

> **Theorem (PASTA):** 
> If customers arrive according to a **Poisson process** (independent exponential interarrival times), then the distribution seen by an arriving customer is **identically equal to the continuous time-average distribution**:
> $$a_n = P_n \quad \text{for all } n \ge 0$$

### Why PASTA Holds (Mathematical Intuition)
A Poisson process possesses the **independent increments property**:
- The probability that an arrival occurs in the infinitesimal interval $[t, t + h)$ is $\lambda h + o(h)$.
- Crucially, the occurrence of an arrival in $[t, t + h)$ is **completely independent of the entire past history** of the system prior to time $t$.

Because knowing that an arrival is occurring at time $t$ provides zero information about what happened before time $t$, conditioning on an arrival taking place gives the exact same probability as an unconditioned, random snapshot of the system:
$$P(X(t) = n \mid \text{arrival at } t) = P(X(t) = n) = P_n \implies a_n = P_n \quad \blacksquare$$

---
### The Inspection Paradox

The contrast between $a_n$ and $P_n$ is closely related to the famous **Inspection Paradox** (or waiting time paradox) in renewal theory:
- If buses arrive randomly at a bus stop with an average headway of $10$ minutes, a passenger arriving at a random time will, on average, wait **more than 5 minutes** (often the full $10$ minutes).
- Why? A randomly arriving passenger is more likely to fall into an unusually long interarrival interval than an unusually short one (sampling proportional to length).

---
### Why PASTA is Fundamental to Queueing Analysis

PASTA provides the magical bridge that allows queueing theorists to solve complex systems:
- It is mathematically much easier to write differential balance equations for the **time-average continuous probabilities $P_n$**.
- But system managers care about the **customer experience $a_n$** (e.g., what percentage of callers find the phone line busy and get dropped?).
- PASTA guarantees that under Poisson arrivals, customer experience matches continuous time averages:
  $$P(\text{customer is blocked}) = a_N = P_N$$

---

## Exam Relevance

### When Does PASTA Fail? (Counterexample)

When arrivals do **not** follow a Poisson process, $a_n$ and $P_n$ can be wildly divergent.

### Counterexample: Regular Deterministic Arrivals (D/D/1)
Consider a doctor's office with 1 server:
- Patients arrive punctually every $10$ minutes (deterministic $D$).
- Each appointment takes exactly $9$ minutes (deterministic $D$).

1. **What an arrival sees ($a_n$):**
   Every patient arrives at minute $0, 10, 20, \dots$. The previous patient departed at minute $9, 19, 29, \dots$.
   Therefore, **every single arriving patient finds an empty office**:
   $$a_0 = 1.0 \quad (100\%), \quad a_1 = 0, \quad a_2 = 0, \dots$$
2. **What a continuous camera sees ($P_n$):**
   During every 10-minute block, the office is occupied for 9 minutes and empty for 1 minute:
   $$P_1 = \frac{9}{10} = 0.90 \quad (90\%), \quad P_0 = \frac{1}{10} = 0.10 \quad (10\%)$$

Notice the stark contradiction:
$$a_0 = 1.0 \ne P_0 = 0.10$$
Arriving patients believe the office is empty 100% of the time, while an external observer sees the office full 90% of the time!
This discrepancy occurs because deterministic arrivals are synchronized with the system state, destroying independence. **PASTA holds only when arrivals are Poisson.**

---

## What to carry forward

[[Exponential Distribution Memorylessness Example]] explains the exponential case. PASTA concerns arrivals observing states; length bias concerns sampling intervals. Neither follows just from calling a system “random.”

## Related notes

- [[Exponential Distribution Memorylessness Example]]

## Sources

- [[01 - Sources/Lectures/CSE301_Queueing_Theory.pdf]]
