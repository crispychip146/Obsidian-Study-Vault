---
type: example
course: cse301
status: active
order: 47
---

# Discrete and Continuous Parameter MLE Reference Examples

> 📖 **Reading Order:** Step 47 of 92 | **Module 7:** Parametric Inference  
> ◄ **Previous:** [[Uniform Distribution Non-Regular MLE Example]] | ► **Next:** [[Problem — Sample Variance Bias and Bessel's Correction Derivation]]

---

---

## Problem

Solve the probability and statistical problem: Discrete and Continuous Parameter MLE Reference Examples.

---

## Given

- Prior parameters, sample observations, state transition matrix, or probability distributions as specified.

---

## Required

- Calculate posterior distributions, point estimates, confidence intervals, or stationary distributions.

---

## Understanding the Problem and Choosing the Method

Identify the random variables, state the conditional distributions, select the appropriate probabilistic law or updating formula, and execute the algebraic substitutions step by step.

---

## Solution

### Overview

This note provides complete, step-by-step Maximum Likelihood Estimator derivations for the fundamental parametric families tested in CSE 301, based on the official course reference sheet.

Throughout this note, let $X_1, X_2, \dots, X_n$ be an i.i.d. sample, with sample mean $\bar{X} = \frac{1}{n}\sum_{i=1}^n X_i$, minimum $X_{(1)} = \min_i X_i$, and maximum $X_{(n)} = \max_i X_i$.

---
### Bernoulli($p$) Distribution

### Setup
- Support: $X_i \in \{0, 1\}$, parameter $p \in (0, 1)$.
- PMF: $P(X = x) = p^x (1 - p)^{1-x}$.

### Derivation
1. **Likelihood:**
   $$L(p) = \prod_{i=1}^n p^{X_i} (1 - p)^{1 - X_i} = p^{\sum X_i} (1 - p)^{n - \sum X_i}$$
2. **Log-Likelihood:**
   $$\ell(p) = \left(\sum_{i=1}^n X_i\right) \log p + \left(n - \sum_{i=1}^n X_i\right) \log(1 - p)$$
3. **Score Equation:**
   $$\frac{d\ell}{dp} = \frac{\sum X_i}{p} - \frac{n - \sum X_i}{1 - p} = 0$$
   $$(1 - p)\sum X_i = p\left(n - \sum X_i\right) \implies \sum X_i = n p$$
4. **MLE:**
   $$\hat{p}_{\text{MLE}} = \frac{1}{n}\sum_{i=1}^n X_i = \bar{X}$$

---
### Binomial($m, p$) Distribution ($m$ Known)

### Setup
- Support: $X_i \in \{0, 1, \dots, m\}$, parameter $p \in (0, 1)$, $m$ known.
- PMF: $P(X = x) = \binom{m}{x} p^x (1 - p)^{m - x}$.

### Derivation
1. **Likelihood (dropping constants independent of $p$):**
   $$L(p) \propto \prod_{i=1}^n p^{X_i} (1 - p)^{m - X_i} = p^{\sum X_i} (1 - p)^{n m - \sum X_i}$$
2. **Log-Likelihood:**
   $$\ell(p) = \left(\sum X_i\right) \log p + \left(n m - \sum X_i\right) \log(1 - p) + C$$
3. **Score Equation:**
   $$\frac{d\ell}{dp} = \frac{\sum X_i}{p} - \frac{n m - \sum X_i}{1 - p} = 0 \implies \sum X_i = n m p$$
4. **MLE:**
   $$\hat{p}_{\text{MLE}} = \frac{\sum X_i}{n m} = \frac{\bar{X}}{m}$$

---
### Geometric($p$) Distribution

### Setup (Convention: Trials until First Success)
- Support: $X_i \in \{1, 2, 3, \dots\}$, parameter $p \in (0, 1)$.
- PMF: $P(X = x) = p (1 - p)^{x - 1}$.

### Derivation
1. **Likelihood:**
   $$L(p) = \prod_{i=1}^n p (1 - p)^{X_i - 1} = p^n (1 - p)^{\sum X_i - n}$$
2. **Log-Likelihood:**
   $$\ell(p) = n \log p + \left(\sum_{i=1}^n X_i - n\right) \log(1 - p)$$
3. **Score Equation:**
   $$\frac{d\ell}{dp} = \frac{n}{p} - \frac{\sum X_i - n}{1 - p} = 0$$
   $$n(1 - p) = p\left(\sum X_i - n\right) \implies n - n p = p \sum X_i - n p \implies n = p \sum X_i$$
4. **MLE:**
   $$\hat{p}_{\text{MLE}} = \frac{n}{\sum X_i} = \frac{1}{\bar{X}}$$

> **Alternative Definition Note:** If $X$ is defined as the number of *failures before the first success* ($X \in \{0, 1, 2, \dots\}$), the PMF is $P(X = x) = p(1-p)^x$, giving the MLE:
> $$\hat{p}_{\text{MLE}} = \frac{1}{1 + \bar{X}}$$

---
### Poisson($\lambda$) Distribution

### Setup
- Support: $X_i \in \{0, 1, 2, \dots\}$, parameter $\lambda > 0$.
- PMF: $P(X = x) = \frac{e^{-\lambda}\lambda^x}{x!}$.

### Derivation
1. **Likelihood:**
   $$L(\lambda) = \prod_{i=1}^n \frac{e^{-\lambda} \lambda^{X_i}}{X_i!} = e^{-n\lambda} \lambda^{\sum X_i} \left(\prod_{i=1}^n \frac{1}{X_i!}\right)$$
2. **Log-Likelihood:**
   $$\ell(\lambda) = -n\lambda + \left(\sum_{i=1}^n X_i\right) \log \lambda - \sum_{i=1}^n \log(X_i!)$$
3. **Score Equation:**
   $$\frac{d\ell}{d\lambda} = -n + \frac{\sum X_i}{\lambda} = 0 \implies n\lambda = \sum_{i=1}^n X_i$$
4. **MLE:**
   $$\hat{\lambda}_{\text{MLE}} = \frac{1}{n}\sum_{i=1}^n X_i = \bar{X}$$

---
### Exponential($\lambda$) Distribution (Rate Parameterization)

### Setup
- Support: $X_i \ge 0$, rate parameter $\lambda > 0$.
- PDF: $f(x; \lambda) = \lambda e^{-\lambda x}$.

### Derivation
1. **Likelihood:**
   $$L(\lambda) = \prod_{i=1}^n \lambda e^{-\lambda X_i} = \lambda^n e^{-\lambda \sum X_i}$$
2. **Log-Likelihood:**
   $$\ell(\lambda) = n \log \lambda - \lambda \sum_{i=1}^n X_i$$
3. **Score Equation:**
   $$\frac{d\ell}{d\lambda} = \frac{n}{\lambda} - \sum_{i=1}^n X_i = 0 \implies \lambda \sum X_i = n$$
4. **MLE:**
   $$\hat{\lambda}_{\text{MLE}} = \frac{n}{\sum X_i} = \frac{1}{\bar{X}}$$

> **Scale Parameterization Note:** If the exponential is parameterized by mean $\theta = 1/\lambda$ ($f(x; \theta) = \frac{1}{\theta} e^{-x/\theta}$), by the equivariance property:
> $$\hat{\theta}_{\text{MLE}} = \frac{1}{\hat{\lambda}_{\text{MLE}}} = \bar{X}$$

---
### Uniform($a, b$) Distribution

### Setup
- Support: $a \le X_i \le b$, parameters $a < b$.
- PDF: $f(x; a, b) = \frac{1}{b - a} \mathbf{1}_{\{a \le x \le b\}}$.

### Derivation
1. **Likelihood:**
   $$L(a, b) = \prod_{i=1}^n \frac{1}{b - a} \mathbf{1}_{\{a \le X_i \le b\}} = \frac{1}{(b - a)^n} \mathbf{1}_{\{a \le X_{(1)} \le X_{(n)} \le b\}}$$
2. **Analysis:**
   To make $L(a, b)$ non-zero, we must satisfy $a \le X_{(1)}$ and $b \ge X_{(n)}$.
   To maximize $\frac{1}{(b - a)^n}$, we must minimize the denominator length $b - a$.
   The smallest valid interval containing all data points is $[X_{(1)}, X_{(n)}]$.
3. **Joint MLE:**
   $$\hat{a}_{\text{MLE}} = X_{(1)} = \min_{1 \le i \le n} X_i, \quad \hat{b}_{\text{MLE}} = X_{(n)} = \max_{1 \le i \le n} X_i$$

---
### Summary Reference Table

| Distribution | Parameter(s) | MLE Formula | Support Property |
|---|---|---|---|
| $\text{Bernoulli}(p)$ | $p$ | $\hat{p} = \bar{X}$ | Regular |
| $\text{Binomial}(m, p)$ | $p$ ($m$ known) | $\hat{p} = \frac{\bar{X}}{m}$ | Regular |
| $\text{Geometric}(p)$ | $p$ (trial of 1st success) | $\hat{p} = \frac{1}{\bar{X}}$ | Regular |
| $\text{Poisson}(\lambda)$ | $\lambda$ | $\hat{\lambda} = \bar{X}$ | Regular |
| $\text{Normal}(\mu, \sigma^2)$ | $\mu, \sigma^2$ | $\hat{\mu} = \bar{X}, \quad \hat{\sigma}^2 = \frac{1}{n}\sum(X_i - \bar{X})^2$ | Regular |
| $\text{Exponential}(\lambda)$ | $\lambda$ (rate) | $\hat{\lambda} = \frac{1}{\bar{X}}$ | Regular |
| $\text{Uniform}(0, \theta)$ | $\theta$ | $\hat{\theta} = X_{(n)} = \max_i X_i$ | Non-regular |
| $\text{Uniform}(a, b)$ | $a, b$ | $\hat{a} = X_{(1)}, \quad \hat{b} = X_{(n)}$ | Non-regular |

---

---

## Result

The mathematical derivation confirms the target probability or estimator value.

---

## Why This Works

The solution holds because every step follows directly from Bayes' rule, the law of total probability, or properties of expectation and variance.

---

## Common Mistakes

- Forgetting normalization constants when evaluating continuous posterior densities.
- Misidentifying degrees of freedom in chi-square tests.

---

## General Method

Extract the reusable problem-solving pattern: define random variables, write down the joint distribution, condition on observed data, and normalize the resulting distribution.

---

## Related Concepts

- [[Maximum Likelihood Estimation]]
- [[Likelihood and Score Equations]]
- [[Normal Distribution Parameter MLE Derivation Example]]
- [[Uniform Distribution Non-Regular MLE Example]]

---

---

## Sources

- [[01 - Sources/Lectures/MLE.pdf]]
- [[01 - Sources/Lectures/CSE301_Maximum_Likelihood_Estimators__MLE_.pdf]]
