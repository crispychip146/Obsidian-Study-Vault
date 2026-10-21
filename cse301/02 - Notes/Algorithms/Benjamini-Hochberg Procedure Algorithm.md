---
type: algorithm
course: cse301
status: active
order: 74
---

# Benjamini-Hochberg Procedure Algorithm

## The Problem and Earlier Tools

The **Benjamini-Hochberg (BH) Procedure** is an algorithmic method for controlling the **False Discovery Rate (FDR)** when performing $m$ simultaneous statistical hypothesis tests.

It ensures that the expected proportion of false positives among all declared discoveries does not exceed a user-specified threshold $q \in (0, 1)$ (typically $q = 0.05$):
$$\text{FDR} = E\left[\frac{V}{\max(R, 1)}\right] \le \frac{m_0}{m} q \le q$$

It solves the excessive conservatism of the classical [[Multiple Testing and False Discovery Rate|Bonferroni correction]], dramatically increasing statistical power to detect real effects in large-scale data science and bioinformatics experiments.

---

## Developing the Core Idea

Rather than comparing every $p$-value against a fixed, brutally small threshold $\alpha/m$ (as Bonferroni does), the BH procedure compares ordered $p$-values against a **linearly increasing threshold**:

$$\ell_i = \frac{i}{m} q, \quad i = 1, 2, \dots, m$$

- The smallest $p$-value ($i = 1$) is compared against $\frac{1}{m}q$ (same as Bonferroni).
- The 10th smallest $p$-value ($i = 10$) is compared against $\frac{10}{m}q$ (10 times more lenient!).
- The largest $p$-value ($i = m$) is compared against $q$.

By finding the largest index $k$ where the data still fall below the threshold line, the algorithm safely rejects **all** hypotheses up to rank $k$.

---

## Inputs

- $m$ raw $p$-values: $P_1, P_2, \dots, P_m$ from individual hypothesis tests.
- Target False Discovery Rate level $q \in (0, 1)$ (commonly $0.05$).

---

## Outputs

- Set of rejected null hypotheses $\{H_{(1)}, H_{(2)}, \dots, H_{(k)}\}$ controlling $\text{FDR} \le q$.
- Crossing rank threshold index $k^*$.

---

## How It Works

1. **Sort:** Order the $m$ raw $p$-values from smallest to largest:
   $$P_{(1)} \le P_{(2)} \le \dots \le P_{(m)}$$
   Let $H_{(1)}, H_{(2)}, \dots, H_{(m)}$ denote the corresponding null hypotheses.
2. **Compute Thresholds:** For each rank $i \in \{1, 2, \dots, m\}$, calculate the adaptive cutoff:
   $$\ell_i = \frac{i}{m} q$$
3. **Find the Crossing Index ($k$):** Scan the ordered pairs and identify the **largest rank $k$** for which the observed $p$-value is less than or equal to its threshold:
   $$k = \max \left\{ i \in \{1, \dots, m\} : P_{(i)} \le \frac{i}{m} q \right\}$$
4. **Decision Rule:**
   - If no such $i$ exists ($k$ does not exist), reject no hypotheses ($R = 0$).
   - If $k \ge 1$, **reject all hypotheses up to rank $k$**:
     $$\text{Reject } H_{(1)}, H_{(2)}, \dots, H_{(k)}$$
     Even if some intermediate $P_{(j)} > \ell_j$ for $j < k$, the procedure still rejects $H_{(j)}$ because $k$ acts as a global step-up anchor!

---

## Pseudocode

```python
def benjamini_hochberg(p_values, q=0.05):
    """
    Controls False Discovery Rate at level q.
    Returns boolean array indicating which hypotheses to reject.
    """
    m = len(p_values)
    # Store original indices and sort
    sorted_indices = sorted(range(m), key=lambda idx: p_values[idx])
    sorted_p = [p_values[idx] for idx in sorted_indices]
    
    # Find largest k such that P_(k) <= (k / m) * q
    k_star = -1
    for i in range(m):
        rank = i + 1
        threshold = (rank / m) * q
        if sorted_p[i] <= threshold:
            k_star = i  # 0-indexed position
            
    # Formulate rejection mask in original order
    rejected = [False] * m
    if k_star >= 0:
        for i in range(k_star + 1):
            original_idx = sorted_indices[i]
            rejected[original_idx] = True
            
    return rejected, k_star + 1
```

---

## Example

### Worked Example: 10 Tests Comparison

Suppose $m = 10$ hypothesis tests yield the following ordered $p$-values with target FDR $q = 0.05$:

| Rank ($i$) | $P_{(i)}$ | BH Cutoff $\ell_i = \frac{i}{10}(0.05)$ | $P_{(i)} \le \ell_i$? | Bonferroni Cutoff $\frac{0.05}{10} = 0.005$ |
|---|---|---|---|---|
| **1** | **0.00017** | $0.0050$ | **Yes** ($\le 0.005$) | **Reject** |
| **2** | **0.00448** | $0.0100$ | **Yes** ($\le 0.010$) | **Reject** |
| **3** | **0.00671** | $0.0150$ | **Yes** ($\le 0.015$) | Fail to reject |
| **4** | **0.00907** | $0.0200$ | **Yes** ($\le 0.020$) | Fail to reject |
| **5** | **0.01220** | $0.0250$ | **Yes** ($\le 0.025$) | Fail to reject |
| 6 | 0.33626 | $0.0300$ | No ($> 0.030$) | Fail to reject |
| 7 | 0.39341 | $0.0350$ | No ($> 0.035$) | Fail to reject |
| 8 | 0.53882 | $0.0400$ | No ($> 0.040$) | Fail to reject |
| 9 | 0.58125 | $0.0450$ | No ($> 0.045$) | Fail to reject |
| 10 | 0.98617 | $0.0500$ | No ($> 0.050$) | Fail to reject |

### Analysis
- **Bonferroni:** Only tests 1 and 2 satisfy $P_i \le 0.005$. Total discoveries = **2**.
- **Benjamini-Hochberg:** The largest index satisfying $P_{(i)} \le \frac{i}{10}(0.05)$ is **$k = 5$** (since $0.01220 \le 0.0250$). 
  Therefore, we reject hypotheses **1, 2, 3, 4, and 5**! Total discoveries = **5**.
- **Result:** The BH procedure safely uncovered **more than double** the number of legitimate discoveries while rigorously bounding the False Discovery Rate at $\le 5\%$.

---

## Complexity

- **Time Complexity:** $O(m \log m)$ dominated by sorting the $m$ $p$-values. The subsequent linear scan is $O(m)$.
- **Space Complexity:** $O(m)$ to store sorted indices and threshold comparisons.

---

## Properties and Guarantees

1. **Exact FDR Bound:** Under independence of test statistics (or positive regression dependency PRDS), Benjamini and Hochberg proved that:
   $$\text{FDR} = E\left[\frac{V}{R}\right] = \frac{m_0}{m} q \le q$$
2. **Monotonicity:** Any hypothesis rejected by Bonferroni is guaranteed to also be rejected by Benjamini-Hochberg ($R_{\text{Bonferroni}} \subseteq R_{\text{BH}}$).

---

## Limitations

- Guarantees FDR control strictly under independent test statistics or Positive Regression Dependency (PRDS); for general arbitrary dependence structures, the more conservative Benjamini-Yekutieli (BY) modification is required.
- Does not bound Family-Wise Error Rate (FWER); if even one false discovery is impermissible, Bonferroni must be used.

---

## Common Mistakes

- Confusing Family-Wise Error Rate (FWER) with False Discovery Rate (FDR).
- Forgetting that the step-up rule rejects *all* hypotheses up to rank $k$, even if individual $P_{(j)} > \ell_j$ for some $j < k$.

---

## Exam Relevance

Appears frequently on CSE 301 examinations testing multiple hypothesis testing, comparing discovery rates between Bonferroni and Benjamini-Hochberg on genomic or algorithmic benchmark datasets.

---

## Related Concepts

- [[Hypothesis Testing Framework]]
- [[Multiple Testing and False Discovery Rate]]
- [[p-Values and Significance]]

---

## Prerequisites

- [[Hypothesis Testing Framework]]
- [[Multiple Testing and False Discovery Rate]]

---

## Problems

- [[Problem — Multiple Testing Correction with Bonferroni and Benjamini-Hochberg]]

---

## Sources

- [[cse301/01 - Sources/Lectures/CSE301_Hypothesis_Test.pdf]]

---

## Navigation

> 📖 **Reading Order:** Step 74 of 103 | **Module 9:** Hypothesis Testing  
> ◄ **Previous:** [[Multiple Testing and False Discovery Rate]] | ► **Next:** [[Mendel's Peas Chi-Square Goodness-of-Fit Example]]
