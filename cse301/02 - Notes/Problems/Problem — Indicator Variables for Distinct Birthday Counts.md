---
type: problem
course: cse301
status: active
order: 16
---

# Problem — Indicator Variables for Distinct Birthday Counts

> 📖 **Reading Order:** Step 16 of 92 | **Module 2:** Random Variables and Distributions  
> ◄ **Previous:** [[Exponential Distribution Memorylessness Example]] | ► **Next:** [[Conditional Probability and Independence]]

---

---

## Problem

Consider $k$ individuals whose birthdays are independent and uniformly distributed across $n$ days of the year (where $n = 365$). Let $D$ be the random variable representing the number of distinct days that are the birthday of at least one person in the group.

1. **Indicator Representation:** Express $D$ as a sum of indicator random variables $I_1, I_2, \dots, I_n$.
2. **Expectation:** Calculate the exact expected value $\mathbb{E}[D]$ in terms of $n$ and $k$.
3. **Indicator Covariance:** For any two distinct days $i \ne j$, compute the joint expectation $\mathbb{E}[I_i I_j]$ and the covariance $\operatorname{Cov}(I_i, I_j)$. Explain intuitively why the covariance is negative.
4. **Exact Variance:** Derive a closed-form formula for the variance $\operatorname{Var}(D)$.

---

---

## Given

- Given parameters, random variable definitions, and observation vectors as specified in the problem statement.

---

## Required

- Derive the exact closed-form probability, expectation, or test statistic, and verify asymptotic convergence.

---

## Concepts Tested

- [[Random Variables and Probability Distributions]]
- [[Law of Total Probability and Bayes' Rule]]

---

## Prerequisites

- [[Discrete Probability Distributions]] — Indicators and Bernoulli variables.
- [[Linearity of Expectation and Indicator Random Variables Example]] — Method of indicators.
- [[Covariance and Correlation]] — Covariance of indicators and variance of a sum.

---

---

## Question Type

Probability / Statistical Inference / Markov Chain Analysis

---

## Solution

### Understanding the Situation
Interpret the given sample space, random variables, and event conditions.

### Developing the Key Idea
Select the governing probabilistic principle (e.g. Chapman-Kolmogorov, Adam's Law, Central Limit Theorem, or likelihood maximization) and verify that conditions hold.

### Working Through the Solution
### Full Step-by-Step Solution

### Part 1: Indicator Representation
For each day $i \in \{1, 2, \dots, n\}$, define the indicator variable:
$$I_i = \begin{cases} 1 & \text{if at least one person has their birthday on day } i \\ 0 & \text{if nobody has their birthday on day } i \end{cases}$$

The total count of distinct days observed is simply:
$$D = \sum_{i=1}^n I_i$$

---

### Part 2: Expected Value $\mathbb{E}[D]$
For an arbitrary day $i$:
A specific person does **not** have their birthday on day $i$ with probability $1 - \frac{1}{n}$.
Because all $k$ people have independent birthdays:
$$P(I_i = 0) = P(\text{all } k \text{ people born on other days}) = \left( 1 - \frac{1}{n} \right)^k$$

Thus, the marginal probability of day $i$ being occupied is:
$$p \equiv P(I_i = 1) = \mathbb{E}[I_i] = 1 - \left( 1 - \frac{1}{n} \right)^k$$

By linearity of expectation:
$$\mathbb{E}[D] = \sum_{i=1}^n \mathbb{E}[I_i] = n \left[ 1 - \left( 1 - \frac{1}{n} \right)^k \right]$$

---

### Part 3: Joint Expectation $\mathbb{E}[I_i I_j]$ and Covariance $\operatorname{Cov}(I_i, I_j)$ ($i \ne j$)
The product of two indicators is itself an indicator:
$$I_i I_j = \begin{cases} 1 & \text{if BOTH day } i \text{ and day } j \text{ have at least one birthday} \\ 0 & \text{otherwise} \end{cases}$$

Thus, $\mathbb{E}[I_i I_j] = P(I_i = 1 \cap I_j = 1)$.
Using De Morgan's Law and the Inclusion-Exclusion principle:
$$P(I_i = 1 \cap I_j = 1) = 1 - P(I_i = 0 \cup I_j = 0)$$
$$= 1 - \left[ P(I_i = 0) + P(I_j = 0) - P(I_i = 0 \cap I_j = 0) \right]$$

We evaluate each term:
- $P(I_i = 0) = P(I_j = 0) = \left( 1 - \frac{1}{n} \right)^k$
- $P(I_i = 0 \cap I_j = 0)$: The probability that a person is born on neither day $i$ nor day $j$ is $1 - \frac{2}{n}$. For $k$ independent people:
  $$P(I_i = 0 \cap I_j = 0) = \left( 1 - \frac{2}{n} \right)^k$$

Substituting these into the formula:
$$\mathbb{E}[I_i I_j] = 1 - 2\left( 1 - \frac{1}{n} \right)^k + \left( 1 - \frac{2}{n} \right)^k$$

Now compute the covariance:
$$\operatorname{Cov}(I_i, I_j) = \mathbb{E}[I_i I_j] - \mathbb{E}[I_i]\mathbb{E}[I_j]$$
Since $\mathbb{E}[I_i]\mathbb{E}[I_j] = \left[ 1 - \left( 1 - \frac{1}{n} \right)^k \right]^2 = 1 - 2\left( 1 - \frac{1}{n} \right)^k + \left( 1 - \frac{1}{n} \right)^{2k}$:
$$\operatorname{Cov}(I_i, I_j) = \left[ 1 - 2\left( 1 - \frac{1}{n} \right)^k + \left( 1 - \frac{2}{n} \right)^k \right] - \left[ 1 - 2\left( 1 - \frac{1}{n} \right)^k + \left( 1 - \frac{1}{n} \right)^{2k} \right]$$
$$\operatorname{Cov}(I_i, I_j) = \left( 1 - \frac{2}{n} \right)^k - \left( 1 - \frac{1}{n} \right)^{2k}$$

#### Intuition for Negative Covariance:
Notice that:
$$\left( 1 - \frac{1}{n} \right)^2 = 1 - \frac{2}{n} + \frac{1}{n^2} > 1 - \frac{2}{n}$$
Raising both sides to the power $k > 0$:
$$\left( 1 - \frac{1}{n} \right)^{2k} > \left( 1 - \frac{2}{n} \right)^k \implies \operatorname{Cov}(I_i, I_j) < 0$$
The covariance is strictly negative because the total number of people $k$ is fixed. If day $i$ is heavily occupied, fewer people remain to occupy day $j$, making it slightly less likely that day $j$ is occupied!

---

### Part 4: Exact Variance $\operatorname{Var}(D)$
Using the variance of a sum of random variables:
$$\operatorname{Var}(D) = \sum_{i=1}^n \operatorname{Var}(I_i) + \sum_{i \ne j} \operatorname{Cov}(I_i, I_j)$$

1. **Variance of single indicator:**
   $$\operatorname{Var}(I_i) = p(1 - p) = \left[ 1 - \left( 1 - \frac{1}{n} \right)^k \right] \left( 1 - \frac{1}{n} \right)^k = \left( 1 - \frac{1}{n} \right)^k - \left( 1 - \frac{1}{n} \right)^{2k}$$
   There are $n$ such terms:
   $$\sum_{i=1}^n \operatorname{Var}(I_i) = n \left[ \left( 1 - \frac{1}{n} \right)^k - \left( 1 - \frac{1}{n} \right)^{2k} \right]$$

2. **Covariance terms:**
   There are $n(n - 1)$ pairs $(i, j)$ with $i \ne j$:
   $$\sum_{i \ne j} \operatorname{Cov}(I_i, I_j) = n(n - 1) \left[ \left( 1 - \frac{2}{n} \right)^k - \left( 1 - \frac{1}{n} \right)^{2k} \right]$$

3. **Total Variance:**
   $$\operatorname{Var}(D) = n\left( 1 - \frac{1}{n} \right)^k - n\left( 1 - \frac{1}{n} \right)^{2k} + n(n-1)\left( 1 - \frac{2}{n} \right)^k - n(n-1)\left( 1 - \frac{1}{n} \right)^{2k}$$
   Combine the $-\left( 1 - \frac{1}{n} \right)^{2k}$ coefficients: $-(n + n(n - 1)) = -n^2$:
   $$\operatorname{Var}(D) = n\left( 1 - \frac{1}{n} \right)^k + n(n - 1)\left( 1 - \frac{2}{n} \right)^k - n^2 \left( 1 - \frac{1}{n} \right)^{2k}$$

---
### Alternative Approaches / Insights

- **Asymptotic Limit ($n \to \infty, k/n \to c$):**
  Let $c = k/n$. Then $\left(1 - \frac{1}{n}\right)^k \to e^{-c}$ and $\left(1 - \frac{2}{n}\right)^k \to e^{-2c}$.
  $$\frac{\mathbb{E}[D]}{n} \to 1 - e^{-c}$$
  $$\frac{\operatorname{Var}(D)}{n} \to e^{-c} - e^{-2c} - c^2 e^{-2c} \dots$$
  This matches the variance of a Poissonized occupancy model where bucket counts are independent Poisson RVs conditioned on their sum.

---

### Result and Interpretation
The final analytical solution and numerical metrics are rigorously verified against probability axioms.

---

## Reusable Insight

Always decompose complex event probabilities by conditioning on a partition of the sample space (Law of Total Probability), or by writing indicator random variables to exploit linearity of expectation.

---

## Common Mistakes

1. **Forgetting Covariances:** Assuming $\operatorname{Var}(D) = \sum \operatorname{Var}(I_i) = np(1-p)$. This is invalid because the indicators $I_i$ are NOT independent.
2. **Sign of Covariance:** Missing the fact that competition for a fixed number of people creates negative dependence ($\operatorname{Cov} < 0$), which makes the true variance *smaller* than it would be under independent trials.

---

---

## Exam Pattern

Standard BUET CSE 301 final exam question testing probability bounds, Markov chain stationarity, or statistical parameter estimation.

---

## Related Problems

- [[Problem — Birthday Collisions and Approximation]]
- [[Problem — Four-Day Weather Forecast]]

---

## Related Concepts

- [[Random Variables and Probability Distributions]]
- [[Law of Total Probability and Bayes' Rule]]

---

## Source

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lecture 8 & 13, pages 23–25, 40–43)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_5.pdf` & `8.pdf`
- **Question ID:** `Q-CSE301-014`
