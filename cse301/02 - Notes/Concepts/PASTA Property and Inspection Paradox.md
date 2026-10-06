---
type: concept
course: cse301
status: active
order: 95
---

# PASTA Property and Inspection Paradox

> 📖 **Reading Order:** Step 95 of 103 | **Module 11:** Queuing Theory  
> ◄ **Previous:** [[Little's Law]] | ► **Next:** [[M-M-1 Queue]]
---
## Starting Point and the Problem

Probability and statistical inference model uncertainty in physical and computer systems. When analyzing stochastic phenomena related to PASTA Property and Inspection Paradox, naive counting or deterministic approximations fail. We establish a formal mathematical foundation to quantify outcomes and evaluate expectations rigorously.

---

## Developing the Idea

By formalizing sample spaces, probability measures, state transitions, or likelihood ratios, PASTA Property and Inspection Paradox reveals the underlying structural mechanics that govern random variables and estimation errors.

---

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
## Example

### Counterexample: Regular Deterministic Arrivals ($D/D/1$) Demonstrating Failure of PASTA

To understand why Poisson arrivals are required, consider a doctor's office with 1 server:
- Patients arrive punctually every $10$ minutes (deterministic arrival process $D$).
- Every appointment takes exactly $9$ minutes (deterministic service duration $D$).

1. **What an arriving customer sees ($a_n$):**
   - Patients arrive at minutes $0, 10, 20, 30, \dots$.
   - The prior patient completed service at minutes $9, 19, 29, 39, \dots$.
   - Consequently, **every arriving patient sees an empty clinic**:
     $$a_0 = 1.0 \quad (100\%), \quad a_1 = 0, \quad a_2 = 0, \dots$$

2. **What a continuous camera sees ($P_n$):**
   - Across every 10-minute interval, the office is occupied for 9 minutes and vacant for 1 minute:
     $$P_1 = \frac{9}{10} = 0.90 \quad (90\%), \quad P_0 = \frac{1}{10} = 0.10 \quad (10\%)$$

Notice the severe divergence:
$$a_0 = 1.0 \ne P_0 = 0.10$$
While patients experience zero waiting time and observe an idle system 100% of the time, an external auditor records an occupied server 90% of the time. This occurs because deterministic arrivals are synchronized with system states. **PASTA holds if and only if arrivals cannot anticipate future states (Poisson).**

For applications in loss systems and buffer blocking, see [[Shoe Shine Shop Queueing Model Example]] and [[Problem — Finite Capacity Queue Loss and Effective Throughput]].

---

## Technical Details

### Wolff's Lack of Anticipation Assumption (LAA) and Renewal Residual Life

1. **Wolff's PASTA Theorem (1982):**
   - Formal theorem establishes that PASTA holds for any process $\{X(t)\}$ and point arrival process $A(t)$ satisfying the **Lack of Anticipation Assumption (LAA)**:
     $$\text{For all } t \ge 0 \text{ and } h > 0, \quad \{A(t + h) - A(t)\} \text{ is independent of } \{X(s) : s \le t\} \text{ and } \{A(s) : s \le t\}$$
   - Poisson arrivals satisfy LAA unconditionally due to independent and memoryless increments.
2. **Renewal Theory and the Inspection Paradox Formula:**
   - Let interarrival times between successive events have mean $\mathbb{E}[X]$ and variance $\operatorname{Var}(X)$.
   - A random observer lands in an interarrival interval with probability proportional to its length (length-biased sampling). The expected length of the sampled interval is:
     $$\mathbb{E}[X_{\text{inspected}}] = \frac{\mathbb{E}[X^2]}{\mathbb{E}[X]} = \mathbb{E}[X] + \frac{\operatorname{Var}(X)}{\mathbb{E}[X]} \ge \mathbb{E}[X]$$
   - The **mean residual waiting time** until the next event is:
     $$\mathbb{E}[R] = \frac{\mathbb{E}[X^2]}{2\mathbb{E}[X]} = \frac{\mathbb{E}[X]}{2}\left(1 + C_V^2\right)$$
     where $C_V = \frac{\sigma}{\mathbb{E}[X]}$ is the coefficient of variation.
   - For exponential interarrivals ($C_V = 1$), $\mathbb{E}[R] = \mathbb{E}[X]$ (memorylessness). For deterministic intervals ($C_V = 0$), $\mathbb{E}[R] = \mathbb{E}[X]/2$.

---

## Important Properties and Why They Hold

- **Mathematical Rigor:** Satisfies Kolmogorov's probability axioms or standard asymptotic regularity conditions.
- **Convergence / Consistency:** Guarantees stability under large sample limits or repeated independent trials.

---

## Common Mistakes

- **Assuming PASTA Holds for Any Queue:** Applying $a_n = P_n$ to deterministic, general renewal ($G/M/1$), or bursty non-Poisson arrivals.
- **Forgetting Length-Biased Sampling in the Inspection Paradox:** Assuming the average waiting time until the next bus is $\mathbb{E}[X]/2$ without adding the variance term $\frac{\operatorname{Var}(X)}{2\mathbb{E}[X]}$.
- **Confusing Time Average $P_n$ with Arrival Average $a_n$:** Believing that system utilization $\rho = 1 - P_0$ guarantees that $(1 - \rho)$ fraction of arriving customers find an empty server when arrivals are not Poisson.

---

## Exam Relevance

In CSE301 examinations:
- Proving $a_n = d_n$ using step-crossing conservation arguments.
- Stating the PASTA theorem and its core independence / memoryless assumptions.
- Constructing or analyzing counterexamples (such as $D/D/1$) where PASTA fails.
- Applying PASTA to compute customer blocking probabilities $P_{\text{loss}} = a_N = P_N$ in [[Finite Capacity M-M-1-N Queue]].

---

## Related Concepts

- [[Queueing Systems and Kendall Notation]]
- [[M-M-1 Queue]]
- [[Finite Capacity M-M-1-N Queue]]
- [[Little's Law]]
- [[Shoe Shine Shop Queueing Model Example]]
---
## Prerequisites

- [[Discrete Probability Distributions]]
- [[Continuous Probability Distributions]]
- [[Queueing Systems and Kendall Notation]]

---

## Problems

- [[Problem — Finite Capacity Queue Loss and Effective Throughput]]

---

## Sources

- [[cse301/01 - Sources/Lectures/CSE301_Queueing_Theory.pdf]]
