---
type: problem
course: cse301
status: active
order: 6
---

# Problem — Birthday Collisions and Approximation

> 📖 **Reading Order:** Step 06 of 92 | **Module 1:** Counting and Discrete Probability  
> ◄ **Previous:** [[Derangements and Card Matching Example]] | ► **Next:** [[Random Variables and Probability Distributions]]

---

## Problem

Suppose $k$ distinct keys are inserted uniformly and independently at random into a hash table with $m$ buckets (numbered $1, 2, \dots, m$).

1. **Exact Collision Probability:** Write an exact formula for the probability $P(\text{collision})$ that at least two keys hash to the same bucket.
2. **Poisson Approximation / Exponential Bound:** Using the inequality $1 - x \le e^{-x}$, derive a closed-form lower bound on the collision probability.
3. **Threshold Calculation:** For a 32-bit hash table ($m = 2^{32} \approx 4.29 \times 10^9$), find the approximate number of keys $k$ required before the probability of a collision reaches $50\%$.
4. **Expected Number of Collisions:** Define an indicator random variable $I_{ij}$ for each pair of keys $\{i, j\}$ ($1 \le i < j \le k$) indicating whether keys $i$ and $j$ collide. Find the exact expected number of colliding pairs $\mathbb{E}[C]$.

---

## Solution

Start by deciding what “collision” means: at least two of the sampled birthdays or identifiers agree. The exact answer comes from avoiding all previously occupied categories at each draw. This is the product developed in [[Birthday Problem and Collisions Example]], under independent uniform sampling.

When solving for a threshold, the complement approximation makes the inverse problem manageable. A collision probability near $1/2$ means $e^{-k(k-1)/(2n)}\approx1/2$, hence $k(k-1)\approx2n\log2$. Solve this quadratic, round to a candidate integer, and check nearby integers with the exact product. Rounding an approximation alone does not establish the first integer that crosses the threshold.

For numerical work, sum $\log(1-j/n)$ instead of multiplying many small factors. This preserves precision and keeps the same underlying argument. If $k>n$, stop: the exact probability is already one.

### Part 1: Exact Collision Probability
Total ways to place $k$ keys into $m$ buckets:
$$\lvert S \rvert = m^k$$

Ways to place $k$ keys into $m$ buckets such that **no bucket receives more than one key**:
- Key 1 can choose any of $m$ buckets.
- Key 2 can choose any of $m - 1$ remaining buckets.
- $\dots$
- Key $k$ can choose any of $m - k + 1$ buckets.
$$\lvert A_{\text{no coll}} \rvert = m(m-1)\dots(m-k+1) = \frac{m!}{(m-k)!} = P(m, k)$$

Therefore, the probability of **no collision** is:
$$P(\text{no coll}) = \prod_{i=1}^{k-1} \left( 1 - \frac{i}{m} \right)$$

By the complement rule:
$$P(\text{collision}) = 1 - \prod_{i=1}^{k-1} \left( 1 - \frac{i}{m} \right)$$

---

### Part 2: Exponential Bound Derivation
From real analysis, $1 - x \le e^{-x}$ for all $x \in \mathbb{R}$ (with strict inequality for $x \ne 0$).
Setting $x = i / m$:
$$1 - \frac{i}{m} \le e^{-i/m}$$

Multiplying these inequalities for $i = 1, 2, \dots, k-1$:
$$P(\text{no coll}) = \prod_{i=1}^{k-1} \left( 1 - \frac{i}{m} \right) \le \prod_{i=1}^{k-1} e^{-i/m} = \exp\left( -\sum_{i=1}^{k-1} \frac{i}{m} \right)$$

Using the arithmetic progression sum $\sum_{i=1}^{k-1} i = \frac{k(k-1)}{2} = \binom{k}{2}$:
$$P(\text{no coll}) \le e^{-\frac{k(k-1)}{2m}}$$

Subtracting from 1 reverses the inequality, yielding a guaranteed lower bound:
$$P(\text{collision}) \ge 1 - e^{-\frac{k(k-1)}{2m}}$$

---

### Part 3: Threshold for 32-bit Hash Table ($m = 2^{32}$)
We want $P(\text{collision}) \approx 0.5$:
$$1 - e^{-\frac{k^2}{2m}} \approx 0.5 \implies e^{-\frac{k^2}{2m}} \approx 0.5$$
Taking natural logarithms:
$$-\frac{k^2}{2m} \approx \ln(0.5) = -\ln 2 \implies k^2 \approx 2m \ln 2$$
$$k \approx \sqrt{2 \ln 2 \cdot m} \approx \sqrt{1.3863 \cdot 2^{32}} = 2^{16} \sqrt{1.3863} \approx 65536 \times 1.1774 \approx 77,163$$

**Insight:** Even though a 32-bit integer space has over 4.29 billion slots, after inserting only about **77,163 keys**, there is already a $50\%$ chance of a hash collision!

---

### Part 4: Expected Number of Colliding Pairs $\mathbb{E}[C]$
Let the total number of colliding pairs be $C = \sum_{1 \le i < j \le k} I_{ij}$, where:
$$I_{ij} = \begin{cases} 1 & \text{if keys } i \text{ and } j \text{ hash to the same bucket} \\ 0 & \text{otherwise} \end{cases}$$

For any fixed pair $(i, j)$:
Whatever bucket key $i$ lands in, key $j$ has a probability of $\frac{1}{m}$ of landing in that exact same bucket:
$$\mathbb{E}[I_{ij}] = P(I_{ij} = 1) = \frac{1}{m}$$

By **linearity of expectation**:
$$\mathbb{E}[C] = \mathbb{E}\left[ \sum_{1 \le i < j \le k} I_{ij} \right] = \sum_{1 \le i < j \le k} \mathbb{E}[I_{ij}] = \binom{k}{2} \frac{1}{m} = \frac{k(k-1)}{2m}$$

Notice that when $k \approx \sqrt{2m \ln 2}$:
$$\mathbb{E}[C] \approx \frac{2m \ln 2}{2m} = \ln 2 \approx 0.693$$

---
### Alternative Approaches / Insights

- **Poisson Paradigm:** When $k$ is moderately large and $m$ is very large, the number of colliding pairs $C$ is approximately Poisson-distributed with parameter $\lambda = \mathbb{E}[C] = \frac{k(k-1)}{2m}$.
  Under Poisson $(\lambda)$:
  $$P(\text{no collision}) = P(C = 0) \approx \frac{\lambda^0 e^{-\lambda}}{0!} = e^{-\lambda} = e^{-\frac{k(k-1)}{2m}}$$
  This provides deep probabilistic justification for why the exponential Taylor approximation is so accurate.

---

## Common Mistakes

1. **Confusing number of items with number of pairs:** Forgetting that collisions happen between *pairs*. The relevant quantity is $\binom{k}{2} \approx k^2/2$, not $k$.
2. **Assuming independence of pairs:** The pairs $I_{12}$ and $I_{23}$ are not independent (if 1 and 2 collide, and 2 and 3 collide, then 1 and 3 must collide!). However, **linearity of expectation does not require independence**, which makes the calculation of $\mathbb{E}[C]$ exact and simple.

---

## What to carry forward

Use the approximation to see the scale and the exact expression to verify the requested cutoff. The size of the category space, rather than its label as birthdays or hash values, drives the calculation.

## Related notes

- [[Birthday Problem and Collisions Example]]

## Source

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lecture 2, pages 4–6)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_1.pdf` (Problem 1)
- **Question ID:** `Q-CSE301-013`
