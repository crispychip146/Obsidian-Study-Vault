---
type: problem
course: cse301
status: active
---

# Problem — Unbiased yet Inconsistent Estimator Analysis

## Problem

Let $X_1, X_2, \dots, X_n$ be an independent and identically distributed (i.i.d.) sample from a population distribution with unknown mean $\mu$ and known finite variance $\sigma^2 > 0$.

An analyst proposes the estimator:
$$\hat{\mu}_n = X_1$$
That is, the analyst simply records the first observed value and discards the remaining $n - 1$ observations.

1. Calculate the bias of $\hat{\mu}_n$. Is $\hat{\mu}_n$ unbiased?
2. Calculate the variance and standard error of $\hat{\mu}_n$.
3. Compute the Mean Squared Error $\text{MSE}(\hat{\mu}_n)$.
4. Determine whether $\hat{\mu}_n$ is a consistent estimator of $\mu$ as $n \to \infty$. Justify your answer formally using the definition of convergence in probability.
5. Contrast $\hat{\mu}_n$ with the standard sample mean $\bar{X}_n = \frac{1}{n}\sum_{i=1}^n X_i$.

---

## Given

- $X_1, \dots, X_n \overset{\text{iid}}{\sim} (\mu, \sigma^2)$
- $E[X_i] = \mu$ for all $i$
- $\text{Var}(X_i) = \sigma^2 > 0$ for all $i$
- Estimator: $\hat{\mu}_n = X_1$

---

## Required

1. $\text{bias}(\hat{\mu}_n)$ and unbiasedness classification.
2. $\text{Var}(\hat{\mu}_n)$ and $\text{se}(\hat{\mu}_n)$.
3. $\text{MSE}(\hat{\mu}_n)$.
4. Rigorous proof of consistency or inconsistency.
5. Comparison of large-sample behaviors.

---

## Concepts Tested

- [[Point Estimation]]
- [[Estimator Consistency and Convergence]]
- [[Bias-Variance Decomposition]]

---

## Prerequisites

- [[Point Estimation]]
- Basic Definition of Expectation, Variance, and Convergence in Probability

---

## Question Type

- Conceptual & Analytical Derivation

---

## Solution

### 1. Bias Calculation
The expected value of $\hat{\mu}_n$ is:
$$E[\hat{\mu}_n] = E[X_1] = \mu$$
The bias is:
$$\text{bias}(\hat{\mu}_n) = E[\hat{\mu}_n] - \mu = \mu - \mu = 0$$
Since the bias is identically zero for all possible values of $\mu$ and all sample sizes $n \ge 1$, $\hat{\mu}_n$ is **strictly unbiased**.

---

### 2. Variance and Standard Error
The variance of $\hat{\mu}_n$ is:
$$\text{Var}(\hat{\mu}_n) = \text{Var}(X_1) = \sigma^2$$
The standard error is:
$$\text{se}(\hat{\mu}_n) = \sqrt{\text{Var}(\hat{\mu}_n)} = \sigma$$
Notice that both the variance and standard error are completely independent of the sample size $n$.

---

### 3. Mean Squared Error (MSE)
Applying the [[Bias-Variance Decomposition]]:
$$\text{MSE}(\hat{\mu}_n) = \text{bias}^2(\hat{\mu}_n) + \text{Var}(\hat{\mu}_n) = 0^2 + \sigma^2 = \sigma^2$$
The MSE is constant and strictly positive ($\sigma^2 > 0$) for every sample size $n$.

---

### 4. Consistency Determination
Recall that by definition, $\hat{\mu}_n$ is consistent if and only if $\hat{\mu}_n \xrightarrow{P} \mu$, which requires that for **every** $\epsilon > 0$:
$$\lim_{n \to \infty} P(\lvert \hat{\mu}_n - \mu \rvert > \epsilon) = 0$$

Let us evaluate this probability for $\hat{\mu}_n = X_1$:
$$P(\lvert \hat{\mu}_n - \mu \rvert > \epsilon) = P(\lvert X_1 - \mu \rvert > \epsilon)$$

Notice that $X_1$ is a single random variable whose distribution does **not change** as $n \to \infty$. 
For example, if $X_i \sim N(\mu, \sigma^2)$, then:
$$\frac{X_1 - \mu}{\sigma} \sim N(0, 1)$$
Choosing $\epsilon = \sigma$:
$$P(\lvert X_1 - \mu \rvert > \sigma) = P(\lvert Z \rvert > 1) = 2(1 - \Phi(1)) \approx 2(1 - 0.8413) = 0.3174$$
Taking the limit as $n \to \infty$:
$$\lim_{n \to \infty} P(\lvert \hat{\mu}_n - \mu \rvert > \sigma) = 0.3174 \ne 0$$

Because the probability of deviating from $\mu$ by more than $\epsilon$ never goes to zero as $n \to \infty$, $\hat{\mu}_n$ does **not** converge in probability to $\mu$.

**Conclusion:** $\hat{\mu}_n$ is **inconsistent**.

---

### 5. Comparison with Sample Mean $\bar{X}_n$

| Property | First Observation $\hat{\mu}_n = X_1$ | Sample Mean $\bar{X}_n = \frac{1}{n}\sum X_i$ |
|---|---|---|
| Bias | $0$ (Unbiased) | $0$ (Unbiased) |
| Variance | $\sigma^2$ (Constant) | $\frac{\sigma^2}{n}$ (Decays to 0) |
| Standard Error | $\sigma$ | $\frac{\sigma}{\sqrt{n}}$ |
| MSE | $\sigma^2$ | $\frac{\sigma^2}{n} \to 0$ |
| Consistency | **Inconsistent** ($\hat{\mu}_n \not\xrightarrow{P} \mu$) | **Consistent** ($\bar{X}_n \xrightarrow{P} \mu$) |
| Value of More Data | None (ignores extra data) | High (variance shrinks linearly with $n$) |

---

## Key Idea

Unbiasedness only guarantees that the *expected center* of the estimator equals the true parameter on average across infinite hypothetical repetitions of size $n$. It says **nothing** about whether the estimator concentrates around that center as $n$ grows. Consistency requires the variance (or MSE) to shrink to zero, which requires pooling information across all $n$ data points.

---

## Common Mistakes

- Concluding that an estimator must be consistent simply because it is unbiased.
- Stating that $X_1$ depends on $n$; $X_1$ is only the first observation and is completely unaffected by whether $n = 1$ or $n = 1,000,000$.

---

## Exam Pattern

This question frequently appears in midterm and final examinations to test whether students understand the fundamental theoretical distinction between finite-sample properties (unbiasedness) and asymptotic large-sample properties (consistency).

---

## Related Problems

- [[Problem — Sample Variance Bias and Bessel's Correction Derivation]]

---

## Related Concepts

- [[Point Estimation]]
- [[Estimator Consistency and Convergence]]
- [[Bias-Variance Decomposition]]

---

## Source

- [[01 - Sources/Lectures/Point Estimation and Confidence Intervals.pdf]]
