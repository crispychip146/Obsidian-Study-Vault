---
type: concept
course: cse301
status: active
order: 18
---

# Conditional Expectation

> 📖 **Reading Order:** Step 18 of 92 | **Module 3:** Conditional Probability and Conditioning  
> ◄ **Previous:** [[Conditional Probability and Independence]] | ► **Next:** [[Law of Total Probability and Bayes' Rule]]

---

---

## Starting Point and the Problem

Probability and statistical inference model uncertainty in physical and computer systems. When analyzing stochastic phenomena related to Conditional Expectation, naive counting or deterministic approximations fail. We establish a formal mathematical foundation to quantify outcomes and evaluate expectations rigorously.

---

## Developing the Idea

By formalizing sample spaces, probability measures, state transitions, or likelihood ratios, Conditional Expectation reveals the underlying structural mechanics that govern random variables and estimation errors.

---

## Definition

Let $X$ and $Y$ be random variables defined on the same probability space. The **conditional expectation** can be viewed in two fundamentally distinct yet intimately connected ways:

### 1. Conditional Expectation as a Number: $\mathbb{E}[Y \mid X = x]$
Given that $X$ takes the specific observed value $x$, the conditional expectation is the mean of the conditional distribution of $Y$:
- **Discrete Case:**
  $$\mathbb{E}[Y \mid X = x] = \sum_y y \, P(Y = y \mid X = x)$$
- **Continuous Case:**
  $$\mathbb{E}[Y \mid X = x] = \int_{-\infty}^\infty y \, f_{Y \mid X}(y \mid x) \, dy$$
This is a standard deterministic real number (a function of $x$, say $g(x)$).

### 2. Conditional Expectation as a Random Variable: $\mathbb{E}[Y \mid X]$
Before observing the actual outcome of $X$, the quantity $\mathbb{E}[Y \mid X]$ is itself a **random variable**!
Specifically:
$$\mathbb{E}[Y \mid X] = g(X)$$
where $g(x) = \mathbb{E}[Y \mid X = x]$.
Because $X$ is random, $g(X)$ is random. It has its own distribution, mean, and variance.

---

---

## How It Works

### Fundamental Algebraic Properties of $\mathbb{E}[Y \mid X]$

1. **Linearity in the Target Variable:**
   $$\mathbb{E}[a Y_1 + b Y_2 + c \mid X] = a\mathbb{E}[Y_1 \mid X] + b\mathbb{E}[Y_2 \mid X] + c$$

2. **Taking Out What Is Known (Factoring Out $X$):**
   If $h(X)$ is any deterministic function of $X$, then given $X$, $h(X)$ acts as a constant:
   $$\mathbb{E}[h(X) Y \mid X] = h(X) \mathbb{E}[Y \mid X]$$
   Special case: $\mathbb{E}[h(X) \mid X] = h(X)$.

3. **Independence Simplification:**
   If $X$ and $Y$ are independent ($X \perp Y$), knowing $X$ provides no information about $Y$:
   $$\mathbb{E}[Y \mid X] = \mathbb{E}[Y] \quad \text{(a constant)}$$

4. **Tower Property / Adam's Law:**
   $$\mathbb{E}\left[ \mathbb{E}[Y \mid X] \right] = \mathbb{E}[Y]$$

5. **Nested Tower Property (Projecting Onto Sub-information):**
   If we condition on more information $(X_1, X_2)$ and then condition on less information $X_1$:
   $$\mathbb{E}\left[ \mathbb{E}[Y \mid X_1, X_2] \mid X_1 \right] = \mathbb{E}[Y \mid X_1]$$
   *(The rougher conditioning dominates)*.

---

---

## Example

See worked numerical applications in the linked example notes.

---

## Technical Details

### The Best Mean Squared Error (MSE) Predictor

In statistics and machine learning, suppose we want to predict an unobserved target $Y$ using a function of observed features $g(X)$. We evaluate the prediction quality using the **Mean Squared Error (MSE)**:
$$\operatorname{MSE}(g) = \mathbb{E}\left[ (Y - g(X))^2 \right]$$

### The Orthogonal Projection Theorem:
The function $g(X)$ that minimizes the MSE among **ALL possible functions** (linear or non-linear) is the **conditional expectation**:
$$g^*(X) = \mathbb{E}[Y \mid X]$$

#### Proof Sketch:
Let $g(X)$ be any arbitrary estimator. Add and subtract $\mathbb{E}[Y \mid X]$:
$$Y - g(X) = (Y - \mathbb{E}[Y \mid X]) + (\mathbb{E}[Y \mid X] - g(X))$$
Square both sides and take expectations:
$$\mathbb{E}\left[(Y - g(X))^2\right] = \mathbb{E}\left[(Y - \mathbb{E}[Y \mid X])^2\right] + \mathbb{E}\left[(\mathbb{E}[Y \mid X] - g(X))^2\right] + 2\mathbb{E}\left[(Y - \mathbb{E}[Y \mid X])(\mathbb{E}[Y \mid X] - g(X))\right]$$

By Adam's Law and conditioning on $X$, the cross-term vanishes identically:
$$\mathbb{E}\left[ (Y - \mathbb{E}[Y \mid X]) h(X) \right] = \mathbb{E}\left[ \mathbb{E}[(Y - \mathbb{E}[Y \mid X])h(X) \mid X] \right] = \mathbb{E}\left[ h(X) (\mathbb{E}[Y \mid X] - \mathbb{E}[Y \mid X]) \right] = 0$$

Therefore:
$$\mathbb{E}\left[(Y - g(X))^2\right] = \mathbb{E}\left[(Y - \mathbb{E}[Y \mid X])^2\right] + \mathbb{E}\left[(\mathbb{E}[Y \mid X] - g(X))^2\right]$$
Since the second term is non-negative and is the only term containing $g(X)$, it is minimized when $g(X) = \mathbb{E}[Y \mid X]$! $\blacksquare$

---

---

## Important Properties and Why They Hold

- **Mathematical Rigor:** Satisfies Kolmogorov's probability axioms or standard asymptotic regularity conditions.
- **Convergence / Consistency:** Guarantees stability under large sample limits or repeated independent trials.

---

## Common Mistakes

### Edge Cases & Common Pitfalls

1. **Treating $\mathbb{E}[Y \mid X]$ as a Constant:**
   - $\mathbb{E}[Y \mid X = 3]$ is a scalar constant.
   - $\mathbb{E}[Y \mid X]$ is a **random variable** that varies as $X$ varies across different runs of the experiment.
2. **Confusing Conditioned and Target Variables:**
   - $\mathbb{E}[X \mid Y] \ne \mathbb{E}[Y \mid X]$.
   - Linearity applies only to the target variable: $\mathbb{E}[Y \mid X_1 + X_2] \ne \mathbb{E}[Y \mid X_1] + \mathbb{E}[Y \mid X_2]$.

---

---

## Exam Relevance

### Cross-Topic Connections / Exam Relevance

- **Adam's & Eve's Laws:** Solves compound random variables (see [[Adam's Law (Law of Total Expectation)]] and [[Eve's Law (Law of Total Variance)]]).
- **Martingales:** A stochastic process $\{M_n\}$ is a martingale if $\mathbb{E}[M_{n+1} \mid M_n, \dots, M_0] = M_n$.
- **Regression Analysis:** In regression modeling, the true regression function is precisely $r(x) = \mathbb{E}[Y \mid X = x]$.

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

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lecture 16, pages 50–53)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_10.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Chapter 9: Conditional Expectation)
