---
type: concept
course: cse301
status: active
order: 31
---

# Law of Large Numbers

> 📖 **Reading Order:** Step 31 of 92 | **Module 5:** Convergence of Random Variables and Asymptotics  
> ◄ **Previous:** [[Problem — Bounding Tail Probabilities with Chebyshev and Chernoff]] | ► **Next:** [[Central Limit Theorem]]

---

---

## Starting Point and the Problem

Probability and statistical inference model uncertainty in physical and computer systems. When analyzing stochastic phenomena related to Law of Large Numbers, naive counting or deterministic approximations fail. We establish a formal mathematical foundation to quantify outcomes and evaluate expectations rigorously.

---

## Developing the Idea

By formalizing sample spaces, probability measures, state transitions, or likelihood ratios, Law of Large Numbers reveals the underlying structural mechanics that govern random variables and estimation errors.

---

## Definition

The **Law of Large Numbers (LLN)** guarantees that as the number of independent, identically distributed (i.i.d.) observations increases, the sample average converges to the true theoretical population mean.

Let $X_1, X_2, \dots, X_n$ be an i.i.d. sequence of random variables with finite expectation $\mathbb{E}[X_i] = \mu$. Define the sample mean as:
$$\bar{X}_n = \frac{1}{n}\sum_{i=1}^n X_i$$

There are two fundamental versions of the Law of Large Numbers, distinguished by the mode of mathematical convergence:
1. **The Weak Law of Large Numbers (WLLN)** — Convergence in Probability.
2. **The Strong Law of Large Numbers (SLLN)** — Almost Sure Convergence.

---

---

## How It Works

### WLLN vs. SLLN: What Is the Difference?

| Feature | Weak Law of Large Numbers (WLLN) | Strong Law of Large Numbers (SLLN) |
|---|---|---|
| **Mode of Convergence** | Convergence in Probability ($\xrightarrow{P}$) | Almost Sure Convergence ($\xrightarrow{\text{a.s.}}$) |
| **Mathematical Meaning** | For any fixed large $n$, the probability of being within $\epsilon$ of $\mu$ is close to 1. | An entire infinite sequence/trajectory $\bar{X}_1, \bar{X}_2, \bar{X}_3, \dots$ enters and stays inside the $\epsilon$-tube forever. |
| **Tail Exceptions** | Allows rare "flukes" infinitely often in the future, as long as they become increasingly improbable at step $n$. | Forbids infinite future deviations; once $n$ is past some threshold $N(\omega)$, $\lvert \bar{X}_n - \mu \rvert < \epsilon$ for **all** subsequent $n$. |
| **Strength** | SLLN $\implies$ WLLN. (The converse is not generally true). | Stronger mathematical guarantee. |

---
### The Gambler's Fallacy

A frequent psychological error is the **Gambler's Fallacy**: believing that after observing 10 consecutive "Tails", the next flip is "due" to be "Heads" to balance the average.
- The Law of Large Numbers works by **dilution**, NOT by compensation!
- The coin has no memory. If you flip 10 Tails, those 10 excess Tails are never "cancelled out" by future Heads.
- Instead, as $n$ grows to $1,000,000$, the 10 excess Tails become negligible:
  $$\frac{\text{Tails} + 10}{1,000,000} \to 0.50001 \approx 0.5$$
- The sample mean converges because the denominator $n$ grows, not because the numerator self-corrects.

---
### Role in Statistics and Machine Learning

1. **Empirical Risk Minimization (ERM):** Justifies replacing unknown population risk $\mathbb{E}[L(f(X), Y)]$ with training error $\frac{1}{n}\sum_{i=1}^n L(f(x_i), y_i)$.
2. **Monte Carlo Integration:** Approximates intractable multidimensional integrals $I = \int g(x) dx$ by sampling $X_i \sim \operatorname{Unif}$ and taking $\frac{1}{n}\sum g(X_i) \to I$.
3. **Consistency of Estimators:** An estimator $\hat{\theta}_n$ is consistent if $\hat{\theta}_n \xrightarrow{P} \theta$ (see [[Estimator Consistency and Convergence]]).

---

---

## Example

See worked numerical applications in the linked example notes.

---

## Technical Details

Refer to Blitzstein & Hwang for measure-theoretic details and moment generating properties.

---

## Important Properties and Why They Hold

### The Weak Law of Large Numbers (WLLN)

### Statement:
For any arbitrary precision tolerance $\epsilon > 0$:
$$\lim_{n \to \infty} P\left( \lvert \bar{X}_n - \mu \rvert < \epsilon \right) = 1$$
Equivalently:
$$\lim_{n \to \infty} P\left( \lvert \bar{X}_n - \mu \rvert \ge \epsilon \right) = 0$$

In formal terminology, $\bar{X}_n$ **converges in probability** to $\mu$:
$$\bar{X}_n \xrightarrow{P} \mu$$

### Proof under Finite Variance $\sigma^2 < \infty$:
Using the [[Chebyshev Inequality]]:
- Mean: $\mathbb{E}[\bar{X}_n] = \mu$
- Variance: $\operatorname{Var}(\bar{X}_n) = \frac{\sigma^2}{n}$

Applying Chebyshev directly:
$$P(\lvert \bar{X}_n - \mu \rvert \ge \epsilon) \le \frac{\operatorname{Var}(\bar{X}_n)}{\epsilon^2} = \frac{\sigma^2}{n\epsilon^2}$$
As $n \to \infty$:
$$\lim_{n \to \infty} \frac{\sigma^2}{n\epsilon^2} = 0$$
By the squeeze theorem, the probability converges to 0. $\blacksquare$

*(Note: The WLLN remains true even if $\sigma^2 = \infty$, as long as $\mathbb{E}[\lvert X_i \rvert] < \infty$, proven using characteristic functions or truncation)*.

---
### The Strong Law of Large Numbers (SLLN)

### Statement:
The sample mean converges to $\mu$ with probability 1:
$$P\left( \lim_{n \to \infty} \bar{X}_n = \mu \right) = 1$$

In formal terminology, $\bar{X}_n$ **converges almost surely (a.s.)** to $\mu$:
$$\bar{X}_n \xrightarrow{\text{a.s.}} \mu$$

---

---

## Common Mistakes

- Confusing conditional probabilities with unconditional joint probabilities.
- Misapplying asymptotic normal approximations when sample sizes are small or distributions are heavily skewed.

---

## Exam Relevance

### Cross-Topic Connections / Exam Relevance

- **Central Limit Theorem:** While LLN tells us **where** $\bar{X}_n$ converges (to $\mu$), the [[Central Limit Theorem]] describes **how** it fluctuates around $\mu$ at rate $1/\sqrt{n}$.
- **Markov Chains:** The Ergodic Theorem for Markov chains is the Markovian generalization of the SLLN: $\frac{1}{n}\sum_{t=1}^n f(X_t) \xrightarrow{\text{a.s.}} \sum_i \pi_i f(i)$ (see [[Stationary and Limiting Distributions in Markov Chains]]).

---

---

## Related Concepts

- [[Probability Axioms and Naive Probability]]
- [[Random Variables and Probability Distributions]]

---

## Prerequisites

- [[Probability Axioms and Naive Probability]]

---

## Problems

- [[Problem — Birthday Collisions and Approximation]]

---

## Sources

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lectures 19–20, pages 60–62)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_11.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Section 10.4: Law of Large Numbers)
