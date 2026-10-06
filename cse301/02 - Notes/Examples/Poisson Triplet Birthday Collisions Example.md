---
type: example
course: cse301
status: active
order: 19
---

# Poisson Triplet Birthday Collisions Example

> 📖 **Reading Order:** Step 19 of 103 | **Module 2:** Random Variables and Distributions  
> ◄ **Previous:** [[Linearity of Expectation and Indicator Random Variables Example]] | ► **Next:** [[Gaussian Normalizing Constant Polar Derivation Example]]

---

## Problem

In a group of $n$ people, what is the approximate probability that **at least three people share the exact same birthday** (a 3-way or triplet collision)? 

Assume birthdays are independent and uniformly distributed across a 365-day non-leap year. Determine the approximate group size $n$ required to have at least a $50\%$ chance of a 3-way birthday collision.

---

## Given

- Group size: $n$ individuals ($n \ge 3$).
- Days in a year: $d = 365$.
- Birthdays are independent and identically distributed uniform categorical variables across the $365$ days.
- A **triplet match** occurs among three specific people $\{i, j, k\}$ if they are all born on the same day.

---

## Required

1. Formulate the rate parameter $\lambda = \mathbb{E}[X]$ representing the expected number of 3-way birthday collisions.
2. Approximate $P(\text{at least one 3-way collision}) = P(X \ge 1)$ using the **Poisson Paradigm**.
3. Calculate the threshold $n$ where $P(X \ge 1) \ge 0.50$.

---

## Understanding the Problem and Choosing the Method

### Why Exact Counting Fails
In the classical 2-person [[Birthday Problem and Collisions Example]], we compute the complement ("all $n$ birthdays are distinct") using a simple product: $\prod_{i=0}^{n-1} \frac{365 - i}{365}$.

However, for 3-way collisions, the complement ("no three people share a birthday") allows arbitrary pairs of people to share birthdays as long as no date has three. The exact combinatorial formula requires summing over partitions of people into singles and pairs, which is analytically intractable.

### Choosing the Method: The Poisson Paradigm
The **Poisson Paradigm (Law of Rare Events)** states that the sum of a large number of Bernoulli indicator variables with small individual probabilities—even when weakly dependent—can be closely approximated by a Poisson distribution:
$$X = \sum_{1 \le i < j < k \le n} I_{ijk} \ \dot\sim \ \operatorname{Pois}(\lambda) \quad \text{where } \lambda = \mathbb{E}[X]$$

---

## Solution

### Step 1: Probability of a Specific Triplet Collision
Consider any fixed triplet of people $\{i, j, k\}$:
- Person $i$ is born on some arbitrary day $D$.
- Person $j$ has birthday $D$ with probability $\frac{1}{365}$.
- Person $k$ has birthday $D$ with probability $\frac{1}{365}$.
- By independence:
  $$P(I_{ijk} = 1) = \left(\frac{1}{365}\right)^2 = \frac{1}{133,225}$$

---

### Step 2: Total Expected Triplet Collisions ($\lambda$)
The number of distinct candidate triplets among $n$ people is:
$$\binom{n}{3} = \frac{n(n - 1)(n - 2)}{6}$$

Let $I_{ijk}$ be the indicator that $\{i, j, k\}$ share a birthday. By Linearity of Expectation:
$$\lambda = \mathbb{E}[X] = \sum_{1 \le i < j < k \le n} \mathbb{E}[I_{ijk}] = \binom{n}{3} P(I_{ijk} = 1)$$
$$\lambda = \frac{n(n - 1)(n - 2)}{6 \times 365^2} = \frac{n(n - 1)(n - 2)}{799,350}$$

---

### Step 3: Apply the Poisson Approximation
By the Poisson paradigm, the total count of triplet collisions $X$ follows approximately $X \sim \operatorname{Pois}(\lambda)$.

The probability of observing **at least one** 3-way collision is:
$$P(X \ge 1) = 1 - P(X = 0) \approx 1 - \frac{e^{-\lambda} \lambda^0}{0!} = \mathbf{1 - e^{-\lambda}}$$

---

### Step 4: Find the $50\%$ Probability Threshold ($n$)
We set $P(X \ge 1) = 0.50$:
$$1 - e^{-\lambda} = 0.50 \implies e^{-\lambda} = 0.50 \implies \lambda = \ln(2) \approx 0.69315$$

Substitute $\lambda$:
$$\frac{n(n - 1)(n - 2)}{799,350} \approx 0.69315$$
$$n(n - 1)(n - 2) \approx 799,350 \times 0.69315 \approx 554,069$$

For large $n$, $n(n - 1)(n - 2) \approx n^3$:
$$n^3 \approx 554,069 \implies n \approx \sqrt[3]{554,069} \approx 82.1$$

Testing exact integer values:
- For $n = 83$:  
  $$\lambda = \frac{83 \times 82 \times 81}{799,350} = \frac{551,286}{799,350} \approx 0.6897 \implies P(X \ge 1) = 1 - e^{-0.6897} \approx 49.8\%$$
- For $n = 84$:  
  $$\lambda = \frac{84 \times 83 \times 82}{799,350} = \frac{571,704}{799,350} \approx 0.7152 \implies P(X \ge 1) = 1 - e^{-0.7152} \approx 51.1\%$$
- For $n = 88$ (accounting for slight correlation correction):  
  $$\lambda = \frac{88 \times 87 \times 86}{799,350} = \frac{658,416}{799,350} \approx 0.8237 \implies P(X \ge 1) = 1 - e^{-0.8237} \approx 56.1\%$$

---

## Result

1. The expected number of 3-way birthday matches is:
   $$\boxed{\lambda = \frac{\binom{n}{3}}{365^2} = \frac{n(n-1)(n-2)}{799,350}}$$
2. The probability of at least one triplet birthday match is:
   $$\boxed{P(X \ge 1) \approx 1 - e^{-\lambda}}$$
3. To achieve a $50\%$ chance of a 3-way collision, approximately **$n \approx 84\text{--}88$ people** are required (compared to only $n = 23$ for a pairwise collision).

---

## Why This Works

The indicator variables $I_{ijk}$ are not strictly independent: for example, knowing that $\{1, 2, 3\}$ match and $\{1, 2, 4\}$ match implies $\{1, 3, 4\}$ also match. 

However, because the individual probability $p = 1/365^2 \approx 7.5 \times 10^{-6}$ is extremely small, the probability of overlapping triplets sharing birthdays is negligible compared to disjoint triplets. Under the **Chen-Stein method** for Poisson approximation, the total variation distance between the true distribution of $X$ and $\operatorname{Pois}(\lambda)$ is $O(1/365)$, making the approximation accurate.

---

## Common Mistakes

- **Incorrect Single Triplet Probability:** Using $1/365^3$ instead of $1/365^2$. Person $1$ can be born on *any* of the 365 days; only the 2nd and 3rd people must match Person 1.
- **Using 2-Way Birthday Logic:** Assuming $n$ scales linearly with collision multiplicity. Pairwise collisions scale as $n \sim \sqrt{d} \approx \sqrt{365} \approx 23$, whereas triplet collisions scale as $n \sim d^{2/3} \approx 365^{2/3} \approx 51\text{--}84$.

---

## Related Concepts

- [[Birthday Problem and Collisions Example]] — Classical pairwise birthday collision problem.
- [[Discrete Probability Distributions]] — Poisson distribution and law of rare events.
- [[Linearity of Expectation and Indicator Random Variables Example]] — Expectation of indicator sums.

---

## Sources

- [[cse301/01 - Sources/Lectures/HT Final Note - Suchi.pdf|HT Final Note: Lecture 11 (pp. 46–47)]]
- [[cse301/01 - Sources/Lectures/ITP.pdf|Textbook: Introduction to Probability (Blitzstein & Hwang, Section 4.6: Poisson Approximation)]]
