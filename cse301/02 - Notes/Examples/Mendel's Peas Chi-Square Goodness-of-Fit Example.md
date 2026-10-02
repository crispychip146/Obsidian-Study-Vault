---
type: example
course: cse301
status: active
order: 64
---

# Mendel's Peas Chi-Square Goodness-of-Fit Example

> 📖 **Reading Order:** Step 64 of 92 | **Module 9:** Hypothesis Testing  
> ◄ **Previous:** [[Benjamini-Hochberg Procedure Algorithm]] | ► **Next:** [[Toy Permutation Test Example]]

---

## Problem

In his historic 1865 genetics experiments on dihybrid inheritance, Gregor Mendel crossed pea plants and predicted four progeny phenotypes based on his Law of Independent Assortment:
1. Round Yellow ($p_1 = 9/16$)
2. Wrinkled Yellow ($p_2 = 3/16$)
3. Round Green ($p_3 = 3/16$)
4. Wrinkled Green ($p_4 = 1/16$)

In an experimental trial with $n = 556$ seeds, Mendel observed the following counts:
- Round Yellow ($X_1$): $315$
- Wrinkled Yellow ($X_2$): $101$
- Round Green ($X_3$): $108$
- Wrinkled Green ($X_4$): $32$

Conduct Pearson's $\chi^2$ goodness-of-fit test at the $\alpha = 0.05$ significance level to evaluate whether Mendel's empirical data support his genetic ratio theory.

---

## Given

- Categories: $k = 4$
- Total observations: $n = 315 + 101 + 108 + 32 = 556$
- Null Hypothesis:
  $$H_0: \mathbf{p} = \left(\frac{9}{16}, \frac{3}{16}, \frac{3}{16}, \frac{1}{16}\right) = (0.5625, 0.1875, 0.1875, 0.0625)$$
- Significance level: $\alpha = 0.05$

---

## Required

1. Expected count $E_j$ for each of the four categories under $H_0$.
2. Degrees of freedom for the test.
3. Pearson $\chi^2$ test statistic $V$.
4. Critical value $\chi^2_{df, 0.05}$, test decision, and scientific conclusion.

---

## Concepts Used

- [[Pearson's Chi-Square Goodness-of-Fit Test]]
- [[Hypothesis Testing Framework]]
- [[p-Values and Significance]]

---

## Solution

### Step 1: Calculate Expected Counts
Under $H_0$, expected count is $E_j = n \cdot p_{0j}$:

1. **Round Yellow ($j = 1$):**
   $$E_1 = 556 \times \frac{9}{16} = 312.75$$
2. **Wrinkled Yellow ($j = 2$):**
   $$E_2 = 556 \times \frac{3}{16} = 104.25$$
3. **Round Green ($j = 3$):**
   $$E_3 = 556 \times \frac{3}{16} = 104.25$$
4. **Wrinkled Green ($j = 4$):**
   $$E_4 = 556 \times \frac{1}{16} = 34.75$$

Check: $\sum_{j=1}^4 E_j = 312.75 + 104.25 + 104.25 + 34.75 = 556 = n \quad \checkmark$
All $E_j \ge 34.75 > 5$, satisfying Cochran's rule for the validity of the $\chi^2$ approximation.

---

### Step 2: Compute Category Discrepancies and Pearson Statistic $V$

| Category | Observed ($O_j$) | Expected ($E_j$) | Deviation ($O_j - E_j$) | Squared $(O_j - E_j)^2$ | Component $\frac{(O_j - E_j)^2}{E_j}$ |
|---|---|---|---|---|---|
| Round Yellow | 315 | 312.75 | $+2.25$ | $5.0625$ | $\frac{5.0625}{312.75} \approx 0.0162$ |
| Wrinkled Yellow | 101 | 104.25 | $-3.25$ | $10.5625$ | $\frac{10.5625}{104.25} \approx 0.1013$ |
| Round Green | 108 | 104.25 | $+3.75$ | $14.0625$ | $\frac{14.0625}{104.25} \approx 0.1349$ |
| Wrinkled Green | 32 | 34.75 | $-2.75$ | $7.5625$ | $\frac{7.5625}{34.75} \approx 0.2176$ |
| **Total** | **556** | **556.00** | **0.00** | — | **$V \approx 0.4700$** |

$$V = 0.0162 + 0.1013 + 0.1349 + 0.2176 = 0.4700$$

---

### Step 3: Determine Degrees of Freedom and Critical Value
- Categories: $k = 4$.
- Degrees of freedom:
  $$df = k - 1 = 4 - 1 = 3$$
- Critical value from the $\chi^2_3$ distribution table at $\alpha = 0.05$:
  $$\chi^2_{3, 0.05} \approx 7.815$$

---

### Step 4: Decision and $p$-Value
- **Comparison:**
  $$V = 0.470 < \chi^2_{3, 0.05} = 7.815$$
- **$p$-Value:**
  $$p = P\left(\chi^2_3 \ge 0.470\right) \approx 0.9254 \quad (92.5\%)$$
- **Conclusion:** 
  Because $p = 0.925 > 0.05$, we **decisively fail to reject $H_0$**.
  Mendel's experimental data match the theoretical $9:3:3:1$ inheritance ratios exceptionally well.

---

## Result

- Pearson statistic: $V = 0.470$
- Degrees of freedom: $df = 3$
- Critical value: $\chi^2_{3, 0.05} = 7.815$
- $p$-value: $0.925$
- Decision: Retain $H_0$. Strong empirical confirmation of Mendel's Law of Independent Assortment.

---

## Historical Note: Fisher's "Too Good to Be True" Critique

In 1936, the great statistician Ronald Fisher analyzed all of Mendel's published pea experiments. Fisher noted that across all experiments, the combined $\chi^2$ values were extraordinarily small ($p \approx 0.99993$). In statistical theory, an exact $H_0$ generates $V$ uniformly distributed in tail areas. A $p$-value of $0.9999$ occurs purely by chance only once in $10,000$ times, leading Fisher to suggest that an overzealous assistant may have slightly "tidied up" the counts to match Mendel's ratios more closely than natural sampling noise would produce!

---

## Related Concepts

- [[Pearson's Chi-Square Goodness-of-Fit Test]]
- [[Hypothesis Testing Framework]]
- [[p-Values and Significance]]

---

## Sources

- [[01 - Sources/Lectures/CSE301_Hypothesis_Test.pdf]]
