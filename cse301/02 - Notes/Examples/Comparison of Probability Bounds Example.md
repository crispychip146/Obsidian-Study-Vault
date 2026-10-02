---
type: example
course: cse301
status: active
order: 29
---

# Comparison of Probability Bounds Example

> 📖 **Reading Order:** Step 29 of 92 | **Module 4:** Probability Bounds and Inequalities  
> ◄ **Previous:** [[Cauchy-Schwarz and Jensen Inequalities]] | ► **Next:** [[Problem — Bounding Tail Probabilities with Chebyshev and Chernoff]]

---

## Problem Context & Setup

Suppose a fair coin is flipped $n = 100$ times independently. Let $X$ denote the total number of heads observed:
$$X \sim \operatorname{Bin}(100, 0.5)$$

We wish to bound or compute the tail probability that at least $75$ heads occur:
$$P(X \ge 75)$$

We will compare the bounds given by:
1. **Markov's Inequality** (utilizing only the mean $\mathbb{E}[X]$)
2. **Chebyshev's Inequality (Two-Sided)** (utilizing mean and variance)
3. **Cantelli's Inequality (One-Sided Chebyshev)**
4. **Chernoff Bound** (utilizing the complete MGF)
5. **Exact Probability** (sum of binomial coefficients)

---

## Step-by-Step Derivations

### Baseline Parameters
- Mean: $\mu = np = 100 \times 0.5 = 50$
- Variance: $\sigma^2 = np(1 - p) = 100 \times 0.5 \times 0.5 = 25$
- Standard deviation: $\sigma = \sqrt{25} = 5$
- Deviation threshold: $a = 75$, corresponding to deviation $c = 75 - 50 = 25$ (which is $k = 5$ standard deviations!).

---

### 1. Markov's Inequality
Since $X$ is non-negative ($X \ge 0$):
$$P(X \ge 75) \le \frac{\mathbb{E}[X]}{75} = \frac{50}{75} = \frac{2}{3} \approx 0.6667 \quad (66.67\%)$$

*Critique:* Extremely loose. It tells us the probability is at most $66.7\%$, which is mathematically correct but practically uninformative.

---

### 2. Chebyshev's Inequality (Two-Sided)
The event $\{X \ge 75\}$ is a subset of the two-sided deviation $\{\lvert X - 50 \rvert \ge 25\}$:
$$P(X \ge 75) \le P(\lvert X - 50 \rvert \ge 25) \le \frac{\sigma^2}{25^2} = \frac{25}{625} = \frac{1}{25} = 0.04 \quad (4.0\%)$$

*Critique:* Using the variance cuts the bound from $66.7\%$ down to $4\%$, an improvement of over $16\times$.

---

### 3. Cantelli's Inequality (One-Sided Chebyshev)
Exploiting the fact that we only care about the upper tail:
$$P(X - \mu \ge 25) \le \frac{\sigma^2}{\sigma^2 + 25^2} = \frac{25}{25 + 625} = \frac{25}{650} = \frac{1}{26} \approx 0.03846 \quad (3.85\%)$$

---

### 4. Chernoff Bound (Hoeffding's Specialization)
For $X = \sum_{i=1}^n Y_i$ where $Y_i \overset{\text{i.i.d.}}{\sim} \operatorname{Bern}(p)$, Hoeffding's inequality provides the closed-form Chernoff bound:
$$P(\bar{X}_n - p \ge \epsilon) \le e^{-2n\epsilon^2}$$

Here $p = 0.5$, $n = 100$, and the sample proportion threshold is $\frac{75}{100} = 0.75$, so $\epsilon = 0.75 - 0.50 = 0.25$:
$$P(X \ge 75) = P(\bar{X}_{100} - 0.5 \ge 0.25) \le e^{-2 \times 100 \times (0.25)^2} = e^{-200 \times 0.0625} = e^{-12.5}$$
$$e^{-12.5} \approx 3.727 \times 10^{-6} \approx 0.000373\%$$

*Critique:* Incorporating exponential moments tightens the bound by a factor of over **10,000** compared to Chebyshev!

---

### 5. Exact Calculation
$$P(X \ge 75) = \sum_{k=75}^{100} \binom{100}{k} (0.5)^{100} \approx 2.824 \times 10^{-7}$$

---

## Comparison Summary Table

| Method | Information Leveraged | Bound for $P(X \ge 75)$ | Relative Ratio to Exact |
|---|---|---|---|
| **Markov** | First moment $\mathbb{E}[X]$ only | $\le 0.6667$ | $\approx 2.36 \times 10^6 \times$ |
| **Chebyshev** | First 2 moments ($\mu, \sigma^2$) | $\le 0.0400$ | $\approx 1.42 \times 10^5 \times$ |
| **Cantelli** | First 2 moments (one-sided) | $\le 0.0385$ | $\approx 1.36 \times 10^5 \times$ |
| **Chernoff** | Entire MGF (all moments) | $\le 3.73 \times 10^{-6}$ | $\approx 13.2 \times$ |
| **Exact** | Complete PMF | $= 2.82 \times 10^{-7}$ | $1.0 \times$ (exact) |

---

## Key Takeaways & Exam Tips

- **Information Principle:** Every additional statistical moment integrated into an inequality tightens the bound by orders of magnitude.
- **Tail Behavior:** For deviations far out in the tail ($k \ge 3$ standard deviations), polynomial bounds (Chebyshev) are very conservative, while Chernoff's exponential decay closely mirrors the true tail probability.

---

## Related Notes

- [[Markov Inequality]] — Derivation and indicator proof.
- [[Chebyshev Inequality]] — Variance-based concentration.
- [[Chernoff Bound]] — MGF optimization.
- [[Problem — Bounding Tail Probabilities with Chebyshev and Chernoff]] — Multi-tier bounding exercises.

---

## Sources & Traceability

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lectures 17 & 18, pages 54–59)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_11.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Chapter 10: Inequalities)
