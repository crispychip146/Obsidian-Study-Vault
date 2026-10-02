---
type: example
course: cse301
status: active
---

# Berger-Wolpert Confidence Set Puzzle Example

## Problem

Let $\theta$ be an unknown, fixed real number. Let $X_1, X_2$ be independent random variables with:
$$P(X_i = +1) = \frac{1}{2}, \quad P(X_i = -1) = \frac{1}{2}$$
We observe two noisy measurements:
$$Y_1 = \theta + X_1, \quad Y_2 = \theta + X_2$$

Define the following confidence set procedure $C$:
$$C = \begin{cases} \{Y_1 - 1\} & \text{if } Y_1 = Y_2 \\ \left\{\frac{Y_1 + Y_2}{2}\right\} & \text{if } Y_1 \ne Y_2 \end{cases}$$

1. Verify that $P_\theta(\theta \in C) = \frac{3}{4} = 75\%$ for all $\theta$, proving that $C$ is a valid $75\%$ confidence set.
2. Suppose we observe the data $(Y_1, Y_2) = (15, 17)$. Evaluate $C$, determine whether $\theta$ is in $C$, and explain the resulting philosophical puzzle between frequentist coverage and post-data certainty.
3. Explain how Bayesian inference resolves this paradox.

---

## Given

- Observation model: $Y_i = \theta + X_i$
- Perturbations: $X_1, X_2 \overset{\text{iid}}{\sim} \text{Uniform}(\{-1, +1\})$
- Four equally likely combinations: $(+1, +1), (+1, -1), (-1, +1), (-1, -1)$, each with probability $1/4$.
- Realized observations: $Y_1 = 15, Y_2 = 17$.

---

## Required

1. Prove $P_\theta(\theta \in C) = 0.75$.
2. Compute realized set $C$ and analyze certainty.
3. Compare frequentist procedure guarantee with Bayesian posterior distribution.

---

## Concepts Used

- [[Confidence Intervals and Confidence Sets]]
- [[Bayesian Inference]]
- [[Credible Intervals]]

---

## Solution

### Step 1: Verification of Coverage Probability $P_\theta(\theta \in C) = 3/4$
Since $X_1, X_2$ are independent and each takes value $\pm 1$ with probability $1/2$, the joint outcome $(X_1, X_2)$ has 4 equally likely realizations, each occurring with probability $\frac{1}{2} \times \frac{1}{2} = \frac{1}{4}$.

Let us trace the realized observations $(Y_1, Y_2)$, the decision rule, the resulting confidence set $C$, and whether $\theta \in C$:

| Row | $(X_1, X_2)$ | $(Y_1, Y_2)$ | Condition | Rule Used | Computed Set $C$ | Does $\theta \in C$? |
|---|---|---|---|---|---|---|
| 1 | $(+1, +1)$ | $(\theta + 1, \theta + 1)$ | $Y_1 = Y_2$ | $\{Y_1 - 1\}$ | $\{\theta + 1 - 1\} = \{\theta\}$ | **Yes** ($\theta = \theta$) |
| 2 | $(+1, -1)$ | $(\theta + 1, \theta - 1)$ | $Y_1 \ne Y_2$ | $\{(Y_1 + Y_2)/2\}$ | $\left\{\frac{2\theta}{2}\right\} = \{\theta\}$ | **Yes** ($\theta = \theta$) |
| 3 | $(-1, +1)$ | $(\theta - 1, \theta + 1)$ | $Y_1 \ne Y_2$ | $\{(Y_1 + Y_2)/2\}$ | $\left\{\frac{2\theta}{2}\right\} = \{\theta\}$ | **Yes** ($\theta = \theta$) |
| 4 | $(-1, -1)$ | $(\theta - 1, \theta - 1)$ | $Y_1 = Y_2$ | $\{Y_1 - 1\}$ | $\{\theta - 1 - 1\} = \{\theta - 2\}$ | **No** ($\theta \ne \theta - 2$) |

**Evaluation of Total Coverage:**
$$\theta \in C \text{ in Rows 1, 2, and 3.}$$
$$\theta \notin C \text{ in Row 4 only.}$$

$$P_\theta(\theta \in C) = P(\text{Row 1}) + P(\text{Row 2}) + P(\text{Row 3}) = \frac{1}{4} + \frac{1}{4} + \frac{1}{4} = \frac{3}{4} = 75\%$$

This holds identically for every possible real number $\theta$. Therefore, $C$ is mathematically a **$75\%$ confidence set**.

---

### Step 2: The Realized Data and the Puzzle
Now, suppose the experiment is run and we observe:
$$Y_1 = 15, \quad Y_2 = 17$$

1. **Apply the Rule:**
   Since $Y_1 \ne Y_2$ ($15 \ne 17$), we follow the second branch:
   $$C = \left\{\frac{15 + 17}{2}\right\} = \left\{\frac{32}{2}\right\} = \{16\}$$

2. **Deduce the True Value of $\theta$:**
   Since $Y_1 = 15$ and $Y_2 = 17$, the difference is $Y_2 - Y_1 = 2$.
   Because $X_i \in \{-1, +1\}$, the difference $Y_2 - Y_1 = X_2 - X_1$ can equal $2$ **if and only if**:
   $$X_2 = +1 \quad \text{and} \quad X_1 = -1$$
   Substituting these into our observation equations:
   $$15 = \theta - 1 \implies \theta = 16$$
   $$17 = \theta + 1 \implies \theta = 16$$
   Both equations uniquely and unequivocally demand that $\theta = 16$.

3. **The Frequentist Paradox:**
   We are **$100\%$ certain** that $\theta = 16$. There is no physical or mathematical possibility of $\theta$ being any other value.
   Yet, by definition, the procedure $C$ is a **$75\%$ confidence interval**.
   If a statistician reports, *"The 75% confidence interval is {16},"*, a non-expert will believe there is a 25% chance of being wrong, whereas the chance of error on this specific dataset is exactly zero!

---

### Step 3: Bayesian Resolution
In Bayesian inference, we specify a prior distribution $f(\theta)$ over possible values of $\theta$.
Given $Y = (15, 17)$, the likelihood function is:
$$L(\theta) = f(15, 17 \mid \theta) = P(X_1 = 15 - \theta) P(X_2 = 17 - \theta)$$
This product is strictly positive if and only if:
$$(15 - \theta) \in \{-1, +1\} \quad \text{and} \quad (17 - \theta) \in \{-1, +1\}$$
The only integer satisfying both conditions is $\theta = 16$.
For any prior $f(\theta) > 0$ at $\theta = 16$:
$$f(\theta \mid Y_1 = 15, Y_2 = 17) = \begin{cases} 1 & \text{if } \theta = 16 \\ 0 & \text{if } \theta \ne 16 \end{cases}$$
The Bayesian **posterior probability** is:
$$P(\theta = 16 \mid \text{data}) = 1.0 \quad (100\%)$$

---

## Result

- Pre-experimental frequentist coverage: $P_\theta(\theta \in C) = 75\%$
- Post-data realized set: $C = \{16\}$
- Actual probability that $\theta \in \{16\}$ given observed data: $100\%$

---

## Why This Works

The confidence coefficient ($75\%$) is a pre-experimental average over all possible future datasets. It reflects the fact that across many random runs, the procedure fails when $(X_1, X_2) = (-1, -1)$. But when the realized data reveal $Y_1 \ne Y_2$, we know with certainty that we are in Rows 2 or 3, where failure is impossible. Frequentist confidence intervals do not condition on the observed ancillary statistic $\lvert Y_1 - Y_2 \rvert$.

---

## Common Mistakes

- Confusing the pre-data coverage probability $P_\theta(\theta \in C) = 0.75$ with the post-data posterior probability $P(\theta \in C \mid \mathbf{Y})$.
- Assuming the failure in Row 4 occurs because the formula is wrong; it fails because when both errors are $-1$, the rule shifts the wrong way, missing $\theta$ by 2 units.

---

## Related Concepts

- [[Confidence Intervals and Confidence Sets]]
- [[Bayesian Inference]]
- [[Credible Intervals]]

---

## Sources

- [[01 - Sources/Lectures/Point Estimation and Confidence Intervals.pdf]]
- [[01 - Sources/Lectures/Bayesian_Inference.pdf]]
