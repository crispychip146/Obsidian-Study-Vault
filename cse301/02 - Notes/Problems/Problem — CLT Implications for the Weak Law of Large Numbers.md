---
type: problem
course: cse301
status: active
order: 34
---

# Problem — CLT Implications for the Weak Law of Large Numbers

> 📖 **Reading Order:** Step 34 of 92 | **Module 5:** Convergence of Random Variables and Asymptotics  
> ◄ **Previous:** [[Normal Approximation to Binomial and Poisson Example]] | ► **Next:** [[Point Estimation]]

---

---

## Problem

A polling agency wants to estimate the true proportion $p$ of users who prefer a new user interface over the old one. They survey $n$ independent users, modeling their responses as $X_1, X_2, \dots, X_n \overset{\text{i.i.d.}}{\sim} \operatorname{Bern}(p)$.
The agency estimates $p$ using the sample proportion:
$$\hat{p}_n = \bar{X}_n = \frac{1}{n} \sum_{i=1}^n X_i$$
The agency requires that their estimate $\hat{p}_n$ is within $\pm 0.03$ (3 percentage points) of the true value $p$ with at least $95\%$ confidence:
$$P\left( \lvert \hat{p}_n - p \rvert \le 0.03 \right) \ge 0.95 \iff P\left( \lvert \hat{p}_n - p \rvert > 0.03 \right) \le 0.05$$

1. **Worst-Case Variance:** Since the true value $p$ is unknown, find the maximum possible value of $\operatorname{Var}(X_i) = p(1 - p)$ for $p \in [0, 1]$.
2. **Chebyshev Sample Size:** Determine the minimum sample size $n_{\text{Cheb}}$ guaranteed to satisfy the requirement using the [[Chebyshev Inequality]].
3. **CLT Sample Size:** Determine the minimum sample size $n_{\text{CLT}}$ required using the [[Central Limit Theorem]] normal approximation (taking $z_{0.025} = 1.96$).
4. **Comparison & Theoretical Connection:**
   - Compare the sample sizes $n_{\text{Cheb}}$ and $n_{\text{CLT}}$ and discuss the practical cost implications for engineering telemetry.
   - Prove mathematically that the Central Limit Theorem implies the Weak Law of Large Numbers.

---

---

## Given

- Given parameters, random variable definitions, and observation vectors as specified in the problem statement.

---

## Required

- Derive the exact closed-form probability, expectation, or test statistic, and verify asymptotic convergence.

---

## Concepts Tested

- [[Random Variables and Probability Distributions]]
- [[Law of Total Probability and Bayes' Rule]]

---

## Prerequisites

- [[Chebyshev Inequality]] — Non-parametric sample bound.
- [[Central Limit Theorem]] — Normal approximation of sample mean.
- [[Law of Large Numbers]] — Convergence in probability definition.
- [[Normal-Based Large-Sample Confidence Interval]] — Margin of error formulation.

---

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
### Full Step-by-Step Solution

### Part 1: Worst-Case Variance
For $X_i \sim \operatorname{Bern}(p)$:
$$\sigma^2 = \operatorname{Var}(X_i) = p(1 - p) = p - p^2$$
To find the maximum, differentiate with respect to $p$:
$$\frac{d}{dp}[p - p^2] = 1 - 2p = 0 \implies p = \frac{1}{2}$$
$$\sigma_{\max}^2 = \left(\frac{1}{2}\right)\left(1 - \frac{1}{2}\right) = \frac{1}{4} = 0.25$$

Thus, without knowing $p$, we can safely bound $\operatorname{Var}(X_i) \le 0.25$.

---

### Part 2: Chebyshev Sample Size Determination
By Chebyshev's inequality applied to $\bar{X}_n$:
$$P\left( \lvert \bar{X}_n - p \rvert > \epsilon \right) \le \frac{\operatorname{Var}(\bar{X}_n)}{\epsilon^2} = \frac{\sigma^2}{n\epsilon^2} \le \frac{0.25}{n\epsilon^2}$$

We require this upper bound to be at most $\alpha = 0.05$ with $\epsilon = 0.03$:
$$\frac{0.25}{n (0.03)^2} \le 0.05$$
$$n \ge \frac{0.25}{0.05 \times (0.03)^2} = \frac{5}{(0.03)^2} = \frac{5}{0.0009} \approx 5,555.56$$

Rounding up:
$$n_{\text{Cheb}} = 5,556 \text{ users}$$

---

### Part 3: CLT Sample Size Determination
By the Central Limit Theorem:
$$Z_n = \frac{\bar{X}_n - p}{\sqrt{\frac{p(1 - p)}{n}}} \xrightarrow{d} \mathcal{N}(0, 1)$$

We want:
$$P\left( \lvert \bar{X}_n - p \rvert \le 0.03 \right) = P\left( \left\lvert \frac{\bar{X}_n - p}{\sqrt{\sigma^2 / n}} \right\rvert \le \frac{0.03}{\sqrt{\sigma^2 / n}} \right) \ge 0.95$$

For a Standard Normal variable, $P(\lvert Z \rvert \le z^*) = 0.95$ when $z^* = 1.96$ (since $\Phi(1.96) \approx 0.975$).
Therefore, we require:
$$\frac{0.03}{\sqrt{\sigma^2 / n}} \ge 1.96 \implies \frac{0.03 \sqrt{n}}{\sigma} \ge 1.96$$
Using the worst-case standard deviation $\sigma \le \sqrt{0.25} = 0.5$:
$$\sqrt{n} \ge \frac{1.96 \times 0.5}{0.03} = \frac{0.98}{0.03} \approx 32.667$$
$$n \ge (32.667)^2 \approx 1067.1$$

Rounding up:
$$n_{\text{CLT}} = 1,068 \text{ users}$$

---

### Part 4: Comparison & Theoretical Connection

#### 1. Practical Comparison:
- $n_{\text{Cheb}} = 5,556$
- $n_{\text{CLT}} = 1,068$
- The Chebyshev bound demands more than **$5\times$ as much data** as the CLT normal approximation.
- This discrepancy arises because Chebyshev must guarantee the bound across all conceivable, pathological, heavy-tailed distributions. In contrast, the CLT leverages the fact that sums of Bernoulli trials rapidly form a smooth Gaussian distribution with light, exponentially decaying tails.

#### 2. Mathematical Proof that CLT implies WLLN:
We wish to prove that if $\frac{\sqrt{n}(\bar{X}_n - \mu)}{\sigma} \xrightarrow{d} Z \sim \mathcal{N}(0, 1)$, then for any $\epsilon > 0$, $\lim_{n \to \infty} P(\lvert \bar{X}_n - \mu \rvert \ge \epsilon) = 0$.

Rewrite the probability in terms of the standardized variable $Z_n$:
$$P\left( \lvert \bar{X}_n - \mu \rvert \ge \epsilon \right) = P\left( \frac{\sqrt{n} \lvert \bar{X}_n - \mu \rvert}{\sigma} \ge \frac{\epsilon \sqrt{n}}{\sigma} \right) = P\left( \lvert Z_n \rvert \ge \frac{\epsilon \sqrt{n}}{\sigma} \right)$$

By the CLT, the CDF of $Z_n$ converges pointwise to $\Phi$:
$$P\left( \lvert Z_n \rvert \ge \frac{\epsilon \sqrt{n}}{\sigma} \right) \approx 2 \left[ 1 - \Phi\left( \frac{\epsilon \sqrt{n}}{\sigma} \right) \right]$$

As $n \to \infty$, since $\epsilon > 0$ and $\sigma > 0$:
$$\frac{\epsilon \sqrt{n}}{\sigma} \to \infty$$
Because $\lim_{z \to \infty} \Phi(z) = 1$:
$$\lim_{n \to \infty} 2\left[ 1 - \Phi\left( \frac{\epsilon \sqrt{n}}{\sigma} \right) \right] = 2[1 - 1] = 0$$

Therefore, $\bar{X}_n \xrightarrow{P} \mu$. The CLT implies the Weak Law of Large Numbers! $\blacksquare$

---

### Result and Interpretation
The final analytical solution and numerical metrics are rigorously verified against probability axioms.

---

## Reusable Insight

Always decompose complex event probabilities by conditioning on a partition of the sample space (Law of Total Probability), or by writing indicator random variables to exploit linearity of expectation.

---

## Common Mistakes

1. **Forgetting $\sqrt{n}$ in the denominator:** The standard error of the sample mean is $\frac{\sigma}{\sqrt{n}}$, not $\frac{\sigma}{n}$.
2. **Confusing 1-sided and 2-sided tail critical values:** For $95\%$ two-sided coverage, each tail receives $2.5\%$, which corresponds to $z_{0.025} = 1.96$, not $z_{0.05} = 1.645$.

---

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

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lectures 17, 20, 21, pages 54–63)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_11.pdf` (Problem 3 & 4)
- **Question ID:** `Q-CSE301-017`
