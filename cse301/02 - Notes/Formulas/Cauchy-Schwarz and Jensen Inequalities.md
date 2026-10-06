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

---

## The Question and Earlier Knowledge

What analytical relationship or closed-form expectation governs Cauchy-Schwarz and Jensen Inequalities, and how can we compute it directly from constituent probability terms? In complex probability models, calculating probabilities or moments directly is often intractable without decomposing expectations across conditioning partitions or inequalities.

---

## Developing the Formula

By decomposing joint distributions into conditional components, expanding algebraic products, or applying geometric series sums, Cauchy-Schwarz and Jensen Inequalities compresses complex probabilistic reasoning into a clean, reusable formula.

---

## Formula

### Cauchy-Schwarz Inequality
For random variables $X, Y$ with finite second moments ($\mathbb{E}[X^2] < \infty, \mathbb{E}[Y^2] < \infty$):
$$\lvert \mathbb{E}[XY] \rvert \le \sqrt{\mathbb{E}[X^2] \, \mathbb{E}[Y^2]}$$

Equivalently for centered random variables:
$$(\operatorname{Cov}(X, Y))^2 \le \operatorname{Var}(X)\operatorname{Var}(Y) \implies -1 \le \rho_{X,Y} \le 1$$

### Jensen's Inequality
For any random variable $X$ and convex function $g: \mathbb{R} \to \mathbb{R}$:
$$\mathbb{E}[g(X)] \ge g(\mathbb{E}[X])$$

For any concave function $h: \mathbb{R} \to \mathbb{R}$ (where $-h$ is convex, e.g. $\ln(x), \sqrt{x}$):
$$\mathbb{E}[h(X)] \le h(\mathbb{E}[X])$$

---

## Variables

| Symbol | Meaning |
|---|---|
| $X, Y$ | Random variables governed by underlying probability distributions |
| $\mathbb{E}[\cdot]$ | Expected value operator |
| $\text{Var}(\cdot)$ | Variance operator |

---

## Conditions

- Random variables must possess finite first and second moments (well-defined expectations).
- Probability distributions must satisfy standard non-negativity and total probability integration axioms.

---

## Intuition

### Cauchy-Schwarz Inequality

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
### Jensen's Inequality

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

---

## Derivation

### Proof of Cauchy-Schwarz via Quadratic Discriminant
For any real scalar $t \in \mathbb{R}$, consider the non-negative random variable $(tX + Y)^2 \ge 0$.
Taking expectations:
$$h(t) = \mathbb{E}\left[ (tX + Y)^2 \right] = t^2 \mathbb{E}[X^2] + 2t \mathbb{E}[XY] + \mathbb{E}[Y^2] \ge 0$$

$h(t)$ is a quadratic polynomial $A t^2 + B t + C$ with $A = \mathbb{E}[X^2]$, $B = 2\mathbb{E}[XY]$, and $C = \mathbb{E}[Y^2]$.
Because $h(t) \ge 0$ for all $t \in \mathbb{R}$, this parabola cannot have two distinct real roots. Hence its discriminant $\Delta = B^2 - 4AC \le 0$:
$$(2\mathbb{E}[XY])^2 - 4\mathbb{E}[X^2]\mathbb{E}[Y^2] \le 0 \implies (\mathbb{E}[XY])^2 \le \mathbb{E}[X^2]\mathbb{E}[Y^2]$$
Taking square roots yields $\lvert \mathbb{E}[XY] \rvert \le \sqrt{\mathbb{E}[X^2]\mathbb{E}[Y^2]}$. $\blacksquare$

### Proof of Jensen via Supporting Tangent
Let $\mu = \mathbb{E}[X]$. Because $g$ is convex, there exists a supporting line $L(x) = g(\mu) + m(x - \mu)$ such that $g(x) \ge L(x)$ for all $x \in \mathbb{R}$ (where $m = g'(\mu)$ if differentiable).
Substituting the random variable $X$:
$$g(X) \ge g(\mu) + m(X - \mu)$$
Taking expectations on both sides:
$$\mathbb{E}[g(X)] \ge g(\mu) + m(\mathbb{E}[X] - \mu) = g(\mu) + m(\mu - \mu) = g(\mathbb{E}[X])$$
$\blacksquare$

---

## Example

### Example 1: Jensen's Inequality on the Reciprocal Function
Let $X \sim \operatorname{Unif}(1, 3)$. The expected value is $\mathbb{E}[X] = 2$.
Consider the reciprocal function $g(x) = \frac{1}{x}$, which is strictly convex on $(0, \infty)$ because $g''(x) = \frac{2}{x^3} > 0$.
- Plugged into expected value: $g(\mathbb{E}[X]) = \frac{1}{\mathbb{E}[X]} = \frac{1}{2} = 0.5$
- Expectation of function:
  $$\mathbb{E}[g(X)] = \mathbb{E}\left[\frac{1}{X}\right] = \int_1^3 \frac{1}{x} \cdot \frac{1}{3 - 1}\, dx = \frac{1}{2} [\ln 3 - \ln 1] = \frac{\ln 3}{2} \approx \frac{1.0986}{2} \approx 0.5493$$

Notice that $\mathbb{E}[1/X] \approx 0.5493 > 0.5000 = 1/\mathbb{E}[X]$, directly demonstrating Jensen's inequality with strict gap caused by the variance of $X$.

### Example 2: Cauchy-Schwarz on Standard Normal Moments
Let $Z \sim \mathcal{N}(0, 1)$. Set $X = Z$ and $Y = Z^3$.
- $\mathbb{E}[X^2] = \mathbb{E}[Z^2] = 1$
- $\mathbb{E}[Y^2] = \mathbb{E}[Z^6] = 1 \cdot 3 \cdot 5 = 15$
- $\mathbb{E}[XY] = \mathbb{E}[Z^4] = 3$

Cauchy-Schwarz predicts:
$$|\mathbb{E}[XY]| = 3 \le \sqrt{\mathbb{E}[X^2]\mathbb{E}[Y^2]} = \sqrt{1 \times 15} = \sqrt{15} \approx 3.873$$
which confirms the inequality $3 \le 3.873$. Strict inequality holds because $Y = Z^3$ is not a linear function of $X$.

For complete bounds comparisons across distributions, see [[Comparison of Probability Bounds Example]] and [[Covariance and Correlation]].

---

## Common Mistakes

- **Reversing the Inequality for Concave Functions:** Applying $\mathbb{E}[g(X)] \ge g(\mathbb{E}[X])$ to concave functions like $\ln(x)$ or $\sqrt{x}$. For concave functions, the inequality is reversed: $\mathbb{E}[\ln X] \le \ln(\mathbb{E}[X])$ and $\mathbb{E}[\sqrt{X}] \le \sqrt{\mathbb{E}[X]}$.
- **Assuming Commutativity of Expectation with Inversion:** Assuming $\mathbb{E}[1/X] = 1/\mathbb{E}[X]$. By Jensen's inequality, $\mathbb{E}[1/X] \ge 1/\mathbb{E}[X]$ for positive random variables.
- **Applying Cauchy-Schwarz without Moment Guarantees:** Applying Cauchy-Schwarz when random variables lack finite second moments ($\mathbb{E}[X^2] = \infty$).

---

## Related Concepts

- [[Covariance and Correlation]] — Bound on correlation from Cauchy-Schwarz.
- [[Random Variables and Probability Distributions]] — Variance positivity via Jensen.
- [[Chernoff Bound]] — Convexity of the log-MGF.
- [[Comparison of Probability Bounds Example]] — Side-by-side numerical comparison.

---

---

## Prerequisites

- [[Random Variables and Probability Distributions]]
- [[Covariance and Correlation]]

---

## Problems

- [[Problem — Bounding Tail Probabilities with Chebyshev and Chernoff]]

---

## Sources

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lecture 18, pages 57–59)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_11.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Section 10.3: Jensen and Cauchy-Schwarz)
