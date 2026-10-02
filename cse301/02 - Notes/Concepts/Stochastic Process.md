---
type: concept
course: cse301
status: active
order: 68
---

# Stochastic Process

> 📖 **Reading Order:** Step 68 of 92 | **Module 10:** Stochastic Processes  
> ◄ **Previous:** [[Problem — Multiple Testing Correction with Bonferroni and Benjamini-Hochberg]] | ► **Next:** [[Markov Chain]]

---

## Definition

A **stochastic process** is an indexed collection of random variables:

$$\{X_t, t \in T\}$$

defined on a common probability space $(\Omega, \mathcal{F}, P)$, where $T$ is the index set (often representing time), and each $X_t$ takes values in a set $S$ called the **state space**.

- If the index set $T = \{0, 1, 2, \dots\}$, the process is a **discrete-time stochastic process**, commonly denoted $\{X_n, n \ge 0\}$.
- If $T = [0, \infty)$, the process is a **continuous-time stochastic process**, denoted $\{X(t), t \ge 0\}$.
- If the state space $S$ is countable (finite or countably infinite), such as $\{0, 1, 2, \dots\}$, it is a **discrete-state process**.

---

## Intuition

In elementary probability, a random variable gives a single probabilistic snapshot: for example, the roll of a die or the height of an individual.

However, many physical, biological, computational, and financial systems evolve dynamically across time:
- The price of a stock at 9:30 AM, 9:31 AM, 9:32 AM...
- The number of packets waiting in a network buffer at each clock cycle.
- The weather (sunny, rainy) observed each morning.

A stochastic process is simply a sequence of random variables that models how a system changes over time under uncertainty. It is a "random variable with a clock."

---

## Why It Exists

- **Overcoming Static Probability:** Classical probability models static outcomes. Real-world systems require modeling temporal dependencies, long-term trends, and sequential transitions.
- **Handling History and Memory:** Unlike independent and identically distributed (i.i.d.) random variables—where the past has no bearing on the future—a stochastic process formalizes different degrees of historical dependence.
- **Unified Framework:** It provides the mathematical foundation for [[Markov Chain]], Poisson processes, Brownian motion, queuing models, and time series analysis.

---

## How It Works

1. **State Space ($S$):** The set of all possible configurations or values the system can occupy.
   - Example: For a communication channel, $S = \{0, 1\}$.
   - Example: For a queue, $S = \{0, 1, 2, \dots\}$.
2. **Time Index ($T$):** The timeline over which observations occur.
   - Discrete steps $n = 0, 1, 2, \dots$ (days, clock ticks, transmission stages).
   - Continuous intervals $t \ge 0$ (seconds, continuous time).
3. **Trajectory / Sample Path:** A realization of the stochastic process across time. For a fixed outcome $\omega \in \Omega$, the mapping $t \mapsto X_t(\omega)$ is a deterministic function of time representing one historical run of the system.
4. **Joint Distributions:** The probabilistic behavior of $\{X_t\}$ is completely determined by the family of all finite-dimensional joint distributions:
   $$P(X_{t_1} \le x_1, X_{t_2} \le x_2, \dots, X_{t_k} \le x_k)$$
   for any selection of times $t_1 < t_2 < \dots < t_k$.

---

## Example

Consider flipping a fair coin repeatedly at times $n = 1, 2, 3, \dots$:
- Let $Y_n = +1$ if heads, and $-1$ if tails ($P(Y_n = 1) = P(Y_n = -1) = 0.5$).
- Let $X_0 = 0$, and define $X_n = \sum_{k=1}^n Y_k$ for $n \ge 1$.

Here, $\{X_n, n \ge 0\}$ is a discrete-time, discrete-state stochastic process known as a **one-dimensional simple random walk**. The value $X_n$ represents the position of a particle (or fortune of a gambler) at step $n$.

---

## Technical Details

### Memory and Dependence Spectrum
Stochastic processes can be categorized by how much past history influences the future:

1. **Independent Process:**
   $$P(X_{n+1} = j \mid X_n = i, X_{n-1} = i_{n-1}, \dots, X_0 = i_0) = P(X_{n+1} = j)$$
   No past information provides predictive power (too restrictive for most real systems).
2. **Markov Process:**
   $$P(X_{n+1} = j \mid X_n = i, X_{n-1} = i_{n-1}, \dots, X_0 = i_0) = P(X_{n+1} = j \mid X_n = i)$$
   The future depends only on the present state, rendering the prior trajectory irrelevant once the present state is known.
3. **General Non-Markov Process:**
   The conditional distribution of $X_{n+1}$ depends non-trivially on the entire path $(X_0, X_1, \dots, X_n)$.

---

## Important Properties

- **Time Homogeneity:** A discrete-time process is time-homogeneous (stationary transition probabilities) if the conditional probability of moving from state $i$ to state $j$ does not depend on the absolute time index $n$:
  $$P(X_{n+1} = j \mid X_n = i) = P(X_1 = j \mid X_0 = i)$$
- **Stationarity:** A process is strictly stationary if the joint distribution of $(X_{t_1+h}, \dots, X_{t_k+h})$ is identical to that of $(X_{t_1}, \dots, X_{t_k})$ for all shifts $h$.

---

## Common Mistakes

- **Confusing State Space with Time Parameter:** Mixing up the possible values $X_t \in S$ with the indices $t \in T$. For example, a process can have continuous time ($T = [0, \infty)$) but a discrete state space ($S = \{0, 1, 2, \dots\}$), as in a Poisson process.
- **Assuming All Processes Are Independent:** Treating $X_{n+1}$ as independent of $X_n$. In almost all stochastic models, temporal correlation is the primary object of study.
- **Overlooking Sample Paths:** Confusing the marginal distribution of $X_t$ at a single time $t$ with the joint distribution across multiple time steps.

---

## Exam Relevance

In CSE301 examinations:
- Questions frequently ask students to classify a process by its time set (discrete vs continuous) and state space (discrete vs continuous).
- Students must identify whether a given physical or probabilistic system satisfies the Markov property or requires state augmentation to become Markovian.
- Serves as the formal gateway to [[Markov Chain]], queuing models, and Poisson processes.

---

## Related Concepts

- [[Markov Chain]]
- [[Classification of States in Markov Chains]]
- [[Stationary and Limiting Distributions in Markov Chains]]

---

## Prerequisites

- [[Random Variables and Probability Distributions|Random Variable]]
- [[Conditional Probability and Independence|Conditional Probability]]

---

## Problems

- [[Problem — Rain Prediction Two Days Ahead]]

---

## Sources

- [[01 - Sources/Lectures/Markov_Chain.pdf]] (Slide 2–3)
- [[01 - Sources/Textbooks/Sheldon M. Ross book Markov Chain Chapter.pdf]] (Section 4.1, pp. 193–194)
