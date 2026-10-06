---
type: concept
course: cse301
status: active
order: 12
---

# Cauchy and Student-t Distributions

> 📖 **Reading Order:** Step 12 of 103 | **Module 2:** Random Variables and Distributions  
> ◄ **Previous:** [[Continuous Probability Distributions]] | ► **Next:** [[St. Petersburg Paradox]]

---

## Starting Point and the Problem

Standard continuous distributions like the [[Continuous Probability Distributions|Gaussian (Normal)]] distribution have exponentially decaying tails ($e^{-x^2/2}$), meaning extreme outliers are vanishingly rare, and all moments (mean, variance, skewness) exist and are finite.

However, in real-world computer systems, robust machine learning, financial risk modeling, and small-sample statistical inference, two crucial phenomena occur:
1. **Ratio of Random Disturbances:** When physical measurements or signals are formed as the ratio of two independent Gaussian disturbances ($X = W / Y$), the resulting distribution exhibits extremely heavy polynomial tails ($1/x^2$).
2. **Inference with Unknown Variance:** When estimating a normal population mean from a small sample $n$ where the true variance $\sigma^2$ is unknown and must be estimated via the sample variance $S^2$, the standardized test statistic does not follow a normal distribution, but rather a heavier-tailed distribution.

These requirements lead directly to the **Cauchy Distribution** and **Student's $t$-Distribution**.

---

## Developing the Idea

### 1. Constructing the Cauchy Distribution from Standard Normals
Let $W, Y \overset{\text{i.i.d.}}{\sim} \mathcal{N}(0, 1)$ be two independent standard normal random variables. Define the ratio:
$$X = \frac{W}{Y}$$

Because $Y$ can take values arbitrarily close to $0$ with non-zero probability density, dividing by $Y$ produces massive values of $X$ with high frequency. 

Using the ratio density formula $f_X(x) = \int_{-\infty}^\infty |y| f_W(xy) f_Y(y) \, dy$:
$$f_X(x) = \int_{-\infty}^\infty |y| \left(\frac{1}{\sqrt{2\pi}} e^{-(xy)^2/2}\right) \left(\frac{1}{\sqrt{2\pi}} e^{-y^2/2}\right) dy = \frac{1}{2\pi} \int_{-\infty}^\infty |y| e^{-y^2(1 + x^2)/2} dy$$
Using the substitution $u = y^2(1 + x^2)/2$, $du = y(1 + x^2)dy$:
$$f_X(x) = \frac{1}{\pi (1 + x^2)}, \quad x \in \mathbb{R}$$

This is the **Standard Cauchy Distribution**. Notice that its density decays as $O(1/x^2)$ rather than exponentially as $O(e^{-x^2})$.

### 2. Constructing Student's $t$-Distribution
In statistical inference, if $X_1, \dots, X_n \overset{\text{i.i.d.}}{\sim} \mathcal{N}(\mu, \sigma^2)$ with unknown $\sigma$, we estimate $\sigma$ using the sample variance $S = \sqrt{\frac{1}{n-1}\sum (X_i - \bar{X})^2}$.

The normalized sample mean is:
$$T = \frac{\bar{X} - \mu}{S / \sqrt{n}} = \frac{\frac{\bar{X} - \mu}{\sigma / \sqrt{n}}}{\frac{S}{\sigma}} = \frac{Z}{\sqrt{V / \nu}}$$
where:
- $Z = \frac{\bar{X} - \mu}{\sigma / \sqrt{n}} \sim \mathcal{N}(0, 1)$ is standard normal,
- $V = \frac{(n-1)S^2}{\sigma^2} \sim \chi^2(\nu)$ follows a Chi-Square distribution with $\nu = n - 1$ degrees of freedom,
- $Z$ and $V$ are statistically independent.

This ratio defines **Student's $t$-distribution** with $\nu$ degrees of freedom.

---

## Definitions and Formulas

### Standard Cauchy Distribution: $\operatorname{Cauchy}(x_0, \gamma)$
- **Location parameter:** $x_0 \in \mathbb{R}$ (median / mode).
- **Scale parameter:** $\gamma > 0$ (half-width at half-maximum).
- **PDF:**
  $$f_X(x) = \frac{1}{\pi \gamma \left[ 1 + \left(\frac{x - x_0}{\gamma}\right)^2 \right]}, \quad x \in \mathbb{R}$$
- **Standard Cauchy ($\operatorname{Cauchy}(0, 1)$):**
  $$f_X(x) = \frac{1}{\pi (1 + x^2)}, \quad F_X(x) = \frac{1}{\pi}\arctan(x) + \frac{1}{2}$$

### Student's $t$-Distribution: $t(\nu)$
- **Parameter:** $\nu > 0$ degrees of freedom.
- **Representation:** $T = \frac{Z}{\sqrt{V / \nu}}$ where $Z \sim \mathcal{N}(0, 1)$ and $V \sim \chi^2(\nu)$ independent.
- **PDF:**
  $$f_T(t) = \frac{\Gamma\left(\frac{\nu + 1}{2}\right)}{\sqrt{\nu\pi} \, \Gamma\left(\frac{\nu}{2}\right)} \left(1 + \frac{t^2}{\nu}\right)^{-\frac{\nu + 1}{2}}, \quad t \in \mathbb{R}$$

---

## Fundamental Properties and Deep Insights

### 1. The Undefined Mean of the Cauchy Distribution
Does the Cauchy distribution have an expected value?
$$\mathbb{E}[X] = \int_{-\infty}^\infty x f(x) \, dx = \frac{1}{\pi} \int_{-\infty}^\infty \frac{x}{1 + x^2} \, dx$$
For an integral $\int_{-\infty}^\infty g(x) dx$ to exist, the integral of absolute values must converge:
$$\int_{-\infty}^\infty \frac{|x|}{1 + x^2} \, dx = 2 \int_0^\infty \frac{x}{1 + x^2} \, dx = \left[ \ln(1 + x^2) \right]_0^\infty = \infty$$
Because $\int_0^\infty x f(x) dx = +\infty$ and $\int_{-\infty}^0 x f(x) dx = -\infty$, the form is $\infty - \infty$.
**The expectation $\mathbb{E}[X]$ does not exist (undefined)!**

### 2. Failure of the Law of Large Numbers (LLN)
If $X_1, X_2, \dots, X_n \overset{\text{i.i.d.}}{\sim} \operatorname{Cauchy}(0, \gamma)$, what is the distribution of their sample average $\bar{X}_n = \frac{1}{n} \sum_{i=1}^n X_i$?

Using characteristic functions:
$$\phi_X(t) = \mathbb{E}[e^{itX}] = e^{-\gamma |t|}$$
The characteristic function of the average is:
$$\phi_{\bar{X}_n}(t) = \left( \phi_X\left(\frac{t}{n}\right) \right)^n = \left( e^{-\gamma |t| / n} \right)^n = e^{-\gamma |t|}$$

**Remarkable Conclusion:**
The sample average $\bar{X}_n$ of 1,000,000 Cauchy random variables has the **exact same Cauchy distribution as a single observation**!
Averaging does not reduce variability or concentrate mass around the median. This provides the canonical counterexample to both the [[Law of Large Numbers]] and the [[Central Limit Theorem]].

### 3. The Relationship Between Student's $t$ and Cauchy
Notice the power in the Student's $t$ density:
- If $\nu = 1$:
  $$f_T(t) = \frac{\Gamma(1)}{\sqrt{\pi}\Gamma(1/2)} \left(1 + t^2\right)^{-1} = \frac{1}{\pi(1 + t^2)}$$
  **Student's $t$ with $1$ degree of freedom is identically the Standard Cauchy distribution!**
- If $\nu \to \infty$:
  Using the standard limit $\lim_{\nu \to \infty} \left(1 + \frac{t^2}{\nu}\right)^{-\nu/2} = e^{-t^2/2}$:
  $$\lim_{\nu \to \infty} f_T(t) = \frac{1}{\sqrt{2\pi}} e^{-t^2/2}$$
  **Student's $t$ converges in distribution to the Standard Normal $\mathcal{N}(0, 1)$!**

---

## Comparison Summary Table

| Feature | Standard Cauchy | Student's $t$ ($\nu$ d.o.f.) | Standard Normal $\mathcal{N}(0, 1)$ |
|---|---|---|---|
| **Support** | $\mathbb{R}$ | $\mathbb{R}$ | $\mathbb{R}$ |
| **Tails** | Heavy: $O(t^{-2})$ | Polynomial: $O(t^{-(\nu+1)})$ | Light: $O(e^{-t^2/2})$ |
| **Mean** | Undefined | $0$ (for $\nu > 1$; undefined if $\nu = 1$) | $0$ |
| **Variance** | Undefined | $\frac{\nu}{\nu - 2}$ (for $\nu > 2$; $\infty$ if $\nu \le 2$) | $1$ |
| **LLN Applies?** | ❌ No | ✅ Yes (if $\nu > 1$) | ✅ Yes |
| **CLT Applies?** | ❌ No | ✅ Yes (if $\nu > 2$) | ✅ Yes |

---

## Common Mistakes

- **Claiming Cauchy Mean is Zero by Symmetry:** Using the Cauchy Principal Value ($\lim_{R \to \infty} \int_{-R}^R x f(x) dx = 0$) to claim the mean is $0$. Under Lebesgue measure and Kolmogorov probability axioms, $\mathbb{E}[X]$ requires $\mathbb{E}[|X|] < \infty$. The mean is mathematically undefined.
- **Applying CLT to Small Samples with Unknown Variance:** Using standard normal critical values $z_{\alpha/2}$ when $n$ is small and $\sigma$ is estimated from data. One must use Student's $t$ quantiles $t_{\alpha/2, n-1}$.

---

## Exam Relevance

In CSE 301, questions involving Cauchy and Student's $t$ test:
1. **Ratio Transformations:** Proving $W / Y \sim \operatorname{Cauchy}(0, 1)$ when $W, Y \overset{\text{i.i.d.}}{\sim} \mathcal{N}(0, 1)$.
2. **Counterexamples to LLN/CLT:** Explaining why the sample mean $\bar{X}_n$ of Cauchy variables fails to concentrate.
3. **Degrees of Freedom & Asymptotics:** Identifying that $t(1) \equiv \operatorname{Cauchy}$ and $\lim_{\nu \to \infty} t(\nu) = \mathcal{N}(0, 1)$.

---

## Related Concepts

- [[Continuous Probability Distributions]] — Standard continuous densities (Normal, Exponential, Uniform).
- [[Law of Large Numbers]] — Finite expectation requirement for empirical convergence.
- [[Central Limit Theorem]] — Finite variance requirement for asymptotic normality.
- [[Point Estimation]] — Sample mean and sample variance estimators.

---

## Prerequisites

- [[Continuous Probability Distributions]] — Continuous PDFs, Gaussian distribution, change of variables.
- [[Joint and Marginal Distributions]] — Ratio distributions of bivariate random vectors.

---

## Sources

- [[cse301/01 - Sources/Lectures/HT Final Note - Suchi.pdf|HT Final Note: Lecture 13, 17]]
- [[cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf|Lecture Notes Complete: Lecture 21 (p. 63, Misc Distributions)]]
- [[cse301/01 - Sources/Lectures/ITP.pdf|Textbook: Introduction to Probability (Blitzstein & Hwang, Section 7.5: Cauchy Distribution)]]
