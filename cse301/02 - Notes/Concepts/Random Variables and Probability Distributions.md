---
type: concept
course: cse301
status: active
---

# Random Variables and Probability Distributions

## Definition

A **random variable (RV)** $X$ is a function that maps outcomes from a sample space $S$ to real numbers:
$$X: S \to \mathbb{R}$$

Despite the name, a random variable is **neither random nor a variable** in the algebraic sense—it is a **deterministic function** whose input is determined by a random experiment.

The **probability distribution** of $X$ describes the allocation of probabilities across the possible values that $X$ can take in $\mathbb{R}$.

---

## Cumulative Distribution Function (CDF)

The **Cumulative Distribution Function (CDF)** of any random variable $X$ (discrete, continuous, or mixed) is defined as:
$$F_X(x) = P(X \le x), \quad \text{for all } x \in \mathbb{R}$$

### Fundamental Properties of Any Valid CDF:
1. **Monotonically Non-Decreasing:** If $x_1 \le x_2$, then $F_X(x_1) \le F_X(x_2)$.
2. **Limits at Infinities:**
   $$\lim_{x \to -\infty} F_X(x) = 0 \quad \text{and} \quad \lim_{x \to \infty} F_X(x) = 1$$
3. **Right-Continuous:** For any $x_0$:
   $$\lim_{x \to x_0^+} F_X(x) = F_X(x_0)$$

### Interval Probabilities via CDF:
- $P(a < X \le b) = F_X(b) - F_X(a)$
- $P(X = b) = F_X(b) - \lim_{x \to b^-} F_X(x)$ (the jump size at $b$)
- $P(X > a) = 1 - F_X(a)$

---

## Discrete vs. Continuous Random Variables

| Feature | Discrete Random Variable | Continuous Random Variable |
|---|---|---|
| **Support** | Finite or countably infinite set $\{x_1, x_2, \dots\}$ | Uncountable interval or union of intervals |
| **Probability Function** | **Probability Mass Function (PMF)** $p_X(x) = P(X = x)$ | **Probability Density Function (PDF)** $f_X(x) = \frac{d}{dx}F_X(x)$ |
| **Probability at a Single Point** | $P(X = x) = p_X(x) \ge 0$ (can be positive) | $P(X = x) = 0$ for every individual point $x$ |
| **Normalization** | $\sum_x p_X(x) = 1$ | $\int_{-\infty}^\infty f_X(x)\,dx = 1$ |
| **Event Probability** | $P(X \in A) = \sum_{x \in A} p_X(x)$ | $P(X \in A) = \int_A f_X(x)\,dx$ |
| **CDF Connection** | $F_X(x) = \sum_{t \le x} p_X(t)$ (step function) | $F_X(x) = \int_{-\infty}^x f_X(t)\,dt$ (continuous curve) |

---

## Expectation and Variance

### Expectation (Mean)
The expectation $\mathbb{E}[X]$ represents the probability-weighted average (center of mass) of the distribution:
- **Discrete:** $\mathbb{E}[X] = \sum_{x} x \, p_X(x)$
- **Continuous:** $\mathbb{E}[X] = \int_{-\infty}^\infty x \, f_X(x) \, dx$

*(Provided the sum or integral converges absolutely: $\mathbb{E}[\lvert X \rvert] < \infty$)*.

#### Properties of Expectation:
1. **Linearity (holds universally, even if variables are dependent):**
   $$\mathbb{E}[aX + bY + c] = a\mathbb{E}[X] + b\mathbb{E}[Y] + c$$
2. **Product Rule for Independent RVs:**
   If $X$ and $Y$ are independent: $\mathbb{E}[XY] = \mathbb{E}[X]\mathbb{E}[Y]$.

### Variance and Standard Deviation
The **variance** quantifies the spread or dispersion around the mean:
$$\operatorname{Var}(X) = \mathbb{E}\left[ (X - \mathbb{E}[X])^2 \right] = \mathbb{E}[X^2] - (\mathbb{E}[X])^2$$

The **standard deviation** is in the same units as $X$:
$$\operatorname{SD}(X) = \sqrt{\operatorname{Var}(X)}$$

#### Properties of Variance:
1. **Non-negativity:** $\operatorname{Var}(X) \ge 0$. $\operatorname{Var}(X) = 0 \iff X$ is a constant.
2. **Scaling:** $\operatorname{Var}(aX + b) = a^2 \operatorname{Var}(X)$.
3. **Sum of Variables:**
   $$\operatorname{Var}(X + Y) = \operatorname{Var}(X) + \operatorname{Var}(Y) + 2\operatorname{Cov}(X, Y)$$
   If $X$ and $Y$ are independent (or uncorrelated), $\operatorname{Cov}(X, Y) = 0$, so:
   $$\operatorname{Var}(X + Y) = \operatorname{Var}(X) + \operatorname{Var}(Y)$$

---

## Edge Cases & Common Pitfalls

1. **Confusing PDF Value with Probability:**
   - For a continuous RV, $f_X(x)$ is a probability **density**, not a probability.
   - $f_X(x)$ can exceed 1 (e.g., Uniform on $[0, 0.5]$ has $f(x) = 2$).
   - The probability of any single point is strictly zero: $P(X = 0.5) = 0$.
2. **Assuming Variance is Linear:**
   - $\operatorname{Var}(2X) = 4\operatorname{Var}(X)$, NOT $2\operatorname{Var}(X)$.
   - $\operatorname{Var}(X - Y) = \operatorname{Var}(X) + \operatorname{Var}(Y) - 2\operatorname{Cov}(X, Y)$. For independent RVs, $\operatorname{Var}(X - Y) = \operatorname{Var}(X) + \operatorname{Var}(Y)$ (variances add, never subtract!).
3. **Non-existent Expectations:**
   - Certain distributions like the Cauchy distribution have no mean because $\int \lvert x \rvert f(x) dx = \infty$.

---

## Cross-Topic Connections / Exam Relevance

- **Distributions:** Foundation for specific families in [[Discrete Probability Distributions]] and [[Continuous Probability Distributions]].
- **Transformations:** Evaluated via [[Law of the Unconscious Statistician (LOTUS)]] and Jacobian changes of variables.
- **Multivariate:** Extended to pairs and vectors in [[Joint and Marginal Distributions]] and [[Covariance and Correlation]].
- **Estimation:** Forms the sample data generating process in [[Point Estimation]] and [[Maximum Likelihood Estimation]].

---

## Sources & Traceability

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lectures 6 & 10, pages 17–19, 30–32)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_4.pdf` & `6.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Chapters 3 & 5)
