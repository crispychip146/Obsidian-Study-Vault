---
type: concept
course: cse301
status: active
order: 98
---

# Finite Capacity M-M-1-N Queue

> 📖 **Reading Order:** Step 98 of 103 | **Module 11:** Queuing Theory  
> ◄ **Previous:** [[M-M-1 Performance Formulas]] | ► **Next:** [[Jackson Networks and Tandem Queues]]
---
## Starting Point and the Problem

Probability and statistical inference model uncertainty in physical and computer systems. When analyzing stochastic phenomena related to Finite Capacity M-M-1-N Queue, naive counting or deterministic approximations fail. We establish a formal mathematical foundation to quantify outcomes and evaluate expectations rigorously.

---

## Developing the Idea

By formalizing sample spaces, probability measures, state transitions, or likelihood ratios, Finite Capacity M-M-1-N Queue reveals the underlying structural mechanics that govern random variables and estimation errors.

---

## Definition

An **M/M/1/N Queue** (also written M/M/1/K) is a single-server queueing system with a **finite physical capacity $N$**.
- At most $N$ customers can be present in the facility simultaneously (1 customer in service and $N - 1$ customers waiting in the buffer).
- When an arriving customer arrives to find the system completely full (all $N$ positions occupied), the customer is **blocked and turned away** (dropped/lost), entering neither the buffer nor the service mechanism.
- The state space is finite: $S = \{0, 1, 2, \dots, N\}$.
---
## How It Works

### State Transition Diagram (Truncated Birth-Death Process)

```
        λ           λ           λ                     λ
    ┌───────>   ┌───────>   ┌───────>             ┌───────>
  ┌───┐       ┌───┐       ┌───┐                 ┌───┐       ┌───┐
  │ 0 │       │ 1 │       │ 2 │   ...           │N-1│       │ N │
  └───┘       └───┘       └───┘                 └───┘       └───┘
    <───────    <───────    <───────              <───────    <───────
        µ           µ           µ                     µ           µ
```

Notice the critical difference at the upper boundary:
- At state $N$, the arrival transition rate is zero because incoming arrivals cannot enter state $N + 1$.
- Departures continue at rate $\mu$ from state $N$ to $N - 1$.

---
### Derivation of Steady-State Probabilities

### 1. Balance Equations
- **State 0:**
  $$\lambda P_0 = \mu P_1$$
- **Intermediate States ($1 \le n \le N - 1$):**
  $$(\lambda + \mu) P_n = \lambda P_{n-1} + \mu P_{n+1}$$
- **Boundary State $N$:**
  $$\mu P_N = \lambda P_{N-1}$$

### 2. Solving via Recurrence
Exactly as in the infinite M/M/1 queue, the detailed balance condition yields:
$$P_n = \left(\frac{\lambda}{\mu}\right)^n P_0 = \rho^n P_0 \quad \text{for } n = 0, 1, 2, \dots, N$$

### 3. Normalization over Finite States
Because the state space is finite, the sum of all probabilities must equal 1:
$$\sum_{n=0}^N P_n = 1 \implies P_0 \sum_{n=0}^N \rho^n = 1$$

Using the finite geometric series formula $\sum_{n=0}^N \rho^n = \frac{1 - \rho^{N+1}}{1 - \rho}$ (for $\rho \ne 1$):
$$P_0 \left( \frac{1 - \rho^{N+1}}{1 - \rho} \right) = 1 \implies \mathbf{P_0 = \frac{1 - \rho}{1 - \rho^{N+1}} \quad (\text{if } \rho \ne 1)}$$

If $\rho = 1$ ($\lambda = \mu$), all $N + 1$ states are equally likely:
$$P_0 = P_1 = \dots = P_N = \frac{1}{N + 1}$$

### 4. General State Probability Formula
$$P_n = \frac{(1 - \rho)\rho^n}{1 - \rho^{N+1}}, \quad n = 0, 1, \dots, N \quad (\text{for } \rho \ne 1)$$

---
### Stability: Finite Buffers Cannot Blow Up

In an infinite M/M/1 queue, stability requires $\rho < 1$.
In an M/M/1/N queue:
> **Theorem:** 
> An M/M/1/N queue is **stable for all finite arrival rates $\lambda \in (0, \infty)$**, even when $\lambda > \mu$ ($\rho > 1$).

*Intuition:* If customers arrive at a rate of 10,000 per second and the server processes only 1 per second, the queue length cannot grow past $N$. The system simply drops $99.99\%$ of incoming traffic.

---
### Blocking Probability and Effective Arrival Rate

By the [[PASTA Property and Inspection Paradox|PASTA property]], because arrivals follow a Poisson process, the proportion of arrivals that find the system full is identical to the time-average probability $P_N$:

### 1. Blocking / Loss Probability
$$P_{\text{loss}} = a_N = P_N = \frac{(1 - \rho)\rho^N}{1 - \rho^{N+1}}$$

### 2. Effective Arrival Rate ($\lambda_a$ or $\lambda_{\text{eff}}$)
Only customers who find fewer than $N$ people present successfully enter the facility. The rate at which customers actually enter the system is the **throughput** or **effective arrival rate**:
$$\lambda_a = \lambda_{\text{eff}} = \lambda(1 - P_N)$$

---
### Average Number of Customers ($L$)

$$L = \sum_{n=0}^N n P_n = P_0 \sum_{n=0}^N n \rho^n$$
Carrying out the finite sum using the identity $\sum_{n=0}^N n \rho^n = \frac{\rho [1 - (N+1)\rho^N + N\rho^{N+1}]}{(1 - \rho)^2}$ yields:

$$L = \frac{\rho}{1 - \rho} - \frac{(N + 1)\rho^{N+1}}{1 - \rho^{N+1}}$$

Notice that:
- As $N \to \infty$ with $\rho < 1$, the second term $\frac{(N+1)\rho^{N+1}}{1 - \rho^{N+1}} \to 0$, recovering the infinite M/M/1 formula $L = \frac{\rho}{1 - \rho}$.
- For finite $N$, $L$ is strictly less than the infinite queue length, bounded above by $N$.

---
### Engineering Trade-Off: Buffer Sizing

Network router engineers use the M/M/1/N model to balance two competing evils:
1. **Small Buffer ($N$ small):**
   - ✅ Small delay ($W$ is very small; no bufferbloat).
   - ❌ High packet drop rate ($P_N$ is large).
2. **Large Buffer ($N$ large):**
   - ✅ Low packet drop rate ($P_N$ is small).
   - ❌ High latency and jitter ($W$ becomes massive under congestion).
---
## Example

### Packet Router Buffer Analysis ($M/M/1/3$)

A network router buffer can hold at most 2 waiting packets plus 1 packet currently transmitting, giving a total system capacity of $N = 3$.
Packets arrive at Poisson rate $\lambda = 4$ packets/ms, and transmission speed is $\mu = 5$ packets/ms.
Traffic intensity: $\rho = \frac{\lambda}{\mu} = \frac{4}{5} = 0.80$.

1. **Stationary State Distribution:**
   - Normalizing probability $P_0$:
     $$P_0 = \frac{1 - \rho}{1 - \rho^{N+1}} = \frac{1 - 0.8}{1 - 0.8^4} = \frac{0.2}{1 - 0.4096} = \frac{0.2}{0.5904} \approx 0.33875$$
   - Probability of each state $n \in \{0, 1, 2, 3\}$:
     $$P_1 = P_0 \rho = 0.33875(0.8) \approx 0.2710$$
     $$P_2 = P_0 \rho^2 = 0.33875(0.64) \approx 0.2168$$
     $$P_3 = P_0 \rho^3 = 0.33875(0.512) \approx 0.1734$$

2. **Packet Blocking (Drop) Probability:**
   By the [[PASTA Property and Inspection Paradox]], arriving packets observe the state distribution. Thus the packet loss probability is:
   $$P_{\text{loss}} = a_3 = P_3 \approx 0.1734 \quad (17.34\% \text{ loss})$$

3. **Effective Carried Load ($\lambda_a$):**
   $$\lambda_a = \lambda(1 - P_3) = 4(1 - 0.1734) = 4(0.8266) \approx 3.3064 \text{ packets/ms}$$

4. **Average Backlog and Residence Time:**
   - Mean packets in system:
     $$L = \sum_{n=0}^3 n P_n = 0(P_0) + 1(0.2710) + 2(0.2168) + 3(0.1734) = 1.2248 \text{ packets}$$
   - Latency experienced by admitted packets via Little's Law:
     $$W = \frac{L}{\lambda_a} = \frac{1.2248}{3.3064} \approx 0.3704 \text{ ms}$$

For complete throughput maximization problems and loss rate proofs, see [[Problem — Finite Capacity Queue Loss and Effective Throughput]].

---

## Technical Details

### Finite State Truncation and Stability under Overload ($\rho \ge 1$)

1. **Finite State Birth-Death Truncation:**
   - The state space is bounded: $S = \{0, 1, \dots, N\}$.
   - Arrival transition rate: $\lambda_n = \lambda$ for $0 \le n < N$, and $\lambda_N = 0$.
   - Departure transition rate: $\mu_n = \mu$ for $1 \le n \le N$.
   - Because the state space is finite, irreducible, and aperiodic, a unique stationary distribution exists **unconditionally**, even if $\rho = 1$ or $\rho > 1$!
2. **Behavior under Heavy Overload ($\rho \ge 1$):**
   - **Case $\rho = 1$ ($\lambda = \mu$):**
     By L'Hôpital's rule:
     $$P_n = \frac{1}{N + 1} \quad \text{for all } n \in \{0, 1, \dots, N\}$$
     All states are equally likely, and loss probability is simply $P_N = \frac{1}{N+1}$.
   - **Case $\rho > 1$ ($\lambda > \mu$):**
     The probability mass concentrates near capacity $N$. As $N \to \infty$, $P_N \to 1 - \frac{1}{\rho}$. The system naturally protects the server from diverging by dropping the excess arrival load $\lambda - \mu$, ensuring $\lambda_a = \lambda(1 - P_N) \to \mu$.

---

## Important Properties and Why They Hold

### Little's Law Subtlety: Which $\lambda$ to Use?

A classic trap in queueing theory exams occurs when applying [[Little's Law]] to finite-capacity loss systems:

$$\mathbf{L = \lambda_a W = \lambda(1 - P_N) W}$$
$$\mathbf{W = \frac{L}{\lambda_a} = \frac{L}{\lambda(1 - P_N)}}$$

### Why You Must Use $\lambda_a$ Instead of $\lambda$
- Little's Law relates the average number of customers **in the system** ($L$) to the average time spent **in the system** ($W$).
- Blocked customers spend exactly **$0$ seconds** in the system. If you divided $L$ by the gross arrival rate $\lambda$, you would dilute the residence time of entering customers with phantom customers who were rejected at the door.
---
## Common Mistakes

- **Using Gross Arrival Rate $\lambda$ in Little's Law:** Dividing $L$ by $\lambda$ instead of effective arrival rate $\lambda_a = \lambda(1 - P_N)$.
- **Assuming Stability Fails for $\rho \ge 1$:** Applying infinite-queue restrictions to $M/M/1/N$. Finite capacity queues are always stable and ergodic for any finite $\lambda, \mu > 0$.
- **Forgetting Geometric Series Power in Denominator:** Writing $1 - \rho^N$ instead of $1 - \rho^{N+1}$ in the $P_0$ formula.

---

## Exam Relevance

Tested regularly in CSE 301 midterms and finals through derivations, numerical probability calculations, and statistical hypothesis testing via [[Little's Law]].

---

## Related Concepts

- [[M-M-1 Queue]]
- [[Little's Law]]
- [[PASTA Property and Inspection Paradox]]
- [[Problem — Finite Capacity Queue Loss and Effective Throughput]]
---
## Prerequisites

- [[M-M-1 Queue]]
- [[Queueing Systems and Kendall Notation]]
- [[Little's Law]]

---

## Problems

- [[Problem — Finite Capacity Queue Loss and Effective Throughput]]

---

## Sources

- [[cse301/01 - Sources/Lectures/CSE301_Queueing_Theory.pdf]]
