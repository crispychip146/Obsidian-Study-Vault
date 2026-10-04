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

See worked numerical applications in the linked example notes.

---

## Technical Details

Refer to Blitzstein & Hwang for measure-theoretic details and moment generating properties.

---

## Important Properties and Why They Hold

- **Mathematical Rigor:** Satisfies Kolmogorov's probability axioms or standard asymptotic regularity conditions.
- **Convergence / Consistency:** Guarantees stability under large sample limits or repeated independent trials.

---

## Common Mistakes

- Confusing conditional probabilities with unconditional joint probabilities.
- Misapplying asymptotic normal approximations when sample sizes are small or distributions are heavily skewed.

---

## Exam Relevance

Tested regularly in CSE 301 midterms and finals through derivations, numerical probability calculations, and statistical hypothesis testing.

---

## Related Concepts

- [[Little's Law]]
- [[PASTA Property and Inspection Paradox]]
- [[M-M-1 Queue]]
- [[Finite Capacity M-M-1-N Queue]]
- [[Jackson Networks and Tandem Queues]]

---

---

## Prerequisites

- [[Probability Axioms and Naive Probability]]

---

## Problems

- [[Problem — Birthday Collisions and Approximation]]

---

## Sources

- [[01 - Sources/Lectures/CSE301_Queueing_Theory.pdf]]
