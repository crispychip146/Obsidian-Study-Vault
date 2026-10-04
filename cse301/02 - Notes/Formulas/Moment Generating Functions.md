---
type: formula
course: cse301
status: active
order: 13
---

# Moment Generating Functions

> 📖 **Reading Order:** Step 13 of 92 | **Module 2:** Random Variables and Distributions  
> ◄ **Previous:** [[Law of the Unconscious Statistician (LOTUS)]] | ► **Next:** [[Linearity of Expectation and Indicator Random Variables Example]]

---

---

## The Question and Earlier Knowledge

What analytical relationship or closed-form expectation governs Moment Generating Functions, and how can we compute it directly from constituent probability terms? In complex probability models, calculating probabilities or moments directly is often intractable without decomposing expectations across conditioning partitions or inequalities.

---

## Developing the Formula

By decomposing joint distributions into conditional components, expanding algebraic products, or applying geometric series sums, Moment Generating Functions compresses complex probabilistic reasoning into a clean, reusable formula.

---

## Formula

The **Moment Generating Function (MGF)** of a random variable $X$ is defined as:
$$M_X(t) = \mathbb{E}[e^{tX}]$$
for all real $t$ in some neighborhood $(-h, h)$ with $h > 0$ where the expectation is finite.

---

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

### The Moment Generating Property

Expanding $e^{tX}$ using its Maclaurin series:
$$e^{tX} = \sum_{n=0}^\infty \frac{(tX)^n}{n!} = 1 + tX + \frac{t^2 X^2}{2!} + \frac{t^3 X^3}{3!} + \dots$$

Taking the expectation term by term (justified by Fubini's theorem within the radius of convergence):
$$M_X(t) = \mathbb{E}\left[ \sum_{n=0}^\infty \frac{t^n X^n}{n!} \right] = \sum_{n=0}^\infty \frac{t^n}{n!} \mathbb{E}[X^n] = 1 + t\mathbb{E}[X] + \frac{t^2}{2!}\mathbb{E}[X^2] + \dots$$

Differentiating $k$ times with respect to $t$ and evaluating at $t = 0$:
$$M_X^{(k)}(0) = \left. \frac{d^k}{dt^k} M_X(t) \right|_{t=0} = \mathbb{E}[X^k]$$

Specifically:
- $\mathbb{E}[X] = M_X'(0)$
- $\mathbb{E}[X^2] = M_X''(0)$
- $\operatorname{Var}(X) = M_X''(0) - [M_X'(0)]^2$

---
### Key Algebraic Properties of MGFs

1. **Affine Transformation:**
   If $Y = aX + b$, then:
   $$M_Y(t) = \mathbb{E}[e^{t(aX + b)}] = e^{bt} \mathbb{E}[e^{(at)X}] = e^{bt} M_X(at)$$

2. **Sum of Independent Random Variables (Convolution becomes Multiplication):**
   If $X_1, X_2, \dots, X_n$ are **mutually independent**:
   $$M_{X_1 + X_2 + \dots + X_n}(t) = \mathbb{E}\left[ e^{t(X_1 + \dots + X_n)} \right] = \prod_{i=1}^n \mathbb{E}[e^{t X_i}] = \prod_{i=1}^n M_{X_i}(t)$$
   If the variables are i.i.d. with common MGF $M_X(t)$:
   $$M_{\sum X_i}(t) = [M_X(t)]^n$$

3. **Uniqueness Theorem:**
   If the MGFs of two random variables $X$ and $Y$ exist and are equal ($M_X(t) = M_Y(t)$) in an open neighborhood around $t = 0$, then $X$ and $Y$ have the **exact same probability distribution** ($F_X(x) = F_Y(x)$ for all $x$).

---
### Reference Table of Common MGFs

| Distribution | Parameters | MGF $M_X(t)$ | Domain / Condition |
|---|---|---|---|
| **Bernoulli** | $p \in (0, 1)$ | $q + p e^t$ | $t \in \mathbb{R}$ |
| **Binomial** | $n, p$ | $(q + p e^t)^n$ | $t \in \mathbb{R}$ |
| **Geometric** | $p \in (0, 1)$ | $\frac{p}{1 - q e^t}$ | $t < -\ln(1 - p)$ |
| **Poisson** | $\lambda > 0$ | $e^{\lambda(e^t - 1)}$ | $t \in \mathbb{R}$ |
| **Uniform** | $\operatorname{Unif}(a, b)$ | $\frac{e^{tb} - e^{ta}}{t(b - a)}$ | $t \ne 0$ ($M(0) = 1$) |
| **Exponential** | $\lambda > 0$ | $\frac{\lambda}{\lambda - t} = \left( 1 - \frac{t}{\lambda} \right)^{-1}$ | $t < \lambda$ |
| **Gamma** | $a, \lambda$ | $\left( 1 - \frac{t}{\lambda} \right)^{-a}$ | $t < \lambda$ |
| **Normal** | $\mathcal{N}(\mu, \sigma^2)$ | $\exp\left( \mu t + \frac{1}{2}\sigma^2 t^2 \right)$ | $t \in \mathbb{R}$ |

---

---

## Derivation

### Classic Proof Example: Sum of Independent Normals

Let $X \sim \mathcal{N}(\mu_1, \sigma_1^2)$ and $Y \sim \mathcal{N}(\mu_2, \sigma_2^2)$ be independent.
$$M_X(t) = e^{\mu_1 t + \frac{1}{2}\sigma_1^2 t^2}, \quad M_Y(t) = e^{\mu_2 t + \frac{1}{2}\sigma_2^2 t^2}$$
$$M_{X+Y}(t) = M_X(t) M_Y(t) = \exp\left( (\mu_1 + \mu_2)t + \frac{1}{2}(\sigma_1^2 + \sigma_2^2)t^2 \right)$$
By inspection, this matches the MGF of a Normal distribution with mean $\mu_1 + \mu_2$ and variance $\sigma_1^2 + \sigma_2^2$.
By the **Uniqueness Theorem**, $X + Y \sim \mathcal{N}(\mu_1 + \mu_2, \sigma_1^2 + \sigma_2^2)$! $\blacksquare$

---

---

## Example

See worked numerical examples in the associated Example and Problem notes.

---

## Common Mistakes

- Confusing conditional variance with the variance of conditional expectation (Eve's Law components).
- Forgetting that linearity of expectation holds unconditionally, whereas $\mathbb{E}[XY] = \mathbb{E}[X]\mathbb{E}[Y]$ requires independence.

---

## Related Concepts

- [[Law of the Unconscious Statistician (LOTUS)]] — Used to compute $\mathbb{E}[e^{tX}]$.
- [[Continuous Probability Distributions]] — Normal and Exponential distributions.
- [[Chernoff Bound]] — Optimizes over $t > 0$ in $M_X(t)$ to establish exponential tail bounds.

---

---

## Prerequisites

- [[Random Variables and Probability Distributions]]

---

## Problems

- [[Problem — Birthday Collisions and Approximation]]

---

## Sources

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lecture 15, pages 47–49)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_9.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Chapter 6: MGFs)
