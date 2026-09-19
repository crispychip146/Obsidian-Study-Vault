---
type: concept
course: cse301
status: active
---

# Stationary and Limiting Distributions in Markov Chains

## Definition

### 1. Limiting Distribution
For a [[Markov Chain]] $\{X_n, n \ge 0\}$, if the multi-step transition probability $P_{ij}^n$ converges to a fixed value that is independent of the initial state $i$ as $n \to \infty$:

$$\pi_j = \lim_{n \to \infty} P_{ij}^n = \lim_{n \to \infty} P(X_n = j \mid X_0 = i), \quad \forall j \ge 0$$

then the vector $\pi = (\pi_0, \pi_1, \pi_2, \dots)$ is called the **limiting distribution** of the Markov chain.

### 2. Stationary Distribution (Invariant Measure)
A probability distribution $\pi = (\pi_0, \pi_1, \pi_2, \dots)$ on the state space $S$ is called a **stationary distribution** (or invariant / equilibrium distribution) if:

$$\pi_j = \sum_{i \in S} \pi_i P_{ij}, \quad \forall j \in S$$

and

$$\sum_{j \in S} \pi_j = 1, \quad \pi_j \ge 0 \quad \forall j \in S$$

In matrix notation, treating $\pi$ as a row vector:

$$\pi P = \pi, \quad \pi \mathbf{1} = 1$$

where $\mathbf{1}$ is a column vector of ones.

---

## Intuition

1. **Forgetting the Past:** Over long time horizons, the chain "forgets" where it started. Whether it began in state 0 or state 5, the probability of finding it in state $j$ after millions of steps approaches $\pi_j$.
2. **Self-Consistent Equilibrium:** Imagine pouring a fluid across the state graph. At each step, a fraction $P_{ij}$ of the fluid at node $i$ flows along the edge to node $j$. If the distribution of fluid is $\pi$, the amount leaving each node is exactly replaced by the inflows from neighboring nodes. Applying one more transition leaves the distribution unchanged: $\pi P = \pi$.
3. **Long-Run Fraction of Time:** $\pi_j$ represents the proportion of time the process spends in state $j$ over an infinite horizon:
   $$\pi_j = \lim_{N \to \infty} \frac{1}{N} \sum_{n=1}^N \mathbf{1}_{\{X_n = j\}}$$
4. **Mean Return Time:** If you are currently in state $j$, the expected number of steps until the chain returns to state $j$, denoted $\mu_{jj}$, is inversely proportional to its stationary probability:
   $$\pi_j = \frac{1}{\mu_{jj}}$$
   (Rare states have tiny $\pi_j$ and huge return times; frequently visited states have large $\pi_j$ and short return times).

---

## Why It Exists

- **Predicting Steady-State Performance:** In queuing models, web browsing (PageRank), and computer systems, we need to know the steady-state load, average buffer occupancy, or server utilization without simulating infinitely many individual transitions.
- **Handling Asymptotic Behavior:** Powers of transition matrices $P^n$ become computationally prohibitive for large $n$. Finding $\pi$ reduces the limit of matrix powers to solving a system of linear equations.
- **MCMC Sampling:** Algorithms like Metropolis-Hastings construct a Markov chain whose unique stationary distribution matches a desired complex target distribution.

---

## The Global Balance Equations

The defining system $\pi_j = \sum_i \pi_i P_{ij}$ is called the system of **Global Balance Equations**.

Because $\sum_k P_{jk} = 1$, we can rewrite $\pi_j$ as $\pi_j \sum_{k \neq j} P_{jk} + \pi_j P_{jj}$.
Subtracting $\pi_j P_{jj}$ from both sides yields the classical physical balance law:

$$\underbrace{\pi_j \sum_{k \neq j} P_{jk}}_{\text{Total Flow Out of State } j} = \underbrace{\sum_{i \neq j} \pi_i P_{ij}}_{\text{Total Flow Into State } j}$$

In steady state, the rate of probability probability flowing out of state $j$ must exactly equal the rate flowing into state $j$.

---

## Critical Distinction: Stationary vs. Limiting Distribution

A common exam pitfall is assuming that a stationary distribution and a limiting distribution are identical. They are related, but not equivalent:

| Concept | Definition | Requirement |
|---|---|---|
| **Stationary Distribution** | $\pi P = \pi, \sum \pi_j = 1$ | Exists for any finite irreducible chain (even periodic ones). |
| **Limiting Distribution** | $\lim_{n \to \infty} P_{ij}^n = \pi_j$ | Requires the chain to be **irreducible AND aperiodic**. |

### The Periodic Counterexample
Consider the deterministic alternating 2-state chain:
$$P = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}$$
- The period is $d = 2$.
- Stationary distribution:
  $$\begin{pmatrix} \pi_0 & \pi_1 \end{pmatrix} \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix} = \begin{pmatrix} \pi_1 & \pi_0 \end{pmatrix} \implies \pi_0 = \pi_1 = \frac{1}{2}$$
  The chain spends exactly half its time in each state, so long-run proportions are well-defined ($\pi = (1/2, 1/2)$).
- **Limiting distribution does NOT exist:**
  $$P^n = \begin{cases} \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix}, & n \text{ even} \\ \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}, & n \text{ odd} \end{cases}$$
  As $n \to \infty$, $P_{00}^n$ oscillates infinitely between $1$ and $0$. The limit does not settle down!

### The Fundamental Convergence Theorem (Ergodic Theorem)
For an **irreducible** and **aperiodic** Markov chain with finite state space:
1. A unique stationary distribution $\pi$ exists.
2. The limiting distribution exists and equals the stationary distribution:
   $$\lim_{n \to \infty} P_{ij}^n = \pi_j \quad \forall i, j \in S$$
3. The convergence is geometric: $|P_{ij}^n - \pi_j| \le C \cdot \lambda^n$, where $\lambda$ is governed by the second-largest eigenvalue of $P$.

---

## Step-by-Step Solving Procedure

To find the stationary / limiting distribution for an $m$-state chain:

1. **Verify Irreducibility and Aperiodicity:**
   - Confirm all states communicate.
   - Confirm period $d=1$ (e.g., at least one self-loop $P_{ii} > 0$).
2. **Write the Balance Equations ($\pi = \pi P$):**
   $$\pi_j = \sum_{i=0}^{m-1} \pi_i P_{ij}, \quad j = 0, 1, \dots, m-1$$
3. **Drop One Redundant Equation:**
   Because $P$ is row-stochastic ($\sum_j P_{ij} = 1$), the columns of $P - I$ are linearly dependent. One of the $m$ balance equations is always redundant.
4. **Substitute the Normalization Condition:**
   Replace the dropped equation with:
   $$\sum_{j=0}^{m-1} \pi_j = 1$$
5. **Solve the Resulting Non-Homogeneous Linear System:**
   Use substitution or Gaussian elimination to find $\pi_0, \pi_1, \dots, \pi_{m-1}$.

---

## Example: Two-State Weather Chain

$$P = \begin{pmatrix} \alpha & 1 - \alpha \\ \beta & 1 - \beta \end{pmatrix}$$
where $0 < \alpha, \beta < 1$.

1. **Balance equations:**
   $$\pi_0 = \alpha \pi_0 + \beta \pi_1$$
   $$\pi_1 = (1 - \alpha) \pi_0 + (1 - \beta) \pi_1$$
2. **From first equation:**
   $$\pi_0 (1 - \alpha) = \beta \pi_1 \implies \pi_1 = \frac{1 - \alpha}{\beta} \pi_0$$
3. **Using normalization $\pi_0 + \pi_1 = 1$:**
   $$\pi_0 + \frac{1 - \alpha}{\beta} \pi_0 = 1 \implies \pi_0 \left( \frac{\beta + 1 - \alpha}{\beta} \right) = 1$$
   $$\pi_0 = \frac{\beta}{1 + \beta - \alpha}$$
   $$\pi_1 = 1 - \pi_0 = \frac{1 - \alpha}{1 + \beta - \alpha}$$

**Numerical Check:**
If $\alpha = 0.7, \beta = 0.4$:
$$\pi_0 = \frac{0.4}{1 + 0.4 - 0.7} = \frac{0.4}{0.7} = \frac{4}{7} \approx 0.5714$$
$$\pi_1 = \frac{1 - 0.7}{0.7} = \frac{0.3}{0.7} = \frac{3}{7} \approx 0.4286$$
In the long run, it rains $57.14\%$ of days.

---

## Common Mistakes

- **Forgetting the Normalization Condition ($\sum \pi_j = 1$):** Trying to solve $\pi(P - I) = 0$ directly without $\sum \pi_j = 1$, yielding the trivial all-zero vector $\pi = 0$.
- **Not Dropping a Redundant Balance Equation:** Attempting to solve all $m$ balance equations plus the normalization equation simultaneously with standard inversion without recognizing linear dependence.
- **Treating Periodic Chains as having Limiting Probabilities:** Stating that $\lim_{n \to \infty} P_{ij}^n = \pi_j$ when the chain has period $d \ge 2$. (Long-run average time proportions still equal $\pi_j$, but point-wise limit $\lim P_{ij}^n$ does not exist).
- **Writing $\pi$ as a Column Vector in $P \pi = \pi$:** In Markov chains, $\pi$ is a **row vector** on the left: $\pi P = \pi$. Writing $P \pi = \pi$ solves for right eigenvectors (which is simply the all-ones vector $\mathbf{1}$, since $P \mathbf{1} = \mathbf{1}$).

---

## Exam Relevance

In CSE301 examinations:
- Setting up and solving balance equations for 2-state and 3-state chains.
- Explaining the physical/flow intuition of balance equations.
- Explaining the necessary conditions (irreducible + aperiodic) for the existence of limiting distributions.
- Interpreting $\pi_j$ as long-run time proportion and calculating mean return time $\mu_{jj} = 1/\pi_j$.
- Verifying whether a proposed distribution is stationary (as in the [[Hardy-Weinberg Law Markov Chain Example]]).

---

## Related Concepts

- [[Markov Chain]]
- [[Classification of States in Markov Chains]]
- [[Chapman-Kolmogorov Equations]]
- [[Weather Forecasting Markov Chain Example]]
- [[Hardy-Weinberg Law Markov Chain Example]]

---

## Prerequisites

- [[Markov Chain]]
- [[Classification of States in Markov Chains]]

---

## Problems

- [[Problem — Four-Day Weather Forecast]]

---

## Sources

- [[01 - Sources/Lectures/Markov_Chain.pdf]] (Slides 19–24, 28–29)
- [[01 - Sources/Textbooks/Sheldon M. Ross book Markov Chain Chapter.pdf]] (Section 4.4, pp. 211–224)
