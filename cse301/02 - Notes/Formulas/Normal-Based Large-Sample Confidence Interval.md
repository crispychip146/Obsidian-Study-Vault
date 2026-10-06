---
type: formula
course: cse301
status: active
order: 50
---

# Normal-Based Large-Sample Confidence Interval

> 📖 **Reading Order:** Step 50 of 103 | **Module 6:** Statistical Inference  
> ◄ **Previous:** [[Confidence Intervals and Confidence Sets]] | ► **Next:** [[Bernoulli Parameter Estimation and Confidence Interval Example]]
---
## The Question and Earlier Knowledge

What analytical relationship or closed-form expectation governs Normal-Based Large-Sample Confidence Interval, and how can we compute it directly from constituent probability terms? In complex probability models, calculating probabilities or moments directly is often intractable without decomposing expectations across conditioning partitions or inequalities.

---

## Developing the Formula

By decomposing joint distributions into conditional components, expanding algebraic products, or applying geometric series sums, Normal-Based Large-Sample Confidence Interval compresses complex probabilistic reasoning into a clean, reusable formula.

---

## Formula

When an estimator $\hat{\theta}_n$ is asymptotically normal, an approximate **$1 - \alpha$ confidence interval** for parameter $\theta$ is:

$$C_n = \left(\hat{\theta}_n - z_{\alpha/2}\widehat{\text{se}}, \quad \hat{\theta}_n + z_{\alpha/2}\widehat{\text{se}}\right)$$

where:
- $\hat{\theta}_n$ is the point estimate.
- $\widehat{\text{se}} = \widehat{\text{se}}(\hat{\theta}_n)$ is the estimated standard error.
- $z_{\alpha/2} = \Phi^{-1}(1 - \alpha/2)$ is the upper $\alpha/2$ quantile of the standard normal distribution $N(0, 1)$.
- Margin of error is $\text{ME} = z_{\alpha/2}\widehat{\text{se}}$.
---
## Variables

| Symbol | Meaning | Standard Values |
|---|---|---|
| $\hat{\theta}_n$ | Point estimator computed from $n$ sample points | Real number |
| $\theta$ | True unknown parameter value | Fixed constant |
| $\widehat{\text{se}}$ | Estimated standard error $\sqrt{\widehat{\text{Var}}(\hat{\theta}_n)}$ | Positive real number |
| $\alpha$ | Significance level (error rate) | $0.05$ (for 95% CI), $0.01$ (for 99% CI) |
| $1 - \alpha$ | Confidence level (coverage probability) | $0.95$ (95%), $0.99$ (99%) |
| $z_{\alpha/2}$ | Normal critical value | $1.645$ (90%), $1.960$ (95%), $2.576$ (99%) |
---
## Conditions

1. **Asymptotic Normality:** The standardized estimator converges in distribution to a standard normal variable:
   $$\frac{\hat{\theta}_n - \theta}{\widehat{\text{se}}} \xrightarrow{d} N(0, 1) \quad \text{as } n \to \infty$$
   This condition is satisfied by:
   - Sample means of i.i.d. observations with finite variance (via Central Limit Theorem).
   - Maximum Likelihood Estimators under standard regularity conditions.
2. **Consistent Standard Error:** $\frac{\widehat{\text{se}}}{\text{se}} \xrightarrow{P} 1$ (by Slutsky's theorem).
3. **Adequate Sample Size:** $n$ must be sufficiently large that the normal approximation is accurate.
---
## Intuition

### Intuition

The standard normal probability density curve $\phi(z)$ is symmetric around zero. The area under the curve between $-z_{\alpha/2}$ and $+z_{\alpha/2}$ equals exactly $1 - \alpha$, leaving area $\alpha/2$ in each of the two outer tails.

Because $\hat{\theta}_n$ behaves approximately like a normal bell curve centered at $\theta$ with standard deviation $\widehat{\text{se}}$, stepping out $z_{\alpha/2}$ standard errors in both directions from $\hat{\theta}_n$ creates a trap that catches the fixed point $\theta$ with probability approaching $1 - \alpha$.
---
## Derivation

### Derivation

Let $Z_n = \frac{\hat{\theta}_n - \theta}{\widehat{\text{se}}}$. By the asymptotic normality assumption, $Z_n \xrightarrow{d} Z \sim N(0, 1)$.

Start from the coverage probability statement:
$$P_\theta(\theta \in C_n) = P_\theta\left(\hat{\theta}_n - z_{\alpha/2}\widehat{\text{se}} < \theta < \hat{\theta}_n + z_{\alpha/2}\widehat{\text{se}}\right)$$

Subtract $\hat{\theta}_n$ across the entire inequality:
$$= P_\theta\left(-z_{\alpha/2}\widehat{\text{se}} < \theta - \hat{\theta}_n < z_{\alpha/2}\widehat{\text{se}}\right)$$

Multiply through by $-1$ (which reverses the inequalities):
$$= P_\theta\left(-z_{\alpha/2}\widehat{\text{se}} < \hat{\theta}_n - \theta < z_{\alpha/2}\widehat{\text{se}}\right)$$

Divide through by the positive estimated standard error $\widehat{\text{se}}$:
$$= P_\theta\left(-z_{\alpha/2} < \frac{\hat{\theta}_n - \theta}{\widehat{\text{se}}} < z_{\alpha/2}\right) = P_\theta\left(-z_{\alpha/2} < Z_n < z_{\alpha/2}\right)$$

Taking the limit as $n \to \infty$:
$$\lim_{n \to \infty} P_\theta(-z_{\alpha/2} < Z_n < z_{\alpha/2}) = P(-z_{\alpha/2} < Z < z_{\alpha/2}) = \Phi(z_{\alpha/2}) - \Phi(-z_{\alpha/2})$$

By the symmetry of the normal distribution, $\Phi(-z) = 1 - \Phi(z)$, so:
$$\Phi(z_{\alpha/2}) - (1 - \Phi(z_{\alpha/2})) = 2\Phi(z_{\alpha/2}) - 1$$

Since $z_{\alpha/2} = \Phi^{-1}(1 - \alpha/2)$, we have $\Phi(z_{\alpha/2}) = 1 - \alpha/2$. Substituting yields:
$$2(1 - \alpha/2) - 1 = 2 - \alpha - 1 = 1 - \alpha \quad \blacksquare$$
---
## Example

### Example

Suppose we sample $n = 400$ consumers and find that $260$ prefer brand A.
We want a $95\%$ confidence interval for the population preference $p$.

1. **Point estimate:**
   $$\hat{p} = \frac{260}{400} = 0.65$$
2. **Estimated standard error:**
   $$\widehat{\text{se}} = \sqrt{\frac{\hat{p}(1 - \hat{p})}{n}} = \sqrt{\frac{0.65 \times 0.35}{400}} = \sqrt{\frac{0.2275}{400}} = \sqrt{0.00056875} \approx 0.02385$$
3. **Critical value:**
   For a $95\%$ CI, $\alpha = 0.05 \implies z_{0.025} = 1.96$.
4. **Margin of error:**
   $$\text{ME} = 1.96 \times 0.02385 \approx 0.04675$$
5. **Confidence Interval:**
   $$C_n = 0.65 \pm 0.04675 \implies [0.6032, 0.6968]$$

We conclude with $95\%$ confidence that the true population proportion lies between $60.32\%$ and $69.68\%$.
---
## Common Mistakes

### Common Mistakes

1. **Using $t$-critical values when $\sigma$ is known or $n$ is very large:**
   The $z$-interval is exact for normal populations with known $\sigma$ and asymptotically valid for any distribution with finite variance for large $n$.
2. **Dividing by $n$ instead of $\sqrt{n}$:**
   The standard error decreases as $1/\sqrt{n}$, not $1/n$.
3. **Plugging true parameter into $\widehat{\text{se}}$:**
   The true parameter $p$ or $\theta$ is unknown. We must plug in the sample estimate $\hat{p}$ or $\hat{\theta}$.
---
## Related Concepts

- [[Confidence Intervals and Confidence Sets]]
- [[Point Estimation]]
- [[Wald Test Statistic]]
- [[Maximum Likelihood Estimation]]
---
## Prerequisites

- [[Point Estimation]]
- [[Continuous Probability Distributions]]
- [[Central Limit Theorem]]
---
## Problems

- [[Bernoulli Parameter Estimation and Confidence Interval Example]]
- [[Problem — Comparing Prediction Algorithms via Paired Wald Test]]
---
## Sources

- [[cse301/01 - Sources/Lectures/Point Estimation and Confidence Intervals.pdf]]
