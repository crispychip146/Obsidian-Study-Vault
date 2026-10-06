---
type: example
course: cse301
status: active
order: 51
---

# Bernoulli Parameter Estimation and Confidence Interval Example

> 📖 **Reading Order:** Step 51 of 103 | **Module 6:** Statistical Inference  
> ◄ **Previous:** [[Normal-Based Large-Sample Confidence Interval]] | ► **Next:** [[Berger-Wolpert Confidence Set Puzzle Example]]
---
## Problem

Let $X_1, X_2, \dots, X_n \overset{\text{iid}}{\sim} \text{Bernoulli}(p)$ be independent random trials with unknown success probability $p \in (0, 1)$.
Consider the standard sample mean estimator:
$$\hat{p}_n = \bar{X} = \frac{1}{n}\sum_{i=1}^n X_i$$

1. Prove that $\hat{p}_n$ is strictly unbiased for $p$.
2. Derive the exact variance and standard error $\text{se}(\hat{p}_n)$.
3. Prove that $\hat{p}_n$ is a consistent estimator of $p$.
4. Construct an approximate large-sample $95\%$ confidence interval for $p$ when $n = 100$ and the observed number of successes is $\sum_{i=1}^{100} X_i = 60$.
---
## Given

- Sample: $X_1, \dots, X_n \overset{\text{iid}}{\sim} \text{Bernoulli}(p)$
- PMF: $P(X_i = 1) = p$, $P(X_i = 0) = 1 - p$
- Mean of single observation: $E[X_i] = 1 \cdot p + 0 \cdot (1-p) = p$
- Variance of single observation: $\text{Var}(X_i) = E[X_i^2] - (E[X_i])^2 = p - p^2 = p(1-p)$
- Numerical data: $n = 100$, $\sum X_i = 60$
---
## Required

1. Prove $\text{bias}(\hat{p}_n) = 0$.
2. Formulate $\text{Var}(\hat{p}_n)$ and $\text{se}(\hat{p}_n)$.
3. Prove $\hat{p}_n \xrightarrow{P} p$.
4. Calculate the numerical $95\%$ confidence interval.
---
## Understanding the Problem and Choosing the Method

Identify the random variables, state the conditional distributions, select the appropriate probabilistic law or updating formula, and execute the algebraic substitutions step by step.

---

## Solution

### Concepts Used

- [[Point Estimation]]
- [[Estimator Consistency and Convergence]]
- [[Bias-Variance Decomposition]]
- [[Normal-Based Large-Sample Confidence Interval]]

---
### Solution

### Step 1: Unbiasedness Proof
Compute the expected value of $\hat{p}_n$:
$$E[\hat{p}_n] = E\left[\frac{1}{n}\sum_{i=1}^n X_i\right]$$
By linearity of expectation:
$$E[\hat{p}_n] = \frac{1}{n}\sum_{i=1}^n E[X_i] = \frac{1}{n} \sum_{i=1}^n p = \frac{1}{n} (n p) = p$$

Compute the bias:
$$\text{bias}(\hat{p}_n) = E[\hat{p}_n] - p = p - p = 0$$
Thus, $\hat{p}_n$ is an **unbiased estimator** of $p$ for every sample size $n \ge 1$.

---

### Step 2: Variance and Standard Error Derivation
Compute the variance of $\hat{p}_n$:
$$\text{Var}(\hat{p}_n) = \text{Var}\left(\frac{1}{n}\sum_{i=1}^n X_i\right)$$
Because constant factors scale squared when pulled out of variance:
$$\text{Var}(\hat{p}_n) = \frac{1}{n^2} \text{Var}\left(\sum_{i=1}^n X_i\right)$$
Since $X_1, \dots, X_n$ are mutually independent, the variance of the sum is the sum of the variances:
$$\text{Var}(\hat{p}_n) = \frac{1}{n^2}\sum_{i=1}^n \text{Var}(X_i) = \frac{1}{n^2} \sum_{i=1}^n p(1-p) = \frac{1}{n^2} \big(n p(1-p)\big) = \frac{p(1-p)}{n}$$

Taking the square root gives the exact standard error:
$$\text{se}(\hat{p}_n) = \sqrt{\frac{p(1-p)}{n}}$$

Because $p$ is unknown, the plug-in estimated standard error is:
$$\widehat{\text{se}}(\hat{p}_n) = \sqrt{\frac{\hat{p}_n(1 - \hat{p}_n)}{n}}$$

---

### Step 3: Consistency Proof
We apply the MSE Consistency Theorem from [[Estimator Consistency and Convergence]]:
$$\text{MSE}(\hat{p}_n) = \text{bias}^2(\hat{p}_n) + \text{Var}(\hat{p}_n) = 0^2 + \frac{p(1-p)}{n} = \frac{p(1-p)}{n}$$

Take the limit as $n \to \infty$:
$$\lim_{n \to \infty} \text{MSE}(\hat{p}_n) = \lim_{n \to \infty} \frac{p(1-p)}{n} = 0$$

Since $\text{MSE}(\hat{p}_n) = E[(\hat{p}_n - p)^2] \to 0$, we have:
$$\hat{p}_n \xrightarrow{qm} p$$
Because convergence in quadratic mean implies convergence in probability (via Markov's inequality):
$$\hat{p}_n \xrightarrow{P} p$$
Thus, $\hat{p}_n$ is a **consistent estimator** of $p$.

---

### Step 4: Constructing the 95% Confidence Interval
Given $n = 100$ and $\sum_{i=1}^{100} X_i = 60$:
1. **Point estimate:**
   $$\hat{p} = \frac{60}{100} = 0.60$$
2. **Estimated standard error:**
   $$\widehat{\text{se}} = \sqrt{\frac{0.60 \times (1 - 0.60)}{100}} = \sqrt{\frac{0.24}{100}} = \sqrt{0.0024} \approx 0.04899$$
3. **Critical value for 95% confidence ($\alpha = 0.05$):**
   $$z_{0.025} = 1.96$$
4. **Margin of error:**
   $$\text{ME} = 1.96 \times 0.04899 \approx 0.0960$$
5. **Confidence Interval:**
   $$C_n = 0.60 \pm 0.0960 \implies [0.5040, 0.6960]$$
---
## Result

1. $\hat{p}_n$ is strictly unbiased ($\text{bias} = 0$).
2. $\text{se}(\hat{p}_n) = \sqrt{\frac{p(1-p)}{n}}$, with estimated version $\widehat{\text{se}} = \sqrt{\frac{\hat{p}(1-\hat{p})}{n}}$.
3. $\hat{p}_n$ is consistent because $\text{MSE} \to 0$ as $n \to \infty$.
4. The $95\%$ confidence interval is $[0.5040, 0.6960]$ (or $50.40\%$ to $69.60\%$).
---
## Why This Works

The sample proportion is an average of i.i.d. indicators. By the Law of Large Numbers, it concentrates around the true mean $p$. By the Central Limit Theorem, the distribution of $\hat{p}_n$ converges rapidly to a normal distribution $N(p, \text{se}^2)$, allowing us to use standard normal quantiles to form valid confidence bounds.
---
## Common Mistakes

- Forgetting to divide the sum of variances by $n^2$, leading to variance growing with $n$ rather than decaying as $1/n$.
- Using the true $p$ inside the formula for standard error when constructing the confidence interval (we do not know $p$, so we must plug in $\hat{p}$).
---
## General Method

Extract the reusable problem-solving pattern: define random variables, write down the joint distribution, condition on observed data, and normalize the resulting distribution.

---

## Related Concepts

- [[Point Estimation]]
- [[Estimator Consistency and Convergence]]
- [[Normal-Based Large-Sample Confidence Interval]]
---
## Sources

- [[cse301/01 - Sources/Lectures/Point Estimation and Confidence Intervals.pdf]]
