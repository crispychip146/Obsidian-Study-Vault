---
type: formula
course: cse301
status: active
order: 21
---

# Eve's Law (Law of Total Variance)

> 📖 **Reading Order:** Step 21 of 92 | **Module 3:** Conditional Probability and Conditioning  
> ◄ **Previous:** [[Adam's Law (Law of Total Expectation)]] | ► **Next:** [[Monty Hall Problem Example]]

---

## Building the idea

Variation can come from two sources: spread within a group and differences between group means. Eve's law separates them: $\operatorname{Var}(Y)=E[\operatorname{Var}(Y\mid X)]+\operatorname{Var}(E[Y\mid X])$.

For a proof strategy, write $Y-E[Y]=(Y-E[Y\mid X])+(E[Y\mid X]-E[Y])$. Squaring produces two square terms and a cross term. Conditional on $X$, the first residual has mean zero and the second term is known, so the cross term averages to zero. The two remaining terms give the decomposition.

The first term is an average conditional variance, not the variance of a conditional variance. The second is the variance of the conditional mean. If all groups have the same mean, the second vanishes even if their spreads differ. If each group has no internal randomness, the first vanishes and all variation comes from which group occurs.

## Formula

The **Law of Total Variance**, known affectionately as **Eve's Law** (complementing [[Adam's Law (Law of Total Expectation)|Adam's Law]]), decomposes the unconditional variance of a random variable $Y$ into two distinct, orthogonal sources of variability when conditioned on $X$:

$$\operatorname{Var}(Y) = \mathbb{E}\left[ \operatorname{Var}(Y \mid X) \right] + \operatorname{Var}\left( \mathbb{E}[Y \mid X] \right)$$

### Mnemonic:
$$\mathbf{E}\mathbf{V} + \mathbf{V}\mathbf{E} \quad \text{("EVVE")}$$
- $\mathbf{E}[\mathbf{V}]$: Expected value of conditional Variance.
- $\mathbf{V}[\mathbf{E}]$: Variance of conditional Expectation.

---

## Intuition

### Statistical Interpretation: ANOVA Decomposition

Eve's Law is the probabilistic foundation of the **Analysis of Variance (ANOVA)**:
- **$\mathbb{E}[\operatorname{Var}(Y \mid X)]$ = Within-Group Variance (Unexplained Variance):**
  The average spread of $Y$ *within* subpopulations with fixed $X$. This variation cannot be explained by knowing $X$.
- **$\operatorname{Var}(\mathbb{E}[Y \mid X])$ = Between-Group Variance (Explained Variance):**
  The spread of the subpopulation group averages across different values of $X$. This measures the variation in $Y$ that is directly accounted for by knowing $X$.

### Consequence for Best Prediction:
Because $\operatorname{Var}(\mathbb{E}[Y \mid X]) \ge 0$:
$$\operatorname{Var}(Y) \ge \mathbb{E}[\operatorname{Var}(Y \mid X)]$$
Conditioning **reduces variance on average**. Information never increases uncertainty on average!

---

## Derivation

### Rigorous Derivation

Recall the fundamental definition of variance:
$$\operatorname{Var}(Y) = \mathbb{E}[Y^2] - (\mathbb{E}[Y])^2$$

Similarly, for the conditional variance given $X$:
$$\operatorname{Var}(Y \mid X) = \mathbb{E}[Y^2 \mid X] - (\mathbb{E}[Y \mid X])^2 \implies \mathbb{E}[Y^2 \mid X] = \operatorname{Var}(Y \mid X) + (\mathbb{E}[Y \mid X])^2$$

Now take the unconditional expectation of $\mathbb{E}[Y^2 \mid X]$ using Adam's Law:
$$\mathbb{E}[Y^2] = \mathbb{E}\left[ \mathbb{E}[Y^2 \mid X] \right] = \mathbb{E}\left[ \operatorname{Var}(Y \mid X) + (\mathbb{E}[Y \mid X])^2 \right]$$
By linearity of expectation:
$$\mathbb{E}[Y^2] = \mathbb{E}\left[ \operatorname{Var}(Y \mid X) \right] + \mathbb{E}\left[ (\mathbb{E}[Y \mid X])^2 \right]$$

Next, examine the second term $(\mathbb{E}[Y])^2$:
By Adam's Law, $\mathbb{E}[Y] = \mathbb{E}[\mathbb{E}[Y \mid X]]$. Therefore:
$$(\mathbb{E}[Y])^2 = \left( \mathbb{E}\left[ \mathbb{E}[Y \mid X] \right] \right)^2$$

Subtracting $(\mathbb{E}[Y])^2$ from $\mathbb{E}[Y^2]$:
$$\operatorname{Var}(Y) = \mathbb{E}\left[ \operatorname{Var}(Y \mid X) \right] + \underbrace{\mathbb{E}\left[ (\mathbb{E}[Y \mid X])^2 \right] - \left( \mathbb{E}\left[ \mathbb{E}[Y \mid X] \right] \right)^2}_{= \operatorname{Var}(\mathbb{E}[Y \mid X])}$$

Recognizing the bracketed term as the definition of the variance of the random variable $\mathbb{E}[Y \mid X]$:
$$\operatorname{Var}(Y) = \mathbb{E}\left[ \operatorname{Var}(Y \mid X) \right] + \operatorname{Var}\left( \mathbb{E}[Y \mid X] \right)$$
$\blacksquare$

---

## Example

### Application: Variance of a Compound Random Sum

Let $S_N = \sum_{i=1}^N X_i$, where $N$ is a random variable, and $X_i$ are i.i.d. with mean $\mu_X$ and variance $\sigma_X^2$, independent of $N$.

1. **Conditional Expectation and Variance given $N$:**
   $$\mathbb{E}[S_N \mid N] = N \mu_X$$
   $$\operatorname{Var}(S_N \mid N) = N \sigma_X^2$$

2. **EV Term:**
   $$\mathbb{E}[\operatorname{Var}(S_N \mid N)] = \mathbb{E}[N \sigma_X^2] = \sigma_X^2 \mathbb{E}[N] = \mathbb{E}[N] \operatorname{Var}(X)$$

3. **VE Term:**
   $$\operatorname{Var}(\mathbb{E}[S_N \mid N]) = \operatorname{Var}(N \mu_X) = \mu_X^2 \operatorname{Var}(N) = (\mathbb{E}[X])^2 \operatorname{Var}(N)$$

4. **Summing via Eve's Law (Wald's Variance Identity):**
   $$\operatorname{Var}(S_N) = \mathbb{E}[N]\operatorname{Var}(X) + (\mathbb{E}[X])^2 \operatorname{Var}(N)$$

---

## What to carry forward

With finite second moments, both components are nonnegative. In [[Problem — Compound Random Sum via Adam and Eve's Laws]], they represent variable summand sizes and a variable number of summands.

## Related notes

- [[Problem — Compound Random Sum via Adam and Eve's Laws]]

## Sources

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lecture 16, pages 50–53)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_10.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Section 9.3: Law of Total Variance)
