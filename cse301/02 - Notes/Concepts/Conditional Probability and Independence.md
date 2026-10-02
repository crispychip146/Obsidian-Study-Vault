---
type: concept
course: cse301
status: active
---

# Conditional Probability and Independence

## Definition

In real-world decision making, new information arrives continuously. **Conditional probability** quantifies how the probability of an event $A$ updates upon learning that an event $B$ has occurred.

For any two events $A$ and $B$ with $P(B) > 0$, the conditional probability of $A$ given $B$ is defined as:
$$P(A \mid B) = \frac{P(A \cap B)}{P(B)}$$

### Intuition: Shrinking the Sample Space
Conditioning on $B$ discards all outcomes outside of $B$. The event $B$ becomes the **new sample space (universe)**. The only part of $A$ that can still occur is $A \cap B$, whose original probability must be normalized by dividing by $P(B)$ so that $P(B \mid B) = 1$.

---

## The Multiplication Rule

Rearranging the definition of conditional probability:
$$P(A \cap B) = P(B) P(A \mid B) = P(A) P(B \mid A)$$

### Generalized Multiplication Rule (Chain Rule):
For any sequence of events $A_1, A_2, \dots, A_n$ with $P(A_1 \cap \dots \cap A_{n-1}) > 0$:
$$P(A_1 \cap A_2 \cap \dots \cap A_n) = P(A_1) P(A_2 \mid A_1) P(A_3 \mid A_1 \cap A_2) \dots P(A_n \mid A_1 \cap \dots \cap A_{n-1})$$

This chain rule is the mathematical bedrock of **Markov processes**, **autoregressive language models**, and sequential Bayesian filtering.

---

## Independence of Events

Two events $A$ and $B$ are **independent** (written $A \perp B$) if learning that $B$ occurred provides zero information about whether $A$ occurred:
$$P(A \mid B) = P(A)$$

Equivalently, substituting this into the definition of conditional probability yields the **product definition of independence**:
$$P(A \cap B) = P(A) P(B)$$

### Important Properties of Independence:
1. **Symmetry:** If $A \perp B$, then $B \perp A$.
2. **Independence of Complements:** If $A \perp B$, then:
   - $A \perp B^c$
   - $A^c \perp B$
   - $A^c \perp B^c$
3. **Empty Set and Universe:** Any event $A$ is independent of the empty set $\emptyset$ and the entire sample space $S$.

---

## Pairwise Independence vs. Mutual Independence

For three or more events, **pairwise independence does NOT imply mutual independence**!

### Definition of Mutual Independence:
A collection of events $A_1, A_2, \dots, A_n$ is **mutually independent** if for **every subset** $J \subseteq \{1, 2, \dots, n\}$:
$$P\left( \bigcap_{j \in J} A_j \right) = \prod_{j \in J} P(A_j)$$
For $n = 3$, mutual independence requires $2^3 - 3 - 1 = 4$ equations:
1. $P(A_1 \cap A_2) = P(A_1)P(A_2)$
2. $P(A_1 \cap A_3) = P(A_1)P(A_3)$
3. $P(A_2 \cap A_3) = P(A_2)P(A_3)$
4. $P(A_1 \cap A_2 \cap A_3) = P(A_1)P(A_2)P(A_3)$

### The Bernstein Counterexample:
Flip two fair independent coins: Coin 1 and Coin 2.
- Let $A$ = Coin 1 lands Heads ($P(A) = 1/2$).
- Let $B$ = Coin 2 lands Heads ($P(B) = 1/2$).
- Let $C$ = Both coins land on the same face (both Heads or both Tails) ($P(C) = 1/2$).

Checking pairwise:
- $P(A \cap B) = P(HH) = 1/4 = P(A)P(B)$ $\implies A \perp B$.
- $P(A \cap C) = P(HH) = 1/4 = P(A)P(C)$ $\implies A \perp C$.
- $P(B \cap C) = P(HH) = 1/4 = P(B)P(C)$ $\implies B \perp C$.

However, checking the 3-way intersection:
$$P(A \cap B \cap C) = P(HH) = \frac{1}{4}$$
Yet $P(A)P(B)P(C) = \frac{1}{2} \times \frac{1}{2} \times \frac{1}{2} = \frac{1}{8} \ne \frac{1}{4}$!
In fact, if you know that both $A$ and $B$ occurred, you know with $100\%$ certainty that $C$ occurred ($P(C \mid A \cap B) = 1 \ne 1/2$).
Thus, $A, B, C$ are **pairwise independent but NOT mutually independent**!

---

## Conditional Independence

Events $A$ and $B$ are **conditionally independent given $C$** if:
$$P(A \cap B \mid C) = P(A \mid C) P(B \mid C)$$
Equivalently:
$$P(A \mid B \cap C) = P(A \mid C)$$

### Critical Distinction:
- Conditional independence does **NOT** imply marginal independence.
- Marginal independence does **NOT** imply conditional independence.
- *Example:* Two symptoms of a single underlying disease are conditionally independent given the disease status, but strongly dependent overall in the population.

---

## Edge Cases & Common Pitfalls

1. **Mutually Exclusive vs. Independent:**
   - If $A$ and $B$ are mutually exclusive ($A \cap B = \emptyset$), then $P(A \cap B) = 0$.
   - For independent events with $P(A) > 0, P(B) > 0$, $P(A \cap B) = P(A)P(B) > 0$.
   - **Mutually exclusive events with non-zero probability can NEVER be independent!**
2. **Conditioning on Measure-Zero Events:**
   - In continuous spaces, $P(X = x) = 0$, requiring conditioning via density ratios $f_{Y \mid X}(y \mid x) = \frac{f_{X, Y}(x, y)}{f_X(x)}$ or infinitesimal limits.

---

## Cross-Topic Connections / Exam Relevance

- **Bayes' Rule:** Inverts condition and effect (see [[Law of Total Probability and Bayes' Rule]]).
- **Markov Property:** Future is conditionally independent of past given the present: $P(X_{n+1} \mid X_n, \dots, X_0) = P(X_{n+1} \mid X_n)$ (see [[Markov Chain]]).
- **Machine Learning:** Naive Bayes classifier assumes all features $X_i$ are conditionally independent given class label $Y$.

---

## Sources & Traceability

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lecture 4, pages 10–12)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_3.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Chapter 2: Conditional Probability)
