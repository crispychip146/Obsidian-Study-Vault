---
type: example
course: cse301
status: active
order: 14
---

# Linearity of Expectation and Indicator Random Variables Example

> 📖 **Reading Order:** Step 14 of 92 | **Module 2:** Random Variables and Distributions  
> ◄ **Previous:** [[Moment Generating Functions]] | ► **Next:** [[Exponential Distribution Memorylessness Example]]

---

## Problem Context & Setup

The **Fundamental Bridge** between probability and expectation is the **indicator random variable**:
For any event $A$:
$$I_A = \begin{cases} 1 & \text{if } A \text{ occurs} \\ 0 & \text{if } A \text{ does not occur} \end{cases}$$
Its expectation is identically the probability of the event:
$$\mathbb{E}[I_A] = 1 \cdot P(A) + 0 \cdot P(A^c) = P(A)$$

Combined with **Linearity of Expectation**:
$$\mathbb{E}\left[ \sum_{i=1}^n c_i X_i \right] = \sum_{i=1}^n c_i \mathbb{E}[X_i]$$
which holds **regardless of whether the variables are independent or dependent**.

We demonstrate the power of this method across two classic problems:
1. **Hypergeometric Mean:** Drawing $n$ balls without replacement from $w$ white and $b$ black balls.
2. **Distinct Birthday Count:** Finding the expected number of distinct days represented by the birthdays of $k$ people.

---

## Step-by-Step Solution

### Problem 1: Expected White Balls Sampled Without Replacement
Let $N = w + b$ total balls. Draw a sample of size $n$ without replacement. Let $X$ be the number of white balls drawn.

- **Decomposition:**
  Let $I_j = 1$ if the $j$-th drawn ball is white, and $0$ otherwise ($j = 1, 2, \dots, n$).
  Then:
  $$X = \sum_{j=1}^n I_j$$

- **Marginal Probability (Symmetry):**
  By symmetry, before looking at the other draws, the $j$-th ball is equally likely to be any of the $N$ balls:
  $$P(I_j = 1) = \frac{w}{w + b} = \frac{w}{N}$$
  Therefore:
  $$\mathbb{E}[I_j] = \frac{w}{N}$$

- **Expectation via Linearity:**
  $$\mathbb{E}[X] = \mathbb{E}\left[ \sum_{j=1}^n I_j \right] = \sum_{j=1}^n \mathbb{E}[I_j] = \sum_{j=1}^n \frac{w}{N} = n \frac{w}{w + b}$$

*Note:* Even though the draws are strongly dependent, the linearity of expectation bypasses all joint dependencies effortlessly!

---

### Problem 2: Expected Number of Distinct Birthdays
Suppose $k$ people are in a room, each having a birthday independently and uniformly distributed across $n = 365$ days.
Let $D$ be the number of **distinct days** of the year that are someone's birthday.

- **Decomposition:**
  Instead of defining indicators per person, define indicators per **day** of the year:
  Let $I_d = 1$ if day $d$ is someone's birthday (at least one person was born on day $d$), and $0$ otherwise ($d = 1, 2, \dots, 365$).
  Then:
  $$D = \sum_{d=1}^{365} I_d$$

- **Probability for Day $d$:**
  Using the complement rule:
  $$P(I_d = 0) = P(\text{nobody has birthday on day } d) = \left( 1 - \frac{1}{365} \right)^k$$
  Therefore:
  $$\mathbb{E}[I_d] = P(I_d = 1) = 1 - \left( 1 - \frac{1}{365} \right)^k$$

- **Expectation via Linearity:**
  $$\mathbb{E}[D] = \sum_{d=1}^{365} \mathbb{E}[I_d] = 365 \left[ 1 - \left( 1 - \frac{1}{365} \right)^k \right]$$

---

## Numerical Evaluation for $k = 30$ and $k = 365$

- **For $k = 30$ people:**
  $$\mathbb{E}[D] = 365 \left[ 1 - \left(\frac{364}{365}\right)^{30} \right] \approx 365 [ 1 - 0.9210 ] \approx 28.84 \text{ days}$$
  *(With 30 people, on average they cover $\approx 28.84$ distinct days, meaning about $1.16$ collisions on average)*.

- **For $k = 365$ people:**
  Using $\left(1 - \frac{1}{n}\right)^n \approx e^{-1} \approx 0.3679$:
  $$\mathbb{E}[D] \approx 365 \left( 1 - \frac{1}{e} \right) \approx 365 \times 0.6321 \approx 230.7 \text{ days}$$
  *(With 365 people, on average only $\approx 231$ distinct days are covered; about 134 days have zero birthdays!)*.

---

## Key Takeaways & Exam Tips

- **The Indicator Choice Trick:** If asked for "the number of occupied bins", define indicators for the **bins**, not the balls!
- **Independence is Irrelevant for Linearity:** $\mathbb{E}[X_1 + \dots + X_n] = \mathbb{E}[X_1] + \dots + \mathbb{E}[X_n]$ is true **always**. Never spend time checking independence when calculating expectations.

---

## Related Notes

- [[Discrete Probability Distributions]] — Hypergeometric and Binomial properties.
- [[Law of the Unconscious Statistician (LOTUS)]] — Expectation mechanics.
- [[Problem — Indicator Variables for Distinct Birthday Counts]] — Full variance calculation via indicator covariance.

---

## Sources & Traceability

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lecture 8, pages 23–25)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_5.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Section 4.2 & Section 4.3)
