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

## Building the idea

You can compute an overall average by first averaging within groups and then weighting the group averages by group size or probability. This is Adam's law: $E[Y]=E[E[Y\mid X]]$.

Why do we need the outer expectation? Group averages are not generally equally weighted. If two groups have means 10 and 100 but probabilities $0.9$ and $0.1$, the overall mean is $0.9(10)+0.1(100)=19$, not 55. The inner expectation answers the question given a group; the outer expectation accounts for which group we actually encounter.

The proof is regrouping joint probability weights. In a discrete model, $\sum_x\sum_y yP(Y=y\mid X=x)P(X=x)$ becomes $\sum_y yP(Y=y)$ after summing over $x$. For more general variables, the same statement is the tower property of conditional expectation, under the usual integrability condition.

## Formula

The **Law of Total Expectation**, often referred to as **Adam's Law** (or the **Tower Property**), states that for any two random variables $X$ and $Y$ defined on the same probability space (provided $\mathbb{E}[\lvert Y \rvert] < \infty$):

$$\mathbb{E}[Y] = \mathbb{E}\left[ \mathbb{E}[Y \mid X] \right]$$

---

## Intuition

### Intuitive Interpretation: "Condition on What You Wish You Knew"

Adam's Law provides a universal divide-and-conquer strategy for difficult expectation problems:
1. Identify a random variable $X$ whose value, if known, would make computing the expectation of $Y$ easy.
2. Compute the conditional expectation $g(x) = \mathbb{E}[Y \mid X = x]$ treating $x$ as fixed and known.
3. Replace $x$ with the random variable $X$ to form the random variable $g(X) = \mathbb{E}[Y \mid X]$.
4. Take the unconditional expectation $\mathbb{E}[g(X)]$ across the distribution of $X$.

---

## Derivation

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

## What to carry forward

Choose a conditioning variable that makes the inner problem simpler. [[Random Number of Random Variables Sum Example]] conditions on the random number of terms so the inside becomes a fixed-length sum.

## Related notes

- [[Random Number of Random Variables Sum Example]]

## Sources

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lecture 16, pages 50–53)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_10.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Section 9.2: Law of Total Expectation)
