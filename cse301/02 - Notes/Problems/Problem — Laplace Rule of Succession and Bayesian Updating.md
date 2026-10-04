---
type: problem
course: cse301
status: active
order: 56
---

# Problem — Laplace Rule of Succession and Bayesian Updating

> 📖 **Reading Order:** Step 56 of 92 | **Module 8:** Bayesian Inference  
> ◄ **Previous:** [[Two Binomial Distributions Comparison via Bayesian Simulation Example]] | ► **Next:** [[Hypothesis Testing Framework]]

---

---

## Problem

An automated safety verification framework evaluates an autonomous vehicle control module across $n = 5$ independent critical road simulation tests. All $5$ tests pass without incident ($s = 5$ successes, $0$ failures). Let $p \in [0, 1]$ be the unknown true probability of passing a critical test.

1. Compute the classical Maximum Likelihood Estimate (MLE) $\hat{p}_{\text{MLE}}$. What does the MLE predict for the probability that the next (6th) test fails? Explain why this prediction is hazardous in safety-critical engineering.
2. Assuming an initial non-informative flat prior $p \sim \text{Uniform}(0, 1) \equiv \text{Beta}(1, 1)$, derive the posterior distribution $f(p \mid X_1 = \dots = X_5 = 1)$.
3. Compute the **posterior predictive probability** that the 6th test will pass:
   $$P(X_6 = 1 \mid X_1 = 1, \dots, X_5 = 1)$$
   Prove that this probability equals the posterior mean of $p$.
4. State the general formula for **Laplace's Rule of Succession** and explain how it prevents the "zero-probability" trap.
5. Contrast the behavior of the Bayesian predictive probability with the MLE as $n \to \infty$ with all successes.

---

---

## Given

- Sample: $n = 5$ independent Bernoulli trials
- Observed successes: $s = 5$, failures: $n - s = 0$
- Prior: $p \sim \text{Beta}(1, 1)$

---

---

## Required

1. $\hat{p}_{\text{MLE}}$ and its predicted failure probability.
2. Posterior distribution $f(p \mid \mathbf{x})$.
3. Rigorous derivation of posterior predictive probability $P(X_{n+1} = 1 \mid \mathbf{X})$.
4. Laplace's Rule of Succession formula $\frac{s+1}{n+2}$.
5. Asymptotic comparison.

---

---

## Concepts Tested

- [[Bayesian Inference]]
- [[Maximum Likelihood Estimation]]
- [[Beta-Binomial Conjugate Updating Formula]]
- Posterior Predictive Distribution

---

---

## Prerequisites

- Law of Total Probability for continuous conditioning
- Properties of the Beta distribution and Gamma function

---

---

## Question Type

- Theoretical Proof & Safety-Critical Application

---

---

## Solution

### Understanding the Situation
Interpret the given sample space, random variables, and event conditions.

### Developing the Key Idea
Select the governing probabilistic principle (e.g. Chapman-Kolmogorov, Adam's Law, Central Limit Theorem, or likelihood maximization) and verify that conditions hold.

### Working Through the Solution
### Solution

### 1. Frequentist MLE and the Zero-Probability Trap
The likelihood function is:
$$L(p) = p^5 (1 - p)^0 = p^5$$
To maximize $p^5$ over $p \in [0, 1]$, we choose:
$$\hat{p}_{\text{MLE}} = 1.0 \quad (100\%)$$

Under the plug-in MLE model, the estimated probability of failure on the 6th test is:
$$\hat{P}(X_6 = 0) = 1 - \hat{p}_{\text{MLE}} = 1 - 1.0 = 0$$

**Why this is dangerous in safety engineering:**
Claiming $P(\text{failure}) = 0$ asserts that vehicle failure is physically impossible simply because five trials passed. In reality, $n = 5$ is a tiny sample size. The true reliability could easily be $p = 0.80$ (a catastrophic $20\%$ failure rate), under which five consecutive successes occur with probability $0.8^5 = 0.3277$ (roughly a 1 in 3 chance). The MLE severely overfits to small samples.

---

### 2. Posterior Distribution Derivation
Prior: $f(p) = 1$ for $p \in [0, 1] \iff \text{Beta}(\alpha = 1, \beta = 1)$.
Likelihood: $L(p) = p^5$.
Applying the [[Beta-Binomial Conjugate Updating Formula]]:
$$\alpha_{\text{post}} = \alpha + s = 1 + 5 = 6$$
$$\beta_{\text{post}} = \beta + n - s = 1 + 0 = 1$$
$$p \mid \mathbf{X} \sim \text{Beta}(6, 1)$$

The explicit posterior probability density function is:
$$f(p \mid \mathbf{X}) = \frac{\Gamma(6 + 1)}{\Gamma(6)\Gamma(1)} p^{6 - 1} (1 - p)^{1 - 1} = \frac{6!}{5! 0!} p^5 = 6 p^5, \quad 0 \le p \le 1$$

---

### 3. Posterior Predictive Probability
To predict the outcome of the unobserved 6th trial $X_6$, we integrate out our uncertainty about $p$ using the continuous Law of Total Probability:
$$P(X_6 = 1 \mid \mathbf{X}) = \int_0^1 P(X_6 = 1 \mid p, \mathbf{X}) f(p \mid \mathbf{X}) dp$$

Since $X_6$ is conditionally independent of past trials given $p$, $P(X_6 = 1 \mid p, \mathbf{X}) = P(X_6 = 1 \mid p) = p$:
$$P(X_6 = 1 \mid \mathbf{X}) = \int_0^1 p \cdot f(p \mid \mathbf{X}) dp = E[p \mid \mathbf{X}]$$

> **Key Result:** The posterior predictive probability of a future success is **identically equal to the posterior mean** of the parameter $p$.

Evaluating the integral with $f(p \mid \mathbf{X}) = 6 p^5$:
$$P(X_6 = 1 \mid \mathbf{X}) = \int_0^1 p \cdot 6 p^5 dp = 6 \int_0^1 p^6 dp = 6 \left[ \frac{p^7}{7} \right]_0^1 = \frac{6}{7} \approx 0.8571 \quad (85.71\%)$$

The posterior predictive probability of **failure** on the next trial is:
$$P(X_6 = 0 \mid \mathbf{X}) = 1 - \frac{6}{7} = \frac{1}{7} \approx 0.1429 \quad (14.29\%)$$
Instead of an unrealistic $0\%$, the Bayesian approach rationally assigns a $14.3\%$ probability to the possibility of failure!

---

### 4. Laplace's Rule of Succession
For general $n$ trials with $s$ successes under a flat $\text{Uniform}(0, 1) \equiv \text{Beta}(1, 1)$ prior:
$$P(X_{n+1} = 1 \mid s \text{ successes in } n \text{ trials}) = \frac{s + 1}{n + 2}$$

- **The Pseudocount Interpretation:** 
  The formula effectively adds $2$ "virtual" trials to the experiment: $1$ virtual success and $1$ virtual failure.
- When $s = 0$ successes in $n$ trials, the predicted success rate is $\frac{1}{n+2} > 0$.
- When $s = n$ successes in $n$ trials, the predicted failure rate is $\frac{1}{n+2} > 0$.
- It eliminates the zero-probability problem (a single unseen event having zero estimated probability), which is foundational in natural language processing (additive / Laplace smoothing).

---

### 5. Large-Sample Limit ($n \to \infty$)
If all $n$ trials are successful ($s = n$):
$$\lim_{n \to \infty} P(X_{n+1} = 1 \mid \mathbf{X}) = \lim_{n \to \infty} \frac{n + 1}{n + 2} = 1.0$$
$$\lim_{n \to \infty} \hat{p}_{\text{MLE}} = 1.0$$
As $n \to \infty$, the Bayesian predictive probability approaches the MLE. For large datasets, the evidence of hundreds of flawless runs rightfully overcomes the prior skepticism.

---
### Summary Comparison

| Concept | Prediction on 6th Trial ($n=5, s=5$) | Prediction on 101st Trial ($n=100, s=100$) |
|---|---|---|
| **Frequentist MLE** | $P(\text{Pass}) = 100\%$, $P(\text{Fail}) = 0\%$ | $P(\text{Pass}) = 100\%$, $P(\text{Fail}) = 0\%$ |
| **Bayesian (Laplace)** | $P(\text{Pass}) = \frac{6}{7} \approx 85.7\%$, $P(\text{Fail}) \approx 14.3\%$ | $P(\text{Pass}) = \frac{101}{102} \approx 99.02\%$, $P(\text{Fail}) \approx 0.98\%$ |

---
### Related Concepts

- [[Bayesian Inference]]
- [[Beta-Binomial Conjugate Updating Formula]]
- [[Maximum Likelihood Estimation]]

---

### Result and Interpretation
The final analytical solution and numerical metrics are rigorously verified against probability axioms.

---

## Reusable Insight

Always decompose complex event probabilities by conditioning on a partition of the sample space (Law of Total Probability), or by writing indicator random variables to exploit linearity of expectation.

---

## Common Mistakes

- Conflating correlation with causation or independence.
- Misapplying the Central Limit Theorem when the variance of the underlying distribution is infinite (e.g. Cauchy).

---

## Exam Pattern

Standard BUET CSE 301 final exam question testing probability bounds, Markov chain stationarity, or statistical parameter estimation.

---

## Related Problems

- [[Problem — Birthday Collisions and Approximation]]
- [[Problem — Four-Day Weather Forecast]]

---

## Related Concepts

- [[Random Variables and Probability Distributions]]
- [[Law of Total Probability and Bayes' Rule]]

---

## Source

- [[01 - Sources/Lectures/Bayesian_Inference.pdf]]
