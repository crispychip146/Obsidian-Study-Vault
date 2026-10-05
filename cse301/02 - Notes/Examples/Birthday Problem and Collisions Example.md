---
type: example
course: cse301
status: active
order: 4
---

# Birthday Problem and Collisions Example

> 📖 **Reading Order:** Step 04 of 92 | **Module 1:** Counting and Discrete Probability  
> ◄ **Previous:** [[Inclusion-Exclusion Principle]] | ► **Next:** [[Derangements and Card Matching Example]]

---

## Problem

Consider a group of $k$ individuals gathered in a room. Assuming:
1. There are $n = 365$ days in a year (ignoring leap years).
2. Each person's birthday is equally likely to be any of the 365 days.
3. Birthdays are mutually independent.

**Goal:**
1. Find the probability $P(\text{match})$ that at least two individuals share a birthday.
2. Determine the minimum group size $k$ such that $P(\text{match}) \ge 0.5$.
3. Derive the general Taylor series approximation for hash table collisions.

---

## Solution

The surprising part of the birthday problem is the number of opportunities for a match. With $k$ people, there are $\binom{k}{2}$ pairs. Nobody needs to match your particular birthday; any pair may collide.

Directly combining pair-match events creates overlaps, so use [[Probability Axioms and Naive Probability|the complement rule]]. For no shared birthdays, the first person may use any of $n$ days, the second must avoid one occupied day, and the third must avoid two. Independent uniform birthdays therefore give $P(\text{no collision})=\prod_{j=0}^{k-1}(1-j/n)$ for $k\le n$.

For a rough scale, take logarithms and use $\log(1-x)\approx-x$ when the factors are close to one. The product becomes approximately $e^{-k(k-1)/(2n)}$. The approximation explains why collisions become likely around $\sqrt n$ people rather than $n$ people. Real birthdays are not perfectly uniform; this is an explicit simplifying model.

### Step 1: Use the Complement Rule
Calculating the probability of "at least one shared birthday" directly is cumbersome because there could be pairs, triplets, multiple pairs, etc.
By the complement rule:
$$P(\text{at least one match}) = 1 - P(\text{no matches})$$

"No matches" means all $k$ individuals have distinct birthdays.

### Step 2: Counting Distinct Birthdays
- **Total possible birthday assignments** for $k$ people:
  Each person has 365 options. By the multiplication rule:
  $$\lvert S \rvert = 365^k$$

- **Favorable outcomes for distinct birthdays:**
  - Person 1 has 365 options.
  - Person 2 has 364 options.
  - Person 3 has 363 options.
  - $\dots$
  - Person $k$ has $365 - (k - 1) = 365 - k + 1$ options.
  $$\lvert A^c \rvert = 365 \times 364 \times \dots \times (365 - k + 1) = P(365, k) = \frac{365!}{(365 - k)!}$$

Hence:
$$P(\text{no match}) = \frac{365 \times 364 \times \dots \times (365 - k + 1)}{365^k} = \prod_{i=1}^{k-1} \left( 1 - \frac{i}{365} \right)$$

Therefore:
$$P(\text{at least one match}) = 1 - \prod_{i=1}^{k-1} \left( 1 - \frac{i}{365} \right)$$

---

### Step 3: Numerical Evaluation for $k = 23$
For $k = 23$:
$$P(\text{no match}) = \frac{365}{365} \times \frac{364}{365} \times \frac{363}{365} \times \dots \times \frac{343}{365} \approx 0.4927$$

$$P(\text{at least one match}) = 1 - 0.4927 = 0.5073 > 0.5$$

Thus, with just **23 people**, the probability that at least two people share a birthday exceeds **50%**!
For $k = 50$, $P(\text{match}) \approx 97.0\%$.
For $k = 70$, $P(\text{match}) \approx 99.9\%$.

---

### Step 4: Mathematical Approximation (Taylor Series)
Using the Taylor approximation $1 - x \approx e^{-x}$ for small $x$:
$$1 - \frac{i}{n} \approx e^{-i/n}$$

Substitute this into the product for $P(\text{no match})$:
$$P(\text{no match}) = \prod_{i=1}^{k-1} \left( 1 - \frac{i}{n} \right) \approx \prod_{i=1}^{k-1} e^{-i/n} = \exp\left( -\sum_{i=1}^{k-1} \frac{i}{n} \right)$$

Using the arithmetic series sum $\sum_{i=1}^{k-1} i = \frac{(k-1)k}{2} = \binom{k}{2}$:
$$P(\text{no match}) \approx e^{-\frac{k(k-1)}{2n}} \approx e^{-\frac{k^2}{2n}}$$

Setting $P(\text{match}) = 1 - e^{-k^2 / (2n)} = \frac{1}{2}$:
$$e^{-k^2 / (2n)} = \frac{1}{2} \implies -\frac{k^2}{2n} = \ln\left(\frac{1}{2}\right) = -\ln 2$$
$$k \approx \sqrt{2n \ln 2} \approx 1.177 \sqrt{n}$$

For $n = 365$:
$$k \approx 1.177 \sqrt{365} \approx 1.177 \times 19.105 \approx 22.49 \implies k = 23$$

---
### Computer Science Application: Hash Collisions

In computer science, this is the foundation of **hash table collision analysis** and **cryptographic birthday attacks**:
- If a hash function produces $b$-bit hashes, the number of possible hash values is $n = 2^b$.
- To find a collision with probability $\ge 50\%$, an attacker only needs approximately:
  $$k \approx \sqrt{2 \ln 2 \cdot 2^b} \approx 1.177 \times 2^{b/2}$$
- Therefore, an 80-bit cryptographic hash provides only $\approx 2^{40}$ security against collision attacks!

---

## Common Mistakes

- **Intuition behind the small $k$:** People intuitively compare themselves to others ($22$ comparisons). But the number of distinct *pairs* in the room is $\binom{23}{2} = \frac{23 \times 22}{2} = 253$ pairs! With 253 opportunities for a match, exceeding 50% is natural.
- **Exam Rule:** If an exam question asks for "at least one...", immediately think of computing $1 - P(\text{none})$.

---

## What to carry forward

For $k>n$, a collision is certain by the pigeonhole principle. Distinguish a birthday collision from the number of distinct birthdays in [[Problem — Indicator Variables for Distinct Birthday Counts]].

## Related notes

- [[Probability Axioms and Naive Probability|the complement rule]]
- [[Problem — Indicator Variables for Distinct Birthday Counts]]

## Sources

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lecture 2, pages 4–6)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_1.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Section 1.4: Birthday Problem)
