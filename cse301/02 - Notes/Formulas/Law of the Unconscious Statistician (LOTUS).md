---
type: formula
course: cse301
status: active
order: 12
---

# Law of the Unconscious Statistician (LOTUS)

> 📖 **Reading Order:** Step 12 of 92 | **Module 2:** Random Variables and Distributions  
> ◄ **Previous:** [[Covariance and Correlation]] | ► **Next:** [[Moment Generating Functions]]

---

## Mathematical Statement

Let $X$ be a random variable, and let $g: \mathbb{R} \to \mathbb{R}$ be a measurable function. The **Law of the Unconscious Statistician (LOTUS)** states that the expected value of the transformed random variable $Y = g(X)$ can be computed directly using the distribution of $X$, without first finding the distribution of $Y$:

- **Discrete Case:**
  $$\mathbb{E}[g(X)] = \sum_{x} g(x) \, p_X(x)$$

- **Continuous Case:**
  $$\mathbb{E}[g(X)] = \int_{-\infty}^\infty g(x) \, f_X(x) \, dx$$

*(Subject to absolute convergence: $\mathbb{E}[\lvert g(X) \rvert] < \infty$)*.

---

## Why Is It Called "Unconscious Statistician"?

Beginners often treat $\mathbb{E}[g(X)]$ as if it were defined as $\int g(x) f_X(x) dx$, applying the rule "unconsciously" without realizing that by definition:
$$\mathbb{E}[Y] = \int_{-\infty}^\infty y \, f_Y(y) \, dy$$
LOTUS is a profound and non-trivial mathematical theorem asserting that these two integrals are strictly equal!

---

## Proof Sketch (Discrete Case)

Let $Y = g(X)$. The possible values of $Y$ form a set $\mathcal{Y}$. By definition of expectation for $Y$:
$$\mathbb{E}[Y] = \sum_{y \in \mathcal{Y}} y \, P(Y = y)$$

Notice that the event $\{Y = y\}$ is the disjoint union of elementary events $\{X = x\}$ for all $x$ such that $g(x) = y$:
$$P(Y = y) = \sum_{x: g(x) = y} P(X = x) = \sum_{x: g(x) = y} p_X(x)$$

Substitute this into the expectation of $Y$:
$$\mathbb{E}[Y] = \sum_{y \in \mathcal{Y}} y \left( \sum_{x: g(x) = y} p_X(x) \right) = \sum_{y \in \mathcal{Y}} \sum_{x: g(x) = y} y \, p_X(x)$$

Since $g(x) = y$ inside the inner summation, we replace $y$ with $g(x)$:
$$\mathbb{E}[Y] = \sum_{y \in \mathcal{Y}} \sum_{x: g(x) = y} g(x) \, p_X(x) = \sum_{x} g(x) \, p_X(x)$$
$\blacksquare$

---

## Multidimensional LOTUS (Joint Case)

For functions of multiple random variables $g(X, Y)$:
- **Discrete:**
  $$\mathbb{E}[g(X, Y)] = \sum_x \sum_y g(x, y) \, p_{X, Y}(x, y)$$
- **Continuous:**
  $$\mathbb{E}[g(X, Y)] = \int_{-\infty}^\infty \int_{-\infty}^\infty g(x, y) \, f_{X, Y}(x, y) \, dx \, dy$$

### Key Immediate Consequence: Linearity of Expectation
Setting $g(X, Y) = aX + bY$:
$$\mathbb{E}[aX + bY] = \iint (ax + by) f_{X,Y}(x, y) dx dy = a \int x \left( \int f_{X,Y}(x, y) dy \right) dx + b \int y \left( \int f_{X,Y}(x, y) dx \right) dy$$
$$= a \int x f_X(x) dx + b \int y f_Y(y) dy = a\mathbb{E}[X] + b\mathbb{E}[Y]$$
This proves that **linearity of expectation holds for ANY random variables**, whether independent or dependent!

---

## Application Examples

### 1. Second Moment and Variance
To calculate $\operatorname{Var}(X) = \mathbb{E}[X^2] - (\mathbb{E}[X])^2$, set $g(x) = x^2$:
$$\mathbb{E}[X^2] = \int_{-\infty}^\infty x^2 f_X(x) \, dx$$
Without LOTUS, one would have to derive the PDF of $Y = X^2$ using change of variables, and then integrate $y f_Y(y) dy$. LOTUS bypasses this entire detour.

### 2. Moment Generating Function Evaluation
Evaluating $M_X(t) = \mathbb{E}[e^{tX}]$ uses $g(x) = e^{tx}$:
$$M_X(t) = \int_{-\infty}^\infty e^{tx} f_X(x) \, dx$$

---

## Common Pitfalls

- **Do NOT distribute non-linear functions inside the expectation:**
  $$\mathbb{E}[g(X)] \ne g(\mathbb{E}[X])$$
  For example, $\mathbb{E}[X^2] \ne (\mathbb{E}[X])^2$ (their difference is $\operatorname{Var}(X) \ge 0$).
  By Jensen's Inequality, if $g$ is convex, $\mathbb{E}[g(X)] \ge g(\mathbb{E}[X])$.

---

## Related Notes

- [[Random Variables and Probability Distributions]] — Foundational expectation definitions.
- [[Joint and Marginal Distributions]] — Joint integration and 2D LOTUS.
- [[Moment Generating Functions]] — Applied to $g(x) = e^{tx}$.

---

## Sources & Traceability

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lectures 6 & 10, pages 18, 31)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_4.pdf` & `6.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Section 4.1 & Section 7.2)
