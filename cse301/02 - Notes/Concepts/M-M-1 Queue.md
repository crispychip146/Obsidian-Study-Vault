---
type: concept
course: cse301
status: active
order: 85
---

# M-M-1 Queue

> 📖 **Reading Order:** Step 85 of 92 | **Module 11:** Queuing Theory  
> ◄ **Previous:** [[PASTA Property and Inspection Paradox]] | ► **Next:** [[M-M-1 Performance Formulas]]

---

---

## Starting Point and the Problem

Probability and statistical inference model uncertainty in physical and computer systems. When analyzing stochastic phenomena related to M-M-1 Queue, naive counting or deterministic approximations fail. We establish a formal mathematical foundation to quantify outcomes and evaluate expectations rigorously.

---

## Developing the Idea

By formalizing sample spaces, probability measures, state transitions, or likelihood ratios, M-M-1 Queue reveals the underlying structural mechanics that govern random variables and estimation errors.

---

## Definition

An **M/M/1 Queue** is the foundational stochastic model of a single-server queueing system characterized by:
- **$M$ (Markovian Arrivals):** Customers arrive according to a Poisson process with constant rate $\lambda$ (interarrival times are independent and exponentially distributed with mean $1/\lambda$).
- **$M$ (Markovian Service):** Service times are independent and exponentially distributed with constant rate $\mu$ (mean service time is $1/\mu$).
- **$1$ (Single Server):** Exactly one server processes customers one at a time.
- **$\infty$ (Infinite Capacity):** The waiting room has infinite capacity; no arriving customers are turned away or blocked.
- **FIFO Discipline:** Customers are served strictly First-In, First-Out.

Let $X(t)$ denote the number of customers in the system at time $t$. The stochastic process $\{X(t), t \ge 0\}$ is a **Continuous-Time Markov Chain (CTMC)**, specifically a **Birth-Death Process** with state space $\{0, 1, 2, \dots\}$.

---

---

## How It Works

### State Transition Diagram (Birth-Death Process)

```
        λ           λ           λ                 λ
    ┌───────>   ┌───────>   ┌───────>         ┌───────>
  ┌───┐       ┌───┐       ┌───┐             ┌───┐
  │ 0 │       │ 1 │       │ 2 │   ...       │ n │   ...
  └───┘       └───┘       └───┘             └───┘
    <───────    <───────    <───────          <───────
        µ           µ           µ                 µ
```

- **Birth Rate (Arrival):** $\lambda_n = \lambda$ for all $n \ge 0$.
- **Death Rate (Departure):** $\mu_n = \mu$ for all $n \ge 1$, and $\mu_0 = 0$ (no departures from an empty system).

---
### Stability Condition: Traffic Intensity $\rho < 1$

Define the **traffic intensity** (server utilization):
$$\rho = \frac{\lambda}{\mu}$$

### The Stability Criterion
$$\mathbf{\rho < 1 \iff \lambda < \mu}$$

- **If $\lambda < \mu$ ($\rho < 1$):** The server works faster on average than the arrival stream. A stationary steady-state probability distribution exists, and the queue length remains finite.
- **If $\lambda \ge \mu$ ($\rho \ge 1$):** Customers arrive faster than (or equal to) the processing rate. The expected number of customers in the system grows without bound as $t \to \infty$ ($L \to \infty, W \to \infty$). No stationary distribution exists.

---
### Derivation of Steady-State Probabilities

In steady state, the principle of **detailed balance** dictates that the long-run probability flux into each state must equal the probability flux out of that state.

### 1. Balance Equations
- **State 0 (Boundary):**
  $$\text{Rate Entering } 0 = \text{Rate Leaving } 0 \implies \mu P_1 = \lambda P_0 \implies P_1 = \left(\frac{\lambda}{\mu}\right) P_0$$
- **State $n$ ($n \ge 1$):**
  $$\text{Rate Entering } n = \text{Rate Leaving } n \implies \lambda P_{n-1} + \mu P_{n+1} = (\lambda + \mu) P_n$$

### 2. Solving via Telescoping Differences
Rearrange the general balance equation by grouping arrival and departure terms:
$$\mu(P_{n+1} - P_n) = \lambda(P_n - P_{n-1})$$
Divide through by $\mu$:
$$P_{n+1} - P_n = \left(\frac{\lambda}{\mu}\right) (P_n - P_{n-1}) = \rho (P_n - P_{n-1})$$

For $n = 1$:
$$P_2 - P_1 = \rho (P_1 - P_0) = \rho (\rho P_0 - P_0) = \rho (\rho - 1) P_0$$
For general $n$:
$$P_{n+1} - P_n = \rho^n (\rho - 1) P_0$$

Summing telescopically from $k = 0$ to $n - 1$:
$$P_n = P_0 + \sum_{k=0}^{n-1} (P_{k+1} - P_k) = P_0 + (\rho - 1) P_0 \sum_{k=0}^{n-1} \rho^k$$
Using the finite geometric sum identity $\sum_{k=0}^{n-1} \rho^k = \frac{1 - \rho^n}{1 - \rho}$:
$$P_n = P_0 + (\rho - 1) P_0 \left(\frac{1 - \rho^n}{1 - \rho}\right) = P_0 - (1 - \rho) P_0 \left(\frac{1 - \rho^n}{1 - \rho}\right) = P_0 - P_0(1 - \rho^n) = \rho^n P_0$$

Thus, for every $n \ge 0$:
$$P_n = \rho^n P_0 = \left(\frac{\lambda}{\mu}\right)^n P_0$$

### 3. Normalization (Finding $P_0$)
All state probabilities must sum to 1:
$$\sum_{n=0}^\infty P_n = 1 \implies P_0 \sum_{n=0}^\infty \rho^n = 1$$
For $\rho < 1$, the infinite geometric series converges to $\frac{1}{1 - \rho}$:
$$P_0 \left(\frac{1}{1 - \rho}\right) = 1 \implies \mathbf{P_0 = 1 - \rho}$$

### 4. The Steady-State Distribution
$$P_n = (1 - \rho)\rho^n, \quad n = 0, 1, 2, \dots$$

> **Key Theoretical Result:** 
> The steady-state number of customers in an M/M/1 queue follows a **Geometric distribution** shifted to include zero, with parameter $1 - \rho$.

---
### Derivation of Performance Measures

### 1. Average Number in System ($L$)
$$L = E[X] = \sum_{n=0}^\infty n P_n = (1 - \rho)\sum_{n=0}^\infty n \rho^n$$
Using the geometric derivative identity $\sum_{n=0}^\infty n \rho^n = \rho \frac{d}{d\rho}\left(\sum_{n=0}^\infty \rho^n\right) = \frac{\rho}{(1 - \rho)^2}$:
$$L = (1 - \rho) \frac{\rho}{(1 - \rho)^2} = \mathbf{\frac{\rho}{1 - \rho} = \frac{\lambda}{\mu - \lambda}}$$

### 2. Average Residence Time in System ($W$)
Applying [[Little's Law]] ($L = \lambda W$):
$$W = \frac{L}{\lambda} = \frac{1}{\lambda}\left(\frac{\lambda}{\mu - \lambda}\right) = \mathbf{\frac{1}{\mu - \lambda}}$$

### 3. Average Wait Time in Queue ($W_Q$)
$$W_Q = W - \frac{1}{\mu} = \frac{1}{\mu - \lambda} - \frac{1}{\mu} = \frac{\mu - (\mu - \lambda)}{\mu(\mu - \lambda)} = \mathbf{\frac{\lambda}{\mu(\mu - \lambda)} = \frac{\rho}{\mu(1 - \rho)}}$$

### 4. Average Number Waiting in Queue ($L_Q$)
Applying Little's Law to the queue ($L_Q = \lambda W_Q$):
$$L_Q = \lambda \left(\frac{\lambda}{\mu(\mu - \lambda)}\right) = \mathbf{\frac{\lambda^2}{\mu(\mu - \lambda)} = \frac{\rho^2}{1 - \rho}}$$
Notice that $L - L_Q = \frac{\rho}{1 - \rho} - \frac{\rho^2}{1 - \rho} = \frac{\rho(1 - \rho)}{1 - \rho} = \rho$, perfectly matching the average number of customers currently receiving service.

---
### Summary Reference Table

| Metric | Formula | Behavior as $\rho \to 1$ |
|---|---|---|
| Server Idle Probability ($P_0$) | $1 - \rho$ | Approaches $0$ |
| Server Utilization ($\rho$) | $\lambda / \mu$ | Approaches $1$ |
| Probability of $\ge k$ customers | $\rho^k$ | Flattens to $1$ |
| Mean Number in System ($L$) | $\frac{\rho}{1 - \rho}$ | Explodes to $\infty$ |
| Mean Number in Queue ($L_Q$) | $\frac{\rho^2}{1 - \rho}$ | Explodes to $\infty$ |
| Mean Time in System ($W$) | $\frac{1}{\mu - \lambda}$ | Explodes to $\infty$ |
| Mean Wait in Queue ($W_Q$) | $\frac{\rho}{\mu - \lambda}$ | Explodes to $\infty$ |

---
### The Non-linear "Hockey Stick" Latency Curve

A critical engineering insight from the formula $W = \frac{1}{\mu(1 - \rho)}$:
- At $\rho = 0.5$ (50% CPU utilization): $W = \frac{2}{\mu}$ (delay is $2 \times$ service time).
- At $\rho = 0.8$ (80% CPU utilization): $W = \frac{5}{\mu}$ (delay is $5 \times$ service time).
- At $\rho = 0.95$ (95% CPU utilization): $W = \frac{20}{\mu}$ (delay is $20 \times$ service time).

As utilization approaches $100\%$, waiting time does **not** increase linearly; it **hyperbolically explodes**. This is why web servers and telecommunications networks are engineered to operate at target utilizations of $60\% - 75\%$.

---

---

## Example

### Numerical Server Performance Calculation ($\lambda = 3, \mu = 5$)

A web service gateway receives HTTP requests at an average Poisson arrival rate of $\lambda = 3$ requests per second. A single backend thread processes requests with exponential service time at rate $\mu = 5$ requests per second.

1. **Traffic Intensity and Idle Fraction:**
   $$\rho = \frac{\lambda}{\mu} = \frac{3}{5} = 0.60$$
   The server is busy $60\%$ of the time and idle with probability $P_0 = 1 - \rho = 0.40$ ($40\%$).

2. **Occupancy Probabilities:**
   - Probability of finding exactly $n$ requests in the system: $P_n = (1 - \rho)\rho^n = (0.40)(0.60)^n$.
   - Probability of finding 2 or more requests (server busy plus at least 1 waiting):
     $$P(N \ge 2) = \rho^2 = (0.60)^2 = 0.36 \quad (36\%)$$

3. **Queue Lengths and Delays:**
   - Average number of requests in system:
     $$L = \frac{\rho}{1 - \rho} = \frac{0.60}{1 - 0.60} = \frac{0.60}{0.40} = 1.50 \text{ requests}$$
   - Average number waiting in buffer:
     $$L_Q = \frac{\rho^2}{1 - \rho} = \frac{0.36}{0.40} = 0.90 \text{ requests}$$
   - Total latency (sojourn time):
     $$W = \frac{1}{\mu - \lambda} = \frac{1}{5 - 3} = 0.50 \text{ seconds}$$
   - Average queue waiting time:
     $$W_Q = W - \frac{1}{\mu} = 0.50 - \frac{1}{5} = 0.30 \text{ seconds}$$

For further parametric evaluations and performance formula summaries, see [[M-M-1 Performance Formulas]], [[Shoe Shine Shop Queueing Model Example]], and [[Problem — M-M-1 Queue Performance Metrics Calculation]].

---

## Technical Details

### CTMC Generator, Exponential Sojourn Distribution, and Heavy-Traffic Limits

1. **Continuous-Time Markov Chain (CTMC) Infinitesimal Generator:**
   - The queue length process $N(t)$ is a continuous-time birth-death process with birth rates $\lambda_n = \lambda$ and death rates $\mu_n = \mu$ for $n \ge 1$.
   - The infinitesimal generator $Q$ satisfies:
     $$q_{n, n+1} = \lambda, \quad q_{n, n-1} = \mu \quad (n \ge 1), \quad q_{n, n} = -(\lambda + \mu)$$
   - The steady-state equation $\pi Q = \mathbf{0}$ yields the detailed balance equations $\lambda \pi_n = \mu \pi_{n+1}$, producing $\pi_n = (1 - \rho)\rho^n$.
2. **Distribution of Total Sojourn Time ($T$):**
   - In an $M/M/1$ queue with FIFO discipline, the total time a customer spends in the system $T$ is **strictly exponentially distributed**:
     $$f_T(t) = (\mu - \lambda) e^{-(\mu - \lambda) t}, \quad t \ge 0$$
   - Despite being a compound sum of a random geometric number of exponential service phases, the distribution collapses into a pure exponential distribution with effective clearance rate $\mu - \lambda$.
3. **Heavy-Traffic Approximation ($\rho \to 1$):**
   - As $\rho \uparrow 1$, the normalized queue length $(1 - \rho) N(t)$ converges in distribution to an exponential random variable with mean $1$.
   - Transient fluctuations converge weakly to a reflected Brownian motion with negative drift.

---

## Important Properties and Why They Hold

- **Mathematical Rigor:** Satisfies Kolmogorov's probability axioms or standard asymptotic regularity conditions.
- **Convergence / Consistency:** Guarantees stability under large sample limits or repeated independent trials.

---

## Common Mistakes

- **Confusing $\mu - \lambda$ with $\lambda - \mu$:** Denominators in $W = \frac{1}{\mu - \lambda}$ require service rate $\mu > \lambda$ to stay positive.
- **Assuming $L_Q = L - 1$:** The difference between $L$ and $L_Q$ is $\rho = \lambda/\mu$ (the average number in service), which is strictly $< 1$, not $1$.
- **Applying $M/M/1$ Formulas to Bounded Buffers:** Infinite queue formulas overestimate queue lengths when finite capacities drop packets (see [[Finite Capacity M-M-1-N Queue]]).

---

## Exam Relevance

Tested regularly in CSE 301 midterms and finals through derivations, numerical probability calculations, and performance metrics via [[M-M-1 Performance Formulas]].

---

## Related Concepts

- [[Queueing Systems and Kendall Notation]]
- [[Little's Law]]
- [[PASTA Property and Inspection Paradox]]
- [[Finite Capacity M-M-1-N Queue]]
- [[M-M-1 Performance Formulas]]
- [[Shoe Shine Shop Queueing Model Example]]

---

---

## Prerequisites

- [[Queueing Systems and Kendall Notation]]
- [[Continuous Probability Distributions]]
- [[Stationary and Limiting Distributions in Markov Chains]]

---

## Problems

- [[Problem — M-M-1 Queue Performance Metrics Calculation]]

---

## Sources

- [[01 - Sources/Lectures/CSE301_Queueing_Theory.pdf]]
