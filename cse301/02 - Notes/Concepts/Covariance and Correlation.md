---
type: concept
course: cse301
status: active
---

# Covariance and Correlation

## Definition

**Covariance** measures the degree of linear association between two random variables $X$ and $Y$:
$$\operatorname{Cov}(X, Y) = \mathbb{E}\left[ (X - \mathbb{E}[X])(Y - \mathbb{E}[Y]) \right]$$

Expanding the algebraic product yields the standard computational formula:
$$\operatorname{Cov}(X, Y) = \mathbb{E}[XY] - \mathbb{E}[X]\mathbb{E}[Y]$$

**Correlation** (Pearson's correlation coefficient) is the dimensionless, standardized measure of linear relationship:
$$\rho(X, Y) = \operatorname{Corr}(X, Y) = \frac{\operatorname{Cov}(X, Y)}{\operatorname{SD}(X)\operatorname{SD}(Y)} = \frac{\operatorname{Cov}(X, Y)}{\sqrt{\operatorname{Var}(X)\operatorname{Var}(Y)}}$$

---

## Fundamental Properties of Covariance

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

## Fundamental Properties of Correlation

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

## Independence vs. Uncorrelatedness

- **Independence implies Uncorrelatedness:**
  If $X$ and $Y$ are independent ($X \perp Y$), then $\mathbb{E}[XY] = \mathbb{E}[X]\mathbb{E}[Y]$. Therefore:
  $$\operatorname{Cov}(X, Y) = 0 \implies \rho(X, Y) = 0$$
- **Uncorrelatedness DOES NOT Imply Independence:**
  $\operatorname{Cov}(X, Y) = 0$ only means there is no *linear* relationship; there can still be a perfect non-linear deterministic relationship.

### Classic Counterexample:
Let $X \sim \operatorname{Unif}(-1, 1)$ (symmetric around 0, so $\mathbb{E}[X] = 0$).
Let $Y = X^2$ (clearly $Y$ is completely dependent on $X$).
Compute covariance:
$$\mathbb{E}[XY] = \mathbb{E}[X \cdot X^2] = \mathbb{E}[X^3] = \int_{-1}^1 \frac{x^3}{2} \, dx = 0$$
$$\mathbb{E}[X]\mathbb{E}[Y] = 0 \times \mathbb{E}[Y] = 0$$
$$\operatorname{Cov}(X, Y) = \mathbb{E}[XY] - \mathbb{E}[X]\mathbb{E}[Y] = 0 - 0 = 0$$
Thus, $X$ and $Y$ are **uncorrelated ($\rho = 0$), yet completely dependent**!

*(Exception: If $(X, Y)$ have a **Bivariate Normal distribution**, then uncorrelatedness DOES imply independence!)*.

---

## Cross-Topic Connections / Exam Relevance

- **Foundations:** Defined from expectation and variance in [[Random Variables and Probability Distributions]].
- **Joint Densities:** Evaluated via double integrals over joint distributions in [[Joint and Marginal Distributions]].
- **Correlation Bound:** The property $-1 \le \rho \le 1$ is a direct consequence of the [[Cauchy-Schwarz and Jensen Inequalities|Cauchy-Schwarz Inequality]].
- **Indicators:** Covariance between indicator variables $I_A, I_B$ is $P(A \cap B) - P(A)P(B)$ (see application in [[Problem — Indicator Variables for Distinct Birthday Counts]]).
- **Portfolio Theory & Estimation:** Central to the variance of sample means $\bar{X}$ and the [[Bias-Variance Decomposition]].
- **Conditioning:** Bivariate linear regression predicts $\mathbb{E}[Y \mid X] = \mu_Y + \rho \frac{\sigma_Y}{\sigma_X}(X - \mu_X)$ (see [[Conditional Expectation]]).

---

## Sources & Traceability

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lecture 13, pages 40–43)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_8.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Chapter 7, Sections 7.1–7.3)
