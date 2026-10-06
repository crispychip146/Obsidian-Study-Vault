---
type: problem
course: cse301
status: active
order: 25
---

# Problem — Expected Number of Local Maxima in Random Permutations

> 📖 **Reading Order:** Step 25 of 103 | **Module 2:** Random Variables and Distributions  
> ◄ **Previous:** [[Problem — Indicator Variables for Distinct Birthday Counts]] | ► **Next:** [[Conditional Probability and Independence]]

---

## Problem

Let $a_1, a_2, \dots, a_n$ be a random permutation of the numbers $\{1, 2, \dots, n\}$ where $n \ge 2$, chosen uniformly at random from all $n!$ possible permutations.

An element $a_j$ is defined as a **local maximum** if it is strictly greater than all of its adjacent neighbors. Specifically:
- For $j = 1$ (the left boundary), $a_1$ is a local maximum if $a_1 > a_2$.
- For $j = n$ (the right boundary), $a_n$ is a local maximum if $a_n > a_{n-1}$.
- For $2 \le j \le n - 1$ (interior positions), $a_j$ is a local maximum if $a_j > a_{j-1}$ and $a_j > a_{j+1}$.

Find the expected number of local maxima in the permutation.

---

## Given

- A permutation of $\{1, 2, \dots, n\}$ with $n \ge 2$.
- All $n!$ permutations are equally likely (uniform distribution).
- Let $X$ denote the total number of local maxima in the permutation.

---

## Required

Calculate the expected value $\mathbb{E}[X]$.

---

## Concepts Tested

- [[Linearity of Expectation and Indicator Random Variables Example]] — Decomposing complex counting problems into sums of Bernoulli indicators.
- [[Random Variables and Probability Distributions]] — Linearity of expectation holding without requiring independence.
- [[Combinatorics and Counting Principles]] — Symmetry of permutations.

---

## Prerequisites

- [[Combinatorics and Counting Principles]] — Permutations and equally likely outcomes.
- [[Random Variables and Probability Distributions]] — Indicator random variables and linearity of expectation.

---

## Question Type

Analytical / Linearity of Expectation Proof.

---

## Solution

### Step 1: Define Indicator Random Variables
Let $I_j$ be the indicator random variable for the event that position $j$ is a local maximum:
$$I_j = \begin{cases} 1 & \text{if position } j \text{ is a local maximum} \\ 0 & \text{otherwise} \end{cases}$$

The total number of local maxima is the sum of these indicators:
$$X = \sum_{j=1}^n I_j = I_1 + I_2 + \dots + I_n$$

By **Linearity of Expectation**, the expected value of the sum is the sum of expectations, **even though the indicator variables $I_j$ are clearly dependent**:
$$\mathbb{E}[X] = \sum_{j=1}^n \mathbb{E}[I_j] = \sum_{j=1}^n P(I_j = 1)$$

---

### Step 2: Calculate Marginal Probabilities for Boundary Positions

1. **Left Boundary ($j = 1$):**
   Position $1$ has only one neighbor ($a_2$).
   In a uniform random permutation, by symmetry, $a_1$ and $a_2$ are equally likely to be the larger of the two:
   $$P(I_1 = 1) = P(a_1 > a_2) = \frac{1}{2}$$

2. **Right Boundary ($j = n$):**
   Position $n$ has only one neighbor ($a_{n-1}$).
   By symmetry:
   $$P(I_n = 1) = P(a_n > a_{n-1}) = \frac{1}{2}$$

---

### Step 3: Calculate Marginal Probabilities for Interior Positions ($2 \le j \le n - 1$)

For any interior position $j$, $a_j$ is a local maximum if and only if it is strictly greater than both its left neighbor $a_{j-1}$ and its right neighbor $a_{j+1}$:
$$\{I_j = 1\} \iff \{a_j > a_{j-1} \text{ and } a_j > a_{j+1}\} \iff a_j = \max(a_{j-1}, a_j, a_{j+1})$$

Consider the triple of distinct numbers $(a_{j-1}, a_j, a_{j+1})$. 
- By the symmetry of uniform random permutations, all $3! = 6$ relative orderings of these three distinct values are equally likely.
- Exactly $2$ of these $6$ orderings place the largest value in the middle position:
  $$( \text{small}, \text{largest}, \text{medium} ) \quad \text{and} \quad ( \text{medium}, \text{largest}, \text{small} )$$
- Equivalently, each of the 3 positions in the triple has an equal $\frac{1}{3}$ probability of holding the maximum value.

Therefore, for any interior position $j \in \{2, 3, \dots, n - 1\}$:
$$P(I_j = 1) = \frac{2}{3!} = \frac{1}{3}$$

---

### Step 4: Sum Across All Positions
There are:
- $2$ boundary positions ($j = 1$ and $j = n$), each with expectation $\frac{1}{2}$.
- $n - 2$ interior positions ($j = 2, 3, \dots, n - 1$), each with expectation $\frac{1}{3}$.

Applying linearity of expectation:
$$\mathbb{E}[X] = \mathbb{E}[I_1] + \left(\sum_{j=2}^{n-1} \mathbb{E}[I_j]\right) + \mathbb{E}[I_n]$$
$$\mathbb{E}[X] = \frac{1}{2} + (n - 2) \cdot \frac{1}{3} + \frac{1}{2}$$
$$\mathbb{E}[X] = 1 + \frac{n - 2}{3} = \frac{3 + n - 2}{3} = \mathbf{\frac{n + 1}{3}}$$

---

## Result

$$\boxed{\mathbb{E}[X] = \frac{n + 1}{3}}$$

---

## Verification on Small Example ($n = 3$)

Let us verify the analytical result by enumerating all $3! = 6$ permutations of $\{1, 2, 3\}$:

| Permutation $(a_1, a_2, a_3)$ | Local Maxima | Count ($X$) |
|---|---|---|
| $(1, 2, 3)$ | $a_3 = 3$ | $1$ |
| $(1, 3, 2)$ | $a_2 = 3$ | $1$ |
| $(2, 1, 3)$ | $a_1 = 2$ and $a_3 = 3$ | $2$ |
| $(2, 3, 1)$ | $a_2 = 3$ | $1$ |
| $(3, 1, 2)$ | $a_1 = 3$ and $a_3 = 2$ | $2$ |
| $(3, 2, 1)$ | $a_1 = 3$ | $1$ |

Total count of local maxima across all $6$ permutations:
$$\sum X = 1 + 1 + 2 + 1 + 2 + 1 = 8$$

Empirical Average:
$$\mathbb{E}[X] = \frac{8}{6} = \frac{4}{3}$$

Formula prediction for $n = 3$:
$$\mathbb{E}[X] = \frac{3 + 1}{3} = \frac{4}{3}$$

The formula matches the exact enumeration.

---

## General Method and Key Takeaways

1. **Power of Indicators Over Global PMFs:** Computing the full PMF $P(X = k)$ of the number of local maxima is extraordinarily difficult due to dependencies between adjacent peaks. Indicator variables bypass the joint distribution completely because expectation is linear regardless of dependence.
2. **Symmetry Simplification:** You do not need to know the specific values of the numbers; by symmetry, any element in a randomly selected subset of size $k$ has probability $1/k$ of being the largest.

---

## Related Problems

- [[Problem — Indicator Variables for Distinct Birthday Counts]] — Applying indicators to coupon collecting and birthday distinct counts.
- [[Problem — Bounding Tail Probabilities with Chebyshev and Chernoff]] — Tail concentration around expected values.

---

## Sources

- [[cse301/01 - Sources/Lectures/HT Final Note - Suchi.pdf|HT Final Note: Lecture 10 (pp. 41–42)]]
- [[cse301/01 - Sources/Lectures/ITP.pdf|Textbook: Introduction to Probability (Blitzstein & Hwang, Chapter 4: Indicator Random Variables)]]
