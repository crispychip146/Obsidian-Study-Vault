---
type: concept
course: cse301
status: active
order: 10
---

# Joint and Marginal Distributions

> 📖 **Reading Order:** Step 10 of 92 | **Module 2:** Random Variables and Distributions  
> ◄ **Previous:** [[Continuous Probability Distributions]] | ► **Next:** [[Covariance and Correlation]]

---

---

## Starting Point and the Problem

Probability and statistical inference model uncertainty in physical and computer systems. When analyzing stochastic phenomena related to Joint and Marginal Distributions, naive counting or deterministic approximations fail. We establish a formal mathematical foundation to quantify outcomes and evaluate expectations rigorously.

---

## Developing the Idea

By formalizing sample spaces, probability measures, state transitions, or likelihood ratios, Joint and Marginal Distributions reveals the underlying structural mechanics that govern random variables and estimation errors.

---

## Definition

When studying two or more random variables simultaneously (e.g., $(X, Y)$), their collective behavior is described by a **joint probability distribution**.

---

---

## How It Works

### Discrete Joint Distributions

Let $X$ and $Y$ be discrete random variables.
- **Joint PMF:**
  $$p_{X, Y}(x, y) = P(X = x, Y = y)$$
  satisfying $p_{X, Y}(x, y) \ge 0$ and $\sum_x \sum_y p_{X, Y}(x, y) = 1$.

- **Marginal PMFs (Summing Out):**
  To recover the individual distribution of one variable, sum over all possible values of the other:
  $$p_X(x) = \sum_y p_{X, Y}(x, y)$$
  $$p_Y(y) = \sum_x p_{X, Y}(x, y)$$

- **Conditional PMF:**
  $$p_{Y \mid X}(y \mid x) = P(Y = y \mid X = x) = \frac{p_{X, Y}(x, y)}{p_X(x)} \quad \text{for } p_X(x) > 0$$

---
### Continuous Joint Distributions

Let $X$ and $Y$ be continuous random variables.
- **Joint CDF:**
  $$F_{X, Y}(x, y) = P(X \le x, Y \le y)$$

- **Joint PDF:**
  $$f_{X, Y}(x, y) = \frac{\partial^2}{\partial x \, \partial y} F_{X, Y}(x, y)$$
  satisfying $f_{X, Y}(x, y) \ge 0$ and $\int_{-\infty}^\infty \int_{-\infty}^\infty f_{X, Y}(x, y) \, dx \, dy = 1$.

- **Probability Over a Region $A \subseteq \mathbb{R}^2$:**
  $$P((X, Y) \in A) = \iint_A f_{X, Y}(x, y) \, dx \, dy$$

- **Marginal PDFs (Integrating Out):**
  $$f_X(x) = \int_{-\infty}^\infty f_{X, Y}(x, y) \, dy$$
  $$f_Y(y) = \int_{-\infty}^\infty f_{X, Y}(x, y) \, dx$$

- **Conditional PDF:**
  $$f_{Y \mid X}(y \mid x) = \frac{f_{X, Y}(x, y)}{f_X(x)} \quad \text{for } f_X(x) > 0$$

---
### Independence of Random Variables

Random variables $X$ and $Y$ are **independent** ($X \perp Y$) if and only if any of the following equivalent conditions hold for all $x, y$:

1. **Joint CDF factorizes:**
   $$F_{X, Y}(x, y) = F_X(x) F_Y(y)$$
2. **Joint PMF/PDF factorizes:**
   $$p_{X, Y}(x, y) = p_X(x) p_Y(y) \quad \text{(discrete)}$$
   $$f_{X, Y}(x, y) = f_X(x) f_Y(y) \quad \text{(continuous)}$$
3. **Factorization Criterion (Sufficient & Necessary):**
   $X$ and $Y$ are independent if and only if the joint density can be factored into a function of $x$ alone and a function of $y$ alone:
   $$f_{X, Y}(x, y) = g(x) h(y)$$
   **AND** the support of $(X, Y)$ is a **Cartesian product (rectangle)** of the form $S_X \times S_Y$.

---

---

## Example

See worked numerical applications in the linked example notes.

---

## Technical Details

Refer to Blitzstein & Hwang for measure-theoretic details and moment generating properties.

---

## Important Properties and Why They Hold

### 2D Law of the Unconscious Statistician (LOTUS)

To compute the expected value of a function $g(X, Y)$ of two random variables without first deriving the distribution of $g(X, Y)$:

- **Discrete:**
  $$\mathbb{E}[g(X, Y)] = \sum_x \sum_y g(x, y) p_{X, Y}(x, y)$$

- **Continuous:**
  $$\mathbb{E}[g(X, Y)] = \int_{-\infty}^\infty \int_{-\infty}^\infty g(x, y) f_{X, Y}(x, y) \, dx \, dy$$

---

---

## Common Mistakes

### Edge Cases & Common Pitfalls

1. **The Support Trap (Crucial Exam Concept):**
   - Consider $f_{X, Y}(x, y) = 8xy$ on $0 \le y \le x \le 1$.
   - The formula looks factorized ($8x \cdot y$), but the **support depends on $x$ and $y$** ($y \le x$).
   - Therefore, $X$ and $Y$ are **DEPENDENT**! Knowing $X = 0.3$ restricts $Y \in [0, 0.3]$.
   - *Rule:* If the boundary of the joint support involves both variables (e.g., $x + y \le 1$ or $y \le x$), $X$ and $Y$ are automatically dependent!
2. **Marginalization Limits:**
   - When integrating $f_{X, Y}(x, y)$ over $y$ to get $f_X(x)$, always check the support boundaries in terms of $x$. For the region $0 \le y \le x \le 1$, the integral is $\int_0^x 8xy \, dy$, NOT $\int_0^1 8xy \, dy$.

---

---

## Exam Relevance

### Cross-Topic Connections / Exam Relevance

- **Covariance:** Linear association measured by integrating against the joint density (see [[Covariance and Correlation]]).
- **Conditioning:** Conditional density $f_{Y \mid X}(y \mid x)$ forms the foundation for [[Conditional Expectation]] and Adam's / Eve's laws.
- **Transformations:** Joint 2D transformations require the 2D Jacobian determinant:
  $$f_{U, V}(u, v) = f_{X, Y}(x(u, v), y(u, v)) \left\lvert \det \frac{\partial(x, y)}{\partial(u, v)} \right\rvert$$

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

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lecture 13, pages 40–43)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_8.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Chapter 7: Joint Distributions)
