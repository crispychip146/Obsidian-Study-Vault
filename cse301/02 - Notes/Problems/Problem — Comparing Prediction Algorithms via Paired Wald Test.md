---
type: problem
course: cse301
status: active
---

# Problem — Comparing Prediction Algorithms via Paired Wald Test

## Problem

A machine learning team compares two image classification models, Algorithm 1 and Algorithm 2.
Rather than using two separate test sets, both models are evaluated on the **exact same benchmark test dataset** of $n = 500$ images.

For each test image $i \in \{1, 2, \dots, n\}$:
- Let $X_i = 1$ if Algorithm 1 misclassifies image $i$, and $0$ if correct.
- Let $Y_i = 1$ if Algorithm 2 misclassifies image $i$, and $0$ if correct.

The test results are summarized in the following $2 \times 2$ contingency table:

| | Algorithm 2 Correct ($Y_i = 0$) | Algorithm 2 Error ($Y_i = 1$) | Total |
|---|---|---|---|
| **Algorithm 1 Correct ($X_i = 0$)** | $410$ | $40$ | $450$ |
| **Algorithm 1 Error ($X_i = 1$)** | $15$ | $35$ | $50$ |
| **Total** | $425$ | $75$ | $500$ |

1. Explain why using an independent two-sample test (unpaired Wald test) is invalid here.
2. Formulate the null hypothesis $H_0$ and alternative hypothesis $H_1$ in terms of the error rates $p_1 = E[X_i]$ and $p_2 = E[Y_i]$.
3. Define the paired difference variable $D_i = X_i - Y_i$. Calculate the sample mean difference $\bar{D}$ and the sample variance $S_D^2$.
4. Compute the paired Wald test statistic $W$ and its two-sided $p$-value.
5. State your decision at significance level $\alpha = 0.05$ and interpret the scientific conclusion.

---

## Given

- Sample size: $n = 500$ paired observations
- Joint outcomes:
  - $(X_i = 0, Y_i = 0)$: $410$ instances (both correct)
  - $(X_i = 0, Y_i = 1)$: $40$ instances (Alg 1 correct, Alg 2 error)
  - $(X_i = 1, Y_i = 0)$: $15$ instances (Alg 1 error, Alg 2 correct)
  - $(X_i = 1, Y_i = 1)$: $35$ instances (both error)
- Error counts:
  - Algorithm 1 errors: $\sum X_i = 15 + 35 = 50 \implies \hat{p}_1 = \frac{50}{500} = 0.10$
  - Algorithm 2 errors: $\sum Y_i = 40 + 35 = 75 \implies \hat{p}_2 = \frac{75}{500} = 0.15$

---

## Required

1. Explanation of why unpaired test fails.
2. Formal hypotheses.
3. $\bar{D}$, $S_D^2$, and $\widehat{\text{se}}(\bar{D})$.
4. Paired Wald statistic $W$ and $p$-value.
5. Final statistical verdict at $\alpha = 0.05$.

---

## Concepts Tested

- [[Wald Test Statistic]]
- [[Hypothesis Testing Framework]]
- [[p-Values and Significance]]
- Paired vs. Unpaired experimental designs

---

## Solution

### 1. Why the Unpaired Test Is Invalid
The unpaired two-sample Wald test assumes that samples $X$ and $Y$ are **statistically independent**.
Here, both algorithms are evaluated on the exact same test images. Easy images (e.g., clear daylight photos) are easy for both models; difficult images (e.g., foggy, occluded photos) induce errors in both models.
Consequently, $X_i$ and $Y_i$ are **positively correlated** ($\text{Cov}(X_i, Y_i) > 0$). An unpaired test ignores this covariance, severely overestimating the variance of $\bar{X} - \bar{Y}$:
$$\text{Var}(\bar{X} - \bar{Y}) = \text{Var}(\bar{X}) + \text{Var}(\bar{Y}) - 2\text{Cov}(\bar{X}, \bar{Y})$$
Ignoring the positive covariance leads to an artificially large standard error and a catastrophic loss of statistical power.

---

### 2. Hypotheses
Let $\delta = p_1 - p_2$.
$$H_0: \delta = 0 \quad (\text{both algorithms have identical true error rates})$$
$$H_1: \delta \ne 0 \quad (\text{one algorithm performs better than the other})$$

---

### 3. Distribution of the Difference $D_i = X_i - Y_i$
For each image $i$, $D_i = X_i - Y_i$ takes values in $\{-1, 0, +1\}$:
- $D_i = +1$ when $X_i = 1, Y_i = 0$ (Alg 1 fails, Alg 2 succeeds): $15$ images.
- $D_i = -1$ when $X_i = 0, Y_i = 1$ (Alg 1 succeeds, Alg 2 fails): $40$ images.
- $D_i = 0$ when both agree ($410 + 35 = 445$ images).

1. **Sample Mean Difference $\bar{D}$:**
   $$\bar{D} = \frac{1}{n}\sum_{i=1}^n D_i = \frac{15(+1) + 40(-1) + 445(0)}{500} = \frac{15 - 40}{500} = \frac{-25}{500} = -0.05$$
   Notice that $\bar{D} = \hat{p}_1 - \hat{p}_2 = 0.10 - 0.15 = -0.05$.

2. **Sample Variance $S_D^2$:**
   $$\sum_{i=1}^n D_i^2 = 15(1)^2 + 40(-1)^2 + 445(0)^2 = 15 + 40 = 55$$
   $$S_D^2 = \frac{1}{n - 1}\left(\sum_{i=1}^n D_i^2 - n \bar{D}^2\right) = \frac{1}{499}\left(55 - 500(-0.05)^2\right)$$
   $$500 \times 0.0025 = 1.25$$
   $$S_D^2 = \frac{55 - 1.25}{499} = \frac{53.75}{499} \approx 0.1077$$

3. **Estimated Standard Error:**
   $$\widehat{\text{se}}(\bar{D}) = \sqrt{\frac{S_D^2}{n}} = \sqrt{\frac{0.1077}{500}} = \sqrt{0.0002154} \approx 0.01468$$

---

### 4. Paired Wald Test Statistic and $p$-Value
$$W = \frac{\bar{D} - 0}{\widehat{\text{se}}(\bar{D})} = \frac{-0.05}{0.01468} \approx -3.41$$

Compute the two-sided $p$-value:
$$p = 2\Phi(-\lvert W \rvert) = 2\Phi(-3.41) = 2(0.000325) = 0.00065$$

---

### 5. Decision and Scientific Conclusion
- At significance level $\alpha = 0.05$, the critical value is $z_{0.025} = 1.96$.
- Since $\lvert W \rvert = 3.41 > 1.96$ and $p = 0.00065 \ll 0.05$, we **strongly reject the null hypothesis $H_0$**.
- **Conclusion:** Algorithm 1 has a statistically significantly lower error rate ($10\%$ vs. $15\%$) than Algorithm 2 on this benchmark ($p < 0.001$). The $5\%$ performance gain is real and cannot be explained by chance variation.

---

### Contrast with Unpaired Test (Why Pairing Matters!)
Had we mistakenly run an unpaired test:
$$\widehat{\text{se}}_{\text{unpaired}} = \sqrt{\frac{0.10(0.90)}{500} + \frac{0.15(0.85)}{500}} = \sqrt{\frac{0.09 + 0.1275}{500}} = \sqrt{0.000435} \approx 0.02086$$
$$W_{\text{unpaired}} = \frac{-0.05}{0.02086} = -2.40 \implies p = 0.016$$
Notice that $\widehat{\text{se}}$ in the paired test ($0.01468$) is **$30\%$ smaller** than the unpaired standard error ($0.02086$), yielding a test statistic that is much more decisive ($W = -3.41$ vs. $-2.40$).

---

## Related Concepts

- [[Wald Test Statistic]]
- [[Hypothesis Testing Framework]]
- [[p-Values and Significance]]

---

## Source

- [[01 - Sources/Lectures/CSE301_Hypothesis_Test.pdf]]
