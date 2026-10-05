---
type: algorithm
course: cse301
status: active
order: 61
---

# Permutation Test Algorithm

> 📖 **Reading Order:** Step 61 of 92 | **Module 9:** Hypothesis Testing  
> ◄ **Previous:** [[Pearson's Chi-Square Goodness-of-Fit Test]] | ► **Next:** [[Multiple Testing and False Discovery Rate]]

---

## Building the idea

A permutation test builds its reference distribution by reassigning labels in ways permitted under the null. If observations are exchangeable between groups, the labels carry no information about their distribution under the null, while the pooled values stay fixed.

Compute the observed statistic first, then enumerate valid label assignments or sample them. The fraction whose statistics are at least as extreme gives the exact enumerated $p$-value. For Monte Carlo testing, a standard valid construction includes the observed configuration and uses $(b+1)/(B+1)$ with $B$ random permutations and $b$ exceedances, rather than reporting zero from no exceedances.

The allowed rearrangements depend on the design. Paired data may require within-pair swaps, and clustered data require preserving their structure. Equal means alone do not generally imply exchangeability when distributions differ. The assumption justifies the randomization; enumeration cannot rescue an invalid permutation scheme.

## Inputs

- Ranked empirical test statistics, p-values, or sample arrays.

---

## Outputs

- Decision vector: rejected null hypotheses or permutation p-value.

---

## How It Works

### Exact Permutation Test (Small $N$)
1. Compute the observed test statistic:
   $$t_{\text{obs}} = T(X_1, \dots, X_m, Y_1, \dots, Y_n)$$
2. Pool all $N = m + n$ observations:
   $$\mathbf{Z} = (X_1, \dots, X_m, Y_1, \dots, Y_n)$$
3. Enumerate all $N!$ permutations of the vector $\mathbf{Z}$ (or all $\binom{N}{m}$ distinct group assignments).
4. For each permutation $j = 1, \dots, K$:
   - Split the permuted vector into the first $m$ elements $\mathbf{X}^*$ and remaining $n$ elements $\mathbf{Y}^*$.
   - Calculate the permuted test statistic $T_j = T(\mathbf{X}^*, \mathbf{Y}^*)$.
5. The exact $p$-value is the fraction of permutations where $T_j$ is at least as extreme as $t_{\text{obs}}$:
   $$p = \frac{1}{K} \sum_{j=1}^K \mathbf{1}_{\{T_j \ge t_{\text{obs}}\}}$$

### Monte Carlo Permutation Test (Large $N$)
When $N$ exceeds $\approx 20$, the number of permutations $N!$ is astronomically large. We approximate the permutation distribution with high precision by drawing $B$ random permutations uniformly at random.

---
### Related Concepts

- [[Hypothesis Testing Framework]]
- [[p-Values and Significance]]
- [[Toy Permutation Test Example]]

---

## Pseudocode

```python
def permutation_test(X, Y, B=10000):
    m = len(X)
    n = len(Y)
    N = m + n
    pooled = concatenate(X, Y)
    
    # 1. Observed test statistic
    t_obs = abs(mean(X) - mean(Y))
    
    # 2. Resampling loop
    count_extreme = 0
    for b in range(B):
        permuted = random_shuffle(pooled)
        X_star = permuted[0:m]
        Y_star = permuted[m:N]
        
        t_star = abs(mean(X_star) - mean(Y_star))
        if t_star >= t_obs:
            count_extreme += 1
            
    # 3. Compute Monte Carlo p-value
    # (Adding 1 to numerator and denominator ensures p > 0 and unbiased test size)
    p_value = (count_extreme + 1) / (B + 1)
    return p_value
```

---

## Example

### Example: Toy Permutation Test

Let sample 1 be $(X_1, X_2) = (1, 9)$ ($m = 2$) and sample 2 be $Y_1 = 3$ ($n = 1$).
- Pooled data: $\mathbf{Z} = (1, 9, 3)$, $N = 3$.
- Statistic: $T = \lvert \bar{X} - Y_1 \rvert$.
- Observed: $\bar{X} = \frac{1 + 9}{2} = 5$, $Y_1 = 3 \implies t_{\text{obs}} = \lvert 5 - 3 \rvert = 2$.

All $3! = 6$ equally likely permutations of $(1, 9, 3)$:

| Permutation | $\mathbf{X}^*$ | $\mathbf{Y}^*$ | $\bar{X}^*$ | $Y_1^*$ | $T^* = \lvert \bar{X}^* - Y_1^* \rvert$ | $T^* \ge 2$? |
|---|---|---|---|---|---|---|
| $(1, 9, 3)$ | $(1, 9)$ | $3$ | $5$ | $3$ | $\lvert 5 - 3 \rvert = 2$ | **Yes** |
| $(9, 1, 3)$ | $(9, 1)$ | $3$ | $5$ | $3$ | $\lvert 5 - 3 \rvert = 2$ | **Yes** |
| $(1, 3, 9)$ | $(1, 3)$ | $9$ | $2$ | $9$ | $\lvert 2 - 9 \rvert = 7$ | **Yes** |
| $(3, 1, 9)$ | $(3, 1)$ | $9$ | $2$ | $9$ | $\lvert 2 - 9 \rvert = 7$ | **Yes** |
| $(9, 3, 1)$ | $(9, 3)$ | $1$ | $6$ | $1$ | $\lvert 6 - 1 \rvert = 5$ | **Yes** |
| $(3, 9, 1)$ | $(3, 9)$ | $1$ | $6$ | $1$ | $\lvert 6 - 1 \rvert = 5$ | **Yes** |

Notice that for this toy dataset, all 6 permutations yield $T^* \ge 2$, so $p = \frac{6}{6} = 1.0$.

---

## Complexity

- **Exact Test:**
  - Time Complexity: $O\left(\binom{N}{m} \cdot N\right)$ or $O(N! \cdot N)$ — Factorial / Exponential time. Only feasible for $N \le 20$.
  - Space Complexity: $O(N)$.
- **Monte Carlo Test:**
  - Time Complexity: $O(B \cdot N)$ — Linear in sample size and number of draws.
  - Space Complexity: $O(N)$.

---

## Properties

- **FDR Control:** Strictly controls false discovery rate or exact non-parametric size under exchangeability.

---

## Limitations

### Properties
1. **Exact Size:** Under $H_0$, the Type I error rate is strictly $\le \alpha$ for **any** sample size $n$, with zero asymptotic approximation error.
2. **Distribution-Free:** Requires no assumption of normality, equal variance, or symmetry.

### Limitations
1. **Exchangeability Assumption:** Observations must be independent and exchangeable under $H_0$. If two groups have different shapes or variances under $H_0$, the permutation test can yield inflated Type I errors.
2. **Computational Overhead:** Requires simulation loops.

---

## Common Mistakes

- Confusing Family-Wise Error Rate (FWER) with False Discovery Rate (FDR).
- Permuting non-exchangeable data under heterogeneous variances.

---

## Exam Relevance

Appears on CSE 301 examinations testing multiple comparisons or non-parametric statistical hypothesis testing.

---

## What to carry forward

[[Toy Permutation Test Example]] is small enough to enumerate completely. Compare label assignments, not merely different orderings within the same group, when reducing repeated configurations.

## Related notes

- [[Toy Permutation Test Example]]

## Sources

- [[01 - Sources/Lectures/CSE301_Hypothesis_Test.pdf]]
