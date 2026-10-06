---
type: concept
course: cse301
status: active
order: 9
---

# Continuous Probability Distributions

> 📖 **Reading Order:** Step 09 of 92 | **Module 2:** Random Variables and Distributions  
> ◄ **Previous:** [[Discrete Probability Distributions]] | ► **Next:** [[Joint and Marginal Distributions]]

---

---

## Starting Point and the Problem

Probability and statistical inference model uncertainty in physical and computer systems. When analyzing stochastic phenomena related to Continuous Probability Distributions, naive counting or deterministic approximations fail. We establish a formal mathematical foundation to quantify outcomes and evaluate expectations rigorously.

---

## Developing the Idea

By formalizing sample spaces, probability measures, state transitions, or likelihood ratios, Continuous Probability Distributions reveals the underlying structural mechanics that govern random variables and estimation errors.

---

## Definition

**Continuous Probability Distributions** is a foundational concept in probability and mathematical statistics governing random variables, probability distributions, or statistical decision-making.

---

## How It Works

### Overview

A continuous random variable $X$ can take any real value within an interval (or union of intervals). Its behavior is characterized by a **Probability Density Function (PDF)** $f_X(x)$ such that:
$$P(a \le X \le b) = \int_a^b f_X(x) \, dx$$
with $f_X(x) \ge 0$ everywhere and $\int_{-\infty}^\infty f_X(x) \, dx = 1$.

---
### Continuous Uniform Distribution: $\operatorname{Unif}(a, b)$

- **Story:** Complete uncertainty over a bounded interval $[a, b]$; all sub-intervals of equal length are equally likely.
- **PDF:**
  $$f_X(x) = \begin{cases} \frac{1}{b - a} & \text{if } a \le x \le b \\ 0 & \text{otherwise} \end{cases}$$
- **CDF:**
  $$F_X(x) = \begin{cases} 0 & x < a \\ \frac{x - a}{b - a} & a \le x \le b \\ 1 & x > b \end{cases}$$
- **Mean & Variance:**
  $$\mathbb{E}[X] = \frac{a + b}{2}, \quad \operatorname{Var}(X) = \frac{(b - a)^2}{12}$$
- **Universality of the Uniform (Probability Integral Transform):**
  - If $U \sim \operatorname{Unif}(0, 1)$ and $F$ is a continuous CDF with inverse $F^{-1}$, then $X = F^{-1}(U)$ has CDF $F$.
  - Conversely, if $X$ has continuous CDF $F_X$, then $F_X(X) \sim \operatorname{Unif}(0, 1)$.
  - *Application:* Fundamental to pseudo-random number generation and Monte Carlo simulations.

---
### Normal (Gaussian) Distribution: $\mathcal{N}(\mu, \sigma^2)$

- **Story:** The central distribution of probability and statistics, arising whenever many small, independent random disturbances add together (formalized by the [[Central Limit Theorem]]).
- **PDF:**
  $$f_X(x) = \frac{1}{\sigma \sqrt{2\pi}} \exp\left( -\frac{(x - \mu)^2}{2\sigma^2} \right), \quad x \in \mathbb{R}$$
- **Mean & Variance:**
  $$\mathbb{E}[X] = \mu, \quad \operatorname{Var}(X) = \sigma^2$$
- **Standard Normal Distribution: $Z \sim \mathcal{N}(0, 1)$**
  - Standardizing: $Z = \frac{X - \mu}{\sigma} \sim \mathcal{N}(0, 1)$.
  - PDF: $\phi(z) = \frac{1}{\sqrt{2\pi}} e^{-z^2/2}$
  - CDF: $\Phi(z) = \int_{-\infty}^z \phi(t) \, dt$ (symmetry: $\Phi(-z) = 1 - \Phi(z)$).
- **The Empirical Rule (68–95–99.7 Rule):**
  - $P(\mu - \sigma \le X \le \mu + \sigma) \approx 68.27\%$
  - $P(\mu - 2\sigma \le X \le \mu + 2\sigma) \approx 95.45\%$
  - $P(\mu - 3\sigma \le X \le \mu + 3\sigma) \approx 99.73\%$
- **Linear Combinations:**
  If $X_1 \sim \mathcal{N}(\mu_1, \sigma_1^2)$ and $X_2 \sim \mathcal{N}(\mu_2, \sigma_2^2)$ are independent:
  $$aX_1 + bX_2 \sim \mathcal{N}(a\mu_1 + b\mu_2, a^2\sigma_1^2 + b^2\sigma_2^2)$$

---
### Exponential Distribution: $\operatorname{Exp}(\lambda)$

- **Story:** The continuous waiting time between events in a Poisson process with rate $\lambda > 0$ (e.g., time until the next packet arrival, customer service duration, or radioactive decay).
- **PDF:**
  $$f_X(x) = \begin{cases} \lambda e^{-\lambda x} & \text{if } x \ge 0 \\ 0 & \text{if } x < 0 \end{cases}$$
- **CDF & Survival Function:**
  $$F_X(x) = 1 - e^{-\lambda x}, \quad P(X > x) = e^{-\lambda x} \quad (x \ge 0)$$
- **Mean & Variance:**
  $$\mathbb{E}[X] = \frac{1}{\lambda}, \quad \operatorname{Var}(X) = \frac{1}{\lambda^2}$$
- **Memoryless Property:**
  $$P(X > s + t \mid X > s) = \frac{P(X > s + t)}{P(X > s)} = \frac{e^{-\lambda(s+t)}}{e^{-\lambda s}} = e^{-\lambda t} = P(X > t)$$
  The Exponential distribution is the **only continuous distribution** with the memoryless property.
- **Minimum of Independent Exponentials:**
  If $X_1 \sim \operatorname{Exp}(\lambda_1), \dots, X_n \sim \operatorname{Exp}(\lambda_n)$ are independent:
  $$\min(X_1, \dots, X_n) \sim \operatorname{Exp}\left(\sum_{i=1}^n \lambda_i\right)$$
  and $P(X_i = \min(X_1, \dots, X_n)) = \frac{\lambda_i}{\sum_{j=1}^n \lambda_j}$.

---
### Gamma Distribution: $\operatorname{Gamma}(a, \lambda)$

- **Story:** Waiting time until the $a$-th event in a Poisson process with rate $\lambda$ (for integer shape $a$, also called the **Erlang distribution**).
- **PDF:**
  $$f_X(x) = \frac{\lambda^a}{\Gamma(a)} x^{a-1} e^{-\lambda x}, \quad x > 0$$
  where $\Gamma(a) = \int_0^\infty t^{a-1} e^{-t} \, dt$ is the Gamma function ($\Gamma(n) = (n-1)!$ for $n \in \mathbb{N}$).
- **Mean & Variance:**
  $$\mathbb{E}[X] = \frac{a}{\lambda}, \quad \operatorname{Var}(X) = \frac{a}{\lambda^2}$$
- **Connection to Exponential:** If $X_1, \dots, X_a \overset{\text{i.i.d.}}{\sim} \operatorname{Exp}(\lambda)$, then $\sum_{i=1}^a X_i \sim \operatorname{Gamma}(a, \lambda)$.

---
### Beta Distribution: $\operatorname{Beta}(a, b)$

- **Story:** Continuous distribution supported on the interval $(0, 1)$, widely used as a prior distribution for probabilities and proportions.
- **PDF:**
  $$f(p) = \frac{1}{B(a, b)} p^{a - 1} (1 - p)^{b - 1}, \quad 0 < p < 1$$
  where $B(a, b) = \frac{\Gamma(a)\Gamma(b)}{\Gamma(a + b)}$ is the Beta function.
- **Mean & Variance:**
  $$\mathbb{E}[X] = \frac{a}{a + b}, \quad \operatorname{Var}(X) = \frac{ab}{(a + b)^2 (a + b + 1)}$$
- **Special Case:** $\operatorname{Beta}(1, 1) \equiv \operatorname{Unif}(0, 1)$.
- **Conjugacy:** Serves as the conjugate prior for Bernoulli/Binomial likelihoods (see [[Beta-Binomial Conjugate Updating Formula]]).

---
### Summary Reference Table

| Distribution | Notation | Parameters | PDF $f(x)$ | Support | Mean $\mathbb{E}[X]$ | Variance $\operatorname{Var}(X)$ |
|---|---|---|---|---|---|---|
| **Uniform** | $\operatorname{Unif}(a, b)$ | $a < b$ | $\frac{1}{b-a}$ | $[a, b]$ | $\frac{a+b}{2}$ | $\frac{(b-a)^2}{12}$ |
| **Normal** | $\mathcal{N}(\mu, \sigma^2)$ | $\mu \in \mathbb{R}, \sigma > 0$ | $\frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{(x-\mu)^2}{2\sigma^2}}$ | $\mathbb{R}$ | $\mu$ | $\sigma^2$ |
| **Exponential** | $\operatorname{Exp}(\lambda)$ | $\lambda > 0$ | $\lambda e^{-\lambda x}$ | $[0, \infty)$ | $\frac{1}{\lambda}$ | $\frac{1}{\lambda^2}$ |
| **Gamma** | $\operatorname{Gamma}(a, \lambda)$ | $a > 0, \lambda > 0$ | $\frac{\lambda^a}{\Gamma(a)}x^{a-1}e^{-\lambda x}$ | $(0, \infty)$ | $\frac{a}{\lambda}$ | $\frac{a}{\lambda^2}$ |
| **Beta** | $\operatorname{Beta}(a, b)$ | $a > 0, b > 0$ | $\frac{p^{a-1}(1-p)^{b-1}}{B(a, b)}$ | $(0, 1)$ | $\frac{a}{a+b}$ | $\frac{ab}{(a+b)^2(a+b+1)}$ |

---

---

## Example

### Worked Example: Gaussian Standardization and Exponential Waiting Times

1. **Gaussian Standardization:**
   Suppose server response time follows $X \sim \mathcal{N}(\mu = 120\text{ ms}, \sigma^2 = 400\text{ ms}^2)$, so $\sigma = 20\text{ ms}$.
   To find the probability that a query takes longer than $150\text{ ms}$:
   $$Z = \frac{X - \mu}{\sigma} = \frac{150 - 120}{20} = 1.5$$
   $$P(X > 150) = P(Z > 1.5) = 1 - \Phi(1.5) \approx 1 - 0.9332 = 0.0668 \quad (6.68\%)$$

2. **Exponential Waiting Probability:**
   Let $T \sim \operatorname{Exp}(\lambda = 0.5)$ minutes be the inter-arrival time of network packets.
   $$P(T > 4) = e^{-\lambda \times 4} = e^{-0.5 \times 4} = e^{-2} \approx 0.1353 \quad (13.53\%)$$

For extended worked applications, see:
- [[Exponential Distribution Memorylessness Example]] — Analytical proof and server queue applications of memorylessness.
- [[Normal Approximation to Binomial and Poisson Example]] — Large-sample Gaussian approximations with continuity correction.

---

## Technical Details

### Universality of the Uniform (Probability Integral Transform)
Let $F$ be any continuous, strictly increasing CDF with inverse $F^{-1}$.
1. **Forward Transformation:** If $U \sim \operatorname{Unif}(0, 1)$, then $X = F^{-1}(U)$ has CDF $F$:
   $$P(X \le x) = P(F^{-1}(U) \le x) = P(U \le F(x)) = F(x)$$
2. **Probability Integral Transform:** If $X$ has continuous CDF $F$, then $Y = F(X) \sim \operatorname{Unif}(0, 1)$.
This universality underpins all pseudo-random simulation, Monte Carlo methods, and copula modeling.

### Gaussian Tail Bounds (Mills' Ratio)
For a standard normal variable $Z \sim \mathcal{N}(0, 1)$ and $z > 0$:
$$\left(\frac{1}{z} - \frac{1}{z^3}\right) \phi(z) < P(Z > z) < \frac{1}{z} \phi(z)$$
where $\phi(z) = \frac{1}{\sqrt{2\pi}} e^{-z^2/2}$. Asymptotically as $z \to \infty$, $P(Z > z) \sim \frac{\phi(z)}{z}$.

### Duality Between Poisson and Gamma Processes
If events occur according to a Poisson process with rate $\lambda$, let $T_a$ be the arrival time of the $a$-th event ($T_a \sim \operatorname{Gamma}(a, \lambda)$) and $N(t)$ be the count of arrivals in $[0, t]$ ($N(t) \sim \operatorname{Pois}(\lambda t)$):
$$P(T_a \le t) = P(N(t) \ge a) = \sum_{k=a}^\infty \frac{e^{-\lambda t}(\lambda t)^k}{k!}$$
This identity connects continuous waiting times with discrete event counts.

For Bayesian prior applications, see [[Beta-Binomial Conjugate Updating Formula]] and [[Normal-Normal Conjugate Updating Formula]].

---

## Important Properties and Why They Hold

- **Mathematical Rigor:** Satisfies Kolmogorov's probability axioms or standard asymptotic regularity conditions.
- **Convergence / Consistency:** Guarantees stability under large sample limits or repeated independent trials.

---

## Common Mistakes

- Confusing conditional probabilities with unconditional joint probabilities.
- Misapplying asymptotic normal approximations when sample sizes are small or distributions are heavily skewed.

---

## Exam Relevance

### Cross-Topic Connections / Exam Relevance

- **Queueing Theory:** Inter-arrival and service times in [[M-M-1 Queue]] are exponentially distributed due to memorylessness.
- **Inference:** Normal and Beta distributions form the pillars of [[Normal-Normal Conjugate Updating Formula]] and [[Maximum Likelihood Estimation]].
- **Asymptotics:** The [[Central Limit Theorem]] guarantees that sums of arbitrary finite-variance distributions converge to the Normal distribution.

---

---

## Related Concepts

- [[Discrete Probability Distributions]]
- [[Random Variables and Probability Distributions]]
- [[Central Limit Theorem]]
- [[M-M-1 Queue]]
- [[Beta-Binomial Conjugate Updating Formula]]

---

## Prerequisites

- [[Random Variables and Probability Distributions]]

---

## Problems

- [[Problem — Compound Random Sum via Adam and Eve's Laws]]

---

## Sources

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lectures 10–12, pages 30–39)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_6.pdf` & `7.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Chapters 5, 6, 8)
