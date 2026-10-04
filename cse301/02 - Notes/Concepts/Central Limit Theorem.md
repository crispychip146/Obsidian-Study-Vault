---
type: concept
course: cse301
status: active
order: 32
---

# Central Limit Theorem

> 📖 **Reading Order:** Step 32 of 92 | **Module 5:** Convergence of Random Variables and Asymptotics  
> ◄ **Previous:** [[Law of Large Numbers]] | ► **Next:** [[Normal Approximation to Binomial and Poisson Example]]

---

---

## Starting Point and the Problem

Probability and statistical inference model uncertainty in physical and computer systems. When analyzing stochastic phenomena related to Central Limit Theorem, naive counting or deterministic approximations fail. We establish a formal mathematical foundation to quantify outcomes and evaluate expectations rigorously.

---

## Developing the Idea

By formalizing sample spaces, probability measures, state transitions, or likelihood ratios, Central Limit Theorem reveals the underlying structural mechanics that govern random variables and estimation errors.

---

## Definition

The **Central Limit Theorem (CLT)** is one of the most remarkable and foundational theorems in all of mathematics. It states that the standardized sum (or sample average) of a large number of independent, identically distributed (i.i.d.) random variables approaches a **Standard Normal distribution**, regardless of the shape of the underlying population distribution (provided the population has finite variance).

---

---

## How It Works

### Formal Statement (Lindeberg-Lévy CLT)

Let $X_1, X_2, \dots, X_n$ be an i.i.d. sequence of random variables with:
- Mean $\mathbb{E}[X_i] = \mu$
- Variance $\operatorname{Var}(X_i) = \sigma^2 \in (0, \infty)$

Let $S_n = \sum_{i=1}^n X_i$ be the sum, and let $\bar{X}_n = \frac{S_n}{n}$ be the sample mean.
Define the standardized random variable:
$$Z_n = \frac{\bar{X}_n - \mu}{\sigma / \sqrt{n}} = \frac{S_n - n\mu}{\sigma \sqrt{n}}$$

Then, as $n \to \infty$, $Z_n$ **converges in distribution** to a Standard Normal random variable:
$$Z_n \xrightarrow{d} \mathcal{N}(0, 1)$$

That is, for any real number $z \in \mathbb{R}$:
$$\lim_{n \to \infty} P(Z_n \le z) = \Phi(z) = \int_{-\infty}^z \frac{1}{\sqrt{2\pi}} e^{-u^2 / 2} \, du$$

---
### Practical Rules of Thumb & Continuity Correction

### 1. Sample Size Rule:
- For moderately symmetric, light-tailed distributions, $n \ge 30$ is usually sufficient for accurate Gaussian approximations.
- For heavily skewed distributions (e.g., Exponential or Pareto), larger samples ($n \ge 100$) may be required.

### 2. Continuity Correction (Discretization Adjustment):
When approximating a discrete integer-valued random variable $X$ (like Binomial or Poisson) with a continuous Normal distribution:
- Each discrete integer $k$ is treated as covering the continuous interval $[k - 0.5, k + 0.5]$:
  - $P(X \le k) \approx P\left( Y_{\text{norm}} \le k + 0.5 \right)$
  - $P(X \ge k) \approx P\left( Y_{\text{norm}} \ge k - 0.5 \right)$
  - $P(X = k) \approx P\left( k - 0.5 \le Y_{\text{norm}} \le k + 0.5 \right)$
  - $P(a \le X \le b) \approx P\left( a - 0.5 \le Y_{\text{norm}} \le b + 0.5 \right)$

---

---

## Example

See worked numerical applications in the linked example notes.

---

## Technical Details

Refer to Blitzstein & Hwang for measure-theoretic details and moment generating properties.

---

## Important Properties and Why They Hold

### Proof Sketch via Moment Generating Functions

Assume the MGF of $X_i$ exists in a neighborhood of 0. Without loss of generality, center the variables by defining $Y_i = \frac{X_i - \mu}{\sigma}$, so that $\mathbb{E}[Y_i] = 0$ and $\operatorname{Var}(Y_i) = \mathbb{E}[Y_i^2] = 1$.
The standardized variable is:
$$Z_n = \frac{1}{\sqrt{n}} \sum_{i=1}^n Y_i$$

1. **MGF of $Y_i$ via Taylor Series:**
   $$M_Y(t) = 1 + t\mathbb{E}[Y] + \frac{t^2}{2}\mathbb{E}[Y^2] + o(t^2) = 1 + 0 + \frac{t^2}{2} + o(t^2)$$

2. **MGF of $Z_n$:**
   Because the $Y_i$ are independent:
   $$M_{Z_n}(t) = \left[ M_Y\left( \frac{t}{\sqrt{n}} \right) \right]^n = \left[ 1 + \frac{t^2}{2n} + o\left(\frac{t^2}{n}\right) \right]^n$$

3. **Limit as $n \to \infty$:**
   Using the standard limit $\lim_{n \to \infty} \left(1 + \frac{c}{n}\right)^n = e^c$ with $c = t^2 / 2$:
   $$\lim_{n \to \infty} M_{Z_n}(t) = e^{t^2 / 2}$$

This is precisely the MGF of a Standard Normal distribution $\mathcal{N}(0, 1)$.
By the **Continuity Theorem for MGFs**, convergence of MGFs implies convergence in distribution:
$$Z_n \xrightarrow{d} \mathcal{N}(0, 1)$$
$\blacksquare$

---
### Contrast: Law of Large Numbers vs. Central Limit Theorem

| Dimension | Law of Large Numbers (LLN) | Central Limit Theorem (CLT) |
|---|---|---|
| **What it describes** | The **destination** of $\bar{X}_n$ | The **distribution of fluctuations** around the destination |
| **Scaling Factor** | $\bar{X}_n - \mu$ (no scaling) | $\sqrt{n}(\bar{X}_n - \mu)$ (magnified by $\sqrt{n}$) |
| **Limiting Behavior** | Collapses to a deterministic constant $\mu$ (a Dirac delta spike) | Spreads out into a universal Gaussian bell curve $\mathcal{N}(0, \sigma^2)$ |
| **Convergence Type** | Convergence in probability ($\xrightarrow{P}$) or almost surely ($\xrightarrow{\text{a.s.}}$) | Convergence in distribution ($\xrightarrow{d}$) |

---

---

## Common Mistakes

- Confusing conditional probabilities with unconditional joint probabilities.
- Misapplying asymptotic normal approximations when sample sizes are small or distributions are heavily skewed.

---

## Exam Relevance

### Cross-Topic Connections / Exam Relevance

- **Confidence Intervals:** Derives the standard $95\%$ confidence interval formula $\bar{X}_n \pm 1.96 \frac{\sigma}{\sqrt{n}}$ (see [[Normal-Based Large-Sample Confidence Interval]]).
- **Hypothesis Testing:** Underpins the asymptotic normality of the [[Wald Test Statistic]].
- **Queueing Theory:** Heavy-traffic limits of queue lengths converge to reflected Brownian motions via functional CLTs.

---

---

## Related Concepts

- [[Probability Axioms and Naive Probability]]
- [[Random Variables and Probability Distributions]]

---

## Prerequisites

- [[Probability Axioms and Naive Probability]]

---

## Problems

- [[Problem — Birthday Collisions and Approximation]]

---

## Sources

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lectures 20–21, pages 61–63)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_11.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Section 10.5: Central Limit Theorem)
