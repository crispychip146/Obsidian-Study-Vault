---
type: formula
course: cse301
status: active
order: 53
---

# Normal-Normal Conjugate Updating Formula

> 📖 **Reading Order:** Step 53 of 92 | **Module 8:** Bayesian Inference  
> ◄ **Previous:** [[Beta-Binomial Conjugate Updating Formula]] | ► **Next:** [[Bernoulli Bayesian Inference with Beta Prior Example]]

---

---

## The Question and Earlier Knowledge

What analytical relationship or closed-form expectation governs Normal-Normal Conjugate Updating Formula, and how can we compute it directly from constituent probability terms? In complex probability models, calculating probabilities or moments directly is often intractable without decomposing expectations across conditioning partitions or inequalities.

---

## Developing the Formula

By decomposing joint distributions into conditional components, expanding algebraic products, or applying geometric series sums, Normal-Normal Conjugate Updating Formula compresses complex probabilistic reasoning into a clean, reusable formula.

---

## Formula

Let $X_1, X_2, \dots, X_n \overset{\text{iid}}{\sim} N(\theta, \sigma^2)$ be a sample with unknown mean $\theta \in (-\infty, \infty)$ and known variance $\sigma^2 > 0$. Let $\bar{X} = \frac{1}{n}\sum_{i=1}^n X_i$.

### Prior Distribution
Assume a conjugate Gaussian prior on the mean parameter $\theta$:
$$\theta \sim N(a, b^2)$$
where $a$ is the prior mean and $b^2$ is the prior variance.

### Posterior Distribution
The posterior distribution of $\theta$ given the data is also Gaussian:
$$\theta \mid \mathbf{X} \sim N(\bar{\theta}, \tau^2)$$

where the posterior variance $\tau^2$ and posterior mean $\bar{\theta}$ are given by:

### 1. Posterior Precision (Precisions Add)
$$\frac{1}{\tau^2} = \frac{n}{\sigma^2} + \frac{1}{b^2}$$
$$\tau^2 = \left(\frac{n}{\sigma^2} + \frac{1}{b^2}\right)^{-1} = \frac{\sigma^2 b^2}{n b^2 + \sigma^2}$$

### 2. Posterior Mean (Precision-Weighted Average)
$$\bar{\theta} = \frac{\frac{n}{\sigma^2}\bar{X} + \frac{1}{b^2}a}{\frac{n}{\sigma^2} + \frac{1}{b^2}} = \left(\frac{n b^2}{n b^2 + \sigma^2}\right)\bar{X} + \left(\frac{\sigma^2}{n b^2 + \sigma^2}\right)a$$

### 3. Bayesian $1 - \alpha$ Credible Interval
$$C = \left(\bar{\theta} - z_{\alpha/2} \tau, \quad \bar{\theta} + z_{\alpha/2} \tau\right)$$
where $z_{\alpha/2} = \Phi^{-1}(1 - \alpha/2)$.

---

---

## Variables

| Symbol | Meaning | Role |
|---|---|---|
| $\theta$ | Unknown population mean | Parameter to infer |
| $\sigma^2$ | Known population variance | Measurement noise |
| $\bar{X}$ | Observed sample mean | Data summary statistic |
| $n$ | Sample size | Amount of empirical data |
| $a$ | Prior mean | Center of prior belief |
| $b^2$ | Prior variance | Uncertainty in prior belief |
| $1/b^2$ | Prior precision | Information content of prior |
| $n/\sigma^2$ | Data precision | Information content of sample |
| $1/\tau^2$ | Posterior precision | Total information content |
| $\bar{\theta}$ | Posterior mean | Updated point estimate |

---

---

## Conditions

- Random variables must possess finite first and second moments (well-defined expectations).
- Probability distributions must satisfy standard non-negativity and total probability integration axioms.

---

## Intuition

### Intuition: The Physics of Information (Precisions Add)

In statistics, **precision** is defined as the reciprocal of variance: $\text{Precision} = \frac{1}{\text{Variance}}$.
Precision measures the certainty or information density of an estimate.

The Normal-Normal conjugate update reveals an elegant law of nature:
$$\mathbf{\text{Posterior Precision} = \text{Data Precision} + \text{Prior Precision}}$$

The posterior mean is simply the **precision-weighted average** of the sample data and the prior:
$$\bar{\theta} = \frac{\text{Data Precision} \times \bar{X} + \text{Prior Precision} \times a}{\text{Data Precision} + \text{Prior Precision}}$$

- **Weak Prior ($b^2 \to \infty$, prior precision $\to 0$):**
  The prior carries zero weight. $\bar{\theta} \to \bar{X}$ and $\tau^2 \to \frac{\sigma^2}{n}$. The Bayesian credible interval becomes identical to the classical frequentist $Z$-confidence interval.
- **Strong Prior ($b^2 \to 0$, prior precision $\to \infty$):**
  The prior is impregnable. $\bar{\theta} \to a$ and data are ignored.
- **Large Sample ($n \to \infty$):**
  The data precision $n/\sigma^2$ dwarfs the prior precision $1/b^2$, washing out any reasonable prior.

---

---

## Derivation

### Derivation

By Bayes' theorem:
$$f(\theta \mid \mathbf{x}) \propto f(\mathbf{x} \mid \theta) f(\theta)$$

1. **Prior:**
   $$f(\theta) \propto \exp\left(-\frac{1}{2b^2}(\theta - a)^2\right)$$
2. **Likelihood:**
   Using the identity $\sum_{i=1}^n (X_i - \theta)^2 = \sum (X_i - \bar{X})^2 + n(\bar{X} - \theta)^2$:
   $$L_n(\theta) \propto \exp\left(-\frac{n}{2\sigma^2}(\theta - \bar{X})^2\right)$$
3. **Posterior Product:**
   $$f(\theta \mid \mathbf{x}) \propto \exp\left( -\frac{1}{2} \left[ \frac{n}{\sigma^2}(\theta - \bar{X})^2 + \frac{1}{b^2}(\theta - a)^2 \right] \right)$$

Expand the quadratic terms in $\theta$:
$$\frac{n}{\sigma^2}(\theta^2 - 2\theta\bar{X} + \bar{X}^2) + \frac{1}{b^2}(\theta^2 - 2\theta a + a^2) = \theta^2 \left(\frac{n}{\sigma^2} + \frac{1}{b^2}\right) - 2\theta \left(\frac{n}{\sigma^2}\bar{X} + \frac{1}{b^2}a\right) + \text{const}$$

Define:
$$\frac{1}{\tau^2} = \frac{n}{\sigma^2} + \frac{1}{b^2}, \quad \bar{\theta} = \tau^2 \left(\frac{n}{\sigma^2}\bar{X} + \frac{1}{b^2}a\right)$$

Completing the square in $\theta$:
$$= \frac{1}{\tau^2}(\theta - \bar{\theta})^2 + \text{const}$$

Exponentiating back:
$$f(\theta \mid \mathbf{x}) \propto \exp\left(-\frac{1}{2\tau^2}(\theta - \bar{\theta})^2\right)$$

This is recognized immediately as a Gaussian density $N(\bar{\theta}, \tau^2) \quad \blacksquare$.

---

---

## Example

### Example

Suppose an instrument measures a physical constant $\theta$. Instrument precision is known with $\sigma = 2$.
- Prior belief: $\theta \sim N(100, 3^2) \implies a = 100, b = 3, b^2 = 9$.
- Sample data: We take $n = 16$ measurements and find sample mean $\bar{X} = 104$.

1. **Calculate Precisions:**
   - Prior precision: $\frac{1}{b^2} = \frac{1}{9} \approx 0.1111$
   - Data precision: $\frac{n}{\sigma^2} = \frac{16}{2^2} = \frac{16}{4} = 4.0000$
   - Posterior precision: $\frac{1}{\tau^2} = 4.0000 + 0.1111 = 4.1111$
2. **Posterior Variance & SD:**
   $$\tau^2 = \frac{1}{4.1111} \approx 0.2432 \implies \tau = \sqrt{0.2432} \approx 0.4932$$
3. **Posterior Mean:**
   $$\bar{\theta} = \frac{4.0000 \times 104 + 0.1111 \times 100}{4.1111} = \frac{416 + 11.11}{4.1111} = \frac{427.11}{4.1111} \approx 103.89$$
4. **95% Credible Interval:**
   $$C = 103.89 \pm 1.96 \times 0.4932 = 103.89 \pm 0.967 \implies [102.92, 104.86]$$

Notice how the data pulled the estimate from $100$ up to $103.89$, but the prior prevented it from going all the way to $104$.

---

---

## Common Mistakes

- Confusing conditional variance with the variance of conditional expectation (Eve's Law components).
- Forgetting that linearity of expectation holds unconditionally, whereas $\mathbb{E}[XY] = \mathbb{E}[X]\mathbb{E}[Y]$ requires independence.

---

## Related Concepts

- [[Bayesian Inference]]
- [[Credible Intervals]]
- [[Beta-Binomial Conjugate Updating Formula]]

---

---

## Prerequisites

- [[Bayesian Inference]]
- [[Random Variables and Probability Distributions]]
- [[Continuous Probability Distributions]]

---

## Problems

- [[Problem — Laplace Rule of Succession and Bayesian Updating]]

---

## Sources

- [[01 - Sources/Lectures/Bayesian_Inference.pdf]]
