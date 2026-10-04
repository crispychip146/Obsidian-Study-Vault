---
type: formula
course: cse301
status: active
order: 3
---

# Inclusion-Exclusion Principle

> 📖 **Reading Order:** Step 03 of 92 | **Module 1:** Counting and Discrete Probability  
> ◄ **Previous:** [[Probability Axioms and Naive Probability]] | ► **Next:** [[Birthday Problem and Collisions Example]]

---

---

## The Question and Earlier Knowledge

What analytical relationship or closed-form expectation governs Inclusion-Exclusion Principle, and how can we compute it directly from constituent probability terms? In complex probability models, calculating probabilities or moments directly is often intractable without decomposing expectations across conditioning partitions or inequalities.

---

## Developing the Formula

By decomposing joint distributions into conditional components, expanding algebraic products, or applying geometric series sums, Inclusion-Exclusion Principle compresses complex probabilistic reasoning into a clean, reusable formula.

---

## Formula

Let $A_1, A_2, \dots, A_n$ be events in a probability space. The probability that **at least one** of these events occurs is given by the **Inclusion-Exclusion Principle**:

$$P\left( \bigcup_{i=1}^n A_i \right) = \sum_{i=1}^n P(A_i) - \sum_{1 \le i < j \le n} P(A_i \cap A_j) + \sum_{1 \le i < j < k \le n} P(A_i \cap A_j \cap A_k) - \dots + (-1)^{n+1} P\left( \bigcap_{i=1}^n A_i \right)$$

More compactly:
$$P\left( \bigcup_{i=1}^n A_i \right) = \sum_{k=1}^n (-1)^{k+1} S_k$$

where $S_k$ is the sum of the probabilities of all distinct $k$-way intersections:
$$S_k = \sum_{1 \le i_1 < i_2 < \dots < i_k \le n} P(A_{i_1} \cap A_{i_2} \cap \dots \cap A_{i_k})$$

There are $\binom{n}{k}$ terms in each sum $S_k$, yielding a total of $2^n - 1$ terms.

---

---

## Variables

The cleanest and most rigorous proof uses indicator random variables:
Let $I_{A_i}$ be the indicator variable for event $A_i$ (i.e., $I_{A_i} = 1$ if $A_i$ occurs, and $0$ otherwise).

Consider the complement event: none of the $A_i$ occur, which means $\bigcap_{i=1}^n A_i^c$ occurs:
$$I_{\left(\bigcup_{i=1}^n A_i\right)^c} = \prod_{i=1}^n (1 - I_{A_i})$$

Expanding this algebraic product:
$$\prod_{i=1}^n (1 - I_{A_i}) = 1 - \sum_{i=1}^n I_{A_i} + \sum_{i < j} I_{A_i} I_{A_j} - \sum_{i < j < k} I_{A_i} I_{A_j} I_{A_k} + \dots + (-1)^n I_{A_1} I_{A_2} \dots I_{A_n}$$

Subtracting both sides from $1$:
$$I_{\bigcup_{i=1}^n A_i} = 1 - \prod_{i=1}^n (1 - I_{A_i}) = \sum_{i=1}^n I_{A_i} - \sum_{i < j} I_{A_i \cap A_j} + \dots + (-1)^{n+1} I_{\bigcap_{i=1}^n A_i}$$

Taking the expectation $\mathbb{E}[\cdot]$ on both sides, and using the fundamental fact that $\mathbb{E}[I_E] = P(E)$ and that expectation is strictly linear:
$$P\left( \bigcup_{i=1}^n A_i \right) = \sum_{i=1}^n P(A_i) - \sum_{i < j} P(A_i \cap A_j) + \dots + (-1)^{n+1} P\left( \bigcap_{i=1}^n A_i \right)$$

$\blacksquare$

---

---

## Conditions

- Random variables must possess finite first and second moments (well-defined expectations).
- Probability distributions must satisfy standard non-negativity and total probability integration axioms.

---

## Intuition

### Bonferroni Inequalities (Truncation Bounds)

When $n$ is large, computing all $2^n - 1$ terms is intractable. The partial sums provide alternating upper and lower bounds:
- **1 term (Boole's inequality):**
  $$P\left( \bigcup_{i=1}^n A_i \right) \le S_1$$
- **2 terms:**
  $$P\left( \bigcup_{i=1}^n A_i \right) \ge S_1 - S_2$$
- **3 terms:**
  $$P\left( \bigcup_{i=1}^n A_i \right) \le S_1 - S_2 + S_3$$

In general, stopping after an **odd** number of sums gives an **upper bound**, while stopping after an **even** number of sums gives a **lower bound**.

---

---

## Derivation

Derived by applying definition of expectation, interchanging summation/integrals via Fubini's theorem, and collecting terms.

---

## Example

### Application Examples

### 1. The Montmort Matching Problem (Derangements)
A deck of $n$ numbered cards ($1, 2, \dots, n$) is shuffled. A match occurs at position $i$ if card $i$ is at the $i$-th position.
- Event $A_i$: match at position $i$. $P(A_i) = \frac{(n-1)!}{n!} = \frac{1}{n}$.
- Pairwise: $P(A_i \cap A_j) = \frac{(n-2)!}{n!} = \frac{1}{n(n-1)}$.
- $k$-way intersection: $P(A_{i_1} \cap \dots \cap A_{i_k}) = \frac{(n-k)!}{n!}$.
- Sum $S_k = \binom{n}{k} \frac{(n-k)!}{n!} = \frac{n!}{k!(n-k)!} \frac{(n-k)!}{n!} = \frac{1}{k!}$.

Hence, the probability of at least one match is:
$$P(\text{at least one match}) = \sum_{k=1}^n (-1)^{k+1} \frac{1}{k!} = 1 - \frac{1}{2!} + \frac{1}{3!} - \dots + (-1)^{n+1} \frac{1}{n!}$$

As $n \to \infty$:
$$P(\text{at least one match}) \to 1 - e^{-1} \approx 0.6321$$
$$P(\text{no matches / derangement}) \to e^{-1} \approx 0.3679$$
Remarkably, for $n \ge 7$, this probability is essentially constant!

---

---

## Common Mistakes

- Confusing conditional variance with the variance of conditional expectation (Eve's Law components).
- Forgetting that linearity of expectation holds unconditionally, whereas $\mathbb{E}[XY] = \mathbb{E}[X]\mathbb{E}[Y]$ requires independence.

---

## Related Concepts

- [[Probability Axioms and Naive Probability]] — Axiomatic basis.
- [[Derangements and Card Matching Example]] — Full step-by-step example.
- [[Linearity of Expectation and Indicator Random Variables Example]] — Exploitation of indicator algebra.

---

---

## Prerequisites

- [[Random Variables and Probability Distributions]]

---

## Problems

- [[Problem — Birthday Collisions and Approximation]]

---

## Sources

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lecture 3, pages 7–9)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_2.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Section 1.6: Inclusion-Exclusion)
