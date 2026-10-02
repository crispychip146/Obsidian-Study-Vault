---
type: example
course: cse301
status: active
order: 46
---

# Uniform Distribution Non-Regular MLE Example

> 📖 **Reading Order:** Step 46 of 92 | **Module 7:** Parametric Inference  
> ◄ **Previous:** [[Normal Distribution Parameter MLE Derivation Example]] | ► **Next:** [[Discrete and Continuous Parameter MLE Reference Examples]]

---

## Problem

Let $X_1, X_2, \dots, X_n$ be an independent and identically distributed (i.i.d.) sample from a continuous uniform distribution on the interval $[0, \theta]$:
$$X_i \overset{\text{iid}}{\sim} \text{Uniform}(0, \theta), \quad \theta > 0$$

1. Explain why standard calculus (differentiating the log-likelihood and setting it to zero) fails to find the Maximum Likelihood Estimator of $\theta$.
2. Formulate the exact likelihood function including the support indicator.
3. Derive the Maximum Likelihood Estimator $\hat{\theta}_{\text{MLE}}$.
4. Calculate the expectation $E[\hat{\theta}_{\text{MLE}}]$ and determine whether it is biased.
5. Construct an unbiased estimator based on the MLE.

---

## Given

- Probability density function:
  $$f(x; \theta) = \frac{1}{\theta} \mathbf{1}_{\{0 \le x \le \theta\}} = \begin{cases} \frac{1}{\theta} & \text{if } 0 \le x \le \theta \\ 0 & \text{otherwise} \end{cases}$$
- Sample order statistics:
  $$X_{(1)} = \min_{1 \le i \le n} X_i, \quad X_{(n)} = \max_{1 \le i \le n} X_i$$

---

## Required

1. Explanation of calculus failure.
2. Form of $L_n(\theta)$.
3. Value of $\hat{\theta}_{\text{MLE}}$.
4. Bias calculation.
5. Unbiased adjustment.

---

## Concepts Used

- [[Maximum Likelihood Estimation]]
- [[Point Estimation]]
- Non-regular estimation (parameter-dependent support)

---

## Solution

### Step 1: Why Standard Calculus Fails
If one ignores the support indicator and writes:
$$L_n(\theta) = \prod_{i=1}^n \frac{1}{\theta} = \frac{1}{\theta^n} \implies \ell_n(\theta) = -n \log \theta$$
Differentiating with respect to $\theta$:
$$\frac{d\ell_n}{d\theta} = -\frac{n}{\theta}$$
Setting $-\frac{n}{\theta} = 0$ yields **no real solution** (or $\theta \to \infty$, which minimizes rather than maximizes the likelihood).
Standard calculus fails because the support of $X_i$ depends directly on the parameter $\theta$, making the likelihood discontinuous at the boundary.

---

### Step 2: Formulation with the Support Indicator
For any individual observation $X_i$, the density $f(X_i; \theta) > 0$ if and only if $0 \le X_i \le \theta$.
If even a single observation $X_i > \theta$, it would be physically impossible to have generated that sample under parameter $\theta$, so $L_n(\theta) = 0$.

Therefore, the likelihood is non-zero if and only if **all** observations satisfy $X_i \le \theta$, which is equivalent to requiring that the maximum observation $X_{(n)} \le \theta$:
$$L_n(\theta) = \prod_{i=1}^n \left( \frac{1}{\theta} \mathbf{1}_{\{0 \le X_i \le \theta\}} \right) = \frac{1}{\theta^n} \mathbf{1}_{\{\theta \ge X_{(n)}\}}$$

In piecewise form:
$$L_n(\theta) = \begin{cases} \frac{1}{\theta^n} & \text{if } \theta \ge X_{(n)} \\ 0 & \text{if } \theta < X_{(n)} \end{cases}$$

---

### Step 3: Finding the Maximizer $\hat{\theta}_{\text{MLE}}$
Observe the behavior of $L_n(\theta)$ on the domain $\theta \in (0, \infty)$:
- For $\theta < X_{(n)}$: $L_n(\theta) = 0$.
- For $\theta \ge X_{(n)}$: $L_n(\theta) = \frac{1}{\theta^n}$.

On the valid domain $[\max(X_i), \infty)$, the function $\frac{1}{\theta^n}$ is a **strictly decreasing function** of $\theta$.
To make $\frac{1}{\theta^n}$ as large as possible, we must choose the **smallest allowable value** of $\theta$.
The smallest value of $\theta$ that does not violate the condition $\theta \ge X_{(n)}$ is $\theta = X_{(n)}$.

Therefore:
$$\hat{\theta}_{\text{MLE}} = X_{(n)} = \max_{1 \le i \le n} X_i$$

---

### Step 4: Expectation and Bias of $\hat{\theta}_{\text{MLE}}$
To find $E[X_{(n)}]$, find the CDF of the maximum:
$$F_{X_{(n)}}(t) = P(X_{(n)} \le t) = P(X_1 \le t, X_2 \le t, \dots, X_n \le t)$$
Since observations are independent and $P(X_i \le t) = \frac{t}{\theta}$ for $t \in [0, \theta]$:
$$F_{X_{(n)}}(t) = \left(\frac{t}{\theta}\right)^n = \frac{t^n}{\theta^n}, \quad 0 \le t \le \theta$$

Differentiate to obtain the probability density function of $X_{(n)}$:
$$f_{X_{(n)}}(t) = \frac{d}{dt} F_{X_{(n)}}(t) = \frac{n t^{n-1}}{\theta^n}, \quad 0 \le t \le \theta$$

Now compute the expected value:
$$E[X_{(n)}] = \int_0^\theta t \cdot \left(\frac{n t^{n-1}}{\theta^n}\right) dt = \frac{n}{\theta^n} \int_0^\theta t^n dt = \frac{n}{\theta^n} \left[ \frac{t^{n+1}}{n+1} \right]_0^\theta = \frac{n}{\theta^n} \frac{\theta^{n+1}}{n+1} = \frac{n}{n+1}\theta$$

Compute the bias:
$$\text{bias}(\hat{\theta}_{\text{MLE}}) = E[\hat{\theta}_{\text{MLE}}] - \theta = \frac{n}{n+1}\theta - \theta = -\frac{\theta}{n+1}$$
Because $-\frac{\theta}{n+1} < 0$, $\hat{\theta}_{\text{MLE}}$ is **negatively biased** (the largest observed value never exceeds the true upper bound $\theta$, so it strictly underestimates $\theta$ on average).

---

### Step 5: Unbiased Estimator
Multiply by the scalar correction factor $\frac{n+1}{n}$:
$$\hat{\theta}_{\text{unbiased}} = \frac{n+1}{n} X_{(n)}$$
$$E[\hat{\theta}_{\text{unbiased}}] = \frac{n+1}{n} E[X_{(n)}] = \frac{n+1}{n}\left(\frac{n}{n+1}\theta\right) = \theta$$

---

## Result

- $\hat{\theta}_{\text{MLE}} = X_{(n)} = \max_{1 \le i \le n} X_i$
- $E[\hat{\theta}_{\text{MLE}}] = \frac{n}{n+1}\theta \implies \text{bias} = -\frac{\theta}{n+1}$ (asymptotically unbiased as $n \to \infty$)
- Unbiased estimator: $\frac{n+1}{n} \max(X_i)$

---

## General Method for Non-Regular Likelihoods

When parameters define the support boundary:
1. Write the joint likelihood explicitly using indicator functions $\mathbf{1}_{\{a \le X_i \le b\}}$.
2. Convert the conditions on all $X_i$ into conditions on order statistics (e.g., $X_{(1)} \ge a$ and $X_{(n)} \le b$).
3. Sketch or analyze the monotonicity of the function within the allowable region.
4. The maximum will lie on the **boundary** of the allowable parameter region.

---

## Related Concepts

- [[Maximum Likelihood Estimation]]
- [[Point Estimation]]
- [[Discrete and Continuous Parameter MLE Reference Examples]]

---

## Sources

- [[01 - Sources/Lectures/CSE301_Maximum_Likelihood_Estimators__MLE_.pdf]]
- [[01 - Sources/Lectures/MLE.pdf]]
