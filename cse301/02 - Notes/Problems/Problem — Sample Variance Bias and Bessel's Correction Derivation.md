---
type: problem
course: cse301
status: active
---

# Problem — Sample Variance Bias and Bessel's Correction Derivation

## Problem

Let $X_1, X_2, \dots, X_n$ be an independent and identically distributed (i.i.d.) random sample from any probability distribution with finite population mean $\mu = E[X_i]$ and finite population variance $\sigma^2 = \text{Var}(X_i) > 0$.

Define the uncorrected sample variance (the Maximum Likelihood Estimator for a Gaussian model):
$$\hat{\sigma}^2_n = S^2_{\text{MLE}} = \frac{1}{n}\sum_{i=1}^n (X_i - \bar{X})^2$$
where $\bar{X} = \frac{1}{n}\sum_{i=1}^n X_i$.

1. Prove rigorously that:
   $$E[\hat{\sigma}^2_n] = \frac{n - 1}{n}\sigma^2$$
2. Determine the exact bias of $\hat{\sigma}^2_n$.
3. Show how Bessel's correction produces an unbiased estimator $S^2$.
4. Explain the intuitive geometric reason why the uncorrected estimator is biased downward.

---

## Given

- $X_1, \dots, X_n \overset{\text{iid}}{\sim} (\mu, \sigma^2)$
- $E[X_i] = \mu$, $\text{Var}(X_i) = \sigma^2$
- $E[\bar{X}] = \mu$, $\text{Var}(\bar{X}) = \frac{\sigma^2}{n}$
- $\hat{\sigma}^2_n = \frac{1}{n}\sum_{i=1}^n (X_i - \bar{X})^2$

---

## Required

1. Complete algebraic proof of $E[\hat{\sigma}^2_n] = \frac{n-1}{n}\sigma^2$.
2. Analytical expression for $\text{bias}(\hat{\sigma}^2_n)$.
3. Proof that $E[S^2] = \sigma^2$ for $S^2 = \frac{1}{n-1}\sum (X_i - \bar{X})^2$.
4. Conceptual degrees-of-freedom explanation.

---

## Concepts Tested

- [[Point Estimation]]
- [[Maximum Likelihood Estimation]]
- [[Bias-Variance Decomposition]]
- Properties of Expectation and Variance

---

## Prerequisites

- Linearity of expectation
- Variance identity: $\text{Var}(Y) = E[Y^2] - (E[Y])^2 \implies E[(Y - E[Y])^2] = \text{Var}(Y)$

---

## Question Type

- Mathematical Proof & Analytical Derivation

---

## Solution

### Step 1: Algebraic Expansion
We begin with the definition of the expectation:
$$E[\hat{\sigma}^2_n] = E\left[ \frac{1}{n}\sum_{i=1}^n (X_i - \bar{X})^2 \right] = \frac{1}{n}\sum_{i=1}^n E\left[(X_i - \bar{X})^2\right]$$

Add and subtract the true population mean $\mu$ inside the squared term:
$$(X_i - \bar{X}) = (X_i - \mu) - (\bar{X} - \mu)$$

Expand the square:
$$(X_i - \bar{X})^2 = \big((X_i - \mu) - (\bar{X} - \mu)\big)^2 = (X_i - \mu)^2 - 2(X_i - \mu)(\bar{X} - \mu) + (\bar{X} - \mu)^2$$

Sum this expression over all $i = 1, \dots, n$:
$$\sum_{i=1}^n (X_i - \bar{X})^2 = \sum_{i=1}^n (X_i - \mu)^2 - 2(\bar{X} - \mu)\sum_{i=1}^n (X_i - \mu) + \sum_{i=1}^n (\bar{X} - \mu)^2$$

Notice that by definition of the sample mean:
$$\sum_{i=1}^n (X_i - \mu) = \sum_{i=1}^n X_i - n\mu = n\bar{X} - n\mu = n(\bar{X} - \mu)$$

Substitute this into the middle cross-product term:
$$-2(\bar{X} - \mu)\cdot n(\bar{X} - \mu) = -2n(\bar{X} - \mu)^2$$

Now combine the middle and right-hand sums:
$$\sum_{i=1}^n (X_i - \bar{X})^2 = \sum_{i=1}^n (X_i - \mu)^2 - 2n(\bar{X} - \mu)^2 + n(\bar{X} - \mu)^2$$
$$\sum_{i=1}^n (X_i - \bar{X})^2 = \sum_{i=1}^n (X_i - \mu)^2 - n(\bar{X} - \mu)^2$$

Divide through by $n$:
$$\hat{\sigma}^2_n = \frac{1}{n}\sum_{i=1}^n (X_i - \mu)^2 - (\bar{X} - \mu)^2$$

---

### Step 2: Evaluating the Expectations
Now take expectations of both terms on the right-hand side:
$$E[\hat{\sigma}^2_n] = \frac{1}{n}\sum_{i=1}^n E\left[(X_i - \mu)^2\right] - E\left[(\bar{X} - \mu)^2\right]$$

1. **First expectation:**
   Since $E[X_i] = \mu$, by definition of variance:
   $$E\left[(X_i - \mu)^2\right] = \text{Var}(X_i) = \sigma^2$$
   Summing over $n$ terms and dividing by $n$:
   $$\frac{1}{n}\sum_{i=1}^n \sigma^2 = \frac{1}{n}(n\sigma^2) = \sigma^2$$

2. **Second expectation:**
   Since $E[\bar{X}] = \mu$, the quantity $E[(\bar{X} - \mu)^2]$ is simply the variance of the sample mean $\bar{X}$:
   $$E\left[(\bar{X} - \mu)^2\right] = \text{Var}(\bar{X})$$
   Because $X_1, \dots, X_n$ are independent:
   $$\text{Var}(\bar{X}) = \text{Var}\left(\frac{1}{n}\sum_{i=1}^n X_i\right) = \frac{1}{n^2}\sum_{i=1}^n \text{Var}(X_i) = \frac{1}{n^2}(n\sigma^2) = \frac{\sigma^2}{n}$$

---

### Step 3: Combining the Results
Substitute the two expectations back:
$$E[\hat{\sigma}^2_n] = \sigma^2 - \frac{\sigma^2}{n} = \sigma^2\left(1 - \frac{1}{n}\right) = \frac{n - 1}{n}\sigma^2 \quad \blacksquare$$

---

### Step 4: Bias Calculation
$$\text{bias}(\hat{\sigma}^2_n) = E[\hat{\sigma}^2_n] - \sigma^2 = \frac{n - 1}{n}\sigma^2 - \sigma^2 = -\frac{\sigma^2}{n}$$
Since $\sigma^2 > 0$ and $n \ge 2$, the bias is strictly negative. $\hat{\sigma}^2_n$ systematically underestimates the true variance.

---

### Step 5: Bessel's Correction
To correct the downward bias, multiply $\hat{\sigma}^2_n$ by $\frac{n}{n-1}$:
$$S^2 = \frac{n}{n - 1}\hat{\sigma}^2_n = \frac{n}{n - 1}\left(\frac{1}{n}\sum_{i=1}^n (X_i - \bar{X})^2\right) = \frac{1}{n - 1}\sum_{i=1}^n (X_i - \bar{X})^2$$

Check unbiasedness:
$$E[S^2] = \frac{n}{n - 1}E[\hat{\sigma}^2_n] = \frac{n}{n - 1}\left(\frac{n - 1}{n}\sigma^2\right) = \sigma^2 \quad \blacksquare$$

---

## Geometric & Intuitive Interpretation

Why does measuring deviations from $\bar{X}$ underestimate variance?
The function $g(c) = \sum_{i=1}^n (X_i - c)^2$ is minimized over all possible choices of $c$ when $c = \bar{X}$.
Therefore, for any fixed dataset:
$$\sum_{i=1}^n (X_i - \bar{X})^2 \le \sum_{i=1}^n (X_i - \mu)^2$$
Because $\bar{X}$ is calculated from the sample itself, the data points cluster closer to $\bar{X}$ than they do to the true population mean $\mu$. Using $\bar{X}$ "uses up" one degree of freedom, reducing the effective sample size from $n$ to $n - 1$.

---

## Common Mistakes

- Forgetting that $\sum (X_i - \mu) = n(\bar{X} - \mu)$, which simplifies the cross-product term.
- Believing this proof requires a Normal distribution assumption. This proof is **completely non-parametric**; it holds for any distribution with finite variance $\sigma^2$.

---

## Related Concepts

- [[Point Estimation]]
- [[Maximum Likelihood Estimation]]
- [[Normal Distribution Parameter MLE Derivation Example]]

---

## Source

- [[01 - Sources/Lectures/CSE301_Maximum_Likelihood_Estimators__MLE_.pdf]]
