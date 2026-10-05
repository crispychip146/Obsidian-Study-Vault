---
type: algorithm
course: cse301
status: active
order: 63
---

# Benjamini-Hochberg Procedure Algorithm

> 📖 **Reading Order:** Step 63 of 92 | **Module 9:** Hypothesis Testing  
> ◄ **Previous:** [[Multiple Testing and False Discovery Rate]] | ► **Next:** [[Mendel's Peas Chi-Square Goodness-of-Fit Example]]

---

## Building the idea

Sort the $m$ $p$-values from smallest to largest. Compare rank $i$ with the increasing threshold $iq/m$, then find the largest rank $k$ that passes. Reject every hypothesis at ranks 1 through $k$.

The last passing rank determines the whole rejection set. Do not stop at the first failure: a later threshold is larger and may pass. Also preserve the mapping from sorted positions back to original hypotheses so the discoveries can be identified correctly.

The usual BH guarantee holds for independent valid $p$-values and certain forms of positive dependence. Arbitrary dependence needs another justified method or adjustment. The target $q$ concerns the expected false discovery proportion across repetitions, not the posterior probability that each individual rejected hypothesis is false.

## Inputs

- Ranked empirical test statistics, p-values, or sample arrays.

---

## Outputs

- Decision vector: rejected null hypotheses or permutation p-value.

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
### Related Concepts

- [[Multiple Testing and False Discovery Rate]]
- [[p-Values and Significance]]
- [[Hypothesis Testing Framework]]
- [[Problem — Multiple Testing Correction with Bonferroni and Benjamini-Hochberg]]

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

## Properties

### Properties and Guarantees

1. **Exact FDR Bound:** Under independence of test statistics (or positive regression dependency PRDS), Benjamini and Hochberg proved that:
   $$\text{FDR} = E\left[\frac{V}{R}\right] = \frac{m_0}{m} q \le q$$
2. **Monotonicity:** Any hypothesis rejected by Bonferroni is guaranteed to also be rejected by Benjamini-Hochberg ($R_{\text{Bonferroni}} \subseteq R_{\text{BH}}$).

---

## Limitations

- Exact permutation calculation requires evaluating $\binom{N}{n}$ combinations, becoming intractable for large $N$ (requiring Monte Carlo sampling).

---

## Common Mistakes

- Confusing Family-Wise Error Rate (FWER) with False Discovery Rate (FDR).
- Permuting non-exchangeable data under heterogeneous variances.

---

## Exam Relevance

Appears on CSE 301 examinations testing multiple comparisons or non-parametric statistical hypothesis testing.

---

## What to carry forward

[[Problem — Multiple Testing Correction with Bonferroni and Benjamini-Hochberg]] compares the rejection sets for the same data. Sorting takes $O(m\log m)$ time; the threshold scan is linear.

## Related notes

- [[Problem — Multiple Testing Correction with Bonferroni and Benjamini-Hochberg]]

## Sources

- [[01 - Sources/Lectures/CSE301_Hypothesis_Test.pdf]]
