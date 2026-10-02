---
type: problem
course: cse301
status: active
order: 30
---

# Problem — Bounding Tail Probabilities with Chebyshev and Chernoff

> 📖 **Reading Order:** Step 30 of 92 | **Module 4:** Probability Bounds and Inequalities  
> ◄ **Previous:** [[Comparison of Probability Bounds Example]] | ► **Next:** [[Law of Large Numbers]]

---

## Problem Statement

A high-frequency network switch processes incoming packets. The number of packets arriving in a 1-millisecond window follows a Poisson distribution with mean $\lambda = 20$:
$$X \sim \operatorname{Pois}(20)$$
The switch's internal buffer can store up to $40$ packets per millisecond before dropping overflow traffic. We wish to estimate and bound the overflow probability:
$$P(X \ge 40)$$

1. **Markov Bound:** Compute the upper bound on $P(X \ge 40)$ using [[Markov Inequality]].
2. **Chebyshev Bound:** Compute the upper bound on $P(X \ge 40)$ using [[Chebyshev Inequality]] (both standard two-sided and one-sided Cantelli versions).
3. **Chernoff Derivation:**
   - Write out the Chernoff bound expression $g(t) = e^{-40t} M_X(t)$ using the Poisson MGF $M_X(t) = e^{\lambda(e^t - 1)}$.
   - Differentiate the exponent to find the exact optimal value $t^*$ that minimizes the bound.
   - Calculate the resulting Chernoff upper bound on $P(X \ge 40)$.
4. **Analysis:** Compare the resulting bounds and explain why the Chernoff bound achieves exponential tightness.

---

## Prerequisites & Relevant Concepts

- [[Discrete Probability Distributions]] — Poisson distribution moments and MGF.
- [[Markov Inequality]] — First-moment bounding.
- [[Chebyshev Inequality]] — Variance-based bounding.
- [[Chernoff Bound]] — MGF convex optimization.

---

## Full Step-by-Step Solution

### Part 1: Markov's Inequality
Since $X$ is a count of packets, $X \ge 0$.
The mean is $\mathbb{E}[X] = \lambda = 20$.
Applying Markov's inequality with threshold $a = 40$:
$$P(X \ge 40) \le \frac{\mathbb{E}[X]}{40} = \frac{20}{40} = 0.50 \quad (50.0\%)$$

---

### Part 2: Chebyshev & Cantelli Bounds
For $X \sim \operatorname{Pois}(20)$:
- Mean: $\mu = 20$
- Variance: $\sigma^2 = \lambda = 20$
- Deviation: $c = 40 - 20 = 20$

1. **Two-Sided Chebyshev:**
   $$P(X \ge 40) \le P(\lvert X - 20 \rvert \ge 20) \le \frac{\sigma^2}{c^2} = \frac{20}{20^2} = \frac{20}{400} = 0.05 \quad (5.0\%)$$

2. **One-Sided Cantelli Bound:**
   $$P(X - \mu \ge 20) \le \frac{\sigma^2}{\sigma^2 + 20^2} = \frac{20}{20 + 400} = \frac{20}{420} = \frac{1}{21} \approx 0.0476 \quad (4.76\%)$$

Chebyshev demonstrates that the overflow probability is at most $\approx 4.8\%$.

---

### Part 3: Chernoff Bound Optimization

The MGF of $X \sim \operatorname{Pois}(\lambda)$ is:
$$M_X(t) = \mathbb{E}[e^{tX}] = \exp\left( \lambda(e^t - 1) \right) = \exp\left( 20(e^t - 1) \right)$$

By the Chernoff bound:
$$P(X \ge 40) \le e^{-40t} M_X(t) = \exp\left( -40t + 20(e^t - 1) \right)$$

Let the exponent function be:
$$h(t) = -40t + 20e^t - 20$$

To minimize $h(t)$ over $t > 0$, compute the first derivative with respect to $t$:
$$h'(t) = -40 + 20e^t = 0 \implies 20e^t = 40 \implies e^t = 2 \implies t^* = \ln 2 \approx 0.69315$$

Notice $t^* = \ln 2 > 0$, which is valid.
Second derivative check: $h''(t) = 20e^t > 0$ everywhere, confirming a global minimum.

Now evaluate $h(t^*)$ at $t^* = \ln 2$:
$$h(\ln 2) = -40 \ln 2 + 20(2 - 1) = -40 \ln 2 + 20$$
Using $\ln 2 \approx 0.693147$:
$$h(\ln 2) = -40(0.693147) + 20 = -27.7259 + 20 = -7.7259$$

Substitute back into the exponential:
$$P(X \ge 40) \le e^{h(t^*)} = e^{-7.7259} \approx 0.0004412 \quad (0.0441\%)$$

---

### Part 4: Comparative Analysis

| Bound Type | Formula / Method | Numerical Bound | Relative Tightness |
|---|---|---|---|
| **Markov** | $\frac{20}{40}$ | $\le 0.5000$ | Baseline ($1\times$) |
| **Two-Sided Chebyshev** | $\frac{20}{400}$ | $\le 0.0500$ | $10\times$ tighter than Markov |
| **Cantelli (One-Sided)** | $\frac{20}{420}$ | $\le 0.0476$ | $10.5\times$ tighter than Markov |
| **Chernoff** | $e^{-40\ln 2 + 20}$ | $\le 0.000441$ | **$1,133\times$ tighter than Markov** |
| **Exact Poisson Tail** | $\sum_{k=40}^\infty \frac{e^{-20}20^k}{k!}$ | $\approx 0.000072$ | True probability |

**Why Chernoff is so tight:**
Chebyshev assumes worst-case heavy-tailed distributions where probability mass can sit indefinitely far out at $\pm 20$.
Chernoff exploits the fact that the Poisson distribution has an analytic MGF with superexponential tail decay (due to the $k!$ denominator in the PMF), ensuring that tail probabilities diminish exponentially fast.

---

## Common Pitfalls

1. **Failure to check $t^* > 0$:** If the requested threshold $a$ is less than the mean ($a < \mu$), the optimal $t^*$ will be negative, meaning one must use the lower tail Chernoff bound ($t < 0$).
2. **Algebraic error in $h(t)$:** Forgetting to subtract the $-40t$ term when substituting $t^* = \ln 2$.

---

## Sources & Traceability

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lectures 17 & 18, pages 54–59)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_11.pdf` (Problem 2)
- **Question ID:** `Q-CSE301-016`
