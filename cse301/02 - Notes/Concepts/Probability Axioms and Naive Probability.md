---
type: concept
course: cse301
status: active
order: 2
---

# Probability Axioms and Naive Probability

> 📖 **Reading Order:** Step 02 of 92 | **Module 1:** Counting and Discrete Probability  
> ◄ **Previous:** [[Combinatorics and Counting Principles]] | ► **Next:** [[Inclusion-Exclusion Principle]]

---

## Building the idea

A probability model starts with possible outcomes and assigns weights to events. The weights must be nonnegative, total one over the whole sample space, and add across disjoint events. These requirements make probability behave consistently when we regroup outcomes.

For a fair die, every face has weight $1/6$, so the probability of an even result is three equally weighted faces out of six. For a loaded die, counting faces would ignore their different weights. We instead add the probabilities assigned to faces 2, 4, and 6.

The complement rule is useful because an event and its complement partition the sample space. If it is easier to describe failure than success, compute $P(A^c)$ and subtract it from one. In contrast, adding $P(A)$ and $P(B)$ double-counts their intersection unless the events are disjoint; [[Inclusion-Exclusion Principle]] repairs that overlap.

## Definition

**Probability** is a mathematical framework for quantifying uncertainty. Formally, a **probability space** is a triple $(S, \mathcal{F}, P)$, where:
- $S$ is the **sample space** (the set of all possible outcomes of a random experiment).
- $\mathcal{F}$ is the **event space** (a $\sigma$-algebra of subsets of $S$, representing observable events).
- $P$ is a **probability function** (a real-valued measure assigning a number $P(A) \in [0, 1]$ to each event $A \in \mathcal{F}$).

Historically and pedagogically, probability began with the **naive definition**, which applies when the sample space $S$ is finite and all basic outcomes are equally likely.

---

## How It Works

### The Naive Definition of Probability

If a sample space $S$ is finite and all elementary outcomes are equally likely (symmetric):
$$P(A) = \frac{\lvert A \rvert}{\lvert S \rvert} = \frac{\# \text{ outcomes favorable to } A}{\text{total } \# \text{ outcomes in } S}$$

### Limitations of Naive Probability
1. **Requires Symmetry:** Fails immediately when outcomes are not equally likely (e.g., a loaded die, weather forecasting, disease prevalence).
2. **Finite Spaces Only:** Cannot handle continuous intervals (e.g., picking a random point in $[0, 1]$) or countably infinite outcomes (e.g., flipping until the first heads).
3. **Circular Logic:** "Equally likely" implicitly defines probability using probability.

To overcome these limitations, modern probability rests on the **Kolmogorov Axioms**.

---
### Kolmogorov Axioms of Probability

Let $S$ be a sample space and $\mathcal{F}$ a collection of events. A function $P: \mathcal{F} \to \mathbb{R}$ is a probability function if it satisfies:

### Axiom 1: Non-negativity
For any event $A$:
$$P(A) \ge 0$$

### Axiom 2: Normalization (Certainty)
The probability of the entire sample space is 1:
$$P(S) = 1$$

### Axiom 3: Countable Additivity
If $A_1, A_2, A_3, \dots$ is a countable sequence of **mutually exclusive (disjoint)** events (i.e., $A_i \cap A_j = \emptyset$ for all $i \ne j$):
$$P\left( \bigcup_{i=1}^\infty A_i \right) = \sum_{i=1}^\infty P(A_i)$$

For finite collections, this implies **finite additivity**:
$$P(A_1 \cup A_2 \cup \dots \cup A_n) = \sum_{i=1}^n P(A_i) \quad \text{when } A_i \cap A_j = \emptyset$$

---

## Important Properties and Why They Hold

### Immediate Consequences and Theorems

From the three axioms, all foundational properties of probability follow deductively:

1. **Probability of the Empty Set:**
   $$P(\emptyset) = 0$$
   *Proof:* $S = S \cup \emptyset \cup \emptyset \dots$ with disjoint sets, so $P(S) = P(S) + P(\emptyset) \implies P(\emptyset) = 0$.

2. **Complement Rule:**
   For any event $A$:
   $$P(A^c) = 1 - P(A)$$
   *Proof:* $A \cup A^c = S$ and $A \cap A^c = \emptyset$, so $P(A) + P(A^c) = P(S) = 1$.

3. **Monotonicity (Subsets):**
   If $A \subseteq B$, then:
   $$P(A) \le P(B)$$
   *Proof:* $B = A \cup (B \setminus A)$ where $A \cap (B \setminus A) = \emptyset$, so $P(B) = P(A) + P(B \setminus A) \ge P(A)$ since $P(B \setminus A) \ge 0$.

4. **Probability Range:**
   For any event $A$:
   $$0 \le P(A) \le 1$$
   *Proof:* Follows from Non-negativity and Monotonicity ($A \subseteq S \implies P(A) \le P(S) = 1$).

5. **Addition Rule for Two Arbitrary Events:**
   $$P(A \cup B) = P(A) + P(B) - P(A \cap B)$$
   *Proof:* Partition $A \cup B$ into disjoint pieces: $A = (A \setminus B) \cup (A \cap B)$ and $B = (B \setminus A) \cup (A \cap B)$. Adding them counts $A \cap B$ twice, requiring one subtraction.

6. **Boole's Inequality (Union Bound):**
   For any countable collection of events $A_1, A_2, \dots$:
   $$P\left( \bigcup_{i=1}^\infty A_i \right) \le \sum_{i=1}^\infty P(A_i)$$

7. **Continuity of Probability:**
   - If $A_1 \subseteq A_2 \subseteq A_3 \subseteq \dots$ (increasing sequence) with $A = \bigcup_{n=1}^\infty A_n$, then:
     $$\lim_{n \to \infty} P(A_n) = P(A)$$
   - If $A_1 \supseteq A_2 \supseteq A_3 \supseteq \dots$ (decreasing sequence) with $A = \bigcap_{n=1}^\infty A_n$, then:
     $$\lim_{n \to \infty} P(A_n) = P(A)$$

---

## Common Mistakes

### Edge Cases & Common Pitfalls

1. **Assuming Equal Likelihood Blindly:**
   - *Error:* Claiming "There are two outcomes (win or lose lottery), so probability of winning is $1/2$."
   - *Correction:* The naive definition requires a justified physical symmetry or equal probability assumption.
2. **Confusing Mutually Exclusive with Independent:**
   - Mutually exclusive means $A \cap B = \emptyset$ (events cannot co-occur; $P(A \cap B) = 0$).
   - Independent means $P(A \cap B) = P(A)P(B)$.
   - *Key Distinction:* If two non-trivial events ($P(A) > 0, P(B) > 0$) are mutually exclusive, they **cannot** be independent, because knowing $A$ occurred tells you $B$ definitely did not occur!
3. **Double Counting in Unions:**
   - Always subtract intersections when computing $P(A \cup B)$ unless sets are known to be disjoint. For multiple sets, apply the [[Inclusion-Exclusion Principle]].

---

## Exam Relevance

### Cross-Topic Connections / Exam Relevance

- **Combinatorics:** Evaluates the numerator $\lvert A \rvert$ and denominator $\lvert S \rvert$ in naive probability (see [[Combinatorics and Counting Principles]]).
- **Conditional Probability:** Axioms extend directly into conditional probability spaces $P(\cdot \mid B)$ (see [[Conditional Probability and Independence]]).
- **Inclusion-Exclusion:** Generalizes the two-set union rule to $n$ sets (see [[Inclusion-Exclusion Principle]]).
- **Exam Patterns:** Frequently tested in warm-up problems, proof derivations (e.g., proving $P(A \cup B) = P(A) + P(B) - P(A \cap B)$ from axioms), and assessing the validity of probability functions.

---

## What to carry forward

Keep the distinction between disjoint and independent events clear. Disjoint events cannot happen together; independence says learning that one occurred does not change the probability of the other.

## Related notes

- [[Inclusion-Exclusion Principle]]

## Sources

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lectures 1–3, pages 1–9)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_1.pdf` and `2.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Chapter 1, Sections 1.2–1.6)
