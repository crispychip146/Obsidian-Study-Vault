---
type: formula
course: cse301
status: active
order: 59
---

# Wald Test Statistic

> 📖 **Reading Order:** Step 59 of 92 | **Module 9:** Hypothesis Testing  
> ◄ **Previous:** [[p-Values and Significance]] | ► **Next:** [[Pearson's Chi-Square Goodness-of-Fit Test]]

---

---

## The Question and Earlier Knowledge

What analytical relationship or closed-form expectation governs Wald Test Statistic, and how can we compute it directly from constituent probability terms? In complex probability models, calculating probabilities or moments directly is often intractable without decomposing expectations across conditioning partitions or inequalities.

---

## Developing the Formula

By decomposing joint distributions into conditional components, expanding algebraic products, or applying geometric series sums, Wald Test Statistic compresses complex probabilistic reasoning into a clean, reusable formula.

---

## Formula

The **Wald Test** is an asymptotic hypothesis test for evaluating null hypotheses about an unknown scalar or vector parameter $\theta$.

### 1. General Scalar Wald Statistic
To test the null hypothesis:
$$H_0: \theta = \theta_0 \quad \text{versus} \quad H_1: \theta \ne \theta_0$$

The Wald test statistic is:
$$W = \frac{\hat{\theta}_n - \theta_0}{\widehat{\text{se}}}$$

where:
- $\hat{\theta}_n$ is an asymptotically normal estimator (most commonly the Maximum Likelihood Estimator).
- $\theta_0$ is the hypothesized null value.
- $\widehat{\text{se}} = \widehat{\text{se}}(\hat{\theta}_n)$ is the estimated standard error.

### 2. Asymptotic Null Distribution and Decision Rule
Under the null hypothesis $H_0$, as $n \to \infty$:
$$W \xrightarrow{d} N(0, 1)$$

For a two-sided test at significance level $\alpha$:
$$\text{Reject } H_0 \iff \lvert W \rvert > z_{\alpha/2}$$
where $z_{\alpha/2} = \Phi^{-1}(1 - \alpha/2)$.

### 3. $p$-Value Calculation
For an observed test statistic value $w = \frac{\hat{\theta} - \theta_0}{\widehat{\text{se}}}$:
$$p = 2 \cdot \Phi(-\lvert w \rvert) = 2 \cdot \big(1 - \Phi(\lvert w \rvert)\big)$$

### 4. Two-Sample Mean Comparison (Wald Test)
To test $H_0: \mu_1 - \mu_2 = 0$ versus $H_1: \mu_1 - \mu_2 \ne 0$ from two independent samples $X_1, \dots, X_m$ and $Y_1, \dots, Y_n$:
$$W = \frac{(\bar{X} - \bar{Y}) - 0}{\sqrt{\frac{S_X^2}{m} + \frac{S_Y^2}{n}}}$$
where $S_X^2$ and $S_Y^2$ are the sample variances.

---

---

## Variables

| Symbol | Meaning | Dimensions |
|---|---|---|
| $\hat{\theta}_n$ | Point estimator (e.g., MLE) | Scalar |
| $\theta_0$ | Value of parameter claimed by $H_0$ | Constant |
| $\widehat{\text{se}}$ | Estimated standard error $\sqrt{\widehat{\text{Var}}(\hat{\theta}_n)}$ | Positive scalar |
| $W$ | Wald test statistic | Standardized score |
| $z_{\alpha/2}$ | Normal critical threshold | $1.96$ for $\alpha = 0.05$ |
| $p$ | Two-sided $p$-value | $(0, 1)$ |

---

---

## Conditions

1. **Asymptotic Normality:** The estimator must satisfy:
   $$\frac{\hat{\theta}_n - \theta_0}{\text{se}} \xrightarrow{d} N(0, 1)$$
   under $H_0$.
2. **Consistent Standard Error:** The estimated standard error must be consistent:
   $$\frac{\widehat{\text{se}}}{\text{se}} \xrightarrow{P} 1$$
   By Slutsky's theorem, dividing by $\widehat{\text{se}}$ preserves standard normal convergence.
3. **Sufficient Sample Size:** $n$ must be large enough that the finite-sample distribution of $\hat{\theta}_n$ is well approximated by a Gaussian.

---

---

## Intuition

### Intuition

The Wald test measures how many **standard errors** the empirical estimate $\hat{\theta}_n$ sits away from the hypothesized center $\theta_0$:
$$\text{Wald Statistic} = \frac{\text{Observed Deviation}}{\text{Standard Error of the Deviation}}$$

If $W = 0.4$, the estimate is less than half a standard error away from the null value—completely ordinary random noise.
If $W = 4.2$, the estimate is over 4 standard errors away from the null value—an occurrence with probability less than $1$ in $10,000$ under $H_0$, warranting immediate rejection.

---
### Duality with Confidence Intervals

Notice that:
$$\lvert W \rvert \le z_{\alpha/2} \iff -z_{\alpha/2} \le \frac{\hat{\theta}_n - \theta_0}{\widehat{\text{se}}} \le z_{\alpha/2} \iff \hat{\theta}_n - z_{\alpha/2}\widehat{\text{se}} \le \theta_0 \le \hat{\theta}_n + z_{\alpha/2}\widehat{\text{se}}$$

Therefore:
$$\text{The size } \alpha \text{ Wald test rejects } H_0: \theta = \theta_0 \iff \theta_0 \notin C_n$$
where $C_n = \hat{\theta}_n \pm z_{\alpha/2}\widehat{\text{se}}$ is the standard $1 - \alpha$ confidence interval!

---

---

## Derivation

### Derivation of Asymptotic Size $\alpha$

We wish to prove that the Wald test has asymptotic size $\alpha$:
$$\lim_{n \to \infty} P_{\theta_0}\left(\lvert W \rvert > z_{\alpha/2}\right) = \alpha$$

By assumption, under $H_0: \theta = \theta_0$:
$$W = \frac{\hat{\theta}_n - \theta_0}{\widehat{\text{se}}} \xrightarrow{d} Z \sim N(0, 1)$$

Evaluate the rejection probability:
$$P_{\theta_0}(\lvert W \rvert > z_{\alpha/2}) = P_{\theta_0}(W < -z_{\alpha/2}) + P_{\theta_0}(W > z_{\alpha/2})$$

By continuous mapping and convergence in distribution:
$$\lim_{n \to \infty} P_{\theta_0}(W < -z_{\alpha/2}) = \Phi(-z_{\alpha/2}) = \frac{\alpha}{2}$$
$$\lim_{n \to \infty} P_{\theta_0}(W > z_{\alpha/2}) = 1 - \Phi(z_{\alpha/2}) = \frac{\alpha}{2}$$

Summing the two tails:
$$\lim_{n \to \infty} P_{\theta_0}(\lvert W \rvert > z_{\alpha/2}) = \frac{\alpha}{2} + \frac{\alpha}{2} = \alpha \quad \blacksquare$$

---

---

## Example

### Example: Comparing Prediction Algorithms (Unpaired)

Algorithm 1 is tested on $m = 100$ independent examples and makes $X = 15$ errors ($\hat{p}_1 = 0.15$).
Algorithm 2 is tested on $n = 100$ independent examples and makes $Y = 25$ errors ($\hat{p}_2 = 0.25$).
We test $H_0: p_1 - p_2 = 0$ versus $H_1: p_1 - p_2 \ne 0$ at $\alpha = 0.05$.

1. **Difference in proportions:**
   $$\hat{\delta} = \hat{p}_1 - \hat{p}_2 = 0.15 - 0.25 = -0.10$$
2. **Estimated standard error:**
   $$\widehat{\text{se}} = \sqrt{\frac{\hat{p}_1(1 - \hat{p}_1)}{m} + \frac{\hat{p}_2(1 - \hat{p}_2)}{n}} = \sqrt{\frac{0.15(0.85)}{100} + \frac{0.25(0.75)}{100}} = \sqrt{0.001275 + 0.001875} = \sqrt{0.00315} \approx 0.0561$$
3. **Wald statistic:**
   $$W = \frac{-0.10 - 0}{0.0561} \approx -1.78$$
4. **Decision:**
   Since $\lvert W \rvert = 1.78 < z_{0.025} = 1.96$, we **fail to reject $H_0$** at the $5\%$ significance level.
5. **$p$-value:**
   $$p = 2\Phi(-1.78) = 2(0.0375) = 0.075 \quad (7.5\%)$$
   There is weak/marginal evidence, but not sufficient proof at the $\alpha = 0.05$ standard to declare one algorithm superior.

---

---

## Common Mistakes

- Confusing conditional variance with the variance of conditional expectation (Eve's Law components).
- Forgetting that linearity of expectation holds unconditionally, whereas $\mathbb{E}[XY] = \mathbb{E}[X]\mathbb{E}[Y]$ requires independence.

---

## Related Concepts

- [[Hypothesis Testing Framework]]
- [[p-Values and Significance]]
- [[Normal-Based Large-Sample Confidence Interval]]
- [[Problem — Comparing Prediction Algorithms via Paired Wald Test]]

---

---

## Prerequisites

- [[Random Variables and Probability Distributions]]

---

## Problems

- [[Problem — Birthday Collisions and Approximation]]

---

## Sources

- [[01 - Sources/Lectures/CSE301_Hypothesis_Test.pdf]]
