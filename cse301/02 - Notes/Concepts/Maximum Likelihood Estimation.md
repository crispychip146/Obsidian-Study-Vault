---
type: concept
course: cse301
status: active
order: 43
---

# Maximum Likelihood Estimation

> 📖 **Reading Order:** Step 43 of 92 | **Module 7:** Parametric Inference  
> ◄ **Previous:** [[Problem — Unbiased yet Inconsistent Estimator Analysis]] | ► **Next:** [[Likelihood and Score Equations]]

---

---

## Starting Point and the Problem

Probability and statistical inference model uncertainty in physical and computer systems. When analyzing stochastic phenomena related to Maximum Likelihood Estimation, naive counting or deterministic approximations fail. We establish a formal mathematical foundation to quantify outcomes and evaluate expectations rigorously.

---

## Developing the Idea

Think of maximum likelihood estimation through the lens of a detective investigating a crime scene:
- You have already observed the clues (the dataset $X_1, \dots, X_n$).
- There are several plausible suspects or explanations (hypotheses / parameters $\theta$).
- For each suspect $\theta$, you compute the probability that their actions would have generated the exact physical evidence found at the scene.
- The MLE picks the suspect under whose hypothesis the observed evidence is **most probable**.

### Connection to Bayes' Rule
Why is maximizing $P(\text{data} \mid \theta)$ a sensible way to choose $\theta$?
By Bayes' rule:
$$P(\theta \mid \text{data}) = \frac{P(\text{data} \mid \theta) P(\theta)}{P(\text{data})}$$

If we assume a priori that all parameter values $\theta$ are equally likely (a flat, non-informative uniform prior $P(\theta) = c$), then:
$$P(\theta \mid \text{data}) \propto P(\text{data} \mid \theta) = L_n(\theta)$$
Thus, **the MLE is exactly the parameter value that maximizes the posterior probability under a uniform prior!**

---

---

## Definition

**Maximum Likelihood Estimation (MLE)** is a method of estimating the unknown parameters $\theta \in \Theta$ of a statistical model by finding the parameter values that maximize the **likelihood function**—the probability (or probability density) of having observed the collected data under that parameter.

Let $X_1, X_2, \dots, X_n \overset{\text{iid}}{\sim} f(x; \theta)$. 
The **likelihood function** $L_n(\theta)$ is the joint probability density function viewed as a function of the parameter $\theta$:
$$L_n(\theta) = \prod_{i=1}^n f(X_i; \theta)$$

The **Maximum Likelihood Estimator (MLE)**, denoted $\hat{\theta}_{\text{MLE}}$ or $\hat{\theta}_n$, is defined as:
$$\hat{\theta}_n = \arg\max_{\theta \in \Theta} L_n(\theta)$$

Because the natural logarithm is a strictly monotonically increasing function, maximizing $L_n(\theta)$ is mathematically equivalent to maximizing the **log-likelihood function** $\ell_n(\theta)$:
$$\ell_n(\theta) = \log L_n(\theta) = \sum_{i=1}^n \log f(X_i; \theta)$$
$$\hat{\theta}_n = \arg\max_{\theta \in \Theta} \ell_n(\theta)$$

---

---

## How It Works

### Why Use the Log-Likelihood?

1. **Summation vs. Multiplication:** Products of $n$ small probabilities $\prod f(X_i; \theta)$ quickly cause floating-point arithmetic underflow on computers. Taking logarithms turns the product into a sum $\sum \log f(X_i; \theta)$, which is numerically stable and straightforward to differentiate.
2. **Identical Maximizer:** Because $\frac{d}{du}\log(u) = \frac{1}{u} > 0$ for all $u > 0$, $\log$ is strictly increasing. Therefore:
   $$\arg\max_\theta L_n(\theta) = \arg\max_\theta \log L_n(\theta)$$
3. **Multiplicative Constants:** Any positive constant factor $c$ that does not depend on $\theta$ adds a constant $\log c$ to $\ell_n(\theta)$, leaving its derivative and maximizer completely unchanged.

---
### How It Works: The Standard Calculus Recipe

For smooth, differentiable models where the support of $X$ does not depend on $\theta$:

1. **Write down the joint likelihood:**
   $$L_n(\theta) = \prod_{i=1}^n f(X_i; \theta)$$
2. **Compute the log-likelihood:**
   $$\ell_n(\theta) = \sum_{i=1}^n \log f(X_i; \theta)$$
3. **Compute the score function (first derivative):**
   $$S_n(\theta) = \frac{\partial \ell_n(\theta)}{\partial \theta}$$
4. **Set the score equations to zero:**
   $$\frac{\partial \ell_n(\theta)}{\partial \theta} = 0$$
   and solve for $\theta$.
5. **Verify the maximum (second derivative / Hessian):**
   Ensure that the second derivative is strictly negative at the root:
   $$\left.\frac{\partial^2 \ell_n(\theta)}{\partial \theta^2}\right|_{\theta = \hat{\theta}} < 0$$
   (or that the Hessian matrix is negative definite for vector parameters).

> ⚠️ **Warning — Non-Regular Distributions:** If the support of $f(x; \theta)$ depends on $\theta$ (such as $\text{Uniform}(0, \theta)$), the likelihood is discontinuous or non-differentiable at the boundary. Calculus fails, and the MLE must be determined by inspecting boundary conditions (see [[Uniform Distribution Non-Regular MLE Example]]).

---
### Important Properties of MLEs

Under standard regularity conditions (smoothness, common support independent of $\theta$, identifiable parameter space), the MLE possesses four stellar theoretical properties:

### 1. Equivariance (Functional Invariance)
If $\hat{\theta}_n$ is the MLE of parameter $\theta$, and $\tau = g(\theta)$ is any arbitrary function of $\theta$, then the MLE of $\tau$ is simply:
$$\hat{\tau}_n = g(\hat{\theta}_n)$$
*Example:* If the MLE of the rate $\lambda$ of an exponential distribution is $\hat{\lambda} = 1/\bar{X}$, then the MLE of the mean lifetime $\mu = 1/\lambda$ is automatically $\hat{\mu} = 1/\hat{\lambda} = \bar{X}$.

### 2. Consistency
As $n \to \infty$, the MLE converges in probability to the true parameter value $\theta_0$:
$$\hat{\theta}_n \xrightarrow{P} \theta_0$$
*Intuition:* By the Law of Large Numbers, the normalized log-likelihood $\frac{1}{n}\ell_n(\theta) \xrightarrow{P} E_{\theta_0}[\log f(X; \theta)]$. By Jensen's inequality and Kullback-Leibler divergence:
$$E_{\theta_0}\left[\log \frac{f(X; \theta)}{f(X; \theta_0)}\right] = - D_{\text{KL}}(f_{\theta_0} \parallel f_\theta) \le 0$$
with equality if and only if $f_\theta = f_{\theta_0}$. If the model is identifiable, the population log-likelihood has a unique global maximum at the true parameter $\theta_0$.

### 3. Asymptotic Normality
As $n \to \infty$, the distribution of $\hat{\theta}_n$ approaches a Gaussian distribution centered at $\theta_0$:
$$\sqrt{n}(\hat{\theta}_n - \theta_0) \xrightarrow{d} N\left(0, \frac{1}{I_1(\theta_0)}\right)$$
where $I_1(\theta) = -E\left[\frac{\partial^2 \log f(X; \theta)}{\partial \theta^2}\right]$ is the [[Likelihood and Score Equations|Fisher Information]].

### 4. Asymptotic Efficiency
Among all consistent, asymptotically normal estimators, the MLE achieves the lowest possible asymptotic variance, meeting the **Cramér-Rao Lower Bound**.

---
### Does MLE Guarantee Unbiasedness?

**No! Unbiasedness is NOT a property of MLEs.**

Maximum likelihood optimizes for making the observed data likely; it makes zero promise that $E[\hat{\theta}_n] = \theta$ for finite sample sizes $n$.
- For a $\text{Normal}(\mu, \sigma^2)$ population, the MLE of $\mu$ is $\hat{\mu} = \bar{X}$ (which happens to be unbiased).
- But the MLE of the variance $\sigma^2$ is:
  $$\hat{\sigma}^2_{\text{MLE}} = \frac{1}{n}\sum_{i=1}^n (X_i - \bar{X})^2$$
  As derived in [[Problem — Sample Variance Bias and Bessel's Correction Derivation]], its expectation is:
  $$E[\hat{\sigma}^2_{\text{MLE}}] = \frac{n-1}{n}\sigma^2 = \sigma^2 - \frac{\sigma^2}{n} \ne \sigma^2$$
  The MLE systematically underestimates the variance! However, because the bias $-\sigma^2/n \to 0$ as $n \to \infty$, the MLE is **asymptotically unbiased**.

---

---

## Example

### Maximum Likelihood Estimator for Exponential Lifetimes

Let $X_1, X_2, \dots, X_n \overset{\text{iid}}{\sim} \operatorname{Exp}(\lambda)$ with PDF $f(x; \lambda) = \lambda e^{-\lambda x}$ for $x \ge 0$ and $\lambda > 0$.

1. **Likelihood Function:**
   $$L(\lambda) = \prod_{i=1}^n f(X_i; \lambda) = \prod_{i=1}^n \lambda e^{-\lambda X_i} = \lambda^n \exp\left(-\lambda \sum_{i=1}^n X_i\right)$$

2. **Log-Likelihood Function:**
   $$\ell(\lambda) = \ln L(\lambda) = n \ln \lambda - \lambda \sum_{i=1}^n X_i$$

3. **Score Equation:**
   Setting the score function $S(\lambda) = \ell'(\lambda)$ to zero:
   $$S(\lambda) = \frac{\partial \ell}{\partial \lambda} = \frac{n}{\lambda} - \sum_{i=1}^n X_i = 0 \implies \frac{n}{\lambda} = \sum_{i=1}^n X_i$$
   Solving for $\lambda$:
   $$\hat{\lambda}_{\text{MLE}} = \frac{n}{\sum_{i=1}^n X_i} = \frac{1}{\bar{X}}$$

4. **Second-Order Condition Check:**
   $$\frac{\partial^2 \ell}{\partial \lambda^2} = -\frac{n}{\lambda^2} < 0 \quad \text{for all } \lambda > 0$$
   The negative second derivative confirms $\hat{\lambda}_{\text{MLE}} = 1/\bar{X}$ is the unique global maximum.

5. **Functional Equivariance:**
   By equivariance, the MLE for the mean lifetime $\tau = \mathbb{E}[X] = 1/\lambda$ is:
   $$\hat{\tau}_{\text{MLE}} = g(\hat{\lambda}_{\text{MLE}}) = \frac{1}{\hat{\lambda}_{\text{MLE}}} = \bar{X}$$

For complete worked derivations across parametric families, including boundary cases where differential calculus fails, see:
- [[Normal Distribution Parameter MLE Derivation Example]]
- [[Uniform Distribution Non-Regular MLE Example]]
- [[Discrete and Continuous Parameter MLE Reference Examples]]
- [[Problem — Sample Variance Bias and Bessel's Correction Derivation]]

---

## Technical Details

### Regularity Conditions and Asymptotic Normality Proof Sketch

1. **Cramér Regularity Conditions for MLE Optimality:**
   - The support $\{x : f(x; \theta) > 0\}$ does not depend on $\theta$ (violating this makes the score equation inapplicable, as in Uniform$(0, \theta)$).
   - The true parameter $\theta_0$ lies in an open interior of the parameter space $\Theta \subset \mathbb{R}^k$.
   - The log-likelihood function is three times continuously differentiable in a neighborhood of $\theta_0$.
   - The Fisher Information $I_1(\theta_0) = \mathbb{E}_\theta\left[-\frac{\partial^2}{\partial \theta^2}\ln f(X; \theta)\right]$ is positive and finite.

2. **Taylor Expansion Proof Sketch of Asymptotic Normality:**
   - Expanding the score $S_n(\hat{\theta}_n) = 0$ around $\theta_0$:
     $$0 = S_n(\hat{\theta}_n) \approx S_n(\theta_0) + S'_n(\theta_0)(\hat{\theta}_n - \theta_0)$$
   - Rearranging terms:
     $$\sqrt{n}(\hat{\theta}_n - \theta_0) \approx \frac{\frac{1}{\sqrt{n}} S_n(\theta_0)}{-\frac{1}{n} S'_n(\theta_0)}$$
   - By the [[Central Limit Theorem]], the numerator converges in distribution: $\frac{1}{\sqrt{n}} S_n(\theta_0) \xrightarrow{d} \mathcal{N}(0, I_1(\theta_0))$ because $\mathbb{E}[S_1(\theta_0)] = 0$ and $\operatorname{Var}(S_1(\theta_0)) = I_1(\theta_0)$.
   - By the [[Law of Large Numbers]], the denominator converges in probability: $-\frac{1}{n} S'_n(\theta_0) \xrightarrow{P} I_1(\theta_0)$.
   - By Slutsky's theorem:
     $$\sqrt{n}(\hat{\theta}_n - \theta_0) \xrightarrow{d} \frac{\mathcal{N}(0, I_1(\theta_0))}{I_1(\theta_0)} = \mathcal{N}\left(0, \frac{1}{I_1(\theta_0)}\right)$$

3. **Non-Regular Boundary Failures:**
   - For $X_i \sim \operatorname{Unif}(0, \theta)$, $\hat{\theta}_{\text{MLE}} = X_{(n)}$. The rate of convergence is super-efficient $O(1/n)$ rather than $O(1/\sqrt{n})$, and $n(\theta - X_{(n)}) \xrightarrow{d} \operatorname{Exp}(1/\theta)$, completely bypassing normal asymptotics (see [[Uniform Distribution Non-Regular MLE Example]]).

---

## Important Properties and Why They Hold

- **Mathematical Rigor:** Satisfies Kolmogorov's probability axioms or standard asymptotic regularity conditions.
- **Convergence / Consistency:** Guarantees stability under large sample limits or repeated independent trials.

---

## Common Mistakes

### Common Mistakes

1. **Attempting to differentiate without checking support:**
   Trying to differentiate the likelihood of $\text{Uniform}(0, \theta)$ with respect to $\theta$ and setting it to zero yields $-n/\theta^{n+1} = 0$, which has no solution!
2. **Ignoring the boundary / support constraints:**
   Omitting indicator functions like $\mathbf{1}_{\{X_{(n)} \le \theta\}}$ when writing down likelihoods for bounded distributions.
3. **Assuming the denominator for sample variance MLE is $n - 1$:**
   The MLE has denominator $n$. The estimator with denominator $n - 1$ ($S^2$) is Bessel's unbiased correction, but it is **not** the MLE.

---

---

## Exam Relevance

### Exam Relevance

MLE is one of the most heavily tested topics in computing and data science examinations. Expected questions include:
1. Setting up likelihood and log-likelihood functions for common distributions (Normal, Bernoulli, Poisson, Exponential, Geometric, Uniform).
2. Solving score equations to find $\hat{\theta}_{\text{MLE}}$.
3. Checking second-order conditions to confirm maximality.
4. Proving whether the derived MLE is unbiased or biased, and computing its exact bias.
5. Invoking the equivariance property to find the MLE of transformed parameters without resolving from scratch.

---
### Examples & Problems

- [[Normal Distribution Parameter MLE Derivation Example]]
- [[Uniform Distribution Non-Regular MLE Example]]
- [[Discrete and Continuous Parameter MLE Reference Examples]]
- [[Problem — Sample Variance Bias and Bessel's Correction Derivation]]

---

---

## Related Concepts

- [[Likelihood and Score Equations]]
- [[Point Estimation]]
- [[Estimator Consistency and Convergence]]
- [[Maximum A Posteriori (MAP) Estimation]]
- [[Bayesian Inference]]

---

---

## Prerequisites

- [[Point Estimation]]
- [[Continuous Probability Distributions]]
- [[Joint and Marginal Distributions]]
- [[Likelihood and Score Equations]]

---

---

## Problems

- [[Problem — Sample Variance Bias and Bessel's Correction Derivation]]

---

## Sources

- [[01 - Sources/Lectures/CSE301_Maximum_Likelihood_Estimators__MLE_.pdf]]
- [[01 - Sources/Lectures/MLE.pdf]]
