---
type: formula
course: cse301
status: active
order: 36
---

# Bias-Variance Decomposition

> 📖 **Reading Order:** Step 36 of 92 | **Module 6:** Statistical Inference  
> ◄ **Previous:** [[Point Estimation]] | ► **Next:** [[Estimator Consistency and Convergence]]

---

## Formula

For any point estimator $\hat{\theta}_n$ of an unknown parameter $\theta$:

$$\text{MSE}(\hat{\theta}_n) = \text{bias}^2(\hat{\theta}_n) + \text{Var}_\theta(\hat{\theta}_n)$$

where:
- $\text{MSE}(\hat{\theta}_n) = E_\theta[(\hat{\theta}_n - \theta)^2]$ is the **Mean Squared Error**.
- $\text{bias}(\hat{\theta}_n) = E_\theta[\hat{\theta}_n] - \theta$ is the **estimator bias**.
- $\text{Var}_\theta(\hat{\theta}_n) = E_\theta[(\hat{\theta}_n - E_\theta[\hat{\theta}_n])^2]$ is the **estimator variance**.

---

## Variables

| Symbol | Meaning | Role |
|---|---|---|
| $\theta$ | True parameter value | Fixed constant |
| $\hat{\theta}_n$ | Estimator calculated from sample of size $n$ | Random variable |
| $\bar{\theta}_n = E_\theta[\hat{\theta}_n]$ | Expected value of the estimator | Constant for fixed $\theta$ |
| $\text{bias}(\hat{\theta}_n)$ | Systematic deviation from the truth | Error due to faulty assumptions/bias |
| $\text{Var}_\theta(\hat{\theta}_n)$ | Variability across random samples | Error due to sampling sensitivity |
| $\text{MSE}(\hat{\theta}_n)$ | Expected squared Euclidean error | Total estimation penalty |

---

## Conditions

1. The second moment of the estimator must exist: $E_\theta[\hat{\theta}_n^2] < \infty$.
2. The parameter $\theta$ is fixed and unknown.
3. The expectation $E_\theta[\cdot]$ is taken over the joint distribution of the sample data $\prod_{i=1}^n f(x_i; \theta)$.

---

## Intuition

Total squared error decomposes cleanly into two orthogonal components:
1. **Bias squared:** How far off your average estimate is from the true parameter value.
2. **Variance:** How much your estimate fluctuates from sample to sample around its own average.

This identity reveals the fundamental **bias-variance trade-off** in statistics and machine learning:
- An overly simple model or restricted estimator may suffer from high bias (underfitting).
- An overly complex or unrestricted estimator may have low bias but high variance (overfitting).
- Minimizing total MSE often requires accepting a small non-zero bias in exchange for a massive reduction in variance.

---

## Derivation

Let $\bar{\theta}_n = E_\theta[\hat{\theta}_n]$. Note that $\bar{\theta}_n$ is a non-random constant for a fixed $\theta$.

Add and subtract $\bar{\theta}_n$ inside the squared difference:
$$\hat{\theta}_n - \theta = (\hat{\theta}_n - \bar{\theta}_n) + (\bar{\theta}_n - \theta)$$

Now expand the square:
$$(\hat{\theta}_n - \theta)^2 = (\hat{\theta}_n - \bar{\theta}_n)^2 + 2(\hat{\theta}_n - \bar{\theta}_n)(\bar{\theta}_n - \theta) + (\bar{\theta}_n - \theta)^2$$

Take the mathematical expectation $E_\theta[\cdot]$ of both sides:
$$E_\theta[(\hat{\theta}_n - \theta)^2] = E_\theta\left[(\hat{\theta}_n - \bar{\theta}_n)^2\right] + 2 E_\theta\left[(\hat{\theta}_n - \bar{\theta}_n)(\bar{\theta}_n - \theta)\right] + E_\theta\left[(\bar{\theta}_n - \theta)^2\right]$$

Evaluate each term individually:
1. **First term:**
   $$E_\theta\left[(\hat{\theta}_n - \bar{\theta}_n)^2\right] = E_\theta\left[(\hat{\theta}_n - E_\theta[\hat{\theta}_n])^2\right] = \text{Var}_\theta(\hat{\theta}_n)$$
2. **Second term (cross-product):**
   Because $(\bar{\theta}_n - \theta)$ is a constant, it can be factored out of the expectation:
   $$E_\theta\left[(\hat{\theta}_n - \bar{\theta}_n)(\bar{\theta}_n - \theta)\right] = (\bar{\theta}_n - \theta) \cdot E_\theta[\hat{\theta}_n - \bar{\theta}_n]$$
   By linearity of expectation:
   $$E_\theta[\hat{\theta}_n - \bar{\theta}_n] = E_\theta[\hat{\theta}_n] - \bar{\theta}_n = \bar{\theta}_n - \bar{\theta}_n = 0$$
   Therefore, the entire cross-product term vanishes identically:
   $$2(\bar{\theta}_n - \theta) \cdot 0 = 0$$
3. **Third term:**
   Since $(\bar{\theta}_n - \theta) = (E_\theta[\hat{\theta}_n] - \theta) = \text{bias}(\hat{\theta}_n)$ is a constant:
   $$E_\theta\left[(\bar{\theta}_n - \theta)^2\right] = (\bar{\theta}_n - \theta)^2 = \text{bias}^2(\hat{\theta}_n)$$

Summing the three terms gives:
$$\text{MSE}(\hat{\theta}_n) = \text{Var}_\theta(\hat{\theta}_n) + \text{bias}^2(\hat{\theta}_n) \quad \blacksquare$$

---

## Example

Suppose $X_1, \dots, X_n \sim N(\mu, \sigma^2)$. We wish to estimate the variance $\sigma^2$.
Consider two competing estimators:
1. Unbiased estimator: $S^2 = \frac{1}{n-1}\sum (X_i - \bar{X})^2$
2. Scaled estimator: $\hat{\sigma}_c^2 = c \sum_{i=1}^n (X_i - \bar{X})^2$ for a constant $c$.

For $S^2$:
- $\text{bias}(S^2) = 0$
- $\text{Var}(S^2) = \frac{2\sigma^4}{n-1}$
- $\text{MSE}(S^2) = \frac{2\sigma^4}{n-1}$

For the estimator $\tilde{\sigma}^2 = \frac{1}{n+1}\sum (X_i - \bar{X})^2$:
- $E[\tilde{\sigma}^2] = \frac{n-1}{n+1}\sigma^2 \implies \text{bias} = -\frac{2}{n+1}\sigma^2 \ne 0$
- $\text{Var}(\tilde{\sigma}^2) = \left(\frac{n-1}{n+1}\right)^2 \frac{2\sigma^4}{n-1} = \frac{2(n-1)}{(n+1)^2}\sigma^4$
- $\text{MSE}(\tilde{\sigma}^2) = \left(-\frac{2}{n+1}\sigma^2\right)^2 + \frac{2(n-1)}{(n+1)^2}\sigma^4 = \frac{4 + 2n - 2}{(n+1)^2}\sigma^4 = \frac{2}{n+1}\sigma^4$

Notice that:
$$\frac{2}{n+1}\sigma^4 < \frac{2}{n-1}\sigma^4 \implies \text{MSE}(\tilde{\sigma}^2) < \text{MSE}(S^2)$$
Even though $\tilde{\sigma}^2$ is biased, it has strictly **lower MSE** than the unbiased sample variance $S^2$ for all sample sizes $n$!

---

## Common Mistakes

1. **Forgetting to square the bias:**
   Writing $\text{MSE} = \text{bias} + \text{Var}$ instead of $\text{bias}^2 + \text{Var}$. Notice units: if $\theta$ is in meters, variance and MSE are in $\text{meters}^2$, so bias must be squared.
2. **Assuming the cross-product term is non-zero:**
   Thinking $E[(\hat{\theta} - \bar{\theta})(\bar{\theta} - \theta)] \ne 0$. It is always zero because $E[\hat{\theta} - \bar{\theta}] = 0$ and the other factor is non-random.

---

## Related Concepts

- [[Point Estimation]]
- [[Estimator Consistency and Convergence]]
- [[Maximum Likelihood Estimation]]

---

## Prerequisites

- [[Point Estimation]]
- Linearity of Expectation and Definition of Variance

---

## Problems

- [[Problem — Unbiased yet Inconsistent Estimator Analysis]]
- [[Problem — Sample Variance Bias and Bessel's Correction Derivation]]

---

## Sources

- [[01 - Sources/Lectures/Point Estimation and Confidence Intervals.pdf]]
