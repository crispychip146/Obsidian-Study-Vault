---
type: problem
course: cse301
status: active
order: 92
---

# Problem — Finite Capacity Queue Loss and Effective Throughput

> 📖 **Reading Order:** Step 92 of 92 | **Module 11:** Queuing Theory  
> ◄ **Previous:** [[Problem — M-M-1 Queue Performance Metrics Calculation]] | ► **Next:** *End of Course*

---

---

## Problem

A cloud microservice endpoint handles incoming API requests using a single database worker thread.
The service has a strictly bounded queue that can hold at most $N = 3$ requests simultaneously (1 request actively executing and at most 2 requests waiting in memory).
Any incoming request arriving when the buffer is full ($N = 3$) is immediately dropped with an HTTP 503 "Service Unavailable" error.

- Requests arrive according to a Poisson process with rate $\lambda = 6$ requests per second.
- The worker executes requests with exponentially distributed service times at rate $\mu = 4$ requests per second.

1. Explain why this queueing system reaches a stationary equilibrium even though the nominal arrival rate exceeds the service rate ($\lambda = 6 > \mu = 4$).
2. Calculate the traffic intensity $\rho$ and the steady-state probabilities $P_0, P_1, P_2, P_3$.
3. Compute the request drop probability $P_{\text{drop}}$.
4. Calculate the effective request throughput $\lambda_{\text{eff}}$ of the service.
5. Compute the average number of requests $L$ present in the microservice.
6. Calculate the average latency $W$ experienced by an accepted request.

---

---

## Given

- Model: M/M/1/3
- Capacity: $N = 3$
- Arrival rate: $\lambda = 6$ req/s
- Service rate: $\mu = 4$ req/s

---

---

## Required

1. Stability explanation for $\rho > 1$.
2. State probabilities $P_0, P_1, P_2, P_3$.
3. Dropping probability $P_{\text{drop}} = P_3$.
4. Effective throughput $\lambda_{\text{eff}} = \lambda(1 - P_3)$.
5. Average inventory $L = \sum_{n=0}^3 n P_n$.
6. Average latency $W = L / \lambda_{\text{eff}}$.

---

---

## Concepts Tested

- [[Finite Capacity M-M-1-N Queue]]
- [[PASTA Property and Inspection Paradox]]
- [[Little's Law]]
- Queueing capacity constraints

---

---

## Prerequisites

- [[Probability Axioms and Naive Probability]]
- [[Discrete Probability Distributions]]

---

## Question Type

Probability / Statistical Inference / Markov Chain Analysis

---

## Solution

### Understanding the Situation
Interpret the given sample space, random variables, and event conditions.

### Developing the Key Idea
Select the governing probabilistic principle (e.g. Chapman-Kolmogorov, Adam's Law, Central Limit Theorem, or likelihood maximization) and verify that conditions hold.

### Working Through the Solution
### Solution

### 1. Stability with $\rho > 1$
In an infinite capacity queue (M/M/1), $\lambda > \mu$ causes the queue to grow to infinity because arrivals can accumulate without bound.
In an M/M/1/3 queue, the physical state space is strictly finite: $S = \{0, 1, 2, 3\}$.
Once the system reaches state $3$, any additional arrivals are dropped and discarded. The system cannot hold more than 3 requests. Because the underlying continuous-time Markov chain has a finite state space and is irreducible, a unique stationary probability distribution is **guaranteed to exist for any finite arrival rate $\lambda$**.

---

### 2. Steady-State Probabilities
The traffic intensity is:
$$\rho = \frac{\lambda}{\mu} = \frac{6}{4} = 1.5$$

Applying the [[Finite Capacity M-M-1-N Queue|M/M/1/N probability formula]]:
$$P_n = \rho^n P_0 \quad \text{for } n = 0, 1, 2, 3$$
$$P_0 = \frac{1 - \rho}{1 - \rho^{N+1}} = \frac{1 - 1.5}{1 - (1.5)^4}$$

Calculate powers of $\rho = 1.5$:
- $\rho^1 = 1.5$
- $\rho^2 = 2.25$
- $\rho^3 = 3.375$
- $\rho^4 = 5.0625$

Calculate $P_0$:
$$P_0 = \frac{1 - 1.5}{1 - 5.0625} = \frac{-0.5}{-4.0625} = \frac{0.5}{4.0625} = \frac{1}{8.125} = \frac{8}{65} \approx \mathbf{0.1231 \quad (12.31\%)}$$

Now compute $P_1, P_2, P_3$:
- **$P_1$:**
  $$P_1 = \rho P_0 = 1.5 \times \frac{8}{65} = \frac{12}{65} \approx \mathbf{0.1846 \quad (18.46\%)}$$
- **$P_2$:**
  $$P_2 = \rho^2 P_0 = 2.25 \times \frac{8}{65} = \frac{18}{65} \approx \mathbf{0.2769 \quad (27.69\%)}$$
- **$P_3$:**
  $$P_3 = \rho^3 P_0 = 3.375 \times \frac{8}{65} = \frac{27}{65} \approx \mathbf{0.4154 \quad (41.54\%)}$$

**Check normalization:**
$$\frac{8 + 12 + 18 + 27}{65} = \frac{65}{65} = 1.0 \quad \checkmark$$

---

### 3. Request Drop Probability
By the [[PASTA Property and Inspection Paradox|PASTA property]], Poisson arrivals see the time-average distribution.
A request is dropped if and only if it arrives to find all $N = 3$ slots occupied:
$$P_{\text{drop}} = a_3 = P_3 = \frac{27}{65} \approx \mathbf{0.4154 \quad (41.54\%)}$$
Roughly $41.5\%$ of all incoming requests receive an HTTP 503 error.

---

### 4. Effective Throughput ($\lambda_{\text{eff}}$)
The rate at which requests are admitted and processed is:
$$\lambda_{\text{eff}} = \lambda(1 - P_{\text{drop}}) = 6 \left(1 - \frac{27}{65}\right) = 6 \times \frac{38}{65} = \frac{228}{65} \approx \mathbf{3.5077 \text{ req/sec}}$$

Notice that $\lambda_{\text{eff}} = 3.508 < \mu = 4.0$. The database worker operates within its processing capacity!

---

### 5. Average Number of Requests in System ($L$)
$$L = \sum_{n=0}^3 n P_n = 0 \cdot P_0 + 1 \cdot P_1 + 2 \cdot P_2 + 3 \cdot P_3$$
$$L = 1\left(\frac{12}{65}\right) + 2\left(\frac{18}{65}\right) + 3\left(\frac{27}{65}\right) = \frac{12 + 36 + 81}{65} = \frac{129}{65} \approx \mathbf{1.9846 \text{ requests}}$$

---

### 6. Average Latency of Accepted Requests ($W$)
Applying [[Little's Law]] with the **effective arrival rate**:
$$W = \frac{L}{\lambda_{\text{eff}}} = \frac{129 / 65}{228 / 65} = \frac{129}{228} = \frac{43}{76} \approx \mathbf{0.5658 \text{ seconds} = 566 \text{ milliseconds}}$$

> ⚠️ **Verification of Little's Law Trap:**
> If we had erroneously used the raw arrival rate $\lambda = 6$:
> $$W_{\text{wrong}} = \frac{1.9846}{6} = 0.3308 \text{ seconds}$$
> This would incorrectly report an overly optimistic latency by averaging in the $41.5\%$ of requests that were dropped immediately without waiting!

---
### Result Summary Table

| Metric | Exact Fraction | Decimal Value | Meaning |
|---|---|---|---|
| Server Idle ($P_0$) | $8/65$ | $12.31\%$ | Worker has no queries |
| Full Buffer ($P_3$) | $27/65$ | $41.54\%$ | Drop probability |
| Effective Throughput | $228/65$ | $3.508$ req/s | Handled load |
| Mean Query Load ($L$) | $129/65$ | $1.985$ req | Average active queries |
| Mean Response Time ($W$) | $43/76$ | $0.566$ s | Average latency per accepted query |

---
### Related Concepts

- [[Finite Capacity M-M-1-N Queue]]
- [[PASTA Property and Inspection Paradox]]
- [[Little's Law]]
- [[M-M-1 Queue]]

---

### Result and Interpretation
The final analytical solution and numerical metrics are rigorously verified against probability axioms.

---

## Reusable Insight

Always decompose complex event probabilities by conditioning on a partition of the sample space (Law of Total Probability), or by writing indicator random variables to exploit linearity of expectation.

---

## Common Mistakes

- Conflating correlation with causation or independence.
- Misapplying the Central Limit Theorem when the variance of the underlying distribution is infinite (e.g. Cauchy).

---

## Exam Pattern

Standard BUET CSE 301 final exam question testing probability bounds, Markov chain stationarity, or statistical parameter estimation.

---

## Related Problems

- [[Problem — Birthday Collisions and Approximation]]
- [[Problem — Four-Day Weather Forecast]]

---

## Related Concepts

- [[Random Variables and Probability Distributions]]
- [[Law of Total Probability and Bayes' Rule]]

---

## Source

- [[01 - Sources/Lectures/CSE301_Queueing_Theory.pdf]]
