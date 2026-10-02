---
type: formula
course: cse301
status: active
---

# Markov Inequality

## Mathematical Statement

Let $X$ be a **non-negative random variable** (i.e., $P(X \ge 0) = 1$) with finite expectation $\mathbb{E}[X] < \infty$. For any threshold $a > 0$:

$$P(X \ge a) \le \frac{\mathbb{E}[X]}{a}$$

Equivalently, setting $a = c \mathbb{E}[X]$ for $c > 0$:
$$P(X \ge c \mathbb{E}[X]) \le \frac{1}{c}$$

---

## Elegant Proof via Indicator Random Variables

Consider the indicator random variable $I_{X \ge a}$, defined as:
$$I_{X \ge a} = \begin{cases} 1 & \text{if } X \ge a \\ 0 & \text{if } X < a \end{cases}$$

Notice the fundamental pointwise inequality:
$$a I_{X \ge a} \le X$$

**Verification:**
- If $X < a$, the left-hand side is $a \cdot 0 = 0$. Since $X \ge 0$, $0 \le X$ holds.
- If $X \ge a$, the left-hand side is $a \cdot 1 = a$. Since $X \ge a$, $a \le X$ holds.

Taking the expectation on both sides and using monotonicity and linearity of expectation:
$$\mathbb{E}[a I_{X \ge a}] \le \mathbb{E}[X]$$
$$a \mathbb{E}[I_{X \ge a}] \le \mathbb{E}[X]$$
Since $\mathbb{E}[I_{X \ge a}] = P(X \ge a)$:
$$a P(X \ge a) \le \mathbb{E}[X] \implies P(X \ge a) \le \frac{\mathbb{E}[X]}{a}$$
$\blacksquare$

---

## When Is Markov's Inequality Tight?

Markov's inequality holds with **strict equality** ($P(X \ge a) = \frac{\mathbb{E}[X]}{a}$) if and only if $X$ takes values only in $\{0, a\}$:
$$P(X = a) = p, \quad P(X = 0) = 1 - p$$
Then $\mathbb{E}[X] = ap$, and $P(X \ge a) = p = \frac{\mathbb{E}[X]}{a}$.

---

## Generalized Markov Inequality

If $g: \mathbb{R} \to [0, \infty)$ is a non-negative, strictly monotonically increasing function on the support of $X$, then for any $a$:
$$P(X \ge a) = P(g(X) \ge g(a)) \le \frac{\mathbb{E}[g(X)]}{g(a)}$$

This generalization is the mother of all major concentration inequalities:
- Choosing $g(x) = (x - \mu)^2$ yields the [[Chebyshev Inequality]].
- Choosing $g(x) = e^{tx}$ ($t > 0$) yields the [[Chernoff Bound]].
- Choosing $g(x) = x^k$ ($k > 0$) yields high-order moment bounds.

---

## Common Pitfalls

1. **Forgetting Non-negativity:** Markov's inequality is **invalid** if $X$ can take negative values!
   - *Counterexample:* Let $X = -100$ with probability $0.5$ and $X = 100$ with probability $0.5$. Then $\mathbb{E}[X] = 0$.
   - Claiming $P(X \ge 50) \le \frac{0}{50} = 0$ is completely false, since $P(X \ge 50) = 0.5$!
2. **Weak Bounds:** Because it only uses the first moment ($\mathbb{E}[X]$), Markov's inequality is generally loose. When variance or MGF is known, Chebyshev or Chernoff should be preferred.

---

## Related Notes

- [[Chebyshev Inequality]] — Second-moment specialization.
- [[Chernoff Bound]] — Exponential moment specialization.
- [[Comparison of Probability Bounds Example]] — Side-by-side numerical comparison.

---

## Sources & Traceability

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lecture 17, pages 54–56)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_11.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Section 10.1: Inequalities)
