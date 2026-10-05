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

## Building the idea

For a variable modeled by a density, probability is area. A density may exceed one over a short interval while its total area remains one; that is perfectly valid. At any individual point the probability is zero, so interval probabilities are the meaningful questions.

Each common density encodes a different shape or mechanism. A uniform distribution gives equal density across a bounded interval. An exponential distribution models a waiting time with constant hazard. A normal distribution describes a symmetric bell-shaped quantity, specified by a center and a variance. Gamma distributions extend exponential waiting times, with shape and either rate or scale parameters.

Check units: an exponential rate has units of inverse time, while its mean is time. In a gamma rate parameterization the mean is shape divided by rate; in a scale parameterization it is shape times scale. Writing the density or naming the convention avoids an otherwise easy reciprocal error.

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

## Exam Relevance

### Cross-Topic Connections / Exam Relevance

- **Queueing Theory:** Inter-arrival and service times in [[M-M-1 Queue]] are exponentially distributed due to memorylessness.
- **Inference:** Normal and Beta distributions form the pillars of [[Normal-Normal Conjugate Updating Formula]] and [[Maximum Likelihood Estimation]].
- **Asymptotics:** The [[Central Limit Theorem]] guarantees that sums of arbitrary finite-variance distributions converge to the Normal distribution.

---

## What to carry forward

Here “continuous distribution” refers to the absolutely continuous, density-based distributions used in the course. In general probability theory, absence of point masses alone does not guarantee a density.

## Sources

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lectures 10–12, pages 30–39)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_6.pdf` & `7.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Chapters 5, 6, 8)
