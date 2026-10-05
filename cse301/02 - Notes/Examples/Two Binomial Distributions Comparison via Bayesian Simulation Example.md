---
type: example
course: cse301
status: active
order: 55
---

# Two Binomial Distributions Comparison via Bayesian Simulation Example

> 📖 **Reading Order:** Step 55 of 92 | **Module 8:** Bayesian Inference  
> ◄ **Previous:** [[Bernoulli Bayesian Inference with Beta Prior Example]] | ► **Next:** [[Problem — Laplace Rule of Succession and Bayesian Updating]]

---

## Problem

A clinical trial evaluates an experimental drug against a standard control treatment:
- **Control Group:** $n_1 = 50$ patients, with $X_1 = 30$ successful recoveries.
- **Treatment Group:** $n_2 = 50$ patients, with $X_2 = 40$ successful recoveries.

Let $p_1$ and $p_2$ denote the true recovery probabilities in the control and treatment populations, respectively.
We wish to evaluate the treatment benefit:
$$\tau = g(p_1, p_2) = p_2 - p_1$$

1. Formulate the joint posterior distribution $f(p_1, p_2 \mid X_1, X_2)$ assuming independent non-informative flat priors $f(p_1) = 1$ and $f(p_2) = 1$.
2. Explain why analytical calculation of the posterior density of $\tau = p_2 - p_1$ is complicated, and design a Monte Carlo simulation algorithm to evaluate the posterior distribution of $\tau$.
3. Detail how to compute the posterior mean $E[\tau \mid \text{data}]$, the $95\%$ credible interval for $\tau$, and the posterior probability that the treatment is superior: $P(p_2 > p_1 \mid \text{data})$.

---

## Given

- Control data: $n_1 = 50, X_1 = 30 \implies X_1 \sim \text{Binomial}(n_1, p_1)$
- Treatment data: $n_2 = 50, X_2 = 40 \implies X_2 \sim \text{Binomial}(n_2, p_2)$
- Independent priors: $f(p_1, p_2) = f(p_1)f(p_2) = 1 \cdot 1 = 1$ on $[0, 1] \times [0, 1]$

---

## Required

1. Closed-form marginal posterior distributions for $p_1$ and $p_2$.
2. Simulation algorithm for the difference parameter $\tau = p_2 - p_1$.
3. Method for extracting credible intervals and superiority probability $P(\tau > 0 \mid \text{data})$.

---

## Solution

We want uncertainty about a difference, so retain uncertainty in both probabilities. Independent flat priors and independent group data give $p_1\mid\text{data}\sim\operatorname{Beta}(31,21)$ and $p_2\mid\text{data}\sim\operatorname{Beta}(41,11)$.

Draw one value from each posterior and subtract to get a possible $\tau=p_2-p_1$. Repeating that pairwise draw approximates the posterior distribution of the difference. Its sample mean estimates the posterior mean, its 2.5% and 97.5% quantiles give an equal-tailed interval, and the fraction of positive draws estimates $P(\tau>0\mid\text{data})$.

The posterior mean is available exactly by linearity: $41/52-31/52=10/52\approx0.1923$, a difference of 19.23 percentage points. A normal approximation to the difference gives another useful summary, but it must be labeled an approximation rather than a simulated exact posterior quantile or probability. Monte Carlo results also have sampling error and depend on the number of draws.

### Concepts Used

- [[Bayesian Inference]]
- [[Beta-Binomial Conjugate Updating Formula]]
- [[Credible Intervals]]
- Monte Carlo simulation of posterior distributions

---
### Step 1: Joint and Marginal Posterior Distributions
Because the trials are conducted independently and the priors are independent, the joint likelihood factorizes:
$$L(p_1, p_2) = L_1(p_1) \cdot L_2(p_2) = \left[ p_1^{X_1} (1 - p_1)^{n_1 - X_1} \right] \cdot \left[ p_2^{X_2} (1 - p_2)^{n_2 - X_2} \right]$$

The joint posterior density is proportional to the product:
$$f(p_1, p_2 \mid X_1, X_2) \propto f(p_1) L_1(p_1) \cdot f(p_2) L_2(p_2) = f(p_1 \mid X_1) \cdot f(p_2 \mid X_2)$$

Because the joint posterior factorizes into a product of univariate densities, $p_1$ and $p_2$ remain **a posteriori independent**.
Applying the [[Beta-Binomial Conjugate Updating Formula]] with $\alpha = 1, \beta = 1$:
$$p_1 \mid X_1 \sim \text{Beta}(X_1 + 1, n_1 - X_1 + 1) = \text{Beta}(30 + 1, 50 - 30 + 1) = \text{Beta}(31, 21)$$
$$p_2 \mid X_2 \sim \text{Beta}(X_2 + 1, n_2 - X_2 + 1) = \text{Beta}(40 + 1, 50 - 40 + 1) = \text{Beta}(41, 11)$$

---

### Step 2: The Challenge of Analytical Derivation
Finding the exact analytical probability density function of the difference $\tau = p_2 - p_1$ requires computing a convolution integral of two Beta distributions:
$$f_\tau(t) = \int_0^1 f_{p_1}(u) f_{p_2}(u + t) du$$
This integral does not have an elementary closed-form solution. However, in Bayesian statistics, **Monte Carlo sampling makes evaluating functions of parameters trivial**.

---

### Step 3: Monte Carlo Simulation Algorithm
To simulate the posterior distribution of $\tau = p_2 - p_1$:

```
Algorithm: Bayesian Two-Sample Binomial Simulation
Inputs: Sample data (n1, X1), (n2, X2); Monte Carlo draws B (e.g., B = 100,000)

1. For b = 1 to B:
     a. Draw P_{1, b} ~ Beta(31, 21)
     b. Draw P_{2, b} ~ Beta(41, 11)
     c. Compute tau_b = P_{2, b} - P_{1, b}

2. Calculate Point Estimate (Posterior Mean):
     tau_hat_Bayes = (1 / B) * sum_{b=1}^B tau_b

3. Calculate 95% Credible Interval:
     Sort tau_{(1)} <= tau_{(2)} <= ... <= tau_{(B)}
     Lower bound = tau_{(0.025 * B)}
     Upper bound = tau_{(0.975 * B)}

4. Calculate Posterior Probability of Treatment Superiority:
     P(tau > 0 | data) = (1 / B) * sum_{b=1}^B 1_{tau_b > 0}
```

---

### Step 4: Analytical Expectations and Approximate Calculations
We can verify the simulation analytically:
1. **Marginal Posterior Means:**
   $$E[p_1 \mid X_1] = \frac{31}{31 + 21} = \frac{31}{52} \approx 0.5962$$
   $$E[p_2 \mid X_2] = \frac{41}{41 + 11} = \frac{41}{52} \approx 0.7885$$
2. **Exact Posterior Mean of $\tau$:**
   $$E[\tau \mid \text{data}] = E[p_2 - p_1 \mid \text{data}] = E[p_2 \mid \text{data}] - E[p_1 \mid \text{data}]$$
   $$E[\tau \mid \text{data}] = 0.7885 - 0.5962 = 0.1923 \quad (19.23\% \text{ improvement})$$
3. **Marginal Variances:**
   For $X \sim \text{Beta}(\alpha, \beta)$, $\text{Var}(X) = \frac{\alpha\beta}{(\alpha+\beta)^2(\alpha+\beta+1)}$:
   $$\text{Var}(p_1 \mid X_1) = \frac{31 \times 21}{(52)^2 (53)} = \frac{651}{143312} \approx 0.00454$$
   $$\text{Var}(p_2 \mid X_2) = \frac{41 \times 11}{(52)^2 (53)} = \frac{451}{143312} \approx 0.00315$$
4. **Posterior Variance of $\tau$ (by independence):**
   $$\text{Var}(\tau \mid \text{data}) = \text{Var}(p_2) + \text{Var}(p_1) \approx 0.00315 + 0.00454 = 0.00769$$
   $$\text{SD}(\tau \mid \text{data}) = \sqrt{0.00769} \approx 0.0877$$
5. **Posterior Probability that Treatment is Superior:**
   Because both Beta distributions are unimodal and moderately sized ($n = 50$), we can compare the difference $\tau$ with a Gaussian approximation $N(0.1923, 0.0877^2)$:
   $$P(\tau > 0 \mid \text{data}) \approx P\left(Z > \frac{0 - 0.1923}{0.0877}\right) = P(Z > -2.19) = \Phi(2.19) \approx 0.9857 \quad (98.57\%)$$

---

## Result

- Marginal posteriors: $p_1 \sim \text{Beta}(31, 21)$ and $p_2 \sim \text{Beta}(41, 11)$.
- Posterior mean of treatment benefit: $\hat{\tau}_{\text{Bayes}} \approx0.1923$, or 19.23 percentage points.
- $95\%$ interval from a **normal approximation** to the posterior difference: approximately $[0.0204,0.3642]$. This is not an exact beta-difference interval or a reported Monte Carlo quantile.
- Normal-approximation posterior probability that treatment outperforms control: about **$98.57\%$**. Exact integration or a specified simulation can refine this approximation.

---

## Why This Works

In frequentist statistics, evaluating a non-linear or multi-parameter hypothesis $p_2 - p_1$ requires asymptotic two-sample $Z$-tests or complex asymptotic delta methods. In Bayesian statistics, having the full joint posterior distribution allows any function $\tau = g(p_1, p_2)$ to be evaluated directly and exactly by forward Monte Carlo sampling.

---

## A numerical check beyond the normal approximation

Under the specified independent beta posteriors, the difference CDF is

$$F_\tau(t)=\int_0^1 f_{\operatorname{Beta}(31,21)}(x)F_{\operatorname{Beta}(41,11)}(x+t)\,dx,$$

where the second CDF is zero below zero and one above one. Numerical quadrature and inversion give an equal-tailed 95% posterior interval of approximately $(0.01842,0.36197)$ and $P(\tau>0\mid\text{data})\approx0.98477$. These are numerical integration results, separate from the normal approximation and from a Monte Carlo run. The exact posterior mean is $10/52$ and its standard deviation is approximately 0.08769.

## What to carry forward

[[Credible Intervals]] describes the posterior interpretation. Report the prior, numerical method, draw count, and uncertainty when claiming a numerical simulation result.

## Related notes

- [[Credible Intervals]]

## Sources

- [[01 - Sources/Lectures/Bayesian_Inference.pdf]]
