---
type: example
course: cse301
status: active
order: 54
---

# Bernoulli Bayesian Inference with Beta Prior Example

> 📖 **Reading Order:** Step 54 of 92 | **Module 8:** Bayesian Inference  
> ◄ **Previous:** [[Normal-Normal Conjugate Updating Formula]] | ► **Next:** [[Two Binomial Distributions Comparison via Bayesian Simulation Example]]

---

---

## Problem

A clinical trial tests a new drug on $n = 20$ patients, observing $s = 14$ successful recoveries and $6$ non-recoveries. Let $p \in (0, 1)$ denote the true recovery probability.

1. Assuming an initial non-informative flat prior $p \sim \text{Uniform}(0, 1)$, derive the posterior distribution of $p$.
2. Compute the Bayes point estimate (posterior mean) and the MAP estimate under this flat prior. Compare both with the Maximum Likelihood Estimator (MLE).
3. Now suppose an expert clinical researcher insists on an informative prior: based on historical treatments, they specify $p \sim \text{Beta}(4, 4)$ (prior mean $0.5$, effective prior sample size $8$). Derive the new posterior distribution, posterior mean, and MAP estimate.
4. Calculate the weight placed on the sample data versus the prior in both scenarios.

---

---

## Given

- Sample size: $n = 20$
- Successes: $s = 14$
- Likelihood: $L(p) \propto p^{14}(1 - p)^6$
- Prior 1: Flat uniform prior $\text{Beta}(1, 1)$
- Prior 2: Informative prior $\text{Beta}(4, 4)$

---

---

## Required

1. Posterior distributions for both priors.
2. Posterior mean, MAP, and MLE comparisons.
3. Weights on empirical sample vs. prior belief.

---

---

## Understanding the Problem and Choosing the Method

Identify the random variables, state the conditional distributions, select the appropriate probabilistic law or updating formula, and execute the algebraic substitutions step by step.

---

## Solution

### Concepts Used

- [[Bayesian Inference]]
- [[Maximum A Posteriori (MAP) Estimation]]
- [[Beta-Binomial Conjugate Updating Formula]]
- [[Maximum Likelihood Estimation]]

---
### Solution

### Scenario A: Flat Uniform Prior $\text{Beta}(1, 1)$

1. **Posterior Derivation:**
   Prior: $\alpha = 1, \beta = 1$.
   Applying [[Beta-Binomial Conjugate Updating Formula]]:
   $$\alpha_{\text{post}} = \alpha + s = 1 + 14 = 15$$
   $$\beta_{\text{post}} = \beta + n - s = 1 + 6 = 7$$
   $$p \mid \mathbf{X} \sim \text{Beta}(15, 7)$$

2. **Bayes Point Estimate (Posterior Mean):**
   $$\hat{p}_{\text{Bayes}} = \frac{\alpha_{\text{post}}}{\alpha_{\text{post}} + \beta_{\text{post}}} = \frac{15}{15 + 7} = \frac{15}{22} \approx 0.6818 \quad (68.18\%)$$

3. **MAP Estimate (Mode):**
   $$\hat{p}_{\text{MAP}} = \frac{\alpha_{\text{post}} - 1}{\alpha_{\text{post}} + \beta_{\text{post}} - 2} = \frac{15 - 1}{22 - 2} = \frac{14}{20} = 0.7000 \quad (70.00\%)$$

4. **Comparison with MLE:**
   $$\hat{p}_{\text{MLE}} = \frac{s}{n} = \frac{14}{20} = 0.7000 \quad (70.00\%)$$
   Notice that $\hat{p}_{\text{MAP}} \equiv \hat{p}_{\text{MLE}} = 0.70$.
   The Bayes posterior mean $\hat{p}_{\text{Bayes}} = \frac{15}{22} \approx 0.6818$ is slightly smoothed toward the center $0.5$ by Laplace's rule of succession.

---

### Scenario B: Informative Prior $\text{Beta}(4, 4)$

1. **Posterior Derivation:**
   Prior: $\alpha = 4, \beta = 4$.
   $$\alpha_{\text{post}} = 4 + 14 = 18$$
   $$\beta_{\text{post}} = 4 + 6 = 10$$
   $$p \mid \mathbf{X} \sim \text{Beta}(18, 10)$$

2. **Bayes Point Estimate (Posterior Mean):**
   $$\hat{p}_{\text{Bayes}} = \frac{18}{18 + 10} = \frac{18}{28} = \frac{9}{14} \approx 0.6429 \quad (64.29\%)$$

3. **MAP Estimate:**
   $$\hat{p}_{\text{MAP}} = \frac{18 - 1}{28 - 2} = \frac{17}{26} \approx 0.6538 \quad (65.38\%)$$

4. **Weighted Average Breakdown:**
   - Sample mean: $\bar{X} = 14/20 = 0.7000$
   - Prior mean: $p_0 = \frac{4}{4+4} = 0.5000$
   - Weight on data:
     $$w = \frac{n}{n + \alpha + \beta} = \frac{20}{20 + 4 + 4} = \frac{20}{28} = \frac{5}{7} \approx 71.43\%$$
   - Weight on prior:
     $$1 - w = \frac{\alpha + \beta}{n + \alpha + \beta} = \frac{8}{28} = \frac{2}{7} \approx 28.57\%$$
   - Verifying the weighted average:
     $$\hat{p}_{\text{Bayes}} = \frac{5}{7}(0.7000) + \frac{2}{7}(0.5000) = 0.5000 + 0.1429 = 0.6429 \quad \checkmark$$

---

---

## Result

| Metric | Flat Prior $\text{Beta}(1, 1)$ | Informative Prior $\text{Beta}(4, 4)$ | Classical Frequentist MLE |
|---|---|---|---|
| Posterior | $\text{Beta}(15, 7)$ | $\text{Beta}(18, 10)$ | N/A (point only) |
| Mean $\hat{p}$ | $\frac{15}{22} \approx 0.6818$ | $\frac{18}{28} \approx 0.6429$ | $0.7000$ |
| Mode (MAP) | $\frac{14}{20} = 0.7000$ | $\frac{17}{26} \approx 0.6538$ | $0.7000$ |
| Data Weight | $90.91\%$ ($w = 20/22$) | $71.43\%$ ($w = 20/28$) | $100\%$ |
| Prior Weight | $9.09\%$ ($1-w = 2/22$) | $28.57\%$ ($1-w = 8/28$) | $0\%$ |

---

---

## Why This Works

The solution holds because every step follows directly from Bayes' rule, the law of total probability, or properties of expectation and variance.

---

## Common Mistakes

Under the flat prior, the MAP estimate equals the MLE, while the posterior mean incorporates mild regularization. When an informative prior centered at $0.5$ is introduced, it exerts a gravitational pull (shrinkage) on the estimate, moving it from $0.70$ down to $0.6429$.

---

---

## General Method

Extract the reusable problem-solving pattern: define random variables, write down the joint distribution, condition on observed data, and normalize the resulting distribution.

---

## Related Concepts

- [[Bayesian Inference]]
- [[Maximum A Posteriori (MAP) Estimation]]
- [[Beta-Binomial Conjugate Updating Formula]]

---

---

## Sources

- [[01 - Sources/Lectures/Bayesian_Inference.pdf]]
