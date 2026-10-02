---
type: formula
course: cse301
status: active
order: 52
---

# Beta-Binomial Conjugate Updating Formula

> 📖 **Reading Order:** Step 52 of 92 | **Module 8:** Bayesian Inference  
> ◄ **Previous:** [[Credible Intervals]] | ► **Next:** [[Normal-Normal Conjugate Updating Formula]]

---

## Formula

Let $p \in (0, 1)$ be the success probability of a Bernoulli or Binomial process.

### Prior Distribution
Assume a conjugate **Beta prior** parameterized by hyperparameters $\alpha > 0$ and $\beta > 0$:
$$p \sim \text{Beta}(\alpha, \beta) \implies f(p) = \frac{\Gamma(\alpha + \beta)}{\Gamma(\alpha)\Gamma(\beta)} p^{\alpha - 1} (1 - p)^{\beta - 1}$$

### Observed Likelihood
From $n$ independent trials, we observe $s = \sum_{i=1}^n X_i$ successes and $n - s$ failures:
$$L_n(p) \propto p^s (1 - p)^{n - s}$$

### Posterior Distribution
The posterior distribution is also a Beta distribution with updated parameters:
$$p \mid \mathbf{X} \sim \text{Beta}(\alpha_{\text{post}}, \beta_{\text{post}})$$
$$\alpha_{\text{post}} = \alpha + s, \quad \beta_{\text{post}} = \beta + n - s$$

### Posterior Point Estimates
1. **Bayes Estimator (Posterior Mean):**
   $$\hat{p}_{\text{Bayes}} = E[p \mid \mathbf{X}] = \frac{\alpha + s}{\alpha + \beta + n}$$
2. **Weighted Average Form:**
   $$\hat{p}_{\text{Bayes}} = w \bar{X} + (1 - w) p_0$$
   where:
   $$w = \frac{n}{n + \alpha + \beta}, \quad 1 - w = \frac{\alpha + \beta}{n + \alpha + \beta}, \quad p_0 = \frac{\alpha}{\alpha + \beta} = E_{\text{prior}}[p], \quad \bar{X} = \frac{s}{n}$$
3. **MAP Estimator (Posterior Mode for $\alpha_{\text{post}}, \beta_{\text{post}} > 1$):**
   $$\hat{p}_{\text{MAP}} = \frac{\alpha + s - 1}{\alpha + \beta + n - 2}$$

---

## Variables

| Symbol | Meaning | Interpretation |
|---|---|---|
| $p$ | Unknown probability of success | Parameter of interest |
| $\alpha, \beta$ | Prior Beta hyperparameters | "Virtual / pseudo" prior counts ($\alpha - 1$ prior successes, $\beta - 1$ prior failures) |
| $n$ | Number of experimental sample trials | Observed sample size |
| $s$ | Number of observed successes | Sample evidence |
| $p_0$ | Prior expectation $\frac{\alpha}{\alpha + \beta}$ | Anchor point of prior belief |
| $w$ | Weight on sample data $\frac{n}{n + \alpha + \beta}$ | Relative strength of data vs. prior |

---

## Conditions

1. The data generating process must be conditionally independent $\text{Bernoulli}(p)$ or $\text{Binomial}(n, p)$ given $p$.
2. The hyperparameters must satisfy $\alpha > 0$ and $\beta > 0$.
3. When $\alpha = \beta = 1$, the prior is the standard continuous $\text{Uniform}(0, 1)$ distribution.

---

## Intuition: Pseudocounts and Shrinkage

The Beta hyperparameters $\alpha$ and $\beta$ act as **fictitious prior observations**:
- Setting $\alpha = 10, \beta = 10$ is mathematically equivalent to entering the laboratory with prior experience of having already observed $10$ successes and $10$ failures.
- When you collect $n$ real observations with $s$ successes, you simply add your real successes to your virtual successes ($\alpha + s$), and your real failures to your virtual failures ($\beta + n - s$).
- **Shrinkage toward the prior:**
  - When sample size $n$ is small ($n \ll \alpha + \beta$), $w \approx 0$, and the estimate shrinks heavily toward the prior belief $p_0$.
  - When sample size $n$ is very large ($n \gg \alpha + \beta$), $w \to 1$, and the posterior mean converges to the sample mean $\bar{X} = s/n$, washing out the prior.

---

## Derivation

By Bayes' theorem, the posterior density satisfies:
$$f(p \mid \mathbf{x}) \propto f(p) \cdot L_n(p)$$

Substitute the prior density and likelihood, dropping constants independent of $p$:
$$f(p \mid \mathbf{x}) \propto \left( p^{\alpha - 1} (1 - p)^{\beta - 1} \right) \cdot \left( p^s (1 - p)^{n - s} \right)$$

Combine exponents with matching bases:
$$f(p \mid \mathbf{x}) \propto p^{(\alpha - 1) + s} (1 - p)^{(\beta - 1) + (n - s)} = p^{(\alpha + s) - 1} (1 - p)^{(\beta + n - s) - 1}$$

We recognize the kernel of a $\text{Beta}(\alpha', \beta')$ density with parameters:
$$\alpha' = \alpha + s, \quad \beta' = \beta + n - s$$

Since a probability density function must integrate to 1 over $[0, 1]$, the normalizing constant is uniquely determined by the Beta function $B(\alpha', \beta')$, proving that:
$$p \mid \mathbf{X} \sim \text{Beta}(\alpha + s, \beta + n - s) \quad \blacksquare$$

### Derivation of the Weighted Average Representation
$$E[p \mid \mathbf{X}] = \frac{\alpha + s}{\alpha + \beta + n} = \frac{s}{\alpha + \beta + n} + \frac{\alpha}{\alpha + \beta + n}$$
Multiply and divide the first term by $n$, and the second term by $(\alpha + \beta)$:
$$= \left(\frac{n}{\alpha + \beta + n}\right)\left(\frac{s}{n}\right) + \left(\frac{\alpha + \beta}{\alpha + \beta + n}\right)\left(\frac{\alpha}{\alpha + \beta}\right)$$
$$= w \bar{X} + (1 - w) p_0 \quad \blacksquare$$

---

## Example: Laplace's Rule of Succession

Suppose we have zero prior knowledge about an event, modeled by a uniform prior $p \sim \text{Uniform}(0, 1) \iff \text{Beta}(1, 1)$, so $\alpha = 1, \beta = 1$.
We observe $n$ consecutive occurrences of the event with zero failures ($s = n$).

What is the posterior probability that the event will happen again on the next trial?
1. **Updated posterior:**
   $$p \mid \mathbf{X} \sim \text{Beta}(1 + n, 1 + 0) = \text{Beta}(n + 1, 1)$$
2. **Posterior Mean (Bayes Point Estimate):**
   $$\hat{p}_{\text{Bayes}} = \frac{n + 1}{(n + 1) + 1} = \frac{n + 1}{n + 2}$$

This is the historic **Laplace's Rule of Succession** (e.g., if the sun has risen $n$ days in a row, the probability it rises tomorrow is $\frac{n+1}{n+2}$, avoiding the absurd MLE claim of $100\%$ certainty when $n = 1$).

---

## Common Mistakes

- Setting $\alpha = 0, \beta = 0$ as a prior. The prior must have $\alpha > 0, \beta > 0$ to be proper. The Haldane prior $\text{Beta}(0, 0)$ is improper.
- Forgetting to subtract $s$ from $n$ when calculating the second parameter: the second parameter is $\beta + (n - s)$, not $\beta + n$.

---

## Related Concepts

- [[Bayesian Inference]]
- [[Maximum A Posteriori (MAP) Estimation]]
- [[Normal-Normal Conjugate Updating Formula]]

---

## Sources

- [[01 - Sources/Lectures/Bayesian_Inference.pdf]]
