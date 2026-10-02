---
type: example
course: cse301
status: active
---

# Normal Distribution Parameter MLE Derivation Example

## Problem

Let $X_1, X_2, \dots, X_n \overset{\text{iid}}{\sim} N(\mu, \sigma^2)$ be a sample of size $n$ from a normal distribution with unknown mean $\mu \in (-\infty, \infty)$ and unknown variance $\sigma^2 > 0$.

1. Derive the joint Maximum Likelihood Estimators $\hat{\mu}_{\text{MLE}}$ and $\hat{\sigma}^2_{\text{MLE}}$.
2. Prove whether $\hat{\mu}_{\text{MLE}}$ is unbiased.
3. Prove whether $\hat{\sigma}^2_{\text{MLE}}$ is unbiased, and if biased, determine the exact bias and explain Bessel's correction.

---

## Given

- PDF of single observation:
  $$f(x; \mu, \sigma^2) = \frac{1}{\sqrt{2\pi\sigma^2}} \exp\left(-\frac{(x - \mu)^2}{2\sigma^2}\right)$$
- Sample mean: $\bar{X} = \frac{1}{n}\sum_{i=1}^n X_i$
- Population moments: $E[X_i] = \mu$, $\text{Var}(X_i) = \sigma^2$.

---

## Required

1. Closed-form expressions for $\hat{\mu}$ and $\hat{\sigma}^2$.
2. Expectation $E[\hat{\mu}]$ and bias.
3. Expectation $E[\hat{\sigma}^2]$, bias, and unbiased alternative $S^2$.

---

## Concepts Used

- [[Maximum Likelihood Estimation]]
- [[Likelihood and Score Equations]]
- [[Point Estimation]]
- [[Bias-Variance Decomposition]]

---

## Solution

### Step 1: Formulate the Likelihood and Log-Likelihood
The likelihood of the sample is the product of individual densities:
$$L_n(\mu, \sigma^2) = \prod_{i=1}^n \left( \frac{1}{\sqrt{2\pi\sigma^2}} \exp\left(-\frac{(X_i - \mu)^2}{2\sigma^2}\right) \right)$$
$$L_n(\mu, \sigma^2) = (2\pi\sigma^2)^{-n/2} \exp\left(-\frac{1}{2\sigma^2}\sum_{i=1}^n (X_i - \mu)^2\right)$$

Taking the natural logarithm:
$$\ell_n(\mu, \sigma^2) = -\frac{n}{2}\log(2\pi) - \frac{n}{2}\log(\sigma^2) - \frac{1}{2\sigma^2}\sum_{i=1}^n (X_i - \mu)^2$$

---

### Step 2: Derive the MLE of $\mu$
Differentiate $\ell_n(\mu, \sigma^2)$ with respect to $\mu$:
$$\frac{\partial \ell_n}{\partial \mu} = -\frac{1}{2\sigma^2}\sum_{i=1}^n 2(X_i - \mu)(-1) = \frac{1}{\sigma^2}\sum_{i=1}^n (X_i - \mu)$$

Set this partial derivative equal to zero:
$$\frac{1}{\sigma^2}\sum_{i=1}^n (X_i - \mu) = 0 \implies \sum_{i=1}^n X_i - n\mu = 0$$
$$\hat{\mu}_{\text{MLE}} = \frac{1}{n}\sum_{i=1}^n X_i = \bar{X}$$

---

### Step 3: Derive the MLE of $\sigma^2$
Treat $\sigma^2$ as a single variable $\theta = \sigma^2$. Differentiate $\ell_n$ with respect to $\sigma^2$:
$$\frac{\partial \ell_n}{\partial (\sigma^2)} = -\frac{n}{2\sigma^2} + \frac{1}{2(\sigma^2)^2}\sum_{i=1}^n (X_i - \mu)^2$$

Set equal to zero:
$$-\frac{n}{2\sigma^2} + \frac{1}{2(\sigma^2)^2}\sum_{i=1}^n (X_i - \mu)^2 = 0$$
Multiply through by $2(\sigma^2)^2$:
$$-n\sigma^2 + \sum_{i=1}^n (X_i - \mu)^2 = 0 \implies n\sigma^2 = \sum_{i=1}^n (X_i - \mu)^2$$

Plugging in the optimal value $\hat{\mu} = \bar{X}$:
$$\hat{\sigma}^2_{\text{MLE}} = \frac{1}{n}\sum_{i=1}^n (X_i - \bar{X})^2$$

---

### Step 4: Bias Analysis of $\hat{\mu}$
$$E[\hat{\mu}] = E\left[\frac{1}{n}\sum_{i=1}^n X_i\right] = \frac{1}{n}\sum_{i=1}^n E[X_i] = \frac{1}{n}(n\mu) = \mu$$
$$\text{bias}(\hat{\mu}) = E[\hat{\mu}] - \mu = 0$$
$\hat{\mu}_{\text{MLE}}$ is strictly **unbiased**.

---

### Step 5: Bias Analysis of $\hat{\sigma}^2$ and Bessel's Correction
As proved rigorously in [[Problem — Sample Variance Bias and Bessel's Correction Derivation]]:
$$E[\hat{\sigma}^2_{\text{MLE}}] = \frac{n-1}{n}\sigma^2 = \sigma^2 - \frac{\sigma^2}{n}$$

The bias of the MLE is:
$$\text{bias}(\hat{\sigma}^2_{\text{MLE}}) = E[\hat{\sigma}^2_{\text{MLE}}] - \sigma^2 = -\frac{\sigma^2}{n}$$
Since the bias is strictly negative ($-\sigma^2/n < 0$), the MLE systematically **underestimates** the true variance.

To construct an unbiased estimator, multiply by the factor $\frac{n}{n-1}$ (known as **Bessel's correction**):
$$S^2 = \frac{n}{n-1} \hat{\sigma}^2_{\text{MLE}} = \frac{1}{n-1}\sum_{i=1}^n (X_i - \bar{X})^2$$
$$E[S^2] = \frac{n}{n-1} E[\hat{\sigma}^2_{\text{MLE}}] = \frac{n}{n-1}\left(\frac{n-1}{n}\sigma^2\right) = \sigma^2 \quad (\text{unbiased})$$

---

## Result

- $\hat{\mu}_{\text{MLE}} = \bar{X} = \frac{1}{n}\sum_{i=1}^n X_i$ (Unbiased)
- $\hat{\sigma}^2_{\text{MLE}} = \frac{1}{n}\sum_{i=1}^n (X_i - \bar{X})^2$ (Biased with $\text{bias} = -\sigma^2/n$)
- Unbiased sample variance: $S^2 = \frac{1}{n-1}\sum_{i=1}^n (X_i - \bar{X})^2$

---

## Why This Works

The score equations find the coordinates $(\mu, \sigma^2)$ at which the Gaussian surface matches the empirical moments of the data. The variance MLE is biased because measuring distances from the sample mean $\bar{X}$ instead of the true population mean $\mu$ absorbs one degree of freedom, systematically reducing the sum of squared deviations.

---

## Common Mistakes

- Setting the denominator of the MLE of $\sigma^2$ to $n - 1$. The MLE is mathematically derived as having denominator $n$.
- Differentiating with respect to $\sigma$ instead of $\sigma^2$ and getting bogged down in messy square roots (by the invariance property of MLE, estimating $\sigma^2$ directly yields the exact same answer as estimating $\sigma$).

---

## Related Concepts

- [[Maximum Likelihood Estimation]]
- [[Likelihood and Score Equations]]
- [[Problem — Sample Variance Bias and Bessel's Correction Derivation]]

---

## Sources

- [[01 - Sources/Lectures/CSE301_Maximum_Likelihood_Estimators__MLE_.pdf]]
- [[01 - Sources/Lectures/MLE.pdf]]
