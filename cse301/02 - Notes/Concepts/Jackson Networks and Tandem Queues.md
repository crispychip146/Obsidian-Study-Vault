---
type: concept
course: cse301
status: active
---

# Jackson Networks and Tandem Queues

## Definition

An **Open Queueing Network** is a directed network of interconnected service stations (nodes) where customers arrive from outside the network, move through a sequence of queues according to probabilistic routing, receive service, and eventually exit the system.

A network of queues is called a **Jackson Network** if:
1. It contains $k$ service nodes, where station $j$ has an exponential service rate $\mu_j$ and infinite queueing buffer.
2. External arrivals to station $j$ form an independent Poisson process with rate $r_j \ge 0$.
3. Upon completing service at station $i$, a customer transitions to station $j$ with routing probability $P_{ij}$, or exits the entire network with probability:
   $$P_{i, \text{exit}} = 1 - \sum_{j=1}^k P_{ij}$$
4. All service times and routing transitions are mutually independent.

---

## 1. Tandem (Sequential) Queues

The simplest queueing network is a **Tandem Queue** consisting of two servers in series:

```
              Queue 1               Queue 2
  Arrivals λ  ┌─────┐   Server 1    ┌─────┐   Server 2    Departures
 ────────────>│  1  │───[ µ1 ]─────>│  2  │───[ µ2 ]─────>
              └─────┘               └─────┘
```

- External arrivals to Server 1 are Poisson with rate $\lambda$.
- Service rates are $\mu_1$ and $\mu_2$.
- Stability condition: $\lambda < \mu_1$ and $\lambda < \mu_2$.

### Burke's Theorem
A foundational result in queueing network theory:
> **Burke's Theorem (1956):** 
> The departure process from a stable M/M/1 queue with Poisson arrival rate $\lambda$ is itself a **Poisson process with rate $\lambda$**. Furthermore, at any fixed time $t$, the number of customers currently in the system is independent of past departure times.

### Intuition: Why Server 2 Sees Arrival Rate $\lambda$ (Not $\mu_1$)
Students often mistakenly expect Server 2 to receive customers at rate $\mu_1$ because Server 1 works at rate $\mu_1$.
- Server 1 only works when it has customers to serve (which occurs a fraction $\rho_1 = \lambda/\mu_1$ of the time).
- When Server 1 is idle, its output rate is zero.
- The average long-run departure rate is:
  $$\text{Effective Output Rate} = \mu_1 \times P(\text{Server 1 is busy}) = \mu_1 \times \left(\frac{\lambda}{\mu_1}\right) = \lambda$$
In steady state, **what goes in must come out**. Server 2 sees a pure Poisson arrival stream with rate $\lambda$!

### Product-Form Solution for Tandem Queues
Because Server 1's departures are Poisson and independent of Server 2's current state:
$$P(n_1, n_2) = P_1(n_1) \cdot P_2(n_2) = (1 - \rho_1)\rho_1^{n_1} \cdot (1 - \rho_2)\rho_2^{n_2}$$
where $\rho_1 = \lambda/\mu_1$ and $\rho_2 = \lambda/\mu_2$.

### Performance Measures Additivity
Because expectation is linear, network metrics add up across servers:
$$L = L_1 + L_2 = \frac{\rho_1}{1 - \rho_1} + \frac{\rho_2}{1 - \rho_2}$$
$$W = W_1 + W_2 = \frac{1}{\mu_1 - \lambda} + \frac{1}{\mu_2 - \lambda}$$

---

## 2. General Open Jackson Networks

In a general $k$-node network, traffic can circulate, split, merge, and form feedback loops.

```
       r1 ────> [ Node 1 ] ──── P12 ────> [ Node 2 ] ────> Exit
                   ▲                         │
                   │                         │
                   └────────── P21 ──────────┘
```

### 1. Jackson Traffic Equations
The total effective arrival rate $\lambda_j$ at node $j$ is the sum of external arrivals and internal routing transfers from all nodes $i$:

$$\mathbf{\lambda_j = r_j + \sum_{i=1}^k \lambda_i P_{ij}, \quad j = 1, 2, \dots, k}$$

In matrix-vector notation:
$$\boldsymbol{\lambda}^T = \mathbf{r}^T + \boldsymbol{\lambda}^T \mathbf{P} \implies \boldsymbol{\lambda}^T (\mathbf{I} - \mathbf{P}) = \mathbf{r}^T \implies \mathbf{\boldsymbol{\lambda}^T = \mathbf{r}^T (\mathbf{I} - \mathbf{P})^{-1}}$$

where:
- $\mathbf{r} = (r_1, \dots, r_k)^T$ is the external arrival rate vector.
- $\mathbf{P} = [P_{ij}]$ is the $k \times k$ substochastic routing matrix.
- $(\mathbf{I} - \mathbf{P})^{-1}$ is the fundamental Leontief inverse matrix.

---

## Jackson's Theorem (Product-Form Stationary Distribution)

> **Jackson's Theorem (1957):**
> Suppose every queue in the open network is stable:
> $$\rho_j = \frac{\lambda_j}{\mu_j} < 1 \quad \text{for all } j = 1, \dots, k$$
> Then the joint stationary probability of finding $n_j$ customers at node $j$ factors into an exact **product-form distribution**:
> $$\mathbf{P(n_1, n_2, \dots, n_k) = \prod_{j=1}^k P_j(n_j) = \prod_{j=1}^k (1 - \rho_j)\rho_j^{n_j}}$$

### The Profound Paradox of Jackson's Theorem
In a network with feedback (e.g., node 2 sending customers back to node 1), the actual internal arrival processes are **not Poisson** because packets traveling in feedback loops create correlated, bursty arrival clusters.
Yet, Jackson's theorem proves that the joint equilibrium distribution behaves **identically to a set of independent M/M/1 queues!**

---

## Network Performance Measures

### 1. Total Average Number of Customers in the Network ($L$)
$$L = \sum_{j=1}^k L_j = \sum_{j=1}^k \frac{\rho_j}{1 - \rho_j} = \sum_{j=1}^k \frac{\lambda_j}{\mu_j - \lambda_j}$$

### 2. Total Average Time in Network ($W$)
By [[Little's Law]] applied to the entire network:
$$W = \frac{L}{\gamma}$$
where $\gamma = \sum_{j=1}^k r_j$ is the **total external arrival rate** into the network:
$$W = \frac{\sum_{j=1}^k L_j}{\sum_{j=1}^k r_j}$$

---

## Common Mistakes

- Using the gross internal rate $\lambda_j$ instead of total external rate $\sum r_i$ in the denominator of network Little's Law $W = L / \gamma$.
- Forgetting that $\rho_j = \lambda_j / \mu_j$ uses the total traffic $\lambda_j$ solved from the traffic equations, not merely the external arrival rate $r_j$.

---

## Related Concepts

- [[M-M-1 Queue]]
- [[Little's Law]]
- [[Queueing Systems and Kendall Notation]]
- [[Tandem Two-Server Queue Performance Example]]

---

## Sources

- [[01 - Sources/Lectures/CSE301_Queueing_Theory.pdf]]
