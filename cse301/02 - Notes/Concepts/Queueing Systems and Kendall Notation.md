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

## Building the idea

A queueing model specifies how customers arrive, how service takes place, how many servers work, and what happens when there is no room. The notation is a compact description of those assumptions, not merely a name for a formula sheet.

In $A/S/c/K$, $A$ describes interarrival times, $S$ service times, $c$ the number of servers, and $K$ total system capacity, including service positions. “M” denotes memoryless exponential times; Poisson arrivals are the associated counting process. “D” denotes deterministic times, and “G” a general distribution. Omitted capacity is commonly infinite, but check the stated convention.

Keep the system and waiting line separate. A customer in service contributes to system population $N$ but not queue population $N_Q$. Likewise, system time includes both waiting and service. These boundaries determine how [[Little's Law]] and performance formulas should be applied.

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

## What to carry forward

Write arrival and service rates with the same time units. A queueing formula only describes the selected assumptions; variable workloads or blocking may require a richer state model.

## Related notes

- [[Little's Law]]

## Sources

- [[01 - Sources/Lectures/CSE301_Queueing_Theory.pdf]]
