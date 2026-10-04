---
type: formula
course: cse301
status: active
order: 26
---

# Chebyshev Inequality

> 📖 **Reading Order:** Step 26 of 92 | **Module 4:** Probability Bounds and Inequalities  
> ◄ **Previous:** [[Markov Inequality]] | ► **Next:** [[Chernoff Bound]]

---

---

## The Question and Earlier Knowledge

What analytical relationship or closed-form expectation governs Chebyshev Inequality, and how can we compute it directly from constituent probability terms? In complex probability models, calculating probabilities or moments directly is often intractable without decomposing expectations across conditioning partitions or inequalities.

---

## Developing the Formula

By decomposing joint distributions into conditional components, expanding algebraic products, or applying geometric series sums, Chebyshev Inequality compresses complex probabilistic reasoning into a clean, reusable formula.

---

## Formula

Let $X$ be a random variable with finite mean $\mu = \mathbb{E}[X]$ and finite variance $\sigma^2 = \operatorname{Var}(X)$. For any constant $c > 0$:

$$P(\lvert X - \mu \rvert \ge c) \le \frac{\sigma^2}{c^2}$$

### Standardized Form (Distance in Standard Deviations)
Setting $c = k\sigma$ for $k > 0$:
$$P(\lvert X - \mu \rvert \ge k\sigma) \le \frac{1}{k^2}$$

Equivalently, the probability of remaining within $k$ standard deviations of the mean is bounded below by:
$$P(\lvert X - \mu \rvert < k\sigma) \ge 1 - \frac{1}{k^2}$$

- For $k = 2$: At least $1 - 1/4 = 75\%$ of probability lies within $[\mu - 2\sigma, \mu + 2\sigma]$.
- For $k = 3$: At least $1 - 1/9 \approx 88.89\%$ lies within $[\mu - 3\sigma, \mu + 3\sigma]$.
- For $k = 5$: At least $1 - 1/25 = 96\%$ lies within $[\mu - 5\sigma, \mu + 5\sigma]$.

*(Notice that this holds for **ANY** probability distribution whatsoever, whether symmetric, skewed, bimodal, or discrete!)*

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

### When Is Chebyshev's Inequality Tight?

Chebyshev's inequality is **sharp** (cannot be improved without further distributional assumptions). Equality is attained by the three-point symmetric distribution:
$$P(X = \mu - k\sigma) = \frac{1}{2k^2}, \quad P(X = \mu + k\sigma) = \frac{1}{2k^2}, \quad P(X = \mu) = 1 - \frac{1}{k^2}$$
Here, $P(\lvert X - \mu \rvert \ge k\sigma) = \frac{1}{k^2}$ exactly.

---
### One-Sided Chebyshev (Cantelli's Inequality)

If we only care about an upper tail deviation $X - \mu \ge c$ (where $c > 0$):
$$P(X - \mu \ge c) \le \frac{\sigma^2}{\sigma^2 + c^2}$$
Setting $c = k\sigma$:
$$P(X - \mu \ge k\sigma) \le \frac{1}{1 + k^2}$$
*(Notice this is strictly tighter than the naive half-Chebyshev bound $\frac{1}{2k^2}$ for large $k$)*.

---

---

## Derivation

### Derivation from Markov's Inequality

Define the auxiliary random variable $Y = (X - \mu)^2$.
Notice that:
1. $Y \ge 0$ (non-negative everywhere).
2. The event $\{\lvert X - \mu \rvert \ge c\}$ is identical to the event $\{Y \ge c^2\}$.
3. $\mathbb{E}[Y] = \mathbb{E}[(X - \mu)^2] = \sigma^2$.

Applying [[Markov Inequality]] to $Y$ with threshold $a = c^2$:
$$P(\lvert X - \mu \rvert \ge c) = P(Y \ge c^2) \le \frac{\mathbb{E}[Y]}{c^2} = \frac{\sigma^2}{c^2}$$
$\blacksquare$

---
### Direct Proof of the Weak Law of Large Numbers (WLLN)

Let $X_1, X_2, \dots, X_n$ be i.i.d. with mean $\mu$ and variance $\sigma^2$. Let the sample mean be $\bar{X}_n = \frac{1}{n}\sum_{i=1}^n X_i$.
- $\mathbb{E}[\bar{X}_n] = \mu$
- $\operatorname{Var}(\bar{X}_n) = \frac{\sigma^2}{n}$

Applying Chebyshev's inequality to $\bar{X}_n$ for any arbitrary precision $\epsilon > 0$:
$$P(\lvert \bar{X}_n - \mu \rvert \ge \epsilon) \le \frac{\operatorname{Var}(\bar{X}_n)}{\epsilon^2} = \frac{\sigma^2}{n\epsilon^2}$$

Taking the limit as the sample size $n \to \infty$:
$$\lim_{n \to \infty} P(\lvert \bar{X}_n - \mu \rvert \ge \epsilon) \le \lim_{n \to \infty} \frac{\sigma^2}{n\epsilon^2} = 0$$

This provides a direct, 3-line proof of the **Weak Law of Large Numbers**!

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

- [[Markov Inequality]] — Foundational inequality.
- [[Chernoff Bound]] — Exponentially sharper bound when MGF exists.
- [[Law of Large Numbers]] — Convergence theorem proven by Chebyshev.

---

---

## Prerequisites

- [[Random Variables and Probability Distributions]]

---

## Problems

- [[Problem — Birthday Collisions and Approximation]]

---

## Sources

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lecture 17, pages 54–56)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_11.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Section 10.1: Chebyshev)
