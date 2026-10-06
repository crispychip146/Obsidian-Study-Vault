---
type: concept
course: cse301
status: active
order: 82
---

# Queueing Systems and Kendall Notation

> 📖 **Reading Order:** Step 82 of 92 | **Module 11:** Queuing Theory  
> ◄ **Previous:** [[Problem — Identification of Communicating Classes and Absorbing States]] | ► **Next:** [[Little's Law]]

---

---

## Starting Point and the Problem

Probability and statistical inference model uncertainty in physical and computer systems. When analyzing stochastic phenomena related to Queueing Systems and Kendall Notation, naive counting or deterministic approximations fail. We establish a formal mathematical foundation to quantify outcomes and evaluate expectations rigorously.

---

## Developing the Idea

By formalizing sample spaces, probability measures, state transitions, or likelihood ratios, Queueing Systems and Kendall Notation reveals the underlying structural mechanics that govern random variables and estimation errors.

---

## Definition

A **Queueing System** is a mathematical model of a service facility where entities ("customers", "packets", "tasks", "jobs") arrive stochastically over time, wait in a buffer or queue if the service facility is currently occupied, receive service from one or more servers, and subsequently depart the system.

Queueing systems are classified using **Kendall's Notation**, expressed as:

$$A \;/\; S \;/\; c \;/\; K \;/\; N \;/\; D$$

where:
1. **$A$ (Arrival Process):** The probability distribution of interarrival times.
   - $M$: Markovian / Exponential (Poisson arrivals)
   - $D$: Deterministic (constant interarrival time)
   - $E_k$: Erlang-$k$ distribution
   - $G$ or $GI$: General independent distribution
2. **$S$ (Service Process):** The probability distribution of service durations.
   - $M$: Markovian / Exponential service times
   - $D$: Deterministic service times
   - $G$: General distribution
3. **$c$ (Number of Servers):** Positive integer ($c = 1, 2, \dots, \infty$).
4. **$K$ (System Capacity):** Maximum number of customers allowed in the system (queue + servers). Default is $\infty$ if omitted.
5. **$N$ (Population Size):** Size of the customer source population. Default is $\infty$ if omitted.
6. **$D$ (Queue Discipline):** Order of service. Default is FIFO (First-In, First-Out).

---

---

## How It Works

### Fundamental Performance Metrics

Queueing theory tracks four core time-average performance quantities:

```
┌─────────────────────────────────────────────────────────────┐
│                       Queueing System                       │
│                                                             │
│                ┌────────────────────┐      ┌────────────┐   │
│  Arrivals λ    │    Waiting Room    │      │   Server   │   │  Departures
│ ──────────────>│   (Queue Length)   │─────>│  (Service) │───┼────────────>
│                │        LQ          │      │   1 / µ    │   │
│                └────────────────────┘      └────────────┘   │
│                ◄───────────────── WQ ───────────────────►   │
│                                                             │
│ ◄───────────────────────── L (Total in System) ───────────► │
│ ◄───────────────────────── W (Total Time in System) ──────► │
└─────────────────────────────────────────────────────────────┘
```

| Symbol | Metric Name | Meaning | Units |
|---|---|---|---|
| $L$ | Average Number in System | Expected total number of customers present (waiting + being served) | Customers |
| $L_Q$ | Average Number in Queue | Expected number of customers waiting in line (excluding those in service) | Customers |
| $W$ | Average Time in System | Expected total residence time a customer spends from arrival to departure | Time units (sec, min) |
| $W_Q$ | Average Time in Queue | Expected time a customer spends waiting before service commences | Time units (sec, min) |
| $\lambda$ | Mean Arrival Rate | Average number of incoming customers per unit time | Customers / time |
| $\mu$ | Mean Service Rate | Average number of customers a single server can process per unit time | Customers / time |
| $\rho$ | Traffic Intensity / Utilization | Fraction of capacity utilized: $\rho = \frac{\lambda}{c \mu}$ | Dimensionless |

### The Fundamental Decomposition of Residence Time
For any customer, total time in the system is the sum of waiting time and actual service duration:
$$W = W_Q + E[\text{Service Time}] = W_Q + \frac{1}{\mu}$$

Multiplying through by $\lambda$ via [[Little's Law]] yields the customer breakdown:
$$L = L_Q + \frac{\lambda}{\mu} = L_Q + \rho$$

---
### Queue Disciplines

1. **FIFO / FCFS (First-In, First-Out):** Standard fair queue (bank line, grocery checkout).
2. **LIFO / LCFS (Last-In, First-Out):** Stack-based processing (interrupt handling, warehouse inventory stacks).
3. **SJF / SPT (Shortest Job First):** Minimizes average waiting time $W_Q$; common in CPU scheduling.
4. **Processor Sharing (PS):** Server divides capacity equally among all currently active jobs (web server bandwidth, round-robin CPU).
5. **Priority Queueing (PRI):** High-priority jobs jump ahead of low-priority jobs (emergency room triage, QoS network packets).

---
### Why Queueing Theory Matters in Computer Science

Queueing phenomena govern virtually every shared computing resource:
- **Cloud Computing & Web Servers:** Sizing server clusters (AWS/Azure autoscaling) to prevent request latency spikes.
- **Computer Networks:** Router packet buffer sizing to prevent packet drop (bufferbloat vs. packet loss).
- **Operating Systems:** Process scheduling queues, I/O disk request dispatchers.
- **Database Systems:** Connection pooling, query concurrency limits, lock contention.

---

---

## Example

### Kendall Classification and Little's Law Application

Consider a cloud database read-replica server:
- Queries arrive according to a Poisson process with rate $\lambda = 80$ queries per second.
- Query processing durations are exponentially distributed with average latency $\mathbb{E}[S] = 10\text{ ms} = 0.010\text{ s} \implies \mu = 100$ queries per second.
- The replica processes queries sequentially on a single thread ($c = 1$) with unbounded buffer space ($K = \infty$).

1. **Kendall Notation Classification:**
   - Arrival process: Poisson / Exponential interarrivals $\implies M$.
   - Service distribution: Exponential $\implies M$.
   - Number of parallel servers: $1 \implies 1$.
   - Queue capacity: $\infty$.
   - Kendall designation: **$M/M/1$**.

2. **System Utilization:**
   $$\rho = \frac{\lambda}{\mu} = \frac{80}{100} = 0.80$$
   Because $\rho < 1$, the queue reaches a stable stationary distribution.

3. **Performance Metrics via Little's Law:**
   If the average number of queries in the system is $L = \frac{\rho}{1 - \rho} = \frac{0.80}{1 - 0.80} = 4$ queries:
   - Total System Time (latency): $W = \frac{L}{\lambda} = \frac{4}{80} = 0.050\text{ s} = 50\text{ ms}$.
   - Average Waiting Time in Queue: $W_Q = W - \frac{1}{\mu} = 50\text{ ms} - 10\text{ ms} = 40\text{ ms}$.
   - Average Number of Waiting Queries: $L_Q = \lambda W_Q = 80 \times 0.040 = 3.2$ queries.
   Notice that $L = L_Q + \rho = 3.2 + 0.8 = 4.0$, satisfying Little's conservation law.

For multi-stage systems, see [[Shoe Shine Shop Queueing Model Example]], [[Tandem Two-Server Queue Performance Example]], and [[Problem — M-M-1 Queue Performance Metrics Calculation]].

---

## Technical Details

### Little's Law Invariance and Operational Assumptions

1. **Universality of Little's Law ($L = \lambda W$):**
   - Holds for *any* black-box queuing system in steady-state, regardless of arrival distribution $A$, service distribution $B$, server count $c$, or queue discipline (FIFO, LIFO, Priority, Processor Sharing).
   - Requires only that the system is stable ($\lambda < c\mu$) and that customer flow is conserved (no creation or destruction of jobs inside).
2. **Subsystem Partitioning:**
   - Applied to the waiting buffer alone: $L_Q = \lambda W_Q$.
   - Applied to the server facility alone: $L_S = \lambda \mathbb{E}[S] = \frac{\lambda}{\mu} = \rho$.
   - By linearity of expectation:
     $$L = L_Q + L_S \iff W = W_Q + \mathbb{E}[S]$$
3. **Queue Stability Boundaries:**
   - For infinite capacity queues ($M/M/c$), a stationary distribution exists if and only if $\rho = \frac{\lambda}{c\mu} < 1$.
   - If $\rho = 1$, the underlying Continuous-Time Markov Chain is null recurrent ($L \to \infty$). If $\rho > 1$, it is transient and diverges to infinity.

---

## Important Properties and Why They Hold

- **Mathematical Rigor:** Satisfies Kolmogorov's probability axioms or standard asymptotic regularity conditions.
- **Convergence / Consistency:** Guarantees stability under large sample limits or repeated independent trials.

---

## Common Mistakes

- **Applying Little's Law with Incompatible Units:** Mixing seconds and hours when computing $L = \lambda W$ (e.g. $\lambda$ in customers/minute and $W$ in seconds).
- **Confusing Waiting Time $W_Q$ with Total Response Time $W$:** Omitting the service time $\mathbb{E}[S] = 1/\mu$ when calculating the total customer delay.
- **Assuming $\rho \ge 1$ Queues Can Reach Equilibrium:** Computing formulas like $L = \rho/(1-\rho)$ when $\rho \ge 1$ yields negative or nonsensical numbers.

---

## Exam Relevance

Tested regularly in CSE 301 midterms and finals through derivations, numerical probability calculations, and performance metrics via [[Little's Law]] and [[M-M-1 Queue]].

---

## Related Concepts

- [[Little's Law]]
- [[PASTA Property and Inspection Paradox]]
- [[M-M-1 Queue]]
- [[Finite Capacity M-M-1-N Queue]]
- [[Jackson Networks and Tandem Queues]]
- [[Shoe Shine Shop Queueing Model Example]]
- [[Tandem Two-Server Queue Performance Example]]

---

---

## Prerequisites

- [[Continuous Probability Distributions]]
- [[Stochastic Process]]
- [[Markov Chain]]

---

## Problems

- [[Problem — M-M-1 Queue Performance Metrics Calculation]]

---

## Sources

- [[01 - Sources/Lectures/CSE301_Queueing_Theory.pdf]]
