---
type: concept
course: cse301
status: active
order: 58
---

# p-Values and Significance

> 📖 **Reading Order:** Step 58 of 92 | **Module 9:** Hypothesis Testing  
> ◄ **Previous:** [[Hypothesis Testing Framework]] | ► **Next:** [[Wald Test Statistic]]

---

---

## Starting Point and the Problem

Probability and statistical inference model uncertainty in physical and computer systems. When analyzing stochastic phenomena related to p-Values and Significance, naive counting or deterministic approximations fail. We establish a formal mathematical foundation to quantify outcomes and evaluate expectations rigorously.

---

## Developing the Idea

Reporting a binary verdict ("reject at $\alpha = 0.05$" or "fail to reject") throws away valuable evidentiary nuance:
- Did you reject with overwhelming, undeniable evidence ($p = 0.00001$)?
- Or did you barely scrape past the arbitrary threshold ($p = 0.049$)?

Imagine the critical rejection cutoff $c_\alpha$ as a sliding high-jump bar:
1. When you demand a very strict significance level (tiny $\alpha = 0.001$), the required cutoff $c_\alpha$ is set extremely high.
2. As you relax $\alpha$ (making the rejection region larger), the bar $c_\alpha$ slides lower and lower.
3. The **$p$-value is the exact point at which the sliding bar touches your observed statistic $t_{\text{obs}}$**.

```
  Decision Rule:
  • If p ≤ α  ===> Reject H₀  (observed data are sufficiently rare under H₀)
  • If p > α  ===> Retain H₀  (observed data are plausibly consistent with H₀)
```

---

---

## Definition

The **$p$-value** is the probability, computed assuming the null hypothesis $H_0$ is true, of observing a test statistic at least as extreme as (or more extreme than) the value actually observed in the sample data.

Formally, let $T(\mathbf{X})$ be a test statistic where large values provide evidence against $H_0$, and let $t_{\text{obs}} = T(\mathbf{x})$ denote the realized value calculated from the observed dataset $\mathbf{x}$. The $p$-value is defined as:

$$p = \sup_{\theta \in \Theta_0} P_\theta\left(T(\mathbf{X}) \ge t_{\text{obs}}\right)$$

### Alternative Operational Definition
The $p$-value is the **smallest significance level $\alpha$** at which a hypothesis test would reject the null hypothesis $H_0$:
$$p = \inf\big\{\alpha \in (0, 1) : T(\mathbf{x}) \in R_\alpha\big\}$$

---

---

## How It Works

### Standard Interpretation Scale

While modern statistics encourages reporting exact numerical $p$-values rather than binary thresholds, the following scientific scale is widely recognized:

| $p$-value Range | Weight of Evidence Against $H_0$ |
|---|---|
| $p < 0.01$ | **Very Strong Evidence** against $H_0$ |
| $0.01 \le p < 0.05$ | **Strong Evidence** against $H_0$ |
| $0.05 \le p < 0.10$ | **Weak / Marginal Evidence** against $H_0$ |
| $p \ge 0.10$ | **Little to No Evidence** against $H_0$ (consistent with chance) |

---
### How to Compute the $p$-Value

### 1. One-Sided Right-Tail Test
For $H_0: \theta \le \theta_0$ vs $H_1: \theta > \theta_0$:
$$p = P_{\theta_0}\left(T(\mathbf{X}) \ge t_{\text{obs}}\right) = 1 - F_{T}(t_{\text{obs}})$$

### 2. One-Sided Left-Tail Test
For $H_0: \theta \ge \theta_0$ vs $H_1: \theta < \theta_0$:
$$p = P_{\theta_0}\left(T(\mathbf{X}) \le t_{\text{obs}}\right) = F_{T}(t_{\text{obs}})$$

### 3. Two-Sided Symmetric Test (e.g., Wald Test)
For $H_0: \theta = \theta_0$ vs $H_1: \theta \ne \theta_0$, where the null distribution of $W$ is standard normal $N(0, 1)$ and $w_{\text{obs}} = \frac{\hat{\theta} - \theta_0}{\widehat{\text{se}}}$:
$$p = P(\lvert Z \rvert \ge \lvert w_{\text{obs}} \rvert) = 2 \cdot P(Z \le -\lvert w_{\text{obs}} \rvert) = 2\Phi(-\lvert w_{\text{obs}} \rvert)$$

---
### Crucial Fallacies and Misinterpretations

The American Statistical Association (ASA) highlighted that the $p$-value is one of the most frequently misunderstood concepts in all of science.

### ❌ Fallacy 1: "The $p$-value is the probability that $H_0$ is true."
**Reality:** 
$$p \ne P(H_0 \mid \text{data})$$
The $p$-value is computed **assuming $H_0$ is $100\%$ true from the beginning**: $P(\text{data} \mid H_0)$. It is a statement about the data, not about the hypothesis. Finding $P(H_0 \mid \text{data})$ requires [[Bayesian Inference]] with a prior distribution.

### ❌ Fallacy 2: "$1 - p$ is the probability that the alternative $H_1$ is true."
**Reality:** 
$$1 - p \ne P(H_1 \mid \text{data})$$
A tiny $p$-value ($p = 0.001$) does **not** mean there is a $99.9\%$ chance that your alternative discovery is correct.

### ❌ Fallacy 3: "$p > 0.05$ proves that the null hypothesis is true."
**Reality:** 
A large $p$-value simply means the sample size was too small or the variance was too large to detect a difference. It indicates **insufficient evidence**, never proof of equality.

---
### The Null Distribution of the $p$-Value

An extraordinary mathematical property of the $p$-value:
> **Theorem:** 
> When the null hypothesis $H_0$ is true and the test statistic $T$ has a continuous distribution, the random variable $P$ is **uniformly distributed on the interval $(0, 1)$**:
> $$P \sim \text{Uniform}(0, 1)$$

### Proof
Under $H_0$, let $F$ be the continuous CDF of $T$. 
The $p$-value is $P = 1 - F(T)$.
For any $u \in (0, 1)$:
$$P_{H_0}(P \le u) = P_{H_0}(1 - F(T) \le u) = P_{H_0}(F(T) \ge 1 - u)$$
By the Probability Integral Transform, $F(T) \sim \text{Uniform}(0, 1)$, so:
$$P_{H_0}(F(T) \ge 1 - u) = 1 - (1 - u) = u$$
Since $P(P \le u) = u$, $P \sim \text{Uniform}(0, 1) \quad \blacksquare$.

This beautiful result explains why setting a threshold $\alpha = 0.05$ guarantees that the Type I error rate is exactly $5\%$: under $H_0$, $P(P \le 0.05) = 0.05$.

---

---

## Example

### One-Sided vs. Two-Sided $p$-Value Calculation

Suppose a software benchmark evaluates runtime improvement over a baseline system, yielding a standard normal test statistic $Z = 2.15$.

1. **One-Sided Upper-Tail Test ($H_0: \mu \le \mu_0$ vs $H_1: \mu > \mu_0$):**
   The $p$-value represents only the area in the upper tail:
   $$p_{\text{one-sided}} = P(Z \ge 2.15) = 1 - \Phi(2.15) = 1 - 0.9842 = 0.0158$$
   Since $p = 0.0158 < 0.05$, $H_0$ is rejected.

2. **Two-Sided Test ($H_0: \mu = \mu_0$ vs $H_1: \mu \ne \mu_0$):**
   Because deviations in either direction provide evidence against $H_0$:
   $$p_{\text{two-sided}} = 2 \cdot P(Z \ge |2.15|) = 2(0.0158) = 0.0316$$

Notice that $p_{\text{two-sided}} = 2 \cdot p_{\text{one-sided}}$. A result with $Z = 1.80$ gives $p_{\text{one-sided}} = 0.0359$ (significant at $\alpha = 0.05$) but $p_{\text{two-sided}} = 0.0718$ (not significant). Choosing a one-sided test after inspecting the sample data is a form of data dredging ($p$-hacking) that doubles the true Type I error rate.

For empirical resampling and permutation $p$-value algorithms, see [[Toy Permutation Test Example]] and [[Problem — Comparing Prediction Algorithms via Paired Wald Test]].

---

## Technical Details

### Probability Integral Transform, Discrete Conservatism, and Lindley's Paradox

1. **Continuous vs. Discrete Null Distributions:**
   - When the test statistic $T$ is continuous, the Probability Integral Transform guarantees $P \sim \operatorname{Unif}(0, 1)$ exactly under $H_0$. Thus $P(P \le \alpha) = \alpha$.
   - When $T$ is discrete (e.g., Binomial counts or Fisher's Exact Test), $P$ has jump discontinuities. Consequently, $P(P \le \alpha) \le \alpha$. Discrete tests are strictly **conservative**, meaning the empirical Type I error rate is strictly less than nominal $\alpha$.
2. **Behavior under the Alternative Hypothesis ($H_1$):**
   - Under $H_1$, the cumulative distribution of $P$ is strictly concave, and its density $f_P(p \mid H_1)$ spikes sharply near $p = 0$.
   - The probability $P(P \le \alpha \mid H_1)$ corresponds exactly to the statistical power $1 - \beta$.
3. **Lindley's Paradox (Jeffreys-Lindley Paradox):**
   - In massive datasets ($n \gg 1$), a realization can simultaneously yield a frequentist $p$-value of $p = 0.01$ (rejecting $H_0$) and a Bayesian posterior probability $P(H_0 \mid \mathbf{x}) > 0.95$ (strongly supporting $H_0$).
   - This occurs because standard errors shrink to 0 as $O(1/\sqrt{n})$, making tiny trivial differences statistically distinguishable even when the point null is far more parsimonious than a diffuse prior.

---

## Important Properties and Why They Hold

- **Mathematical Rigor:** Satisfies Kolmogorov's probability axioms or standard asymptotic regularity conditions.
- **Convergence / Consistency:** Guarantees stability under large sample limits or repeated independent trials.

---

## Common Mistakes

- **Transposition Fallacy:** Believing $p = P(H_0 \mid \text{data})$ rather than $P(\text{extreme data} \mid H_0)$.
- **Switching Hypotheses Post-Hoc:** Selecting a one-sided test after observing that the sample statistic landed in that direction (inflates false positive rate).
- **Treating $0.05$ as an Ontological Boundary:** Thinking $p = 0.049$ proves an effect while $p = 0.051$ proves no effect.

---

## Exam Relevance

Tested regularly in CSE 301 midterms and finals through derivations, numerical probability calculations, and statistical hypothesis testing via [[Hypothesis Testing Framework]] and [[Wald Test Statistic]].

---

## Related Concepts

- [[Hypothesis Testing Framework]]
- [[Wald Test Statistic]]
- [[Multiple Testing and False Discovery Rate]]
- [[Permutation Test Algorithm]]
- [[Toy Permutation Test Example]]

---

---

## Prerequisites

- [[Hypothesis Testing Framework]]
- [[Continuous Probability Distributions]]
- [[Central Limit Theorem]]

---

## Problems

- [[Problem — Comparing Prediction Algorithms via Paired Wald Test]]

---

## Sources

- [[01 - Sources/Lectures/CSE301_Hypothesis_Test.pdf]]
