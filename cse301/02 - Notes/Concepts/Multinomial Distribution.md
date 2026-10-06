---
type: concept
course: cse301
status: active
order: 10
---

# Multinomial Distribution

> 📖 **Reading Order:** Step 10 of 103 | **Module 2:** Random Variables and Distributions  
> ◄ **Previous:** [[Discrete Probability Distributions]] | ► **Next:** [[Continuous Probability Distributions]]

---

## Starting Point and the Problem

The [[Discrete Probability Distributions|Binomial Distribution]] models the count of successes in $n$ independent trials where each trial has only two mutually exclusive outcomes: success (with probability $p$) and failure (with probability $1 - p$).

However, countless computational and statistical phenomena involve trials with **three or more possible categories**:
- Rolling a 6-sided die $n$ times and recording how many times each face $1, \dots, 6$ appears.
- Genomics: Sequencing a DNA fragment of length $n$ with four nucleotide bases ($\text{A}, \text{C}, \text{G}, \text{T}$).
- Natural Language Processing (Bag-of-Words): Generating a text document of $n$ words chosen from a vocabulary of $k$ words.
- Machine Learning: Classifying objects into $k$ distinct classes.

When an experiment has $k \ge 3$ possible outcomes, a single scalar random variable is insufficient. We require a **random vector** $\mathbf{X} = (X_1, X_2, \dots, X_k)$ that jointly tracks the counts across all categories.

---

## Developing the Idea

### The Distribution Story
Suppose we perform $n$ independent and identically distributed trials. Each trial must result in exactly one of $k$ distinct categories $\{1, 2, \dots, k\}$.
- The probability that any trial results in category $j$ is $p_j$, where:
  $$p_j \ge 0 \quad \text{for all } j \in \{1, \dots, k\}, \quad \text{and} \quad \sum_{j=1}^k p_j = 1$$
- Let $X_j$ denote the total number of trials resulting in category $j$.
- Because every trial produces exactly one outcome, the total count is strictly constrained:
  $$\sum_{j=1}^k X_j = n$$

### Constructing the Joint PMF
Suppose we observe a specific sequence of outcomes that yields exactly $n_1$ occurrences of category 1, $n_2$ of category 2, $\dots$, and $n_k$ of category $k$ (with $\sum n_j = n$).

1. **Probability of Any Specific Sequence:**
   By independence of trials, the probability of any particular ordering with these counts is:
   $$p_1^{n_1} p_2^{n_2} \cdots p_k^{n_k}$$

2. **Counting the Permutations (Multinomial Coefficient):**
   How many distinct sequences of length $n$ contain exactly $n_1$ category 1s, $n_2$ category 2s, $\dots$, $n_k$ category $k$s?
   From [[Combinatorics and Counting Principles|multinomial counting]]:
   $$\binom{n}{n_1, n_2, \dots, n_k} = \frac{n!}{n_1! \, n_2! \, \cdots \, n_k!}$$

3. **Multiplying Count by Sequence Probability:**
   Summing over all disjoint sequences yields the joint PMF:
   $$P(X_1 = n_1, X_2 = n_2, \dots, X_k = n_k) = \frac{n!}{n_1! \, n_2! \, \cdots \, n_k!} p_1^{n_1} p_2^{n_2} \cdots p_k^{n_k}$$

---

## Definition

A random vector $\mathbf{X} = (X_1, X_2, \dots, X_k)$ follows a **Multinomial Distribution** with parameters $n \in \mathbb{N}$ and probability vector $\mathbf{p} = (p_1, p_2, \dots, p_k)$, denoted:
$$\mathbf{X} \sim \operatorname{Mult}(n, \mathbf{p})$$

### Joint PMF
$$P(X_1 = n_1, \dots, X_k = n_k) = \frac{n!}{\prod_{j=1}^k n_j!} \prod_{j=1}^k p_j^{n_j}$$
defined on the support:
$$\{(n_1, \dots, n_k) \in \mathbb{N}_0^k : \sum_{j=1}^k n_j = n\}$$

---

## How It Works: The Lumping Property and Marginals

One of the most important structural properties of the multinomial distribution is the **Lumping Property**.

### Marginal Distribution of a Single Component
What is the marginal distribution of an individual component $X_j$?
- We can "lump" all categories other than $j$ into a single composite category called "not $j$".
- Each trial now has two outcomes:
  - Category $j$ with probability $p_j$
  - Any other category with probability $1 - p_j$
- By definition of the Binomial distribution, the count $X_j$ is simply:
  $$X_j \sim \operatorname{Bin}(n, p_j)$$

From this immediate reduction, the expectation and variance follow without any integration or summation:
$$\mathbb{E}[X_j] = n p_j$$
$$\operatorname{Var}(X_j) = n p_j (1 - p_j)$$

### Lumping Multiple Categories
If we collapse the first $m$ categories together into a single group $Y = X_1 + X_2 + \dots + X_m$, then:
$$Y \sim \operatorname{Bin}\left(n, \sum_{i=1}^m p_i\right)$$
More generally, collapsing the $k$ original categories into $m < k$ super-categories preserves the multinomial distribution:
$$(Y_1, \dots, Y_m) \sim \operatorname{Mult}\left(n, (q_1, \dots, q_m)\right) \quad \text{where } q_r = \sum_{j \in G_r} p_j$$

---

## Covariance Between Categories

Because the total sum of counts is fixed to $n$ ($\sum_{j=1}^k X_j = n$), the components cannot be independent. If category $i$ occurs very frequently, fewer trials remain for category $j$. Hence, the categories are **negatively correlated**.

### Derivation of Covariance
Consider the variance of the sum $X_i + X_j$ for $i \ne j$:
1. By the lumping property, $X_i + X_j \sim \operatorname{Bin}(n, p_i + p_j)$.
2. The variance of this binomial sum is:
   $$\operatorname{Var}(X_i + X_j) = n (p_i + p_j)(1 - p_i - p_j)$$
3. Expanding via the general variance-of-sums formula:
   $$\operatorname{Var}(X_i + X_j) = \operatorname{Var}(X_i) + \operatorname{Var}(X_j) + 2\operatorname{Cov}(X_i, X_j)$$
   $$n(p_i + p_j)(1 - p_i - p_j) = n p_i(1 - p_i) + n p_j(1 - p_j) + 2\operatorname{Cov}(X_i, X_j)$$
4. Simplifying the algebra:
   $$2\operatorname{Cov}(X_i, X_j) = -2 n p_i p_j \implies \operatorname{Cov}(X_i, X_j) = -n p_i p_j$$

The correlation coefficient is:
$$\rho(X_i, X_j) = \frac{\operatorname{Cov}(X_i, X_j)}{\sqrt{\operatorname{Var}(X_i)\operatorname{Var}(X_j)}} = -\sqrt{\frac{p_i p_j}{(1 - p_i)(1 - p_j)}} < 0$$

---

## Example

### Worked Example: Dice Rolling Counts

A fair 6-sided die is rolled $n = 12$ times. What is the probability of rolling:
- Exactly three 1s ($X_1 = 3$),
- Exactly two 2s ($X_2 = 2$),
- Exactly one 3 ($X_3 = 1$),
- Exactly zero 4s ($X_4 = 0$),
- Exactly four 5s ($X_5 = 4$),
- Exactly two 6s ($X_6 = 2$)?

Here $k = 6$, $p_1 = p_2 = \dots = p_6 = 1/6$.
The counts sum to $3 + 2 + 1 + 0 + 4 + 2 = 12 = n$.

Applying the Multinomial PMF:
$$P(X_1=3, X_2=2, X_3=1, X_4=0, X_5=4, X_6=2) = \frac{12!}{3! \, 2! \, 1! \, 0! \, 4! \, 2!} \left(\frac{1}{6}\right)^{12}$$

1. **Calculate the Multinomial Coefficient:**
   $$\frac{12!}{6 \times 2 \times 1 \times 1 \times 24 \times 2} = \frac{479,001,600}{576} = 831,600$$
2. **Calculate the Probability:**
   $$P = 831,600 \times \left(\frac{1}{6}\right)^{12} = \frac{831,600}{2,176,782,336} \approx 0.000382 \quad (0.0382\%)$$

---

## Technical Details

### Conditional Distribution Given One Component
If we condition on observing $X_1 = n_1$, how are the remaining counts $(X_2, \dots, X_k)$ distributed?
- There are $n - n_1$ trials remaining.
- The remaining categories have relative probabilities $p_j / (1 - p_1)$ for $j \in \{2, \dots, k\}$.
- Therefore, conditional on $X_1 = n_1$:
  $$(X_2, \dots, X_k) \mid (X_1 = n_1) \sim \operatorname{Mult}\left(n - n_1, \left(\frac{p_2}{1 - p_1}, \dots, \frac{p_k}{1 - p_1}\right)\right)$$

### Connection to Poisson Sampling
If $Y_1, \dots, Y_k$ are independent Poisson random variables $Y_j \sim \operatorname{Pois}(\lambda_j)$, and we condition on their sum being a fixed constant $\sum_{j=1}^k Y_j = n$:
$$(Y_1, \dots, Y_k) \mid \left(\sum_{j=1}^k Y_j = n\right) \sim \operatorname{Mult}(n, \mathbf{p}) \quad \text{where } p_j = \frac{\lambda_j}{\sum_{i=1}^k \lambda_i}$$
This identity connects Poisson processes directly to multinomial sampling and is fundamental in queuing theory and statistical mechanics.

---

## Common Mistakes

- **Assuming Category Independence:** Forgetting that $\operatorname{Cov}(X_i, X_j) = -n p_i p_j \ne 0$. Categories are negatively correlated because their counts compete for the fixed budget $n$.
- **Ignoring the Support Constraint:** Computing joint probabilities when $\sum n_j \ne n$. The PMF is strictly $0$ if $\sum n_j \ne n$.
- **Overcomplicating Marginals:** Attempting to compute $\mathbb{E}[X_j]$ via high-dimensional summation instead of applying the lumping property to directly show $X_j \sim \operatorname{Bin}(n, p_j)$.

---

## Exam Relevance

In CSE 301 examinations, questions on the Multinomial Distribution assess:
1. **Marginal and Expectation Identification:** Stating the marginal distribution of any single category ($X_j \sim \operatorname{Bin}(n, p_j)$) using lumping arguments.
2. **Covariance Proofs:** Deriving $\operatorname{Cov}(X_i, X_j) = -n p_i p_j$ via $\operatorname{Var}(X_i + X_j)$.
3. **Chi-Square Goodness-of-Fit Connection:** Serving as the underlying sampling distribution for [[Pearson's Chi-Square Goodness-of-Fit Test]] with $k - 1$ degrees of freedom.

---

## Related Concepts

- [[Discrete Probability Distributions]] — Binomial distribution as the $k=2$ special case.
- [[Joint and Marginal Distributions]] — Multivariable joint PMF and marginalization.
- [[Covariance and Correlation]] — Negative correlation in constrained random vectors.
- [[Pearson's Chi-Square Goodness-of-Fit Test]] — Hypothesis testing on multinomial observation vectors.

---

## Prerequisites

- [[Combinatorics and Counting Principles]] — Multinomial coefficients and permutations with repetition.
- [[Discrete Probability Distributions]] — Binomial distribution.

---

## Sources

- [[cse301/01 - Sources/Lectures/HT Final Note - Suchi.pdf|HT Final Note: Lecture 17 (p. 72)]]
- [[cse301/01 - Sources/Lectures/ITP.pdf|Textbook: Introduction to Probability (Blitzstein & Hwang, Section 3.7: Multinomial Distribution)]]
