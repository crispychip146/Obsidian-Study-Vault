---
type: concept
course: cse301
status: active
order: 51
---

# Credible Intervals

> 📖 **Reading Order:** Step 51 of 92 | **Module 8:** Bayesian Inference  
> ◄ **Previous:** [[Maximum A Posteriori (MAP) Estimation]] | ► **Next:** [[Beta-Binomial Conjugate Updating Formula]]

---

## Definition

In Bayesian statistics, a **$1 - \alpha$ Credible Interval** (also called a **posterior interval**) for an unknown parameter $\theta \in \Theta$ is an interval $C \subset \Theta$ such that the posterior probability that $\theta$ lies within $C$, given the observed sample data $\mathbf{X} = \mathbf{x}$, is equal to $1 - \alpha$:

$$P(\theta \in C \mid \mathbf{X} = \mathbf{x}) = \int_C f(\theta \mid \mathbf{x}) d\theta = 1 - \alpha$$

For a vector parameter $\boldsymbol{\theta} \in \mathbb{R}^d$, $C$ is referred to as a **credible set** or **posterior region**.

Common choices of significance level $\alpha$ include $\alpha = 0.05$ (a $95\%$ credible interval) and $\alpha = 0.10$ (a $90\%$ credible interval).

---

## Intuition

A credible interval provides the exact answer to the intuitive question that most non-statisticians mistakenly believe a frequentist confidence interval answers:

> *"Given the data I actually observed, what is a range of values that contains the unknown parameter with 95% probability?"*

Because Bayesian statistics treats $\theta$ as a random variable conditional on the observed data $\mathbf{x}$, we can integrate the posterior density $f(\theta \mid \mathbf{x})$ directly between two endpoints $[a, b]$ to calculate the exact probability that $\theta \in [a, b]$.

---

## Credible Interval vs. Frequentist Confidence Interval

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

## Types of Credible Intervals

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

## Example: Normal-Normal Model

Let $X_1, \dots, X_n \sim N(\theta, \sigma^2)$ with known variance $\sigma^2$.
Assign a Gaussian prior $\theta \sim N(a, b^2)$.
As derived in [[Normal-Normal Conjugate Updating Formula]], the posterior distribution is Gaussian:
$$\theta \mid \mathbf{X} \sim N(\bar{\theta}, \tau^2)$$
where $\bar{\theta}$ is the posterior mean and $\tau^2$ is the posterior variance.

Because the normal distribution is perfectly symmetric and unimodal:
1. The equal-tailed credible interval and the HPD interval are **identical**.
2. Standardizing the posterior:
   $$\frac{\theta - \bar{\theta}}{\tau} \;\Bigg|\; \mathbf{X} \sim N(0, 1)$$
3. The exact $1 - \alpha$ Bayesian credible interval is:
   $$C = \left(\bar{\theta} - z_{\alpha/2} \tau, \quad \bar{\theta} + z_{\alpha/2} \tau\right)$$

For a $95\%$ credible interval ($\alpha = 0.05, z_{0.025} = 1.96$):
$$C = \left[\bar{\theta} - 1.96\tau, \quad \bar{\theta} + 1.96\tau\right]$$

---

## Asymptotic Agreement with Frequentist Intervals (Bernstein-von Mises Theorem)

As the sample size $n \to \infty$:
- The likelihood dominates the prior distribution.
- The posterior distribution converges to a Gaussian centered at the MLE with variance equal to the inverse Fisher information:
  $$\theta \mid \mathbf{X} \approx N\left(\hat{\theta}_{\text{MLE}}, \frac{1}{I_n(\hat{\theta}_{\text{MLE}})}\right)$$
- Consequently, for large $n$, the **Bayesian credible interval asymptotically coincides with the Frequentist Wald confidence interval**:
  $$C_{\text{Bayes}} \approx C_{\text{Frequentist}} \approx \hat{\theta}_{\text{MLE}} \pm z_{\alpha/2}\widehat{\text{se}}$$

---

## Common Mistakes

- Setting equal-tail cutoffs on a highly skewed posterior (such as an exponential or heavily skewed Beta) and expecting it to yield the shortest interval (the HPD region is shorter).
- Believing that credible intervals require large samples (unlike frequentist Wald intervals, Bayesian credible intervals are exact for any sample size $n$, even $n = 1$, provided the prior and likelihood models are correct).

---

## Related Concepts

- [[Bayesian Inference]]
- [[Confidence Intervals and Confidence Sets]]
- [[Normal-Normal Conjugate Updating Formula]]
- [[Beta-Binomial Conjugate Updating Formula]]

---

## Sources

- [[01 - Sources/Lectures/Bayesian_Inference.pdf]]
- [[01 - Sources/Lectures/Point Estimation and Confidence Intervals.pdf]]
