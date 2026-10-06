---
type: formula
course: cse301
status: active
order: 60
---

# Pearson's Chi-Square Goodness-of-Fit Test

> 📖 **Reading Order:** Step 60 of 92 | **Module 9:** Hypothesis Testing  
> ◄ **Previous:** [[Wald Test Statistic]] | ► **Next:** [[Permutation Test Algorithm]]

---

---

## The Question and Earlier Knowledge

What analytical relationship or closed-form expectation governs Pearson's Chi-Square Goodness-of-Fit Test, and how can we compute it directly from constituent probability terms? In complex probability models, calculating probabilities or moments directly is often intractable without decomposing expectations across conditioning partitions or inequalities.

---

## Developing the Formula

By decomposing joint distributions into conditional components, expanding algebraic products, or applying geometric series sums, Pearson's Chi-Square Goodness-of-Fit Test compresses complex probabilistic reasoning into a clean, reusable formula.

---

## Formula

**Pearson's Chi-Square ($\chi^2$) Goodness-of-Fit Test** evaluates whether an observed categorical frequency distribution differs significantly from a hypothesized theoretical distribution.

Let an observation vector $\mathbf{X} = (X_1, X_2, \dots, X_k)$ follow a **Multinomial distribution** with total sample size $n = \sum_{j=1}^k X_j$ and category probabilities $\mathbf{p} = (p_1, p_2, \dots, p_k)$ where $\sum_{j=1}^k p_j = 1$.

To test the simple null hypothesis:
$$H_0: p_1 = p_{01}, \; p_2 = p_{02}, \; \dots, \; p_k = p_{0k} \quad \text{versus} \quad H_1: p_j \ne p_{0j} \text{ for at least one } j$$

### 1. Test Statistic
$$V = \sum_{j=1}^k \frac{(X_j - n p_{0j})^2}{n p_{0j}} = \sum_{j=1}^k \frac{(O_j - E_j)^2}{E_j}$$

where:
- $O_j = X_j$ is the **observed count** in category $j$.
- $E_j = n p_{0j}$ is the **expected count** in category $j$ under $H_0$.

### 2. Asymptotic Null Distribution
Under the null hypothesis $H_0$, as $n \to \infty$:
$$V \xrightarrow{d} \chi^2_{k - 1}$$
The statistic converges in distribution to a **Chi-Square distribution with $k - 1$ degrees of freedom**.

### 3. Decision Rule
For a significance level $\alpha$:
$$\text{Reject } H_0 \iff V > \chi^2_{k - 1, \alpha}$$
where $\chi^2_{k - 1, \alpha}$ is the upper $\alpha$ critical value of the $\chi^2$ distribution with $k - 1$ degrees of freedom.

---

---

## Variables

| Symbol | Meaning | Role |
|---|---|---|
| $k$ | Number of mutually exclusive categories | Degrees of freedom parameter ($k - 1$) |
| $n$ | Total sample size $\sum_{j=1}^k X_j$ | Sample size scaling |
| $O_j = X_j$ | Observed frequency count in bin $j$ | Empirical data |
| $p_{0j}$ | Hypothesized theoretical probability | Specified by $H_0$ |
| $E_j = n p_{0j}$ | Expected frequency count in bin $j$ | Theoretical baseline |
| $V$ | Pearson test statistic | Aggregate standardized discrepancy |
| $df = k - 1$ | Degrees of freedom | Accounts for constraint $\sum O_j = n$ |

---

---

## Conditions

1. **Independent Observations:** The $n$ trials must be independent.
2. **Mutually Exclusive & Exhaustive:** Every observation must fall into exactly one of the $k$ categories.
3. **Cochran's Sample Size Rule:**
   - All expected cell counts must satisfy $E_j \ge 1$.
   - At least $80\%$ of expected cell counts must satisfy $E_j \ge 5$.
   If expected cell counts are too small, adjacent categories should be collapsed, or an exact multinomial test used.

---

---

## Intuition

### Intuition

Each term in the sum:
$$\frac{(O_j - E_j)^2}{E_j} = \left( \frac{O_j - E_j}{\sqrt{E_j}} \right)^2$$
is the squared standardized deviation of category $j$. Since the variance of a Binomial count under $H_0$ is $\text{Var}(X_j) = n p_j (1 - p_j) \approx n p_j = E_j$ when $k$ is large, each term behaves approximately like the square of a standard normal random variable:
$$\left(\frac{O_j - E_j}{\sqrt{E_j}}\right)^2 \approx Z_j^2 \sim \chi^2_1$$

### Why $k - 1$ Degrees of Freedom?
Although there are $k$ categories, the counts are constrained to sum to $n$:
$$\sum_{j=1}^k O_j = n \quad \text{and} \quad \sum_{j=1}^k E_j = n \implies \sum_{j=1}^k (O_j - E_j) = 0$$
Once the deviations of the first $k - 1$ categories are known, the deviation of the $k$-th category is completely fixed. This single linear constraint removes one degree of freedom, yielding $k - 1$.

---
### Connection to the Chi-Square Distribution

Recall the definition of the $\chi^2_m$ distribution:
> If $Z_1, Z_2, \dots, Z_m \overset{\text{iid}}{\sim} N(0, 1)$, then the sum of their squares follows a Chi-Square distribution with $m$ degrees of freedom:
> $$\sum_{i=1}^m Z_i^2 \sim \chi^2_m$$
> - Mean: $E[\chi^2_m] = m$
> - Variance: $\text{Var}(\chi^2_m) = 2m$

Because $V$ is asymptotically the sum of $k - 1$ independent standard normal squares, under $H_0$ the expected value of $V$ is:
$$E[V] \approx k - 1$$
If $V \approx k - 1$, the observed data match theoretical expectations. If $V \gg k - 1$, the discrepancy is far too large to explain by chance alone.

---

---

## Derivation

Derived by applying definition of expectation, interchanging summation/integrals via Fubini's theorem, and collecting terms.

---

## Example

### Example: Rolling a Die for Fairness

A die is rolled $n = 60$ times to test whether it is fair ($H_0: p_1 = \dots = p_6 = 1/6$).
- Number of categories: $k = 6 \implies df = 6 - 1 = 5$.
- Expected count for each face: $E_j = 60 \times \frac{1}{6} = 10$.
- Observed counts: $O = (8, 12, 9, 11, 7, 13)$.

Calculate $V$:
$$V = \frac{(8-10)^2}{10} + \frac{(12-10)^2}{10} + \frac{(9-10)^2}{10} + \frac{(11-10)^2}{10} + \frac{(7-10)^2}{10} + \frac{(13-10)^2}{10}$$
$$V = \frac{4 + 4 + 1 + 1 + 9 + 9}{10} = \frac{28}{10} = 2.8$$

For $df = 5$ at $\alpha = 0.05$, the critical value is $\chi^2_{5, 0.05} = 11.07$.
Since $V = 2.8 < 11.07$, we **fail to reject $H_0$**. The die is consistent with fairness.

---

---

## Common Mistakes

- Confusing conditional variance with the variance of conditional expectation (Eve's Law components).
- Forgetting that linearity of expectation holds unconditionally, whereas $\mathbb{E}[XY] = \mathbb{E}[X]\mathbb{E}[Y]$ requires independence.

---

## Related Concepts

- [[Hypothesis Testing Framework]]
- [[p-Values and Significance]]
- [[Mendel's Peas Chi-Square Goodness-of-Fit Example]]

---

---

## Prerequisites

- [[Hypothesis Testing Framework]]
- [[Random Variables and Probability Distributions]]
- [[Discrete Probability Distributions]]

---

## Problems

- [[Mendel's Peas Chi-Square Goodness-of-Fit Example]]
- [[Problem — Comparing Prediction Algorithms via Paired Wald Test]]

---

## Sources

- [[01 - Sources/Lectures/CSE301_Hypothesis_Test.pdf]]
