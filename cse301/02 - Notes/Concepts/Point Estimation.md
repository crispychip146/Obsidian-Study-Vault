---
type: concept
course: cse301
status: active
---

# Point Estimation

## Definition

**Point estimation** is the process of using sample data to calculate a single best-guess numerical value (a "point") for an unknown population parameter $\theta$, distribution characteristic, or functional quantity.

Formally, let $X_1, X_2, \dots, X_n \overset{\text{iid}}{\sim} F_\theta$, where $F_\theta = \{f(x; \theta) : \theta \in \Theta\}$ is a parametric family of distributions indexed by parameter $\theta \in \Theta$. 

A **point estimator** $\hat{\theta}_n$ of $\theta$ is any statistic (a function of the observed sample data):
$$\hat{\theta}_n = g(X_1, X_2, \dots, X_n)$$

A crucial distinction in statistical theory:
- $\theta$: The true population parameter. It is a **fixed, unknown constant** (under the frequentist paradigm). It does NOT have a probability distribution.
- $\hat{\theta}_n$: The estimator. Because it is a function of random sample variables $X_1, \dots, X_n$, it is itself a **random variable** with its own probability distribution, known as the **sampling distribution**.
- $\hat{\theta}$: An estimate (the observed numerical realization when actual sample values $x_1, \dots, x_n$ are plugged into $g$).

---

## Intuition

Imagine you are an archer shooting arrows at a hidden bullseye ($\theta$):
- Each sample dataset $X_1, \dots, X_n$ represents one shot.
- Because each sample contains different random data points, your arrow lands at a different spot $\hat{\theta}_n$ each time.
- If you repeat the experiment many times with new datasets, you generate a scatter of arrow marks.

Point estimation asks two intuitive questions:
1. **Is your aim centered on the bullseye?** If your arrows cluster symmetrically around the bullseye without systematic drift, your estimator is **unbiased**. If your arrows consistently veer to the upper-right, your estimator is **biased**.
2. **How tightly clustered are your shots?** Even if you are aimed at the center, do your arrows scatter all over the target (high standard error) or land in a tight cluster (low standard error)?

A great estimator has both **zero bias** (centered on truth) and **low standard error** (tightly clustered).

---

## Why It Exists

In the real world, we rarely or never observe an entire population:
- We cannot measure the exact blood pressure of every human on Earth.
- We cannot test every microchip produced by a semiconductor fab until destruction.
- We cannot observe infinite flips of a coin.

Instead, we collect a finite random sample of size $n$. Point estimation provides a principled mathematical framework for extracting a single optimal guess of the underlying true data-generating parameter from noisy, incomplete observations.

---

## How It Works

Point estimation evaluates estimators using several fundamental statistical metrics:

### 1. Estimator Bias
The **bias** of an estimator $\hat{\theta}_n$ is the difference between its expected value (the center of its sampling distribution) and the true parameter value $\theta$:
$$\text{bias}(\hat{\theta}_n) = E_\theta[\hat{\theta}_n] - \theta$$

- An estimator is **unbiased** if $\text{bias}(\hat{\theta}_n) = 0$ for all $\theta \in \Theta$, which means:
  $$E_\theta[\hat{\theta}_n] = \theta$$
- An estimator is **positively biased** if $E_\theta[\hat{\theta}_n] > \theta$ (it systematically overestimates).
- An estimator is **negatively biased** if $E_\theta[\hat{\theta}_n] < \theta$ (it systematically underestimates).

> **Notation Note:** The subscript $\theta$ in $E_\theta[\cdot]$ indicates that the expectation is taken with respect to the data distribution $f(x; \theta)$. It does **not** mean averaging over $\theta$.

### 2. Standard Error
The **standard error** ($\text{se}$) is the standard deviation of the sampling distribution of $\hat{\theta}_n$:
$$\text{se}(\hat{\theta}_n) = \sqrt{\text{Var}_\theta(\hat{\theta}_n)}$$

The standard error measures the dispersion or variability of the estimator across different hypothetical samples of size $n$.
Because $\text{se}(\hat{\theta}_n)$ usually depends on the unknown true parameter $\theta$, we replace unknown parameters with their estimates to obtain the **estimated standard error**:
$$\widehat{\text{se}}(\hat{\theta}_n)$$

### 3. Mean Squared Error (MSE)
To measure total error combining both systematic offset (bias) and random scatter (variance), we use the Mean Squared Error:
$$\text{MSE}(\hat{\theta}_n) = E_\theta[(\hat{\theta}_n - \theta)^2]$$

By the [[Bias-Variance Decomposition]], MSE splits neatly into:
$$\text{MSE}(\hat{\theta}_n) = \text{bias}^2(\hat{\theta}_n) + \text{Var}_\theta(\hat{\theta}_n)$$

---

## Example

Consider estimating the success probability $p$ of a $\text{Bernoulli}(p)$ coin from $n$ independent flips $X_1, \dots, X_n \sim \text{Bernoulli}(p)$, where $E[X_i] = p$ and $\text{Var}(X_i) = p(1-p)$.

Define the sample mean estimator:
$$\hat{p}_n = \bar{X} = \frac{1}{n}\sum_{i=1}^n X_i$$

1. **Check Bias:**
   $$E[\hat{p}_n] = E\left[\frac{1}{n}\sum_{i=1}^n X_i\right] = \frac{1}{n}\sum_{i=1}^n E[X_i] = \frac{1}{n}(np) = p$$
   $$\text{bias}(\hat{p}_n) = E[\hat{p}_n] - p = p - p = 0 \implies \hat{p}_n \text{ is strictly unbiased.}$$

2. **Calculate Standard Error:**
   Because $X_i$ are mutually independent:
   $$\text{Var}(\hat{p}_n) = \text{Var}\left(\frac{1}{n}\sum_{i=1}^n X_i\right) = \frac{1}{n^2}\sum_{i=1}^n \text{Var}(X_i) = \frac{1}{n^2} n p(1-p) = \frac{p(1-p)}{n}$$
   $$\text{se}(\hat{p}_n) = \sqrt{\frac{p(1-p)}{n}}$$

3. **Compute Estimated Standard Error:**
   Since $p$ is unknown, plug in the estimate $\hat{p}_n$:
   $$\widehat{\text{se}}(\hat{p}_n) = \sqrt{\frac{\hat{p}_n(1 - \hat{p}_n)}{n}}$$

---

## Technical Details

### Point Estimation Targets
Point estimation applies far beyond scalar distribution parameters:
1. **Parametric parameters:** Mean $\mu$, variance $\sigma^2$, rate $\lambda$, probability $p$.
2. **Nonparametric functions:** Empirical Cumulative Distribution Function $\hat{F}_n(x)$, probability density function $\hat{f}(x)$ via kernel density estimation.
3. **Regression functions:** Conditional expectation $r(x) = E[Y \mid X = x]$.
4. **Predictive estimation:** Guessing the value of an unobserved future outcome $Y_{n+1}$.

### Bias vs. Variance Trade-off
Unbiasedness is often considered an overrated property in modern statistics. An unbiased estimator can have immense variance, making it practically useless on any individual dataset. Conversely, introducing a tiny amount of bias can substantially reduce the variance, leading to a much smaller total $\text{MSE}$.

---

## Important Properties

| Property | Symbol | Formula | Ideal Value |
|---|---|---|---|
| Unbiasedness | $\text{bias}(\hat{\theta}_n)$ | $E_\theta[\hat{\theta}_n] - \theta$ | $0$ |
| Precision (Variance) | $\text{Var}_\theta(\hat{\theta}_n)$ | $E_\theta[(\hat{\theta}_n - E[\hat{\theta}_n])^2]$ | $\to 0$ as $n \to \infty$ |
| Standard Error | $\text{se}(\hat{\theta}_n)$ | $\sqrt{\text{Var}_\theta(\hat{\theta}_n)}$ | Decreases at rate $1/\sqrt{n}$ |
| Mean Squared Error | $\text{MSE}(\hat{\theta}_n)$ | $\text{bias}^2(\hat{\theta}_n) + \text{Var}_\theta(\hat{\theta}_n)$ | $\to 0$ as $n \to \infty$ |
| Consistency | $\hat{\theta}_n \xrightarrow{P} \theta$ | $P(\lvert\hat{\theta}_n - \theta\rvert > \epsilon) \to 0$ | Holds for large samples |

---

## Common Mistakes

1. **Confusing parameter $\theta$ with estimator $\hat{\theta}_n$:** 
   Treating $\theta$ as a random variable. Under classical frequentist inference, $\theta$ is a fixed number. $\hat{\theta}_n$ is the random variable because it changes from sample to sample.
2. **Confusing Standard Deviation ($\sigma$) with Standard Error ($\text{se}$):**
   Standard deviation $\sigma$ measures the spread of individual data points in the population. Standard error $\text{se} = \sigma / \sqrt{n}$ measures the spread of the sample average $\hat{\theta}_n$ across multiple datasets.
3. **Believing unbiasedness implies consistency:**
   An estimator can be completely unbiased for every $n$ yet fail to converge to the truth (e.g., ignoring all data except the first observation: $\hat{\mu} = X_1$).

---

## Exam Relevance

In exam problems, you will typically be asked to:
1. Determine whether an estimator is unbiased by computing $E[\hat{\theta}_n]$.
2. Compute the exact standard error $\text{se}(\hat{\theta}_n)$ using independence and variance rules.
3. Construct the plug-in estimated standard error $\widehat{\text{se}}$.
4. Evaluate Mean Squared Error and discuss the trade-off between bias and variance.
5. Contrast point estimation with interval estimation ([[Confidence Intervals and Confidence Sets]]).

---

## Related Concepts

- [[Estimator Consistency and Convergence]]
- [[Bias-Variance Decomposition]]
- [[Confidence Intervals and Confidence Sets]]
- [[Maximum Likelihood Estimation]]
- [[Bayesian Inference]]

---

## Prerequisites

- [[Stochastic Process]] (Random Variables, Expectation, Variance)
- Linearity of Expectation and Properties of Variance

---

## Problems

- [[Problem — Unbiased yet Inconsistent Estimator Analysis]]
- [[Problem — Sample Variance Bias and Bessel's Correction Derivation]]

---

## Sources

- [[01 - Sources/Lectures/Point Estimation and Confidence Intervals.pdf]]
