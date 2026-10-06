---
type: concept
course: cse301
status: active
order: 35
---

# Point Estimation

> 📖 **Reading Order:** Step 35 of 92 | **Module 6:** Statistical Inference  
> ◄ **Previous:** [[Problem — CLT Implications for the Weak Law of Large Numbers]] | ► **Next:** [[Bias-Variance Decomposition]]

---

---

## Starting Point and the Problem

In the real world, we rarely or never observe an entire population:
- We cannot measure the exact blood pressure of every human on Earth.
- We cannot test every microchip produced by a semiconductor fab until destruction.
- We cannot observe infinite flips of a coin.

Instead, we collect a finite random sample of size $n$. Point estimation provides a principled mathematical framework for extracting a single optimal guess of the underlying true data-generating parameter from noisy, incomplete observations.

---

---

## Developing the Idea

Imagine you are an archer shooting arrows at a hidden bullseye ($\theta$):
- Each sample dataset $X_1, \dots, X_n$ represents one shot.
- Because each sample contains different random data points, your arrow lands at a different spot $\hat{\theta}_n$ each time.
- If you repeat the experiment many times with new datasets, you generate a scatter of arrow marks.

Point estimation asks two intuitive questions:
1. **Is your aim centered on the bullseye?** If your arrows cluster symmetrically around the bullseye without systematic drift, your estimator is **unbiased**. If your arrows consistently veer to the upper-right, your estimator is **biased**.
2. **How tightly clustered are your shots?** Even if you are aimed at the center, do your arrows scatter all over the target (high standard error) or land in a tight cluster (low standard error)?

A great estimator has both **zero bias** (centered on truth) and **low standard error** (tightly clustered).

---

---

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

---

## How It Works

### How It Works

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
### Important Properties

| Property | Symbol | Formula | Ideal Value |
|---|---|---|---|
| Unbiasedness | $\text{bias}(\hat{\theta}_n)$ | $E_\theta[\hat{\theta}_n] - \theta$ | $0$ |
| Precision (Variance) | $\text{Var}_\theta(\hat{\theta}_n)$ | $E_\theta[(\hat{\theta}_n - E[\hat{\theta}_n])^2]$ | $\to 0$ as $n \to \infty$ |
| Standard Error | $\text{se}(\hat{\theta}_n)$ | $\sqrt{\text{Var}_\theta(\hat{\theta}_n)}$ | Decreases at rate $1/\sqrt{n}$ |
| Mean Squared Error | $\text{MSE}(\hat{\theta}_n)$ | $\text{bias}^2(\hat{\theta}_n) + \text{Var}_\theta(\hat{\theta}_n)$ | $\to 0$ as $n \to \infty$ |
| Consistency | $\hat{\theta}_n \xrightarrow{P} \theta$ | $P(\lvert\hat{\theta}_n - \theta\rvert > \epsilon) \to 0$ | Holds for large samples |

---

---

## Example

### Bernoulli Parameter Estimation and Standard Error

Consider estimating the success probability $p$ of a $\text{Bernoulli}(p)$ process from $n$ independent trials $X_1, \dots, X_n \sim \text{Bernoulli}(p)$, where $\mathbb{E}[X_i] = p$ and $\operatorname{Var}(X_i) = p(1-p)$.

Define the sample mean estimator:
$$\hat{p}_n = \bar{X} = \frac{1}{n}\sum_{i=1}^n X_i$$

1. **Unbiasedness Verification:**
   $$\mathbb{E}[\hat{p}_n] = \mathbb{E}\left[\frac{1}{n}\sum_{i=1}^n X_i\right] = \frac{1}{n}\sum_{i=1}^n \mathbb{E}[X_i] = \frac{1}{n}(np) = p$$
   $$\text{bias}(\hat{p}_n) = \mathbb{E}[\hat{p}_n] - p = p - p = 0 \implies \hat{p}_n \text{ is strictly unbiased.}$$

2. **Standard Error Derivation:**
   Because $X_i$ are independent:
   $$\operatorname{Var}(\hat{p}_n) = \operatorname{Var}\left(\frac{1}{n}\sum_{i=1}^n X_i\right) = \frac{1}{n^2}\sum_{i=1}^n \operatorname{Var}(X_i) = \frac{1}{n^2} n p(1-p) = \frac{p(1-p)}{n}$$
   $$\text{se}(\hat{p}_n) = \sqrt{\frac{p(1-p)}{n}}$$

3. **Plug-in Estimated Standard Error:**
   Since $p$ is unknown, substituting $\hat{p}_n$ yields:
   $$\widehat{\text{se}}(\hat{p}_n) = \sqrt{\frac{\hat{p}_n(1 - \hat{p}_n)}{n}}$$

For a full numerical demonstration comparing sample proportion point estimates and interval bounds, see [[Bernoulli Parameter Estimation and Confidence Interval Example]].

---

## Technical Details

### Point Estimation Targets
Point estimation applies to a diverse hierarchy of statistical objectives:
1. **Parametric parameters:** Mean $\mu$, variance $\sigma^2$, rate $\lambda$, success probability $p$.
2. **Nonparametric functions:** Empirical Cumulative Distribution Function $\hat{F}_n(x) = \frac{1}{n}\sum_{i=1}^n I(X_i \le x)$, kernel density estimators $\hat{f}_h(x)$.
3. **Regression functions:** Conditional expectation $r(x) = \mathbb{E}[Y \mid X = x]$.
4. **Predictive estimation:** Bounding errors on an unobserved future outcome $Y_{n+1}$.

### Cramér-Rao Lower Bound (CRLB) and Efficiency
Let $X_1, \dots, X_n \overset{\text{iid}}{\sim} f(x; \theta)$. The **Fisher Information** in a single observation is:
$$I_1(\theta) = \mathbb{E}_\theta\left[ \left( \frac{\partial}{\partial \theta} \ln f(X; \theta) \right)^2 \right] = -\mathbb{E}_\theta\left[ \frac{\partial^2}{\partial \theta^2} \ln f(X; \theta) \right]$$
For an i.i.d. sample of size $n$, $I_n(\theta) = n I_1(\theta)$.
Under mild regularity conditions, the variance of any **unbiased estimator** $\hat{\theta}_n$ satisfies the **Cramér-Rao Lower Bound**:
$$\operatorname{Var}(\hat{\theta}_n) \ge \frac{1}{n I_1(\theta)}$$
- An unbiased estimator whose variance achieves the CRLB for all $\theta$ is termed **efficient**.
- An estimator that attains minimum variance among all unbiased estimators is a **Uniformly Minimum Variance Unbiased Estimator (UMWUE)**.

### Bias vs. Variance Trade-off in MSE
Unbiasedness alone is insufficient for practical optimality:
$$\text{MSE}(\hat{\theta}_n) = \text{bias}^2(\hat{\theta}_n) + \operatorname{Var}(\hat{\theta}_n)$$
An unbiased estimator with huge variance is inferior to a slightly biased shrinkage estimator with substantially reduced variance (e.g. Ridge regression or Bayesian posterior means).

---

---

## Important Properties and Why They Hold

- **Mathematical Rigor:** Satisfies Kolmogorov's probability axioms or standard asymptotic regularity conditions.
- **Convergence / Consistency:** Guarantees stability under large sample limits or repeated independent trials.

---

## Common Mistakes

### Common Mistakes

1. **Confusing parameter $\theta$ with estimator $\hat{\theta}_n$:** 
   Treating $\theta$ as a random variable. Under classical frequentist inference, $\theta$ is a fixed number. $\hat{\theta}_n$ is the random variable because it changes from sample to sample.
2. **Confusing Standard Deviation ($\sigma$) with Standard Error ($\text{se}$):**
   Standard deviation $\sigma$ measures the spread of individual data points in the population. Standard error $\text{se} = \sigma / \sqrt{n}$ measures the spread of the sample average $\hat{\theta}_n$ across multiple datasets.
3. **Believing unbiasedness implies consistency:**
   An estimator can be completely unbiased for every $n$ yet fail to converge to the truth (e.g., ignoring all data except the first observation: $\hat{\mu} = X_1$).

---

---

## Exam Relevance

In exam problems, you will typically be asked to:
1. Determine whether an estimator is unbiased by computing $\mathbb{E}[\hat{\theta}_n]$.
2. Compute the exact standard error $\text{se}(\hat{\theta}_n)$ using independence and variance rules.
3. Construct the plug-in estimated standard error $\widehat{\text{se}}$.
4. Evaluate Mean Squared Error and discuss the trade-off between bias and variance via [[Bias-Variance Decomposition]].
5. Contrast point estimation with interval estimation ([[Confidence Intervals and Confidence Sets]]).

---

---

## Related Concepts

- [[Estimator Consistency and Convergence]]
- [[Bias-Variance Decomposition]]
- [[Confidence Intervals and Confidence Sets]]
- [[Maximum Likelihood Estimation]]
- [[Bayesian Inference]]

---

---

## Prerequisites

- [[Random Variables and Probability Distributions]]
- [[Covariance and Correlation]]
- [[Law of Large Numbers]]

---

---

## Problems

- [[Problem — Unbiased yet Inconsistent Estimator Analysis]]
- [[Problem — Sample Variance Bias and Bessel's Correction Derivation]]

---

---

## Sources

- [[01 - Sources/Lectures/Point Estimation and Confidence Intervals.pdf]]
