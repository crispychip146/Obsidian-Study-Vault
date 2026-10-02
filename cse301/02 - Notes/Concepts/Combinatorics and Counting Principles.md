---
type: concept
course: cse301
status: active
---

# Combinatorics and Counting Principles

## Definition

**Combinatorics** is the branch of discrete mathematics dedicated to counting the number of configurations, arrangements, and selections of elements from a finite set without explicitly listing them.

In probability theory, combinatorics forms the operational backbone of the **naive definition of probability**: when all outcomes in a finite sample space $S$ are equally likely, the probability of an event $A$ is simply:
$$P(A) = \frac{\lvert A \rvert}{\lvert S \rvert} = \frac{\# \text{ favorable outcomes}}{\text{total } \# \text{ possible outcomes}}$$

---

## Core Counting Rules

### 1. The Multiplication Rule
If an experiment consists of $k$ sequential stages, where:
- Stage 1 can result in $n_1$ possible outcomes,
- For each outcome of Stage 1, Stage 2 has $n_2$ possible outcomes,
- $\dots$,
- For each outcome of the first $k - 1$ stages, Stage $k$ has $n_k$ possible outcomes,

then the total number of composite outcomes is:
$$\text{Total Outcomes} = n_1 \times n_2 \times \dots \times n_k$$

### 2. Permutations (Order Matters)
A **permutation** is an ordered arrangement of distinct objects.
- The number of ways to arrange all $n$ distinct objects in a line is:
  $$n! = n \times (n - 1) \times (n - 2) \times \dots \times 2 \times 1$$
  (with the convention $0! = 1$).
- The number of ways to select and arrange $k$ objects from $n$ distinct objects without replacement is:
  $$P(n, k) = n(n - 1)(n - 2)\dots(n - k + 1) = \frac{n!}{(n - k)!}$$

### 3. Combinations (Order Does NOT Matter)
A **combination** is an unordered selection (a subset) of $k$ elements from a set of $n$ distinct elements.
Because each subset of size $k$ can be ordered in $k!$ different ways:
$$\binom{n}{k} = \frac{P(n, k)}{k!} = \frac{n!}{k!(n - k)!}$$
The symbol $\binom{n}{k}$ is read as *"n choose k"* and is known as the **binomial coefficient**.

---

## The Four Sampling Paradigms

When drawing $k$ items from a set of $n$ distinct objects:

| | Order Matters (Sequences) | Order Does Not Matter (Subsets) |
|---|---|---|
| **With Replacement** | $n^k$ | $\binom{n + k - 1}{k}$ (Bose-Einstein / Stars & Bars) |
| **Without Replacement** | $\frac{n!}{(n - k)!}$ | $\binom{n}{k}$ |

---

## Stars and Bars (Bose-Einstein Allocation)

To find the number of ways to distribute $k$ indistinguishable items into $n$ distinguishable bins:
Imagine lining up the $k$ items (represented by stars $\star$) and placing $n - 1$ dividers (bars $\mid$) between them to create $n$ compartments:
$$\star \star \mid \star \mid \mid \star \star \star \quad (k = 6 \text{ stars}, n = 4 \text{ bins})$$
Total positions in the line = $k + (n - 1)$.
The number of valid arrangements is the number of ways to choose the positions of the $k$ stars:
$$\binom{n + k - 1}{k} = \binom{n + k - 1}{n - 1}$$

---

## Story Proofs (Combinatorial Proofs)

A **story proof** (or combinatorial proof) proves an algebraic identity by interpreting both sides of the equation as two different ways of counting the exact same physical collection of items, avoiding tedious algebraic manipulation.

### Example 1: Symmetry of Binomial Coefficients
$$\binom{n}{k} = \binom{n}{n - k}$$
- **Story:** To pick a sports team of $k$ players from $n$ candidates, you can either choose the $k$ players who **make** the team ($\binom{n}{k}$ ways), or choose the $n - k$ players who are **cut** from the team ($\binom{n}{n-k}$ ways). Every selection of $k$ players uniquely specifies the $n - k$ players left behind.

### Example 2: Pascal's Identity
$$\binom{n}{k} = \binom{n - 1}{k - 1} + \binom{n - 1}{k}$$
- **Story:** Consider a group of $n$ people including one designated person, Alice. We want to form a committee of size $k$.
  - Group 1: Committees containing Alice. Alice takes 1 spot, leaving us to choose $k - 1$ members from the remaining $n - 1$ people: $\binom{n-1}{k-1}$ ways.
  - Group 2: Committees excluding Alice. All $k$ members must be chosen from the remaining $n - 1$ people: $\binom{n-1}{k}$ ways.
  - Every committee either contains Alice or does not. Summing the disjoint groups gives $\binom{n-1}{k-1} + \binom{n-1}{k} = \binom{n}{k}$.

### Example 3: Vandermonde's Identity
$$\binom{m + n}{k} = \sum_{j=0}^k \binom{m}{j} \binom{n}{k - j}$$
- **Story:** A class consists of $m$ computer science students and $n$ data science students. We choose a delegation of $k$ students. We can select $j$ computer science students ($\binom{m}{j}$ ways) and $k - j$ data science students ($\binom{n}{k-j}$ ways). Summing over all possible CS headcounts $j \in \{0, 1, \dots, k\}$ yields the identity.

---

## Common Mistakes

1. **Overcounting by treating identical items as distinct:**
   Forgetting to divide by $k!$ when order is irrelevant.
2. **Confusing "with replacement" vs. "without replacement":**
   Using $n^k$ when items cannot be reused.
3. **Assuming equally likely outcomes without checking symmetry:**
   The naive probability formula $\frac{\lvert A \rvert}{\lvert S \rvert}$ requires that every elementary outcome has the exact same probability of occurring.

---

## Related Concepts

- [[Probability Axioms and Naive Probability]]
- [[Inclusion-Exclusion Principle]]
- [[Birthday Problem and Collisions Example]]
- [[Derangements and Card Matching Example]]

---

## Sources

- [[01 - Sources/Lectures/Lecture_Notes_Complete.pdf]] (Lectures 1–2: Counting & Story Proofs)
- [[01 - Sources/Lectures/strategic_practice_and_homework_1.pdf]]
- [[01 - Sources/Lectures/ITP.pdf]] (Chapter 1: Probability and Counting)
