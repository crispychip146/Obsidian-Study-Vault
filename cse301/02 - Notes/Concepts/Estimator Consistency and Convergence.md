---
type: concept
course: cse301
status: active
order: 37
---

# Estimator Consistency and Convergence

> 📖 **Reading Order:** Step 37 of 92 | **Module 6:** Statistical Inference  
> ◄ **Previous:** [[Bias-Variance Decomposition]] | ► **Next:** [[Confidence Intervals and Confidence Sets]]

---

## Building the idea

Consistency says an estimator learns from increasing data: for every fixed tolerance $\epsilon>0$, the chance that $\hat\theta_n$ misses $\theta$ by more than $\epsilon$ tends to zero. It is a statement about a sequence of estimation rules, not just one sample size.

Unbiasedness answers a different question. A rule that always uses $X_1$ may have the correct expectation but ignore all later data, so its distribution never tightens. A biased rule can still be consistent if its bias vanishes and its variability also shrinks.

One convenient sufficient route is $E[(\hat\theta_n-\theta)^2]\to0$. Apply Markov to the squared error to bound the probability of a fixed error. Through [[Bias-Variance Decomposition]], vanishing bias and variance imply this condition. The reverse implication from consistency to vanishing MSE requires additional control of rare large errors.

## Definition

An estimator is **consistent** if, as the sample size $n$ grows toward infinity, the estimator converges in probability to the true underlying parameter value $\theta$.

Formally, a point estimator $\hat{\theta}_n$ of parameter $\theta$ is **consistent** if:
$$\hat{\theta}_n \xrightarrow{P} \theta \quad \text{as } n \to \infty$$

which means that for every tolerance threshold $\epsilon > 0$:
$$\lim_{n \to \infty} P_\theta\left(\lvert \hat{\theta}_n - \theta \rvert > \epsilon\right) = 0$$
or equivalently,
$$\lim_{n \to \infty} P_\theta\left(\lvert \hat{\theta}_n - \theta \rvert \le \epsilon\right) = 1$$

---

## How It Works

### Modes of Convergence

To analyze consistency rigorously, we define two fundamental modes of stochastic convergence:

### 1. Convergence in Probability ($\xrightarrow{P}$)
A sequence of random variables $X_n$ converges in probability to $X$, denoted $X_n \xrightarrow{P} X$, if for every $\epsilon > 0$:
$$\lim_{n \to \infty} P(\lvert X_n - X \rvert > \epsilon) = 0$$
*Intuition:* As $n$ grows, it becomes increasingly improbable that $X_n$ deviates from $X$ by more than $\epsilon$.

### 2. Convergence in Quadratic Mean / Mean Square ($\xrightarrow{qm}$)
A sequence of random variables $X_n$ converges to $X$ in quadratic mean, denoted $X_n \xrightarrow{qm} X$, if:
$$\lim_{n \to \infty} E[(X_n - X)^2] = 0$$
*Intuition:* The average squared Euclidean distance between $X_n$ and $X$ shrinks to zero.

---
### Theorem
If $X_n \xrightarrow{qm} X$, then $X_n \xrightarrow{P} X$.

### Proof (via Markov's Inequality)
Assume $E[(X_n - X)^2] \to 0$ as $n \to \infty$. Fix any $\epsilon > 0$.
The event $\{\lvert X_n - X \rvert > \epsilon\}$ is identical to the event $\{\lvert X_n - X \rvert^2 > \epsilon^2\}$.

Because $(X_n - X)^2$ is a non-negative random variable, we apply [[Markov Inequality]] (or Chebyshev's inequality form):
$$P(\lvert X_n - X \rvert > \epsilon) = P\left((X_n - X)^2 > \epsilon^2\right) \le \frac{E[(X_n - X)^2]}{\epsilon^2}$$

Since $E[(X_n - X)^2] \to 0$ as $n \to \infty$ and $\epsilon^2 > 0$ is fixed:
$$\lim_{n \to \infty} P(\lvert X_n - X \rvert > \epsilon) \le \lim_{n \to \infty} \frac{E[(X_n - X)^2]}{\epsilon^2} = 0$$

Because probabilities are bounded below by zero, the squeeze theorem yields:
$$\lim_{n \to \infty} P(\lvert X_n - X \rvert > \epsilon) = 0 \implies X_n \xrightarrow{P} X \quad \blacksquare$$

---
### Consistency via Mean Squared Error (MSE)

Testing convergence in probability directly using probabilities can be mathematically challenging. The standard method to prove consistency is via the **MSE Consistency Criterion**:

### Theorem
If for an estimator $\hat{\theta}_n$:
1. $\lim_{n \to \infty} \text{bias}(\hat{\theta}_n) = 0$, and
2. $\lim_{n \to \infty} \text{se}(\hat{\theta}_n) = 0$ (or equivalently $\text{Var}(\hat{\theta}_n) \to 0$)

then $\hat{\theta}_n$ is a **consistent estimator** of $\theta$.

### Proof
Recall the [[Bias-Variance Decomposition]]:
$$\text{MSE}(\hat{\theta}_n) = E_\theta[(\hat{\theta}_n - \theta)^2] = \text{bias}^2(\hat{\theta}_n) + \text{Var}_\theta(\hat{\theta}_n)$$

If $\text{bias}(\hat{\theta}_n) \to 0$ and $\text{Var}_\theta(\hat{\theta}_n) \to 0$, then:
$$\lim_{n \to \infty} \text{MSE}(\hat{\theta}_n) = 0^2 + 0 = 0$$

By definition of quadratic mean convergence:
$$\hat{\theta}_n \xrightarrow{qm} \theta$$

Since convergence in quadratic mean implies convergence in probability:
$$\hat{\theta}_n \xrightarrow{P} \theta \quad \blacksquare$$

---
### Relationship Between Unbiasedness and Consistency

A common student misconception is that unbiasedness and consistency are equivalent, or that one implies the other. **Neither implication holds in general.**

```
          Unbiasedness  ⇏  Consistency
          Consistency   ⇏  Unbiasedness
```

### Case 1: Unbiased, yet Inconsistent
Consider estimating a population mean $\mu$ from independent samples $X_1, X_2, \dots, X_n \sim (\mu, \sigma^2)$.
Define the "lazy" estimator that always returns the first data point:
$$\hat{\mu}_n = X_1$$

1. **Unbiasedness:**
   $$E[\hat{\mu}_n] = E[X_1] = \mu \implies \text{bias} = 0 \text{ for every } n.$$
   It is strictly unbiased!
2. **Inconsistency:**
   $$\text{Var}(\hat{\mu}_n) = \text{Var}(X_1) = \sigma^2 > 0$$
   The variance remains constant at $\sigma^2$ regardless of whether $n = 10$ or $n = 10,000,000$. The distribution of $\hat{\mu}_n$ never concentrates around $\mu$. Therefore, $\hat{\mu}_n$ does **not** converge in probability to $\mu$; it is **inconsistent**.

### Case 2: Consistent, yet Biased
Consider the Maximum Likelihood Estimator of the population variance $\sigma^2$:
$$\hat{\sigma}^2_n = \frac{1}{n}\sum_{i=1}^n (X_i - \bar{X})^2$$

1. **Biased:**
   As proved in [[Problem — Sample Variance Bias and Bessel's Correction Derivation]],
   $$E[\hat{\sigma}^2_n] = \frac{n-1}{n}\sigma^2 = \sigma^2 - \frac{\sigma^2}{n} \ne \sigma^2$$
   $$\text{bias}(\hat{\sigma}^2_n) = -\frac{\sigma^2}{n} \ne 0 \implies \hat{\sigma}^2_n \text{ is biased for any finite } n.$$
2. **Consistent:**
   As $n \to \infty$:
   $$\text{bias}(\hat{\sigma}^2_n) = -\frac{\sigma^2}{n} \to 0$$
   $$\text{Var}(\hat{\sigma}^2_n) = O\left(\frac{1}{n}\right) \to 0$$
   Both bias and variance vanish asymptotically, so $\text{MSE} \to 0$, which proves that $\hat{\sigma}^2_n \xrightarrow{P} \sigma^2$. The estimator is **consistent** despite being biased!

---
### Important Properties

1. **Continuous Mapping Theorem:** If $\hat{\theta}_n \xrightarrow{P} \theta$ and $g(\cdot)$ is a continuous function at $\theta$, then $g(\hat{\theta}_n) \xrightarrow{P} g(\theta)$.
2. **Slutsky's Theorem:** If $X_n \xrightarrow{d} X$ and $Y_n \xrightarrow{P} c$ (a constant), then:
   - $X_n + Y_n \xrightarrow{d} X + c$
   - $X_n Y_n \xrightarrow{d} c X$
   - $X_n / Y_n \xrightarrow{d} X / c$ (provided $c \ne 0$)
3. **Weak Law of Large Numbers (WLLN):** For i.i.d. observations with finite mean $\mu$, the sample mean $\bar{X}_n$ is a consistent estimator of $\mu$:
   $$\bar{X}_n \xrightarrow{P} \mu$$

---

## Common Mistakes

1. **Confusing almost sure convergence with convergence in probability:**
   Consistency requires convergence in probability ($\xrightarrow{P}$). Strong consistency requires almost sure convergence ($\xrightarrow{\text{a.s.}}$), which is a strictly stronger condition.
2. **Assuming $\lim_{n \to \infty} E[\hat{\theta}_n] = \theta$ implies consistency:**
   An estimator can be asymptotically unbiased ($\text{bias} \to 0$) without being consistent if its variance does not go to zero (e.g., $\hat{\theta}_n = X_1 + \frac{1}{n}$).

---

## Exam Relevance

Exam questions often test:
1. Proving that an estimator is consistent using the $\text{MSE} \to 0$ theorem.
2. Identifying or constructing counterexamples of estimators that are unbiased yet inconsistent, or consistent yet biased.
3. Applying Markov's inequality to prove that quadratic mean convergence implies convergence in probability.

---

## What to carry forward

Convergence in probability, almost sure convergence, and quadratic-mean convergence are different strengths of control. [[Problem — Unbiased yet Inconsistent Estimator Analysis]] demonstrates why a correct average alone is not enough.

## Related notes

- [[Bias-Variance Decomposition]]
- [[Problem — Unbiased yet Inconsistent Estimator Analysis]]

## Sources

- [[01 - Sources/Lectures/Point Estimation and Confidence Intervals.pdf]]
