---
type: problem
course: cse301
status: active
order: 67
---

# Problem — Multiple Testing Correction with Bonferroni and Benjamini-Hochberg

> 📖 **Reading Order:** Step 67 of 92 | **Module 9:** Hypothesis Testing  
> ◄ **Previous:** [[Problem — Comparing Prediction Algorithms via Paired Wald Test]] | ► **Next:** [[Stochastic Process]]

---

## Problem

A bioinformatics researcher evaluates $m = 10$ genes to determine whether their expression levels differ between cancer patients and healthy controls. The $10$ independent hypothesis tests produce the following ordered $p$-values:

$$0.00017, \; 0.00448, \; 0.00671, \; 0.00907, \; 0.01220, \; 0.33626, \; 0.39341, \; 0.53882, \; 0.58125, \; 0.98617$$

1. If the researcher performs **unadjusted testing** at individual significance level $\alpha = 0.05$:
   - Which null hypotheses are rejected?
   - What is the theoretical maximum Family-Wise Error Rate (FWER) under complete nullity?
2. Apply the **Bonferroni correction** to control the Family-Wise Error Rate at $\text{FWER} \le 0.05$:
   - State the adjusted threshold $\alpha_{\text{Bonferroni}}$.
   - Determine which null hypotheses are rejected.
3. Apply the **Benjamini-Hochberg (BH) procedure** to control the False Discovery Rate at $\text{FDR} \le 0.05$:
   - Tabulate each rank $i$, observed $p$-value $P_{(i)}$, and adaptive threshold $\ell_i = \frac{i}{m} q$.
   - Identify the maximum index $k$ and state which hypotheses are rejected.
4. Contrast the trade-off between Type I error control and statistical discovery power across the three methods.

---

## Given

- Number of tests: $m = 10$
- Target significance level: $\alpha = 0.05$, target FDR: $q = 0.05$
- Ordered $p$-values:
  $P_{(1)} = 0.00017$, $P_{(2)} = 0.00448$, $P_{(3)} = 0.00671$, $P_{(4)} = 0.00907$, $P_{(5)} = 0.01220$,
  $P_{(6)} = 0.33626$, $P_{(7)} = 0.39341$, $P_{(8)} = 0.53882$, $P_{(9)} = 0.58125$, $P_{(10)} = 0.98617$.

---

## Required

1. Unadjusted rejections and theoretical FWER.
2. Bonferroni threshold and rejections.
3. Benjamini-Hochberg rank table, threshold comparison, and rejections.
4. Methodological comparison.

---

## Concepts Tested

- [[Multiple Testing and False Discovery Rate]]
- [[p-Values and Significance]]
- [[Benjamini-Hochberg Procedure Algorithm]]
- Family-Wise Error Rate vs. False Discovery Rate

---

## Solution

### 1. Unadjusted Hypothesis Testing
Each test is compared against the raw threshold $\alpha = 0.05$.
- $P_{(1)} = 0.00017 \le 0.05$ (Reject)
- $P_{(2)} = 0.00448 \le 0.05$ (Reject)
- $P_{(3)} = 0.00671 \le 0.05$ (Reject)
- $P_{(4)} = 0.00907 \le 0.05$ (Reject)
- $P_{(5)} = 0.01220 \le 0.05$ (Reject)
- Hypotheses $6$ through $10$ have $P_{(i)} \ge 0.33626 > 0.05$ (Retain)

**Unadjusted Discoveries:** **5 genes rejected** ($H_{(1)}$ through $H_{(5)}$).

**Theoretical FWER under complete nullity:**
Assuming the tests are independent:
$$\text{FWER} = 1 - (1 - \alpha)^m = 1 - (1 - 0.05)^{10} = 1 - (0.95)^{10} = 1 - 0.5987 = 0.4013 \quad (40.13\%)$$
There is a massive $40.1\%$ chance of declaring at least one false discovery!

---

### 2. Bonferroni Correction
The Bonferroni threshold controls the FWER at $\alpha = 0.05$:
$$\alpha_{\text{Bonferroni}} = \frac{\alpha}{m} = \frac{0.05}{10} = 0.0050$$

Compare each $P_{(i)}$ against $0.0050$:
- $P_{(1)} = 0.00017 \le 0.0050 \implies \mathbf{\text{Reject } H_{(1)}}$
- $P_{(2)} = 0.00448 \le 0.0050 \implies \mathbf{\text{Reject } H_{(2)}}$
- $P_{(3)} = 0.00671 > 0.0050 \implies \text{Retain } H_{(3)}$
- All subsequent $p$-values exceed $0.0050$.

**Bonferroni Discoveries:** **Only 2 genes rejected** ($H_{(1)}$ and $H_{(2)}$).

---

### 3. Benjamini-Hochberg (BH) Procedure
To control $\text{FDR} \le q = 0.05$, compute the threshold line $\ell_i = \frac{i}{m} q = \frac{i}{10}(0.05) = 0.005 \times i$:

| Rank ($i$) | Ordered $p$-value $P_{(i)}$ | BH Cutoff $\ell_i = \frac{i}{10}(0.05)$ | Condition $P_{(i)} \le \ell_i$? | Decision |
|---|---|---|---|---|
| **1** | **0.00017** | $0.0050$ | **Yes** ($0.00017 \le 0.0050$) | **Reject** |
| **2** | **0.00448** | $0.0100$ | **Yes** ($0.00448 \le 0.0100$) | **Reject** |
| **3** | **0.00671** | $0.0150$ | **Yes** ($0.00671 \le 0.0150$) | **Reject** |
| **4** | **0.00907** | $0.0200$ | **Yes** ($0.00907 \le 0.0200$) | **Reject** |
| **5** | **0.01220** | $0.0250$ | **Yes** ($0.01220 \le 0.0250$) | **Reject** |
| 6 | 0.33626 | $0.0300$ | No ($0.33626 > 0.0300$) | Retain |
| 7 | 0.39341 | $0.0350$ | No ($0.39341 > 0.0350$) | Retain |
| 8 | 0.53882 | $0.0400$ | No ($0.53882 > 0.0400$) | Retain |
| 9 | 0.58125 | $0.0450$ | No ($0.58125 > 0.0450$) | Retain |
| 10 | 0.98617 | $0.0500$ | No ($0.98617 > 0.0500$) | Retain |

**Finding the Crossing Index:**
The largest index $i$ satisfying $P_{(i)} \le \ell_i$ is **$k = 5$** (since $0.01220 \le 0.0250$).
According to the BH decision rule, we reject **all null hypotheses with rank $i \le 5$**:
$$\mathbf{\text{Reject } H_{(1)}, H_{(2)}, H_{(3)}, H_{(4)}, H_{(5)}}$$

**Benjamini-Hochberg Discoveries:** **5 genes rejected**.

---

### 4. Methodological Comparison

| Method | Discoveries ($R$) | Hypotheses Rejected | Error Controlled | Practical Consequence |
|---|---|---|---|---|
| **Unadjusted** | 5 | $H_{(1)}$ to $H_{(5)}$ | None ($\text{FWER} \approx 40.1\%$) | High risk of false discoveries |
| **Bonferroni** | 2 | $H_{(1)}, H_{(2)}$ | $\text{FWER} \le 0.05$ | Severe Type II error (misses 3 real genes) |
| **Benjamini-Hochberg** | 5 | $H_{(1)}$ to $H_{(5)}$ | $\text{FDR} \le 0.05$ | **Best:** Recovers all 5 discoveries with $\le 5\%$ expected false positive rate |

---

## Key Takeaway

The Bonferroni correction is so harsh that it discards genes 3, 4, and 5 ($p$-values between $0.006$ and $0.012$, which are highly significant in isolation). The Benjamini-Hochberg procedure recognizes that having multiple small $p$-values increases our collective confidence that real discoveries are present, adaptively relaxing the threshold and safely declaring all 5 discoveries.

---

## Related Concepts

- [[Multiple Testing and False Discovery Rate]]
- [[Benjamini-Hochberg Procedure Algorithm]]
- [[p-Values and Significance]]

---

## Source

- [[01 - Sources/Lectures/CSE301_Hypothesis_Test.pdf]]
