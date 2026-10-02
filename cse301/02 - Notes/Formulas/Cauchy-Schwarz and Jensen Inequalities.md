---
type: formula
course: cse301
status: active
order: 28
---

# Cauchy-Schwarz and Jensen Inequalities

> 📖 **Reading Order:** Step 28 of 92 | **Module 4:** Probability Bounds and Inequalities  
> ◄ **Previous:** [[Chernoff Bound]] | ► **Next:** [[Comparison of Probability Bounds Example]]

---

## 1. Cauchy-Schwarz Inequality

### Mathematical Statement
For any two random variables $X$ and $Y$ with finite second moments ($\mathbb{E}[X^2] < \infty, \mathbb{E}[Y^2] < \infty$):

$$\lvert \mathbb{E}[XY] \rvert \le \sqrt{\mathbb{E}[X^2] \, \mathbb{E}[Y^2]}$$

with equality if and only if $P(Y = cX) = 1$ for some constant $c \in \mathbb{R}$ (or $X = 0$ almost surely).

---

### Proof Sketch
For any real scalar $t \in \mathbb{R}$, consider the squared random variable $(tX + Y)^2 \ge 0$.
Taking expectations:
$$h(t) = \mathbb{E}\left[ (tX + Y)^2 \right] = t^2 \mathbb{E}[X^2] + 2t \mathbb{E}[XY] + \mathbb{E}[Y^2] \ge 0$$

$h(t)$ is a quadratic polynomial $At^2 + Bt + C$ where $A = \mathbb{E}[X^2]$, $B = 2\mathbb{E}[XY]$, and $C = \mathbb{E}[Y^2]$.
Because $h(t) \ge 0$ for all $t \in \mathbb{R}$, the quadratic can have at most one real root, meaning its discriminant $\Delta = B^2 - 4AC \le 0$:
$$(2\mathbb{E}[XY])^2 - 4\mathbb{E}[X^2]\mathbb{E}[Y^2] \le 0$$
$$4(\mathbb{E}[XY])^2 \le 4\mathbb{E}[X^2]\mathbb{E}[Y^2] \implies (\mathbb{E}[XY])^2 \le \mathbb{E}[X^2]\mathbb{E}[Y^2]$$
Taking square roots on both sides yields the inequality. $\blacksquare$

---

### Fundamental Applications of Cauchy-Schwarz
1. **Correlation Bound:**
   Replace $X$ with $X - \mu_X$ and $Y$ with $Y - \mu_Y$:
   $$[\operatorname{Cov}(X, Y)]^2 \le \operatorname{Var}(X) \operatorname{Var}(Y) \implies -1 \le \rho(X, Y) \le 1$$
2. **Mean and Square Root:**
   Setting $Y = 1$:
   $$(\mathbb{E}[X])^2 \le \mathbb{E}[X^2] \implies \mathbb{E}[X] \le \sqrt{\mathbb{E}[X^2]}$$

---

## 2. Jensen's Inequality

### Mathematical Statement
Let $X$ be a random variable, and let $g: \mathbb{R} \to \mathbb{R}$ be a **convex function** (i.e., $g''(x) \ge 0$ whenever twice differentiable). Provided the expectations exist:

$$\mathbb{E}[g(X)] \ge g(\mathbb{E}[X])$$

For a **concave function** $h$ (where $-h$ is convex, e.g. $h(x) = \ln x$):
$$\mathbb{E}[h(X)] \le h(\mathbb{E}[X])$$

If $g$ is strictly convex, equality holds if and only if $X$ is a constant ($P(X = c) = 1$).

---

### Intuitive Geometric Proof via Supporting Tangents

By definition of convexity, a convex curve lies entirely on or above any of its tangent lines.
Let $\mu = \mathbb{E}[X]$. Construct the tangent line to $g$ at the point $\mu$:
$$L(x) = g(\mu) + g'(\mu)(x - \mu)$$
By convexity:
$$g(x) \ge L(x) = g(\mu) + g'(\mu)(x - \mu) \quad \text{for all } x$$

Replace the deterministic variable $x$ with the random variable $X$:
$$g(X) \ge g(\mu) + g'(\mu)(X - \mu)$$
Take expectations on both sides:
$$\mathbb{E}[g(X)] \ge \mathbb{E}\left[ g(\mu) + g'(\mu)(X - \mu) \right] = g(\mu) + g'(\mu)(\mathbb{E}[X] - \mu)$$
Since $\mathbb{E}[X] - \mu = \mu - \mu = 0$:
$$\mathbb{E}[g(X)] \ge g(\mu) = g(\mathbb{E}[X])$$
$\blacksquare$

---

### Fundamental Applications of Jensen's Inequality

1. **Non-negativity of Variance:**
   Let $g(x) = x^2$ (strictly convex). Then:
   $$\mathbb{E}[X^2] \ge (\mathbb{E}[X])^2 \implies \operatorname{Var}(X) = \mathbb{E}[X^2] - (\mathbb{E}[X])^2 \ge 0$$

2. **Arithmetic Mean – Geometric Mean (AM-GM) Inequality:**
   Let $h(x) = \ln x$ (concave on $(0, \infty)$).
   Let $X$ take values $x_1, x_2, \dots, x_n > 0$ with equal probability $1/n$.
   $$\mathbb{E}[\ln X] = \frac{1}{n} \sum_{i=1}^n \ln x_i = \ln \left( \prod_{i=1}^n x_i \right)^{1/n}$$
   $$\ln(\mathbb{E}[X]) = \ln \left( \frac{1}{n} \sum_{i=1}^n x_i \right)$$
   By Jensen's inequality for concave functions ($\mathbb{E}[\ln X] \le \ln(\mathbb{E}[X])$):
   $$\ln\left( \prod_{i=1}^n x_i \right)^{1/n} \le \ln \left( \frac{1}{n}\sum_{i=1}^n x_i \right) \implies \left(\prod_{i=1}^n x_i\right)^{1/n} \le \frac{1}{n}\sum_{i=1}^n x_i$$

3. **Information Theory (Kullback-Leibler Divergence):**
   Proves that relative entropy $D_{KL}(P \parallel Q) = \sum_x P(x) \ln \frac{P(x)}{Q(x)} \ge 0$, establishing that Shannon entropy is maximized by the uniform distribution.

---

## Related Notes

- [[Covariance and Correlation]] — Bound on correlation from Cauchy-Schwarz.
- [[Random Variables and Probability Distributions]] — Variance positivity via Jensen.
- [[Chernoff Bound]] — Convexity of the log-MGF.

---

## Sources & Traceability

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lecture 18, pages 57–59)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_11.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Section 10.3: Jensen and Cauchy-Schwarz)
