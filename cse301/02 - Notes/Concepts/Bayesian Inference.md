---
type: concept
course: cse301
status: active
order: 49
---

# Bayesian Inference

> 📖 **Reading Order:** Step 49 of 92 | **Module 8:** Bayesian Inference  
> ◄ **Previous:** [[Problem — Sample Variance Bias and Bessel's Correction Derivation]] | ► **Next:** [[Maximum A Posteriori (MAP) Estimation]]

---

## Definition

**Bayesian Inference** is an approach to statistical inference in which probabilities are interpreted as degrees of belief or measures of uncertainty about unknown states of nature, rather than as objective limiting relative frequencies.

In the Bayesian framework:
- The parameter $\theta \in \Theta$ is treated as a **random variable** governed by a probability distribution.
- Prior beliefs about $\theta$ before observing data are encoded in a **prior distribution** $f(\theta)$ (or $P(\theta)$).
- Given observed data $\mathbf{X} = (X_1, X_2, \dots, X_n)$ generated according to model $f(\mathbf{x} \mid \theta)$, beliefs are updated using **Bayes' Theorem** to form the **posterior distribution** $f(\theta \mid \mathbf{x})$:

$$f(\theta \mid \mathbf{x}) = \frac{f(\mathbf{x} \mid \theta) f(\theta)}{m(\mathbf{x})} = \frac{L_n(\theta) f(\theta)}{\int_\Theta L_n(\theta) f(\theta) d\theta} \propto L_n(\theta) f(\theta)$$

where:
- $f(\theta)$: The **prior probability density** (our belief before seeing the data).
- $L_n(\theta) = f(\mathbf{x} \mid \theta)$: The **likelihood function** (the probability of the data given parameter $\theta$).
- $m(\mathbf{x}) = \int_\Theta f(\mathbf{x} \mid \theta) f(\theta) d\theta$: The **marginal likelihood** or **evidence** (a normalizing constant independent of $\theta$).
- $f(\theta \mid \mathbf{x})$: The **posterior probability density** (our updated belief after observing the data).

---

## Frequentist vs. Bayesian Philosophies

| Dimension | Frequentist School (Classical) | Bayesian School |
|---|---|---|
| **Nature of Probability** | Limiting relative frequency of repeatable events in the physical world. | Quantified degree of personal belief, rational uncertainty, or information state. |
| **Status of Parameters $\theta$** | Fixed, constant, non-random unknown truth. Cannot make probability statements about $\theta$. | Random variable. Has a probability distribution before and after seeing data. |
| **Source of Randomness** | The data collection procedure / random sampling across hypothetical repetitions. | The observer's epistemic uncertainty about the parameter. |
| **Updating Mechanism** | Estimators, tests, asymptotic sampling distributions. | Bayes' theorem: $\text{Posterior} \propto \text{Likelihood} \times \text{Prior}$. |
| **Interval Estimation** | [[Confidence Intervals and Confidence Sets]]: $95\%$ of random intervals cover fixed $\theta$. | [[Credible Intervals]]: Given the observed data, $P(\theta \in C \mid \mathbf{x}) = 0.95$. |

---

## Intuition: The Information Updating Pipeline

```
           Prior Belief f(θ)
                  ↓
          Collect Data x
                  ↓
       Compute Likelihood L(θ)
                  ↓
  Combine: Posterior ∝ Likelihood × Prior
                  ↓
     Extract Point / Interval Estimates
```

1. **Before the study:** You believe a coin is probably fair ($\theta \approx 0.5$), but you leave room for some bias (Prior).
2. **Experiment:** You flip the coin 100 times and observe 80 heads (Likelihood strongly favors $\theta = 0.8$).
3. **After the study:** Your updated belief (Posterior) is a compromise: you no longer believe the coin is perfectly fair, but because of your prior skepticism, you don't immediately believe it has an $80\%$ bias either. Your posterior centers around $\approx 0.72$.

---

## Bayesian Point Estimation

Unlike frequentist inference which focuses heavily on the single point $\hat{\theta}_{\text{MLE}}$, Bayesian inference provides the **entire continuous posterior distribution** $f(\theta \mid \mathbf{x})$. To compress this distribution into a single number, one selects a summary metric based on a loss function:

1. **Posterior Mean (Bayes Estimator):**
   $$\hat{\theta}_{\text{Bayes}} = E[\theta \mid \mathbf{X}] = \int_\Theta \theta f(\theta \mid \mathbf{x}) d\theta$$
   *Optimality:* Minimizes expected squared error loss $L(\theta, \hat{\theta}) = (\theta - \hat{\theta})^2$.
2. **Posterior Median:**
   $$\int_{-\infty}^{\hat{\theta}_{\text{median}}} f(\theta \mid \mathbf{x}) d\theta = 0.5$$
   *Optimality:* Minimizes expected absolute error loss $L(\theta, \hat{\theta}) = \lvert \theta - \hat{\theta} \rvert$.
3. **Maximum A Posteriori (MAP) Estimator:**
   $$\hat{\theta}_{\text{MAP}} = \arg\max_{\theta \in \Theta} f(\theta \mid \mathbf{x})$$
   *Optimality:* The mode of the posterior distribution (minimizes 0-1 classification loss). See [[Maximum A Posteriori (MAP) Estimation]].

---

## Conjugate Priors

A prior distribution $f(\theta)$ is called **conjugate** to a likelihood model $f(x \mid \theta)$ if the resulting posterior distribution $f(\theta \mid x)$ belongs to the **exact same parametric probability family** as the prior.

Conjugate priors allow exact closed-form algebraic Bayesian updating without having to numerically evaluate intractable integrals in the denominator $m(\mathbf{x})$.

### Major Conjugate Families

| Data Likelihood | Parameter | Conjugate Prior | Posterior Distribution |
|---|---|---|---|
| $\text{Bernoulli}(p)$ / $\text{Binomial}(n, p)$ | $p \in (0, 1)$ | $\text{Beta}(\alpha, \beta)$ | $\text{Beta}(\alpha + s, \beta + n - s)$ |
| $\text{Poisson}(\lambda)$ | $\lambda > 0$ | $\text{Gamma}(\alpha, \beta)$ | $\text{Gamma}(\alpha + \sum X_i, \beta + n)$ |
| $N(\theta, \sigma^2)$ ($\sigma^2$ known) | $\theta \in \mathbb{R}$ | $N(a, b^2)$ | $N(\bar{\theta}, \tau^2)$ |
| $\text{Exponential}(\lambda)$ | $\lambda > 0$ | $\text{Gamma}(\alpha, \beta)$ | $\text{Gamma}(\alpha + n, \beta + \sum X_i)$ |

---

## Types of Priors

1. **Informative / Subjective Priors:**
   Reflect genuine historical data, scientific consensus, or physical constraints (e.g., historical medical trials).
2. **Non-informative / Flat Priors:**
   Designed to let the data "speak for itself" without introducing strong prior bias (e.g., $\text{Uniform}(0, 1)$ on probability $p$).
3. **Improper Priors:**
   Priors that do not integrate to 1 ($\int f(\theta) d\theta = \infty$), such as $f(\theta) = 1$ on $\theta \in (-\infty, \infty)$. As long as the posterior $\propto L_n(\theta) f(\theta)$ integrates to a finite number, the resulting posterior is a valid, proper probability distribution.
4. **Jeffreys' Prior:**
   An objective prior invariant under parameter transformation, defined using the [[Likelihood and Score Equations|Fisher Information]]:
   $$f(\theta) \propto \sqrt{I_1(\theta)}$$
   For a $\text{Bernoulli}(p)$ model, Jeffreys' prior is $f(p) \propto \frac{1}{\sqrt{p(1-p)}} \iff \text{Beta}\left(\frac{1}{2}, \frac{1}{2}\right)$.

---

## When NOT to Use Bayesian Inference

1. **Weak Data + Strong Subjective Prior:**
   When sample size $n$ is very small, a poorly calibrated or overly dogmatic subjective prior dominates the likelihood, causing severe bias.
2. **Computational Tractability:**
   Outside of simple conjugate models, normalizing constants $\int L_n(\theta) f(\theta) d\theta$ in high dimensions require computationally intensive Markov Chain Monte Carlo (MCMC) simulations.
3. **Legal or Regulatory Contexts:**
   In clinical drug approvals or legal court proceedings, regulators frequently mandate objective frequentist guarantees that are completely immune to subjective investigator biases.

---

## Common Mistakes

1. **Treating the posterior as proportional to the prior alone:**
   Forgetting that the likelihood acts as the filter: $\text{Posterior} \propto \text{Likelihood} \times \text{Prior}$.
2. **Confusing Posterior Mean and MAP:**
   The posterior mean is the center of mass $\int \theta f(\theta \mid x) d\theta$, while MAP is the peak/mode $\arg\max f(\theta \mid x)$. They only coincide for symmetric unimodal posteriors (such as Gaussians).
3. **Integrating over data instead of parameters:**
   The normalizing constant integrates out the parameter $\theta$: $m(x) = \int f(x \mid \theta) f(\theta) d\theta$. The data $x$ are fixed constants during this integration.

---

## Exam Relevance

In examinations, expect to:
1. Identify the philosophical differences between frequentist and Bayesian inference.
2. Set up Bayes' rule for conjugate models and derive posterior hyperparameters.
3. Calculate posterior means, medians, MAP estimators, and credible intervals.
4. Explain how prior parameters act as "fictitious prior observations" (pseudocounts).

---

## Related Concepts

- [[Maximum A Posteriori (MAP) Estimation]]
- [[Credible Intervals]]
- [[Beta-Binomial Conjugate Updating Formula]]
- [[Normal-Normal Conjugate Updating Formula]]
- [[Maximum Likelihood Estimation]]

---

## Prerequisites

- [[Point Estimation]]
- Bayes' Theorem for events and continuous distributions
- Probability Density Functions and Expectation Integrals

---

## Examples & Problems

- [[Bernoulli Bayesian Inference with Beta Prior Example]]
- [[Two Binomial Distributions Comparison via Bayesian Simulation Example]]
- [[Problem — Laplace Rule of Succession and Bayesian Updating]]
- [[Berger-Wolpert Confidence Set Puzzle Example]]

---

## Sources

- [[01 - Sources/Lectures/Bayesian_Inference.pdf]]
