---
type: example
course: cse301
status: active
order: 7
---

# Newton-Pepys Dice Problem Example

> 📖 **Reading Order:** Step 07 of 103 | **Module 1:** Counting and Discrete Probability  
> ◄ **Previous:** [[Problem — Birthday Collisions and Approximation]] | ► **Next:** [[Random Variables and Probability Distributions]]

---

## Problem

In 1693, Samuel Pepys wrote to Sir Isaac Newton posing a famous gambling puzzle:
Which of the following three events is the most likely to occur when rolling fair 6-sided dice?

- **Event A:** Rolling at least one 6 when rolling 6 dice.
- **Event B:** Rolling at least two 6s when rolling 12 dice.
- **Event C:** Rolling at least three 6s when rolling 18 dice.

---

## Given

- A fair 6-sided die has outcome probabilities $P(\text{rolling a } 6) = p = \frac{1}{6}$ and $P(\text{not rolling a } 6) = q = \frac{5}{6}$.
- All die rolls are mutually independent.
- The experiments correspond to independent Binomial trials:
  - For Event A: $X_A \sim \operatorname{Bin}\left(6, \frac{1}{6}\right)$, testing $P(X_A \ge 1)$.
  - For Event B: $X_B \sim \operatorname{Bin}\left(12, \frac{1}{6}\right)$, testing $P(X_B \ge 2)$.
  - For Event C: $X_C \sim \operatorname{Bin}\left(18, \frac{1}{6}\right)$, testing $P(X_C \ge 3)$.

---

## Required

Calculate the exact probabilities $P(A)$, $P(B)$, and $P(C)$, and determine which event is most probable.

---

## Understanding the Problem and Choosing the Method

### The Intuitive Trap
In all three cases, the expected number of 6s matches the required threshold exactly:
$$\mathbb{E}[X_A] = 6 \times \frac{1}{6} = 1$$
$$\mathbb{E}[X_B] = 12 \times \frac{1}{6} = 2$$
$$\mathbb{E}[X_C] = 18 \times \frac{1}{6} = 3$$

Because the ratio of required 6s to total dice rolled is constant ($\frac{1}{6} = \frac{2}{12} = \frac{3}{18}$), naive intuition—and Pepys himself—assumed that all three events must be equally likely, or that Event C might even be more likely due to "more chances to roll."

### Choosing the Method
Direct summation of $P(X \ge k)$ involves many terms. The most efficient approach uses the **complement rule**:
$$P(X \ge k) = 1 - P(X \le k - 1) = 1 - \sum_{j=0}^{k-1} \binom{n}{j} p^j q^{n-j}$$

---

## Solution

### Step 1: Compute $P(A)$ (At least one 6 in 6 rolls)
The complement of "at least one 6" is "zero 6s":
$$P(X_A = 0) = \binom{6}{0} \left(\frac{1}{6}\right)^0 \left(\frac{5}{6}\right)^6 = \left(\frac{5}{6}\right)^6$$
$$\left(\frac{5}{6}\right)^6 = \frac{15625}{46656} \approx 0.334898$$

Therefore:
$$P(A) = 1 - \left(\frac{5}{6}\right)^6 \approx 1 - 0.334898 = \mathbf{0.6651} \quad (66.51\%)$$

---

### Step 2: Compute $P(B)$ (At least two 6s in 12 rolls)
The complement of "at least two 6s" is "zero or one 6":
$$P(X_B \le 1) = P(X_B = 0) + P(X_B = 1)$$

1. **Zero 6s:**
   $$P(X_B = 0) = \left(\frac{5}{6}\right)^{12} \approx 0.112157$$
2. **Exactly one 6:**
   $$P(X_B = 1) = \binom{12}{1} \left(\frac{1}{6}\right)^1 \left(\frac{5}{6}\right)^{11} = 12 \times \frac{1}{6} \times \left(\frac{5}{6}\right)^{11} = 2 \times \left(\frac{5}{6}\right)^{11} \approx 0.269176$$
3. **Sum of complement:**
   $$P(X_B \le 1) \approx 0.112157 + 0.269176 = 0.381333$$

Therefore:
$$P(B) = 1 - P(X_B \le 1) \approx 1 - 0.381333 = \mathbf{0.6187} \quad (61.87\%)$$

---

### Step 3: Compute $P(C)$ (At least three 6s in 18 rolls)
The complement is "at most two 6s" ($0, 1, \text{ or } 2$ sixes):
$$P(X_C \le 2) = P(X_C = 0) + P(X_C = 1) + P(X_C = 2)$$

1. **Zero 6s:**
   $$P(X_C = 0) = \left(\frac{5}{6}\right)^{18} \approx 0.037561$$
2. **Exactly one 6:**
   $$P(X_C = 1) = \binom{18}{1} \left(\frac{1}{6}\right)^1 \left(\frac{5}{6}\right)^{17} = 3 \times \left(\frac{5}{6}\right)^{17} \approx 0.135219$$
3. **Exactly two 6s:**
   $$P(X_C = 2) = \binom{18}{2} \left(\frac{1}{6}\right)^2 \left(\frac{5}{6}\right)^{16} = 153 \times \frac{1}{36} \times \left(\frac{5}{6}\right)^{16} = 4.25 \times \left(\frac{5}{6}\right)^{16} \approx 0.229873$$
4. **Sum of complement:**
   $$P(X_C \le 2) \approx 0.037561 + 0.135219 + 0.229873 = 0.402653$$

Therefore:
$$P(C) = 1 - P(X_C \le 2) \approx 1 - 0.402653 = \mathbf{0.5973} \quad (59.73\%)$$

---

## Result

Comparing the three probabilities:
$$P(A) \approx 66.51\% > P(B) \approx 61.87\% > P(C) \approx 59.73\%$$

**Event A is strictly the most likely.**

---

## Why This Works

Why does the probability strictly decrease as the number of dice increases, even though the expected proportion of 6s is constant?

1. **Asymmetry and Skewness of the Binomial Distribution:**
   For small $n$, the Binomial distribution with $p = 1/6$ is highly right-skewed. The probability of obtaining strictly less than the mean ($0$ sixes out of $6$) is only $0.3349$. Hence, beating the mean or meeting it has a high probability ($1 - 0.3349 = 0.6651$).

2. **Asymptotic Convergence to the Normal Distribution (CLT):**
   As $n \to \infty$ with $k_n = n/6$, the standardized variable converges to the standard normal distribution:
   $$\frac{X_n - n/6}{\sqrt{n(1/6)(5/6)}} \xrightarrow{d} \mathcal{N}(0, 1)$$
   The probability of meeting or exceeding the mean in a symmetric normal distribution is:
   $$\lim_{n \to \infty} P\left(X_n \ge \frac{n}{6}\right) = \frac{1}{2} = 50.0\%$$
   Because the initial probability starts at $66.51\%$ (for $n=6$) and must monotonically converge to $50.0\%$ as $n \to \infty$, the probability **must strictly decrease** as $n$ increases!

---

## Common Mistakes

- **Equal Probability Fallacy:** Assuming that equal ratios $\frac{k}{n} = \frac{1}{6}$ guarantee equal probabilities.
- **Forgetting Lower Complementary Terms:** In Event B, subtracting only $P(X=0)$ while forgetting $P(X=1)$.
- **Misunderstanding Dice Dependencies:** Assuming that rolling 12 dice is equivalent to two independent 6-dice games where each game gives at least one 6. Rolling two 6s across 12 dice includes the case of two 6s in the first 6 rolls and zero in the second.

---

## Related Concepts

- [[Discrete Probability Distributions]] — Binomial distribution PMF and expectation.
- [[Central Limit Theorem]] — Normal approximation explaining the limiting behavior toward $50\%$.
- [[Probability Axioms and Naive Probability]] — Complement rule.

---

## Sources

- [[cse301/01 - Sources/Lectures/HT Final Note - Suchi.pdf|HT Final Note: Lecture 4 (pp. 13–14)]]
- [[cse301/01 - Sources/Lectures/ITP.pdf|Textbook: Introduction to Probability (Blitzstein & Hwang, Section 3.8: Newton-Pepys Problem)]]
