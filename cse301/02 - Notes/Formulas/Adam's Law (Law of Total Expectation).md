---
type: formula
course: cse301
status: active
order: 20
---

# Adam's Law (Law of Total Expectation)

> 📖 **Reading Order:** Step 20 of 92 | **Module 3:** Conditional Probability and Conditioning  
> ◄ **Previous:** [[Law of Total Probability and Bayes' Rule]] | ► **Next:** [[Eve's Law (Law of Total Variance)]]

---

---

## The Question and Earlier Knowledge

What analytical relationship or closed-form expectation governs Adam's Law (Law of Total Expectation), and how can we compute it directly from constituent probability terms? In complex probability models, calculating probabilities or moments directly is often intractable without decomposing expectations across conditioning partitions or inequalities.

---

## Developing the Formula

By decomposing joint distributions into conditional components, expanding algebraic products, or applying geometric series sums, Adam's Law (Law of Total Expectation) compresses complex probabilistic reasoning into a clean, reusable formula.

---

## Formula

The **Law of Total Expectation**, often referred to as **Adam's Law** (or the **Tower Property**), states that for any two random variables $X$ and $Y$ defined on the same probability space (provided $\mathbb{E}[\lvert Y \rvert] < \infty$):

$$\mathbb{E}[Y] = \mathbb{E}\left[ \mathbb{E}[Y \mid X] \right]$$

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

### Intuitive Interpretation: "Condition on What You Wish You Knew"

Adam's Law provides a universal divide-and-conquer strategy for difficult expectation problems:
1. Identify a random variable $X$ whose value, if known, would make computing the expectation of $Y$ easy.
2. Compute the conditional expectation $g(x) = \mathbb{E}[Y \mid X = x]$ treating $x$ as fixed and known.
3. Replace $x$ with the random variable $X$ to form the random variable $g(X) = \mathbb{E}[Y \mid X]$.
4. Take the unconditional expectation $\mathbb{E}[g(X)]$ across the distribution of $X$.

---

---

## Derivation

### Detailed Mathematical Proof

### Discrete Case:
Let $g(x) = \mathbb{E}[Y \mid X = x] = \sum_y y \, P(Y = y \mid X = x)$.
Then by the [[Law of the Unconscious Statistician (LOTUS)]]:
$$\mathbb{E}\left[ \mathbb{E}[Y \mid X] \right] = \mathbb{E}[g(X)] = \sum_x g(x) P(X = x)$$
$$= \sum_x \left( \sum_y y \, P(Y = y \mid X = x) \right) P(X = x)$$
Using the definition of conditional probability $P(Y = y \mid X = x) P(X = x) = P(X = x, Y = y)$:
$$= \sum_x \sum_y y \, P(X = x, Y = y)$$
Interchanging the order of summation (justified by absolute convergence):
$$= \sum_y y \left( \sum_x P(X = x, Y = y) \right) = \sum_y y \, P(Y = y) = \mathbb{E}[Y]$$
$\blacksquare$

### Continuous Case:
Let $g(x) = \mathbb{E}[Y \mid X = x] = \int_{-\infty}^\infty y f_{Y \mid X}(y \mid x) \, dy$.
$$\mathbb{E}[g(X)] = \int_{-\infty}^\infty g(x) f_X(x) \, dx = \int_{-\infty}^\infty \left( \int_{-\infty}^\infty y \frac{f_{X,Y}(x, y)}{f_X(x)} \, dy \right) f_X(x) \, dx$$
$$= \int_{-\infty}^\infty \int_{-\infty}^\infty y f_{X,Y}(x, y) \, dy \, dx = \int_{-\infty}^\infty y \left( \int_{-\infty}^\infty f_{X,Y}(x, y) \, dx \right) dy$$
$$= \int_{-\infty}^\infty y f_Y(y) \, dy = \mathbb{E}[Y]$$
$\blacksquare$

---

---

## Example

### Application Example: Expected Sum of a Random Number of Terms

Let $N$ be a non-negative integer random variable, and let $X_1, X_2, \dots$ be i.i.d. random variables with mean $\mu_X$, independent of $N$.
Define the random sum:
$$S_N = \sum_{i=1}^N X_i \quad (\text{with } S_0 = 0)$$

**Goal:** Find $\mathbb{E}[S_N]$.

### Step 1: Condition on $N$
If we know $N = n$, $S_N$ is simply the sum of $n$ i.i.d. variables:
$$\mathbb{E}[S_N \mid N = n] = \mathbb{E}\left[ \sum_{i=1}^n X_i \right] = \sum_{i=1}^n \mathbb{E}[X_i] = n \mu_X$$

### Step 2: Form the Random Variable $\mathbb{E}[S_N \mid N]$
$$\mathbb{E}[S_N \mid N] = N \mu_X$$

### Step 3: Apply Adam's Law
$$\mathbb{E}[S_N] = \mathbb{E}\left[ \mathbb{E}[S_N \mid N] \right] = \mathbb{E}[N \mu_X] = \mu_X \mathbb{E}[N] = \mathbb{E}[N]\mathbb{E}[X]$$

**Result (Wald's Identity for Expectation):**
$$\mathbb{E}\left[ \sum_{i=1}^N X_i \right] = \mathbb{E}[N] \mathbb{E}[X]$$

---

---

## Common Mistakes

- Confusing conditional variance with the variance of conditional expectation (Eve's Law components).
- Forgetting that linearity of expectation holds unconditionally, whereas $\mathbb{E}[XY] = \mathbb{E}[X]\mathbb{E}[Y]$ requires independence.

---

## Related Concepts

- [[Conditional Expectation]] — The theoretical projection framework.
- [[Eve's Law (Law of Total Variance)]] — Variance companion to Adam's Law.
- [[Random Number of Random Variables Sum Example]] — Full compound process example.

---

---

## Prerequisites

- [[Random Variables and Probability Distributions]]

---

## Problems

- [[Problem — Birthday Collisions and Approximation]]

---

## Sources

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lecture 16, pages 50–53)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_10.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Section 9.2: Law of Total Expectation)
