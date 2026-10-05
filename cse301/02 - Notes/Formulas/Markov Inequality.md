---
type: formula
course: cse301
status: active
order: 25
---

# Markov Inequality

> 📖 **Reading Order:** Step 25 of 92 | **Module 4:** Probability Bounds and Inequalities  
> ◄ **Previous:** [[Problem — Compound Random Sum via Adam and Eve's Laws]] | ► **Next:** [[Chebyshev Inequality]]

---

## Building the idea

If $X$ is nonnegative and sometimes reaches at least $a>0$, those occasions contribute at least $a$ each to its average. Consequently $E[X]\ge aP(X\ge a)$, which rearranges to Markov's inequality.

This proof shows both its strength and its limitation. It needs no distributional shape, but uses only the mean. A variable that is zero most of the time and exactly $a$ on the remaining occasions reaches equality. Much sharper answers require more information about the distribution.

The nonnegative assumption is essential: negative outcomes could cancel large positive ones in the mean. For a signed variable, apply the inequality to a nonnegative transformation such as $|X|$ or $(X-\mu)^2$ if its expectation exists. [[Chebyshev Inequality]] uses the squared transformation to bound distance from the mean.

## Formula

Let $X$ be a **non-negative random variable** (i.e., $P(X \ge 0) = 1$) with finite expectation $\mathbb{E}[X] < \infty$. For any threshold $a > 0$:

$$P(X \ge a) \le \frac{\mathbb{E}[X]}{a}$$

Equivalently, setting $a = c \mathbb{E}[X]$ for $c > 0$:
$$P(X \ge c \mathbb{E}[X]) \le \frac{1}{c}$$

---

## Variables

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

## Intuition

### When Is Markov's Inequality Tight?

Markov's inequality holds with **strict equality** ($P(X \ge a) = \frac{\mathbb{E}[X]}{a}$) if and only if $X$ takes values only in $\{0, a\}$:
$$P(X = a) = p, \quad P(X = 0) = 1 - p$$
Then $\mathbb{E}[X] = ap$, and $P(X \ge a) = p = \frac{\mathbb{E}[X]}{a}$.

---
### Generalized Markov Inequality

If $g: \mathbb{R} \to [0, \infty)$ is a non-negative, strictly monotonically increasing function on the support of $X$, then for any $a$:
$$P(X \ge a) = P(g(X) \ge g(a)) \le \frac{\mathbb{E}[g(X)]}{g(a)}$$

This generalization is the mother of all major concentration inequalities:
- Choosing $g(x) = (x - \mu)^2$ yields the [[Chebyshev Inequality]].
- Choosing $g(x) = e^{tx}$ ($t > 0$) yields the [[Chernoff Bound]].
- Choosing $g(x) = x^k$ ($k > 0$) yields high-order moment bounds.

---

## Common Mistakes

### Common Pitfalls

1. **Forgetting Non-negativity:** Markov's inequality is **invalid** if $X$ can take negative values!
   - *Counterexample:* Let $X = -100$ with probability $0.5$ and $X = 100$ with probability $0.5$. Then $\mathbb{E}[X] = 0$.
   - Claiming $P(X \ge 50) \le \frac{0}{50} = 0$ is completely false, since $P(X \ge 50) = 0.5$!
2. **Weak Bounds:** Because it only uses the first moment ($\mathbb{E}[X]$), Markov's inequality is generally loose. When variance or MGF is known, Chebyshev or Chernoff should be preferred.

---

## What to carry forward

A bound greater than one may be replaced by one, but it gives no useful improvement over the probability axioms. When writing $P(X\ge cE[X])\le1/c$, handle the zero-mean case separately.

## Related notes

- [[Chebyshev Inequality]]

## Sources

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lecture 17, pages 54–56)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_11.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Section 10.1: Inequalities)
