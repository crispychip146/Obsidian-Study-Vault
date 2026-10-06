---
type: example
course: cse301
status: active
order: 76
---

# Toy Permutation Test Example

> 📖 **Reading Order:** Step 76 of 103 | **Module 9:** Hypothesis Testing  
> ◄ **Previous:** [[Mendel's Peas Chi-Square Goodness-of-Fit Example]] | ► **Next:** [[Problem — Comparing Prediction Algorithms via Paired Wald Test]]
---
## Problem

Consider a tiny dataset consisting of two samples:
- Sample $X$: $X_1 = 1, X_2 = 9$ ($m = 2$)
- Sample $Y$: $Y_1 = 3$ ($n = 1$)

We want to test whether the two groups originate from identical distributions:
$$H_0: F_X = F_Y \quad \text{versus} \quad H_1: F_X \ne F_Y$$
using the absolute difference in sample means as our test statistic:
$$T = \lvert \bar{X} - \bar{Y} \rvert$$

1. Calculate the observed value of the test statistic $t_{\text{obs}}$.
2. List all $N! = (2 + 1)! = 6$ possible permutations of the pooled data vector.
3. Compute the value of $T$ for each permutation.
4. Calculate the exact two-sided $p$-value for the test.
---
## Given

- Pooled data vector: $\mathbf{Z} = (1, 9, 3)$ of length $N = 3$.
- Group sizes: $m = 2, n = 1$.
- Test statistic formula: $T = \left\lvert \frac{X_1 + X_2}{2} - Y_1 \right\rvert$.
---
## Required

1. Observed value $t_{\text{obs}}$.
2. Complete permutation table.
3. Exact permutation $p$-value:
   $$p = \frac{1}{N!}\sum_{j=1}^{N!} \mathbf{1}_{\{T_j \ge t_{\text{obs}}\}}$$
---
## Understanding the Problem and Choosing the Method

Identify the random variables, state the conditional distributions, select the appropriate probabilistic law or updating formula, and execute the algebraic substitutions step by step.

---

## Solution

### Concepts Used

- [[Permutation Test Algorithm]]
- [[Hypothesis Testing Framework]]
- [[p-Values and Significance]]

---
### Solution

### Step 1: Observed Statistic Calculation
From the original group assignment:
$$\bar{X} = \frac{1 + 9}{2} = 5, \quad \bar{Y} = Y_1 = 3$$
$$t_{\text{obs}} = \lvert 5 - 3 \rvert = 2$$

---

### Step 2: Enumerate All Permutations
There are $N! = 3! = 6$ permutations of the indices $(1, 2, 3)$. 
Under $H_0$, each of the 6 permutations has equal probability $\frac{1}{6}$ of being observed.
For each permutation, the first two numbers form the pseudogroup $\mathbf{X}^*$, and the third number forms the pseudogroup $Y^*$:

| Index ($j$) | Permuted Order | $\mathbf{X}^*$ | $Y^*$ | $\bar{X}^*$ | $Y^*$ | $T_j = \lvert \bar{X}^* - Y^* \rvert$ | Is $T_j \ge t_{\text{obs}} = 2$? |
|---|---|---|---|---|---|---|---|
| **1** | $(1, 9, 3)$ (Original) | $(1, 9)$ | $3$ | $\frac{1+9}{2} = 5$ | $3$ | $\lvert 5 - 3 \rvert = 2$ | **Yes** ($2 \ge 2$) |
| **2** | $(9, 1, 3)$ | $(9, 1)$ | $3$ | $\frac{9+1}{2} = 5$ | $3$ | $\lvert 5 - 3 \rvert = 2$ | **Yes** ($2 \ge 2$) |
| **3** | $(1, 3, 9)$ | $(1, 3)$ | $9$ | $\frac{1+3}{2} = 2$ | $9$ | $\lvert 2 - 9 \rvert = 7$ | **Yes** ($7 \ge 2$) |
| **4** | $(3, 1, 9)$ | $(3, 1)$ | $9$ | $\frac{3+1}{2} = 2$ | $9$ | $\lvert 2 - 9 \rvert = 7$ | **Yes** ($7 \ge 2$) |
| **5** | $(9, 3, 1)$ | $(9, 3)$ | $1$ | $\frac{9+3}{2} = 6$ | $1$ | $\lvert 6 - 1 \rvert = 5$ | **Yes** ($5 \ge 2$) |
| **6** | $(3, 9, 1)$ | $(3, 9)$ | $1$ | $\frac{3+9}{2} = 6$ | $1$ | $\lvert 6 - 1 \rvert = 5$ | **Yes** ($5 \ge 2$) |

---

### Step 3: Exact Permutation Distribution
The permutation distribution of $T$ under $H_0$ assigns probability:
- $P(T = 2) = \frac{2}{6} = \frac{1}{3}$
- $P(T = 5) = \frac{2}{6} = \frac{1}{3}$
- $P(T = 7) = \frac{2}{6} = \frac{1}{3}$

Notice that the observed value $t_{\text{obs}} = 2$ is actually the **smallest possible value** the statistic could ever take across all combinations!

---

### Step 4: Compute the Exact $p$-Value
$$p = \frac{\# \{j : T_j \ge 2\}}{6} = \frac{6}{6} = 1.0$$

Since $p = 1.0 \gg 0.05$, we fail to reject $H_0$. There is zero evidence that the distributions differ.
---
## Result

- Observed difference: $t_{\text{obs}} = 2$.
- Permutation values: $\{2, 2, 7, 7, 5, 5\}$.
- Exact $p$-value: $p = 1.0$.
---
## Why This Works

The solution holds because every step follows directly from Bayes' rule, the law of total probability, or properties of expectation and variance.

---

## Common Mistakes

1. **Exactness:** The permutation test is exact; it does not rely on the Central Limit Theorem. With $N = 3$, an asymptotic test (like a $z$-test) would be absurd and completely invalid.
2. **Minimal Achievable $p$-value:** Notice that even if the observed data had yielded the most extreme statistic possible ($T = 7$), the $p$-value would have been $p = \frac{2}{6} = 0.333$. This demonstrates that with $N = 3$, it is mathematically impossible to reject $H_0$ at the $\alpha = 0.05$ level, regardless of how extreme the data are. A permutation test requires at least $\binom{N}{m} \ge \frac{1}{\alpha} = 20$ permutations (e.g., $N \ge 6$) to ever reach a $p$-value below $0.05$.
---
## General Method

Extract the reusable problem-solving pattern: define random variables, write down the joint distribution, condition on observed data, and normalize the resulting distribution.

---

## Related Concepts

- [[Permutation Test Algorithm]]
- [[Hypothesis Testing Framework]]
- [[p-Values and Significance]]
---
## Sources

- [[cse301/01 - Sources/Lectures/CSE301_Hypothesis_Test.pdf]]
