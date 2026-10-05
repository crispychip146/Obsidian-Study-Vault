---
type: concept
course: cse301
status: active
order: 11
---

# Covariance and Correlation

> 📖 **Reading Order:** Step 11 of 92 | **Module 2:** Random Variables and Distributions  
> ◄ **Previous:** [[Joint and Marginal Distributions]] | ► **Next:** [[Law of the Unconscious Statistician (LOTUS)]]

---

## Building the idea

Center each variable by subtracting its mean. When both centered values usually have the same sign, their product tends to be positive; when their signs oppose, it tends to be negative. The expectation of this product is covariance.

Dividing covariance by the two standard deviations removes measurement units and produces correlation. This makes a relationship measured in meters comparable with the same relationship measured in centimeters, provided both variances are finite and positive.

Covariance detects linear association. It can be zero while dependence remains: if $X$ is symmetric about zero and $Y=X^2$, the positive and negative contributions to $E[X^3]$ cancel even though $Y$ is determined by $X$. Independence implies zero covariance when the moments exist; the reverse does not generally follow.

Covariance also tells us what is missing when we add variances: $\operatorname{Var}(X+Y)=\operatorname{Var}(X)+\operatorname{Var}(Y)+2\operatorname{Cov}(X,Y)$.

## Definition

**Covariance** measures the degree of linear association between two random variables $X$ and $Y$:
$$\operatorname{Cov}(X, Y) = \mathbb{E}\left[ (X - \mathbb{E}[X])(Y - \mathbb{E}[Y]) \right]$$

Expanding the algebraic product yields the standard computational formula:
$$\operatorname{Cov}(X, Y) = \mathbb{E}[XY] - \mathbb{E}[X]\mathbb{E}[Y]$$

**Correlation** (Pearson's correlation coefficient) is the dimensionless, standardized measure of linear relationship:
$$\rho(X, Y) = \operatorname{Corr}(X, Y) = \frac{\operatorname{Cov}(X, Y)}{\operatorname{SD}(X)\operatorname{SD}(Y)} = \frac{\operatorname{Cov}(X, Y)}{\sqrt{\operatorname{Var}(X)\operatorname{Var}(Y)}}$$

---

## How It Works

### Fundamental Properties of Covariance

1. **Self-Covariance is Variance:**
   $$\operatorname{Cov}(X, X) = \operatorname{Var}(X)$$

2. **Symmetry:**
   $$\operatorname{Cov}(X, Y) = \operatorname{Cov}(Y, X)$$

3. **Invariance to Constants:**
   $$\operatorname{Cov}(X + c, Y + d) = \operatorname{Cov}(X, Y) \quad \text{for any constants } c, d \in \mathbb{R}$$

4. **Bilinearity (Strictly Linear in Both Arguments):**
   $$\operatorname{Cov}(aX + bY, Z) = a\operatorname{Cov}(X, Z) + b\operatorname{Cov}(Y, Z)$$
   More generally, for linear combinations $\sum_{i=1}^m a_i X_i$ and $\sum_{j=1}^n b_j Y_j$:
   $$\operatorname{Cov}\left( \sum_{i=1}^m a_i X_i, \sum_{j=1}^n b_j Y_j \right) = \sum_{i=1}^m \sum_{j=1}^n a_i b_j \operatorname{Cov}(X_i, Y_j)$$

5. **Variance of a Sum:**
   $$\operatorname{Var}(X + Y) = \operatorname{Var}(X) + \operatorname{Var}(Y) + 2\operatorname{Cov}(X, Y)$$
   For $n$ random variables:
   $$\operatorname{Var}\left( \sum_{i=1}^n X_i \right) = \sum_{i=1}^n \operatorname{Var}(X_i) + 2 \sum_{1 \le i < j \le n} \operatorname{Cov}(X_i, X_j)$$

---
### Fundamental Properties of Correlation

1. **Bounded Range (Cauchy-Schwarz):**
   $$-1 \le \rho(X, Y) \le 1$$
   *Proof Sketch:* Consider the non-negative quadratic function $g(t) = \operatorname{Var}(tX + Y) \ge 0$. Expanding: $t^2 \operatorname{Var}(X) + 2t\operatorname{Cov}(X, Y) + \operatorname{Var}(Y) \ge 0$. A quadratic in $t$ is non-negative everywhere if and only if its discriminant $\Delta \le 0$:
   $$4\operatorname{Cov}(X, Y)^2 - 4\operatorname{Var}(X)\operatorname{Var}(Y) \le 0 \implies \operatorname{Cov}(X, Y)^2 \le \operatorname{Var}(X)\operatorname{Var}(Y)$$
   Dividing both sides by $\operatorname{Var}(X)\operatorname{Var}(Y)$ gives $\rho^2 \le 1 \implies -1 \le \rho \le 1$.

2. **Perfect Linear Relationships:**
   - $\rho(X, Y) = 1 \iff Y = aX + b$ with $a > 0$ almost surely.
   - $\rho(X, Y) = -1 \iff Y = aX + b$ with $a < 0$ almost surely.

3. **Invariance to Positive Affine Scaling:**
   If $a, c > 0$: $\rho(aX + b, cY + d) = \rho(X, Y)$.

---

## Exam Relevance

### Cross-Topic Connections / Exam Relevance

- **Foundations:** Defined from expectation and variance in [[Random Variables and Probability Distributions]].
- **Joint Densities:** Evaluated via double integrals over joint distributions in [[Joint and Marginal Distributions]].
- **Correlation Bound:** The property $-1 \le \rho \le 1$ is a direct consequence of the [[Cauchy-Schwarz and Jensen Inequalities|Cauchy-Schwarz Inequality]].
- **Indicators:** Covariance between indicator variables $I_A, I_B$ is $P(A \cap B) - P(A)P(B)$ (see application in [[Problem — Indicator Variables for Distinct Birthday Counts]]).
- **Portfolio Theory & Estimation:** Central to the variance of sample means $\bar{X}$ and the [[Bias-Variance Decomposition]].
- **Conditioning:** Bivariate linear regression predicts $\mathbb{E}[Y \mid X] = \mu_Y + \rho \frac{\sigma_Y}{\sigma_X}(X - \mu_X)$ (see [[Conditional Expectation]]).

---

## What to carry forward

[[Cauchy-Schwarz and Jensen Inequalities]] explains why correlation lies between $-1$ and $1$. Zero variance makes the usual correlation undefined rather than zero.

## Related notes

- [[Cauchy-Schwarz and Jensen Inequalities]]

## Sources

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lecture 13, pages 40–43)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_8.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Chapter 7, Sections 7.1–7.3)
