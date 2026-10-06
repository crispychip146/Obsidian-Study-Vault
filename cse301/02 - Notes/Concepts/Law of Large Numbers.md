---
type: concept
course: cse301
status: active
order: 42
---

# Law of Large Numbers

> 📖 **Reading Order:** Step 42 of 103 | **Module 5:** Convergence of Random Variables and Asymptotics  
> ◄ **Previous:** [[Problem — Bounding Tail Probabilities with Chebyshev and Chernoff]] | ► **Next:** [[Central Limit Theorem]]
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
## Example

### Empirical Convergence of Bernoulli Coin Flips

Consider tossing a fair coin with $X_i \sim \operatorname{Bernoulli}(p = 0.5)$, where $\mu = 0.5$ and $\sigma^2 = p(1-p) = 0.25$.
Let $\bar{X}_n = \frac{1}{n} \sum_{i=1}^n X_i$ denote the proportion of heads after $n$ flips.
Suppose we demand that the sample proportion be within $\epsilon = 0.02$ of $0.5$ (i.e., between $0.48$ and $0.52$) with probability at least $0.95$.

Using the [[Chebyshev Inequality]] bound established in the WLLN proof:
$$P(|\bar{X}_n - 0.5| \ge 0.02) \le \frac{\sigma^2}{n \epsilon^2} = \frac{0.25}{n (0.02)^2} = \frac{0.25}{0.0004 n} = \frac{625}{n}$$
Setting $\frac{625}{n} \le 0.05$ yields:
$$n \ge \frac{625}{0.05} = 12,500$$

While Chebyshev's distribution-free bound provides an upper bound of $n = 12,500$, applying the [[Central Limit Theorem]] (see [[Normal Approximation to Binomial and Poisson Example]]) reveals that standard normal tails require only $n \approx (1.96 \times 0.5 / 0.02)^2 \approx 2,401$ flips.
This illustrates how the WLLN guarantees certainty in the limit $n \to \infty$, while concentration inequalities and the CLT quantify the rate of convergence.

For a detailed problem walking through Chebyshev sample-sizing versus CLT asymptotics, see [[Problem — CLT Implications for the Weak Law of Large Numbers]].

---

## Technical Details

### Modes of Convergence and Measure-Theoretic Foundations

1. **Convergence in Probability vs. Almost Sure Convergence:**
   - **WLLN ($\bar{X}_n \xrightarrow{P} \mu$):** For all $\epsilon > 0$, $\lim_{n \to \infty} P(\{\omega : |\bar{X}_n(\omega) - \mu| \ge \epsilon\}) = 0$. In measure-theoretic terms, the measure of the set of exceptional sample paths shrinks to zero at each fixed $n$.
   - **SLLN ($\bar{X}_n \xrightarrow{\text{a.s.}} \mu$):** $P(\{\omega : \lim_{n \to \infty} \bar{X}_n(\omega) = \mu\}) = 1$. The set of paths along which $\bar{X}_n(\omega)$ fails to converge to $\mu$ is a null set.
   - By the Borel-Cantelli lemma, if $\sum_{n=1}^\infty P(|\bar{X}_n - \mu| \ge \epsilon) < \infty$ for all $\epsilon > 0$, then $\bar{X}_n \xrightarrow{\text{a.s.}} \mu$.

2. **Khinchin's Weak Law vs. Kolmogorov's Strong Law:**
   - **Khinchin's WLLN:** Requires only that $X_i$ are i.i.d. with finite mean $\mathbb{E}[|X_i|] < \infty$. Finite variance is **not** required. This is proved using characteristic functions: $\phi_{\bar{X}_n}(t) = (\phi_X(t/n))^n = (1 + i\mu t/n + o(t/n))^n \to e^{i\mu t}$.
   - **Kolmogorov's Strong Law of Large Numbers:** For i.i.d. $X_i$, $\bar{X}_n \xrightarrow{\text{a.s.}} \mu$ if and only if $\mathbb{E}[|X_i|] < \infty$. If $\mathbb{E}[|X_i|] = \infty$, then $\limsup_{n \to \infty} |\bar{X}_n| = \infty$ almost surely.

3. **Failure of LLN: The Cauchy Distribution:**
   - If $X_i \sim \operatorname{Cauchy}(0, \gamma)$, the characteristic function is $\phi_X(t) = e^{-\gamma |t|}$.
   - Then $\phi_{\bar{X}_n}(t) = (e^{-\gamma |t/n|})^n = e^{-\gamma |t|}$.
   - The sample average $\bar{X}_n$ has the exact same Cauchy distribution as a single draw! Averaging 1,000,000 Cauchy variables does not reduce the variance or converge to any constant because the mean is undefined ($\mathbb{E}[|X_i|] = \infty$).

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
## Common Mistakes

- **Believing in Compensation (Gambler's Fallacy):** Believing that the sample mean converges because future flips compensate for past imbalances. The LLN works through dilution (growing denominator $n$), not compensation.
- **Confusing Sample Mean with Sample Sum:** Forgetting that while $\bar{X}_n \xrightarrow{P} \mu$, the sum $S_n = \sum X_i$ diverges with variance $n\sigma^2$, so deviations $|S_n - n\mu|$ grow on the order of $O(\sqrt{n})$.
- **Applying LLN without Finite Expectation:** Assuming every empirical average converges. If $\mathbb{E}[|X|] = \infty$ (e.g., Cauchy distribution), LLN fails completely.

---

## Exam Relevance

### Cross-Topic Connections / Exam Relevance

- **Central Limit Theorem:** While LLN tells us **where** $\bar{X}_n$ converges (to $\mu$), the [[Central Limit Theorem]] describes **how** it fluctuates around $\mu$ at rate $1/\sqrt{n}$.
- **Markov Chains:** The Ergodic Theorem for Markov chains is the Markovian generalization of the SLLN: $\frac{1}{n}\sum_{t=1}^n f(X_t) \xrightarrow{\text{a.s.}} \sum_i \pi_i f(i)$ (see [[Stationary and Limiting Distributions in Markov Chains]]).
---
## Related Concepts

- [[Central Limit Theorem]] — Asymptotic distribution of deviations.
- [[Estimator Consistency and Convergence]] — Statistical consistency formalization.
- [[Chebyshev Inequality]] — Non-parametric concentration tool.
- [[Normal Approximation to Binomial and Poisson Example]] — Application of normal approximations.

---

## Prerequisites

- [[Random Variables and Probability Distributions]]
- [[Chebyshev Inequality]]
- [[Continuous Probability Distributions]]

---

## Problems

- [[Problem — CLT Implications for the Weak Law of Large Numbers]]

---

## Sources

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lectures 19–20, pages 60–62)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_11.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Section 10.4: Law of Large Numbers)
