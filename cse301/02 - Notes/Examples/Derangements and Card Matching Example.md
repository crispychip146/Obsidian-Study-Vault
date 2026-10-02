---
type: example
course: cse301
status: active
---

# Derangements and Card Matching Example

## Problem Context & Setup

Consider the classic **de Montmort Matching Problem** (also known as the Hat Check Problem or Secret Santa Problem):
A deck of $n$ distinct cards numbered $1, 2, \dots, n$ is thoroughly shuffled and dealt one by one into $n$ spots labeled $1, 2, \dots, n$.
- A **match** occurs at position $i$ if card $i$ is dealt into position $i$.
- A **derangement** is a permutation where **no** element appears in its original position (i.e., zero matches).

**Questions to Solve:**
1. What is the probability that at least one match occurs?
2. What is the probability of a complete derangement (no matches)?
3. What is the expected number of matches?
4. What happens as the number of cards $n \to \infty$?

---

## Step-by-Step Solution

### Question 1: Probability of At Least One Match
Let $A_i$ be the event that card $i$ is in position $i$, for $i \in \{1, 2, \dots, n\}$.
We wish to compute:
$$P(\text{at least one match}) = P\left( \bigcup_{i=1}^n A_i \right)$$

We apply the [[Inclusion-Exclusion Principle]]:
$$P\left( \bigcup_{i=1}^n A_i \right) = \sum_{k=1}^n (-1)^{k+1} S_k$$
where $S_k = \sum_{1 \le i_1 < i_2 < \dots < i_k \le n} P(A_{i_1} \cap A_{i_2} \cap \dots \cap A_{i_k})$.

1. **Single match probability $P(A_i)$:**
   Fix card $i$ in position $i$. The remaining $n-1$ cards can be ordered in $(n-1)!$ ways out of $n!$ total permutations:
   $$P(A_i) = \frac{(n-1)!}{n!} = \frac{1}{n}$$
   Since there are $\binom{n}{1} = n$ such events:
   $$S_1 = \sum_{i=1}^n P(A_i) = n \times \frac{1}{n} = 1$$

2. **Double match probability $P(A_i \cap A_j)$ ($i \ne j$):**
   Fix cards $i$ and $j$ in their respective positions. The remaining $n-2$ cards can be arranged in $(n-2)!$ ways:
   $$P(A_i \cap A_j) = \frac{(n-2)!}{n!} = \frac{1}{n(n-1)}$$
   The number of pairs is $\binom{n}{2} = \frac{n(n-1)}{2!}$:
   $$S_2 = \binom{n}{2} \frac{1}{n(n-1)} = \frac{n(n-1)}{2!} \frac{1}{n(n-1)} = \frac{1}{2!}$$

3. **General $k$-match intersection:**
   For any distinct $k$ positions:
   $$P(A_{i_1} \cap \dots \cap A_{i_k}) = \frac{(n-k)!}{n!}$$
   There are $\binom{n}{k}$ such subsets, hence:
   $$S_k = \binom{n}{k} \frac{(n-k)!}{n!} = \frac{n!}{k!(n-k)!} \frac{(n-k)!}{n!} = \frac{1}{k!}$$

4. **Summing via Inclusion-Exclusion:**
   $$P\left( \bigcup_{i=1}^n A_i \right) = \sum_{k=1}^n (-1)^{k+1} \frac{1}{k!} = 1 - \frac{1}{2!} + \frac{1}{3!} - \frac{1}{4!} + \dots + (-1)^{n+1}\frac{1}{n!}$$

---

### Question 2: Probability of a Derangement
By the complement rule:
$$P(\text{derangement}) = 1 - P\left( \bigcup_{i=1}^n A_i \right) = 1 - \left(1 - \frac{1}{2!} + \frac{1}{3!} - \dots + (-1)^{n+1}\frac{1}{n!}\right)$$
$$P(\text{derangement}) = \frac{1}{0!} - \frac{1}{1!} + \frac{1}{2!} - \frac{1}{3!} + \dots + (-1)^n \frac{1}{n!} = \sum_{k=0}^n \frac{(-1)^k}{k!}$$

The number of derangements of $n$ elements, denoted $D_n$ or $!n$, is:
$$D_n = n! \sum_{k=0}^n \frac{(-1)^k}{k!}$$

---

### Question 3: Expected Number of Matches
Let $X$ be the total number of matches:
$$X = I_{A_1} + I_{A_2} + \dots + I_{A_n}$$
where $I_{A_i} = 1$ if card $i$ matches, and $0$ otherwise.

Using **linearity of expectation** (which does NOT require independence):
$$\mathbb{E}[X] = \sum_{i=1}^n \mathbb{E}[I_{A_i}] = \sum_{i=1}^n P(A_i) = \sum_{i=1}^n \frac{1}{n} = n \times \frac{1}{n} = 1$$

Regardless of whether there are $10$ cards or $10,000,000$ cards, the **expected number of matches is always exactly 1**!

---

### Question 4: Asymptotic Limits as $n \to \infty$
Recall the Maclaurin series for $e^x$:
$$e^x = \sum_{k=0}^\infty \frac{x^k}{k!}$$
For $x = -1$:
$$e^{-1} = \frac{1}{e} = \sum_{k=0}^\infty \frac{(-1)^k}{k!} = 1 - 1 + \frac{1}{2!} - \frac{1}{3!} + \dots \approx 0.367879$$

Therefore, as $n \to \infty$:
$$\lim_{n \to \infty} P(\text{derangement}) = e^{-1} \approx 36.79\%$$
$$\lim_{n \to \infty} P(\text{at least one match}) = 1 - e^{-1} \approx 63.21\%$$

---

## Verification / Sanity Checks

Let's test small values of $n$:
- **$n = 1$:** 1 card, must match. $P(\text{match}) = 1$. Formula gives $1$. Correct.
- **$n = 2$:** Permutations are $(1,2)$ [2 matches] and $(2,1)$ [0 matches].
  $P(\text{match}) = 1/2$. Formula: $1 - 1/2 = 1/2$. Correct.
- **$n = 3$:** 6 permutations:
  - $(1,2,3)$ [3 matches]
  - $(1,3,2), (3,2,1), (2,1,3)$ [1 match each]
  - $(2,3,1), (3,1,2)$ [0 matches]
  Matches = $4/6 = 2/3 \approx 0.6667$.
  Formula: $1 - 1/2 + 1/6 = 4/6 = 2/3$. Correct.

---

## Key Takeaways & Exam Tips

- **Symmetry Trick:** Notice how $P(A_i \cap \dots \cap A_{i_k})$ only depends on the size $k$, not which specific indices are chosen. This allows pulling the probability outside the summation: $\sum_{1 \le i_1 < \dots < i_k \le n} \dots = \binom{n}{k} P(A_1 \cap \dots \cap A_k)$.
- **Convergence Speed:** Because $k!$ grows astronomically fast, $P(\text{derangement})$ converges to $1/e$ within 4 decimal places already at $n = 7$.

---

## Related Notes

- [[Inclusion-Exclusion Principle]] — Theoretical formula and indicator variable proof.
- [[Linearity of Expectation and Indicator Random Variables Example]] — Method of indicator variables.
- [[Probability Axioms and Naive Probability]] — Naive counting and sample spaces.

---

## Sources & Traceability

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lecture 3, pages 7–9)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_2.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Section 1.6 & Example 1.6.4)
