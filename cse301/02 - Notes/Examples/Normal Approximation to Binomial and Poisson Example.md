---
type: example
course: cse301
status: active
---

# Normal Approximation to Binomial and Poisson Example

## Problem Context & Setup

The [[Central Limit Theorem]] allows us to approximate complicated discrete probability sums with simple standard normal CDF evaluations $\Phi(z)$.

We examine two classic applications:
1. **Election Polling (Binomial / de Moivre–Laplace):** A polling organization samples $n = 400$ registered voters at random. The true proportion supporting Candidate A is $p = 0.52$. What is the probability that the poll incorrectly shows Candidate A with at most $50\%$ of the vote ($X \le 200$)?
2. **Web Server Burst (Poisson Approximation):** A web server receives $X \sim \operatorname{Pois}(100)$ requests in an hour. What is the probability that the traffic stays within $\pm 10\%$ of its mean ($90 \le X \le 110$)?

---

## Part 1: Binomial Normal Approximation

Let $X \sim \operatorname{Bin}(n, p)$ with $n = 400$ and $p = 0.52$.
- Mean: $\mu = np = 400 \times 0.52 = 208$
- Variance: $\sigma^2 = np(1 - p) = 400 \times 0.52 \times 0.48 = 99.84$
- Standard Deviation: $\sigma = \sqrt{99.84} \approx 9.9920 \approx 10$

We want $P(X \le 200)$.

### Step 1: Without Continuity Correction (Crude Approximation)
Standardize directly:
$$z = \frac{200 - \mu}{\sigma} = \frac{200 - 208}{9.992} = \frac{-8}{9.992} \approx -0.8006$$
$$P(X \le 200) \approx \Phi(-0.80) = 1 - \Phi(0.80) \approx 1 - 0.7881 = 0.2119 \quad (21.19\%)$$

### Step 2: With Continuity Correction (Refined Approximation)
Because $X$ takes discrete integer values, the event $\{X \le 200\}$ includes all the probability mass in the discrete bar at $k = 200$, which spans $[199.5, 200.5]$ on the continuous real line.
To include the full bar for $200$, we integrate up to $200.5$:
$$z_{\text{corrected}} = \frac{200.5 - 208}{9.992} = \frac{-7.5}{9.992} \approx -0.7506$$
$$P(X \le 200) \approx \Phi(-0.75) = 1 - \Phi(0.75) \approx 1 - 0.7734 = 0.2266 \quad (22.66\%)$$

### Step 3: Exact Binomial Calculation
Summing the exact binomial terms:
$$P(X \le 200) = \sum_{k=0}^{200} \binom{400}{k} (0.52)^k (0.48)^{400 - k} \approx 0.2265$$

**Observation:**
- Crude normal approximation: $21.19\%$ (error $\approx 1.46\%$)
- Continuity-corrected normal approximation: **$22.66\%$ (error $< 0.01\%$)**
The continuity correction improves precision dramatically!

---

## Part 2: Poisson Normal Approximation

Let $X \sim \operatorname{Pois}(\lambda)$ with $\lambda = 100$.
Because a Poisson RV with integer parameter $\lambda$ can be viewed as the sum of $\lambda$ independent $\operatorname{Pois}(1)$ random variables, the CLT applies directly as $\lambda \to \infty$.
- Mean: $\mu = \lambda = 100$
- Variance: $\sigma^2 = \lambda = 100 \implies \sigma = 10$

We want $P(90 \le X \le 110)$.

### Step 1: Applying Continuity Correction
The interval of integers $\{90, 91, \dots, 110\}$ corresponds to the continuous range $[89.5, 110.5]$.
- Lower standardized threshold:
  $$z_1 = \frac{89.5 - 100}{10} = \frac{-10.5}{10} = -1.05$$
- Upper standardized threshold:
  $$z_2 = \frac{110.5 - 100}{10} = \frac{10.5}{10} = +1.05$$

### Step 2: Compute Normal Probability
$$P(90 \le X \le 110) \approx \Phi(1.05) - \Phi(-1.05) = 2\Phi(1.05) - 1$$
Using standard normal tables, $\Phi(1.05) \approx 0.8531$:
$$P(90 \le X \le 110) \approx 2(0.8531) - 1 = 1.7062 - 1 = 0.7062 \quad (70.62\%)$$

### Step 3: Comparison with Exact Poisson Sum
The exact sum $\sum_{k=90}^{110} \frac{e^{-100}100^k}{k!} \approx 0.7065$.
The error is just $0.03\%$!

---

## Key Takeaways & Exam Tips

- **When is the Normal Approximation Valid?**
  - For Binomial: $np \ge 10$ and $n(1 - p) \ge 10$.
  - For Poisson: $\lambda \ge 20$.
- **Continuity Correction Rule:**
  - $P(X \le k) \to k + 0.5$
  - $P(X < k) = P(X \le k - 1) \to k - 0.5$
  - $P(X \ge k) \to k - 0.5$
  - $P(X > k) = P(X \ge k + 1) \to k + 0.5$

---

## Related Notes

- [[Central Limit Theorem]] — Foundational limit theorem.
- [[Continuous Probability Distributions]] — Standard normal distribution and $\Phi(z)$.
- [[Discrete Probability Distributions]] — Binomial and Poisson definitions.

---

## Sources & Traceability

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lectures 20–21, pages 61–63)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_11.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Section 10.5: CLT Examples)
