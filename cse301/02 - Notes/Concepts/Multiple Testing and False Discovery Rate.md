---
type: concept
course: cse301
status: active
order: 73
---

# Multiple Testing and False Discovery Rate

> 📖 **Reading Order:** Step 73 of 103 | **Module 9:** Hypothesis Testing  
> ◄ **Previous:** [[Permutation Test Algorithm]] | ► **Next:** [[Benjamini-Hochberg Procedure Algorithm]]
---
## Starting Point and the Problem

Probability and statistical inference model uncertainty in physical and computer systems. When analyzing stochastic phenomena related to Multiple Testing and False Discovery Rate, naive counting or deterministic approximations fail. We establish a formal mathematical foundation to quantify outcomes and evaluate expectations rigorously.

---

## Developing the Idea

Imagine flipping a fair coin:
- Getting 5 heads in a row has a small probability: $(0.5)^5 = 0.03125 < 0.05$. In a single test, this would be deemed "statistically significant" ($p < 0.05$).
- However, if 100 students in a lecture hall all flip fair coins 5 times, on average $\approx 3$ students will achieve 5 straight heads purely by chance!

In modern data science, genomics, and A/B testing:
- A DNA microarray tests $m = 20,000$ genes simultaneously.
- If we conduct every test at unadjusted $\alpha = 0.05$:
  $$\text{Expected False Discoveries} = 20,000 \times 0.05 = 1,000 \text{ fake discoveries!}$$
Researchers would waste millions of dollars chasing 1,000 ghost genes that have zero actual biological effect.
---
## Definition

The **Multiple Testing Problem** arises when a researcher conducts $m > 1$ statistical hypothesis tests simultaneously. If each individual test is evaluated at the nominal significance level $\alpha$ (e.g., $\alpha = 0.05$), the probability of committing at least one false positive (Type I error) across the family of tests escalates rapidly toward certainty.

### The Classification Matrix of $m$ Tests

| Reality \ Decision | Retain $H_0$ | Reject $H_0$ (Discovery) | Total |
|---|---|---|---|
| **$H_0$ is True** (Null) | $U$ (True Negatives) | $V$ (**False Positives / Type I**) | $m_0$ |
| **$H_1$ is True** (Non-null) | $T$ (**False Negatives / Type II**) | $S$ (True Positives) | $m_1$ |
| **Total** | $m - R$ | $R$ (Total Declared Discoveries) | $m$ |

- $m$: Total number of hypothesis tests conducted (known).
- $R$: Number of rejected null hypotheses (observed).
- $V$: Number of falsely rejected nulls (unobserved random variable).
---
## How It Works

### Two Error Metrics: FWER vs. FDR

To guard against multiple testing inflation, statisticians define two fundamentally different error metrics:

```
             ┌──────────────────────────────────────────────┐
             │       Multiple Testing Error Metrics         │
             └──────────────────────┬───────────────────────┘
                                    │
           ┌────────────────────────┴────────────────────────┐
           ▼                                                 ▼
   Family-Wise Error Rate (FWER)                  False Discovery Rate (FDR)
   Goal: P(V ≥ 1) ≤ α                             Goal: E[V / R] ≤ q
   "Make NO false discoveries."                   "Keep the % of false discoveries low."
   Method: Bonferroni Correction                  Method: Benjamini-Hochberg (BH)
```

---
### Family-Wise Error Rate (FWER) and the Bonferroni Method

### Definition
The **Family-Wise Error Rate (FWER)** is the probability of committing **at least one** Type I error across all $m$ tests:
$$\text{FWER} = P(V \ge 1)$$

If the $m$ tests are mutually independent and all null hypotheses are true ($m_0 = m$):
$$\text{FWER} = 1 - P(\text{all } m \text{ tests correct}) = 1 - (1 - \alpha)^m$$

| Number of Tests ($m$) | Unadjusted FWER ($1 - (1 - 0.05)^m$) |
|---|---|
| 1 | $5.0\%$ |
| 5 | $22.6\%$ |
| 20 | $64.2\%$ |
| 100 | $99.4\%$ |
| 1,000 | $> 99.99999\%$ |

### The Bonferroni Method
To guarantee that $\text{FWER} \le \alpha$, the **Bonferroni Method** tests each individual hypothesis at the scaled threshold:
$$\alpha_{\text{Bonferroni}} = \frac{\alpha}{m}$$
That is, reject $H_{0i}$ if and only if:
$$P_i \le \frac{\alpha}{m}$$

### Proof via Boole's Inequality (Union Bound)
Let $R_i$ be the event that the $i$-th true null hypothesis is falsely rejected ($P_i \le \alpha/m$).
The event of making at least one false rejection is $E = \bigcup_{i \in I_0} R_i$, where $I_0$ is the set of true nulls ($\lvert I_0 \rvert = m_0 \le m$).
By the union bound:
$$\text{FWER} = P\left(\bigcup_{i \in I_0} R_i\right) \le \sum_{i \in I_0} P(R_i) \le \sum_{i \in I_0} \frac{\alpha}{m} = m_0 \frac{\alpha}{m} \le m \frac{\alpha}{m} = \alpha \quad \blacksquare$$

### Limitations of Bonferroni
The Bonferroni method is **drastically conservative**. When $m = 10,000$, $\alpha/m = 0.000005$. While it successfully prevents false positives, it destroys statistical power, failing to detect genuinely real scientific effects.

---
### False Discovery Rate (FDR) and the Benjamini-Hochberg Method

Introduced by Yoav Benjamini and Yosef Hochberg in their landmark 1995 paper, the **False Discovery Rate (FDR)** represents a modern paradigm shift.

### Definition: False Discovery Proportion and FDR
The **False Discovery Proportion (FDP)** is the proportion of false positives among all declared discoveries:
$$\text{FDP} = Q = \begin{cases} \frac{V}{R} & \text{if } R > 0 \\ 0 & \text{if } R = 0 \end{cases}$$

The **False Discovery Rate (FDR)** is the expected value of this proportion:
$$\text{FDR} = E[Q] = E\left[ \frac{V}{\max(R, 1)} \right]$$

### The Intuitive Philosophy
In high-throughput screening, researchers do not need a 100% clean sheet with zero mistakes. If a genomics experiment flags 200 candidate genes, a biologist is thrilled if **$95\%$ of them are real**, even if $5\%$ ($10$ genes) are false positives that will be filtered out in laboratory follow-up experiments.

Controlling FDR at $q = 0.05$ guarantees that **on average, no more than 5% of your reported discoveries are false alarms**.

---
### Comparison of Testing Methodologies

| Criterion | Unadjusted Testing | Bonferroni Correction (FWER) | Benjamini-Hochberg (FDR) |
|---|---|---|---|
| **Threshold Rule** | $P_i \le \alpha$ | $P_i \le \alpha/m$ | Adaptive rank threshold $P_{(i)} \le \frac{i}{m} q$ |
| **Error Controlled** | Individual error rate $\alpha$ | Family-wise error rate $\text{FWER} \le \alpha$ | False discovery rate $\text{FDR} \le q$ |
| **Type I Error Risk** | Extremely High (runaway false alarms) | Minimal (strictly bounded $\le \alpha$) | Controlled proportion of discoveries |
| **Statistical Power** | Highest (many false positives) | Lowest (overly strict, misses true hits) | **Optimal balance** between power and precision |
| **Best Used When** | Exploratory single tests | Confirmatory trials (clinical drug approval) | Big Data, genomics, large A/B test suites |
---
## Example

### Comparing Bonferroni vs. Benjamini-Hochberg on $m = 5$ Hypotheses

Suppose a high-throughput screening pipeline executes $m = 5$ independent hypothesis tests with target significance level $\alpha = q = 0.05$.
The sorted $p$-values in ascending order $P_{(1)} \le P_{(2)} \le \dots \le P_{(5)}$ are:
$$P_{(1)} = 0.005, \quad P_{(2)} = 0.012, \quad P_{(3)} = 0.028, \quad P_{(4)} = 0.035, \quad P_{(5)} = 0.120$$

1. **Unadjusted Testing (Threshold $\alpha = 0.05$):**
   - Rejects $H_{(1)}, H_{(2)}, H_{(3)}, H_{(4)}$ (4 rejections).
   - The overall probability of at least one false positive (FWER) is inflated to $1 - (1 - 0.05)^5 \approx 22.6\%$.

2. **Bonferroni FWER Correction (Threshold $\alpha / m = 0.05 / 5 = 0.010$):**
   - $P_{(1)} = 0.005 \le 0.010 \implies$ **Reject** $H_{(1)}$.
   - $P_{(2)} = 0.012 > 0.010 \implies$ **Fail to Reject**.
   - Tests $H_{(3)}, H_{(4)}, H_{(5)}$ also fail to meet $0.010$.
   - **Total Rejections = 1**. Bonferroni is excessively conservative and incurs severe Type II error (false negatives).

3. **Benjamini-Hochberg FDR Control (Thresholds $T_i = \frac{i}{m} q = \frac{i}{5}(0.05) = 0.010 \cdot i$):**
   - $i = 1$: $P_{(1)} = 0.005 \le 0.010$ (Condition holds)
   - $i = 2$: $P_{(2)} = 0.012 \le 0.020$ (Condition holds)
   - $i = 3$: $P_{(3)} = 0.028 \le 0.030$ (Condition holds!)
   - $i = 4$: $P_{(4)} = 0.035 > 0.040$ (Condition fails)
   - $i = 5$: $P_{(5)} = 0.120 > 0.050$ (Condition fails)
   - The largest index where $P_{(i)} \le \frac{i}{m}q$ is $k^* = 3$.
   - **BH Decision:** Reject all $H_{(i)}$ for $i \le 3$, yielding rejections $\{H_{(1)}, H_{(2)}, H_{(3)}\}$.
   - **Total Rejections = 3**. BH discovers two additional real effects while mathematically bounding $\text{FDR} \le 0.05$.

For step-by-step pseudo-code and practice problems, see [[Benjamini-Hochberg Procedure Algorithm]] and [[Problem — Multiple Testing Correction with Bonferroni and Benjamini-Hochberg]].

---

## Technical Details

### Proof Foundations of BH, Simes' Inequality, and Arbitrary Dependence

1. **Benjamini-Hochberg FDR Theorem:**
   - When test statistics are independent (or satisfy Positive Regression Dependency on a Subset, PRDS), the BH procedure guarantees:
     $$\text{FDR} = \mathbb{E}\left[\frac{V}{\max(R, 1)}\right] = \frac{m_0}{m} q \le q$$
     where $m_0$ is the number of true nulls. When all nulls are true ($m_0 = m$), controlling FDR automatically controls FWER at level $q$.
2. **Simes' Global Test:**
   - Under the global intersection null $H_0 = \bigcap_{i=1}^m H_{0,i}$, Simes' (1986) inequality establishes that:
     $$P\left( \min_{1 \le i \le m} \frac{m P_{(i)}}{i} \le \alpha \right) = \alpha$$
   - The Benjamini-Hochberg procedure essentially inverts Simes' test sequentially to identify the maximum non-null subset.
3. **Arbitrary Dependence: The Benjamini-Yekutieli (BY) Procedure:**
   - If test statistics have arbitrary or unknown dependencies (e.g. complex negative correlations), standard BH can exceed $q$. The **Benjamini-Yekutieli (BY)** variant restores strict FDR control by dividing the threshold by the harmonic sum:
     $$T_i = \frac{i}{m \cdot c(m)} q, \quad \text{where } c(m) = \sum_{j=1}^m \frac{1}{j} \approx \ln(m) + 0.5772$$

---

## Important Properties and Why They Hold

- **Mathematical Rigor:** Satisfies Kolmogorov's probability axioms or standard asymptotic regularity conditions.
- **Convergence / Consistency:** Guarantees stability under large sample limits or repeated independent trials.

---

## Common Mistakes

- **Confusing FDR with FWER:** FWER guarantees the probability of making *even one* false positive is $\le \alpha$. FDR guarantees the expected *proportion* of false positives among all declared discoveries is $\le q$.
- **Ignoring Multiple Testing in Feature Selection:** Running hundreds of univariate regressions or correlation tests without adjustment, cherry-picking tests with $p < 0.05$.
- **Sorting Direction:** Forgetting that Benjamini-Hochberg thresholds compare sorted $p$-values against an increasing line $\frac{i}{m}q$, and identifying the *largest* index $k^*$ satisfying the bound.

---

## Exam Relevance

Tested regularly in CSE 301 midterms and finals through derivations, numerical probability calculations, and statistical hypothesis testing via [[Benjamini-Hochberg Procedure Algorithm]].

---

## Related Concepts

- [[Hypothesis Testing Framework]]
- [[p-Values and Significance]]
- [[Benjamini-Hochberg Procedure Algorithm]]
- [[Problem — Multiple Testing Correction with Bonferroni and Benjamini-Hochberg]]
---
## Prerequisites

- [[Hypothesis Testing Framework]]
- [[p-Values and Significance]]
- [[Inclusion-Exclusion Principle]]

---

## Problems

- [[Problem — Multiple Testing Correction with Bonferroni and Benjamini-Hochberg]]

---

## Sources

- [[cse301/01 - Sources/Lectures/CSE301_Hypothesis_Test.pdf]]
