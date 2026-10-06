---
type: concept
course: cse301
status: active
order: 62
---

# Credible Intervals

> 📖 **Reading Order:** Step 62 of 103 | **Module 8:** Bayesian Inference  
> ◄ **Previous:** [[Maximum A Posteriori (MAP) Estimation]] | ► **Next:** [[Beta-Binomial Conjugate Updating Formula]]
---
## Starting Point and the Problem

Probability and statistical inference model uncertainty in physical and computer systems. When analyzing stochastic phenomena related to Credible Intervals, naive counting or deterministic approximations fail. We establish a formal mathematical foundation to quantify outcomes and evaluate expectations rigorously.

---

## Developing the Idea

A credible interval provides the exact answer to the intuitive question that most non-statisticians mistakenly believe a frequentist confidence interval answers:

> *"Given the data I actually observed, what is a range of values that contains the unknown parameter with 95% probability?"*

Because Bayesian statistics treats $\theta$ as a random variable conditional on the observed data $\mathbf{x}$, we can integrate the posterior density $f(\theta \mid \mathbf{x})$ directly between two endpoints $[a, b]$ to calculate the exact probability that $\theta \in [a, b]$.
---
## Definition

In Bayesian statistics, a **$1 - \alpha$ Credible Interval** (also called a **posterior interval**) for an unknown parameter $\theta \in \Theta$ is an interval $C \subset \Theta$ such that the posterior probability that $\theta$ lies within $C$, given the observed sample data $\mathbf{X} = \mathbf{x}$, is equal to $1 - \alpha$:

$$P(\theta \in C \mid \mathbf{X} = \mathbf{x}) = \int_C f(\theta \mid \mathbf{x}) d\theta = 1 - \alpha$$

For a vector parameter $\boldsymbol{\theta} \in \mathbb{R}^d$, $C$ is referred to as a **credible set** or **posterior region**.

Common choices of significance level $\alpha$ include $\alpha = 0.05$ (a $95\%$ credible interval) and $\alpha = 0.10$ (a $90\%$ credible interval).
---
## How It Works

### Credible Interval vs. Frequentist Confidence Interval

Understanding the philosophical and mathematical differences between these two concepts is a cornerstone of modern statistical theory:

| Feature | Frequentist Confidence Interval ($C_n$) | Bayesian Credible Interval ($C$) |
|---|---|---|
| **Mathematical Statement** | $P_\theta(\theta \in C_n) \ge 1 - \alpha$ | $P(\theta \in C \mid \mathbf{X} = \mathbf{x}) = 1 - \alpha$ |
| **What is Random?** | The interval endpoints $a(\mathbf{X}), b(\mathbf{X})$. | The parameter $\theta$ conditional on $\mathbf{X} = \mathbf{x}$. |
| **What is Fixed?** | The parameter $\theta$. | The realized interval $[a, b]$ and data $\mathbf{x}$. |
| **Meaning of $95\%$** | If the experiment is repeated across 100 hypothetical samples, $\approx 95$ realized intervals will contain $\theta$. | Given the specific dataset collected, there is a $95\%$ probability that $\theta$ lies inside $[a, b]$. |
| **Depends on Prior?** | No. Uses only the data likelihood. | Yes. Depends on the chosen prior distribution $f(\theta)$. |
| **Post-Data Certainty** | Subject to pre-data paradoxes (e.g., [[Berger-Wolpert Confidence Set Puzzle Example]]). | Directly reflects post-data certainty. |

> 🔑 **Golden Rule for Exams:** 
> You can **only** say *"There is a 95% probability that $\theta$ lies in $[a, b]$"* when referring to a **Bayesian Credible Interval**. Making that statement about a frequentist confidence interval is mathematically incorrect under classical statistical definitions.

---
### Types of Credible Intervals

Because there are infinite intervals that contain $1 - \alpha$ area under the posterior curve, two primary conventions are used:

### 1. Equal-Tailed Credible Interval
The interval $C = (q_{\alpha/2}, q_{1 - \alpha/2})$ is constructed by placing equal probability mass $\alpha/2$ in both the lower and upper tails:
$$\int_{-\infty}^{q_{\alpha/2}} f(\theta \mid \mathbf{x}) d\theta = \frac{\alpha}{2} \quad \text{and} \quad \int_{q_{1 - \alpha/2}}^\infty f(\theta \mid \mathbf{x}) d\theta = \frac{\alpha}{2}$$

- **Advantage:** Trivial to compute from standard quantile functions (e.g., using `qbeta` or standard normal quantiles).
- **Limitation:** Can include points with lower posterior density than points outside the interval if the posterior is asymmetric or skewed.

### 2. Highest Posterior Density (HPD) Region
An HPD region is defined as the subset of the parameter space:
$$C_{\text{HPD}} = \{\theta \in \Theta : f(\theta \mid \mathbf{x}) \ge k\}$$
where $k$ is the largest constant chosen such that $\int_{C_{\text{HPD}}} f(\theta \mid \mathbf{x}) d\theta = 1 - \alpha$.

- **Key Properties:**
  1. Every point inside $C_{\text{HPD}}$ has higher posterior density than any point outside it.
  2. It is the **narrowest (shortest) possible interval** with coverage $1 - \alpha$.
  3. If the posterior is multimodal, the HPD set can naturally split into disjoint intervals.
---
## Example

### Normal-Normal Conjugate Model Credible Interval

Let $X_1, \dots, X_n \overset{\text{iid}}{\sim} \mathcal{N}(\theta, \sigma^2)$ with known variance $\sigma^2$.
Assign a Gaussian prior $\theta \sim \mathcal{N}(\mu_0, \sigma_0^2)$.
From [[Normal-Normal Conjugate Updating Formula]], the posterior distribution is Gaussian:
$$\theta \mid \mathbf{X} \sim \mathcal{N}(\mu_n, \sigma_n^2)$$
where the posterior precision is $\frac{1}{\sigma_n^2} = \frac{1}{\sigma_0^2} + \frac{n}{\sigma^2}$ and posterior mean is $\mu_n = \sigma_n^2 \left( \frac{\mu_0}{\sigma_0^2} + \frac{n\bar{X}}{\sigma^2} \right)$.

Because the posterior density is symmetric and unimodal:
1. The equal-tailed credible interval and the Highest Posterior Density (HPD) interval are **identical**.
2. Standardizing the posterior:
   $$\frac{\theta - \mu_n}{\sigma_n} \;\Bigg|\; \mathbf{X} \sim \mathcal{N}(0, 1)$$
3. The exact $1 - \alpha$ Bayesian credible interval is:
   $$C = \left[\mu_n - z_{\alpha/2} \sigma_n, \quad \mu_n + z_{\alpha/2} \sigma_n\right]$$

For a $95\%$ credible interval ($\alpha = 0.05, z_{0.025} = 1.96$):
$$C_{0.95} = \left[\mu_n - 1.96\sigma_n, \quad \mu_n + 1.96\sigma_n\right]$$

**Bayesian Interpretation:** Conditional on the observed data $\mathbf{X}$, the probability that $\theta$ lies inside $C_{0.95}$ is precisely $0.95$. Contrast this with frequentist [[Confidence Intervals and Confidence Sets]], where $\theta$ is fixed and only the interval bounds are random before data observation.

For worked discrete and conjugate interval examples, see [[Bernoulli Bayesian Inference with Beta Prior Example]] and [[Normal-Normal Conjugate Updating Formula]].

---

## Technical Details

### Equal-Tailed vs. HPD Regions and Bernstein-von Mises Theorem

1. **Equal-Tailed vs. Highest Posterior Density (HPD) Regions:**
   - **Equal-Tailed:** Sets cutoffs $q_{\alpha/2}$ and $q_{1 - \alpha/2}$ such that $P(\theta < q_{\alpha/2} \mid x) = \alpha/2$ and $P(\theta > q_{1 - \alpha/2} \mid x) = \alpha/2$. Computationally trivial via posterior CDF quantiles, but suboptimal for skewed posteriors.
   - **HPD Region:** Defined as $C_{\text{HPD}} = \{\theta : f(\theta \mid x) \ge k_\alpha\}$. By the Neyman-Pearson-style lemma for sets, $C_{\text{HPD}}$ has the **smallest volume (length)** among all sets with posterior probability $1 - \alpha$. If the posterior is multimodal, the HPD set naturally splits into disconnected intervals.
2. **Bernstein-von Mises Theorem (Bayesian-Frequentist Asymptotics):**
   - As $n \to \infty$, under standard regularity conditions:
     $$\lVert f(\theta \mid \mathbf{X}) - \mathcal{N}\left(\hat{\theta}_{\text{MLE}}, [n I_1(\hat{\theta}_{\text{MLE}})]^{-1}\right) \rVert_{\text{TV}} \xrightarrow{P} 0$$
   - The effect of the prior distribution washes out completely at rate $O(1/\sqrt{n})$.
   - As a consequence, a Bayesian $1 - \alpha$ credible interval asymptotically has exact frequentist coverage probability $1 - \alpha$.

---

## Important Properties and Why They Hold

### Asymptotic Agreement with Frequentist Intervals (Bernstein-von Mises Theorem)

As the sample size $n \to \infty$:
- The likelihood dominates the prior distribution.
- The posterior distribution converges to a Gaussian centered at the MLE with variance equal to the inverse Fisher information:
  $$\theta \mid \mathbf{X} \approx N\left(\hat{\theta}_{\text{MLE}}, \frac{1}{I_n(\hat{\theta}_{\text{MLE}})}\right)$$
- Consequently, for large $n$, the **Bayesian credible interval asymptotically coincides with the Frequentist Wald confidence interval**:
  $$C_{\text{Bayes}} \approx C_{\text{Frequentist}} \approx \hat{\theta}_{\text{MLE}} \pm z_{\alpha/2}\widehat{\text{se}}$$
---
## Common Mistakes

### Common Mistakes

- Setting equal-tail cutoffs on a highly skewed posterior (such as an exponential or heavily skewed Beta) and expecting it to yield the shortest interval (the HPD region is shorter).
- Believing that credible intervals require large samples (unlike frequentist Wald intervals, Bayesian credible intervals are exact for any sample size $n$, even $n = 1$, provided the prior and likelihood models are correct).
---
## Exam Relevance

In exam problems, expect to:
1. Contrast the philosophical interpretations of Bayesian credible intervals ($P(\theta \in C \mid x) = 1 - \alpha$) and frequentist confidence intervals ($P_\theta(\theta \in C(X)) = 1 - \alpha$).
2. Compute equal-tailed credible intervals using Gaussian and Beta quantiles.
3. State conditions under which equal-tailed and HPD credible intervals coincide (symmetry and unimodality).
---
## Related Concepts

- [[Bayesian Inference]]
- [[Confidence Intervals and Confidence Sets]]
- [[Normal-Normal Conjugate Updating Formula]]
- [[Beta-Binomial Conjugate Updating Formula]]
---
## Prerequisites

- [[Bayesian Inference]]
- [[Confidence Intervals and Confidence Sets]]
- [[Continuous Probability Distributions]]

---

## Problems

- [[Problem — Laplace Rule of Succession and Bayesian Updating]]

---

## Sources

- [[cse301/01 - Sources/Lectures/Bayesian_Inference.pdf]]
- [[cse301/01 - Sources/Lectures/Point Estimation and Confidence Intervals.pdf]]
