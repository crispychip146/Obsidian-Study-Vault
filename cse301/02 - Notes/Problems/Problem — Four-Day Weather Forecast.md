---
type: problem
course: cse301
status: active
order: 89
---

# Problem — Four-Day Weather Forecast

> 📖 **Reading Order:** Step 89 of 103 | **Module 10:** Stochastic Processes  
> ◄ **Previous:** [[Problem — Patty and Max Gambler's Ruin]] | ► **Next:** [[Problem — Rain Prediction Two Days Ahead]]
---
## Problem

Consider a two-state [[Markov Chain]] modeling weather, with states $0$ (Rain) and $1$ (No Rain). The one-step transition probability matrix is:

$$P = \begin{pmatrix}
0.7 & 0.3 \\
0.4 & 0.6
\end{pmatrix}$$

1. If it is raining today ($X_0 = 0$), what is the probability that it rains two days from now?
2. What is the probability that it rains four days from now?
3. Calculate the limiting probability of rain $\pi_0 = \lim_{n \to \infty} P_{00}^n$, and explain why $P_{00}^4$ is already extremely close to this value.
---
## Given

- State space $S = \{0, 1\}$ ($0 = \text{Rain}$, $1 = \text{No Rain}$)
- Transition probability matrix:
  $$P = \begin{pmatrix} 0.7 & 0.3 \\ 0.4 & 0.6 \end{pmatrix}$$
- Starting state: $X_0 = 0$
---
## Required

1. Two-step probability: $P_{00}^{(2)}$
2. Four-step probability: $P_{00}^{(4)}$
3. Limiting probability $\pi_0$ and analysis of rate of convergence.
---
## Concepts Tested

- [[Markov Chain]]
- [[Chapman-Kolmogorov Equations]]
- [[Stationary and Limiting Distributions in Markov Chains]]
---
## Prerequisites

- [[Markov Chain]]
- [[Chapman-Kolmogorov Equations]]
---
## Question Type

Numerical / Multi-step Matrix Power
---
## Solution

### Understanding the Situation
Interpret the given sample space, random variables, and event conditions.

### Developing the Key Idea
Select the governing probabilistic principle (e.g. Chapman-Kolmogorov, Adam's Law, Central Limit Theorem, or likelihood maximization) and verify that conditions hold.

### Working Through the Solution
### Solution

### Step 1: Compute Two-Step Matrix $P^{(2)} = P \cdot P$
By the [[Chapman-Kolmogorov Equations]], $P^{(2)} = P^2$:
$$P^{(2)} = \begin{pmatrix} 0.7 & 0.3 \\ 0.4 & 0.6 \end{pmatrix} \begin{pmatrix} 0.7 & 0.3 \\ 0.4 & 0.6 \end{pmatrix}$$

Computing each entry via row-by-column dot products:
- $P_{00}^{(2)} = (0.7)(0.7) + (0.3)(0.4) = 0.49 + 0.12 = 0.61$
- $P_{01}^{(2)} = (0.7)(0.3) + (0.3)(0.6) = 0.21 + 0.18 = 0.39$
- $P_{10}^{(2)} = (0.4)(0.7) + (0.6)(0.4) = 0.28 + 0.24 = 0.52$
- $P_{11}^{(2)} = (0.4)(0.3) + (0.6)(0.6) = 0.12 + 0.36 = 0.48$

$$P^{(2)} = \begin{pmatrix}
0.61 & 0.39 \\
0.52 & 0.48
\end{pmatrix}$$

Thus, the probability of rain two days from now is:
$$P_{00}^{(2)} = 0.61 \quad (61.0\%)$$

---

### Step 2: Compute Four-Step Matrix $P^{(4)} = P^{(2)} \cdot P^{(2)}$
Instead of multiplying three times, square $P^{(2)}$:
$$P^{(4)} = \begin{pmatrix} 0.61 & 0.39 \\ 0.52 & 0.48 \end{pmatrix} \begin{pmatrix} 0.61 & 0.39 \\ 0.52 & 0.48 \end{pmatrix}$$

Computing each entry:
- $P_{00}^{(4)} = (0.61)(0.61) + (0.39)(0.52) = 0.3721 + 0.2028 = 0.5749$
- $P_{01}^{(4)} = (0.61)(0.39) + (0.39)(0.48) = 0.2379 + 0.1872 = 0.4251$
- $P_{10}^{(4)} = (0.52)(0.61) + (0.48)(0.52) = 0.3172 + 0.2496 = 0.5668$
- $P_{11}^{(4)} = (0.52)(0.39) + (0.48)(0.48) = 0.2028 + 0.2304 = 0.4332$

$$P^{(4)} = \begin{pmatrix}
0.5749 & 0.4251 \\
0.5668 & 0.4332
\end{pmatrix}$$

Thus, the probability of rain four days from now is:
$$P_{00}^{(4)} = 0.5749 \quad (57.49\%)$$

---

### Step 3: Compute Limiting Distribution $\pi$
Solve $\pi P = \pi$ subject to $\pi_0 + \pi_1 = 1$:
$$\pi_0 = 0.7 \pi_0 + 0.4 \pi_1 \implies 0.3 \pi_0 = 0.4 \pi_1 \implies \pi_1 = \frac{3}{4} \pi_0$$
$$\pi_0 + \frac{3}{4}\pi_0 = 1 \implies \frac{7}{4}\pi_0 = 1 \implies \pi_0 = \frac{4}{7} \approx 0.571428$$
$$\pi_1 = \frac{3}{7} \approx 0.428571$$

Notice:
$$|P_{00}^{(4)} - \pi_0| = |0.5749 - 0.5714| = 0.0035$$
The probability after just 4 steps is within $0.35\%$ of the infinite-horizon limit.

---
### Key Idea

Repeated squaring allows computation of $P^n$ in $O(\log n)$ matrix multiplications rather than $O(n)$. For ergodic Markov chains, $P^n$ converges geometrically fast to the matrix with identical rows equal to $\pi$.

---
### Exam Pattern

Exam questions frequently ask for:
1. $P^{(2)}$ or $P^{(4)}$ via matrix multiplication.
2. Comparison with the steady-state vector $\pi$ to demonstrate understanding of convergence.

---
### Related Problems

- [[Problem — Rain Prediction Two Days Ahead]]

---
### Related Concepts

- [[Chapman-Kolmogorov Equations]]
- [[Weather Forecasting Markov Chain Example]]
- [[Stationary and Limiting Distributions in Markov Chains]]

---

### Result and Interpretation
The final analytical solution and numerical metrics are rigorously verified against probability axioms.

---

## Reusable Insight

Always decompose complex event probabilities by conditioning on a partition of the sample space (Law of Total Probability), or by writing indicator random variables to exploit linearity of expectation.

---

## Common Mistakes

- **Incorrect Element Multiplication:** Computing $(P_{00})^4 = (0.7)^4 = 0.2401$ instead of matrix exponentiation.
- **Transposed Dot Product:** Multiplying column 0 by row 0 instead of row 0 by column 0.
- **Arithmetic Inaccuracy in Intermediate Steps:** Rounding intermediate decimals excessively (e.g., rounding $0.61$ to $0.6$), which compounds into large errors in $P^4$.
---
## Exam Pattern

Standard BUET CSE 301 final exam question testing probability bounds, Markov chain stationarity, or statistical parameter estimation.

---

## Related Problems

- [[Problem — Birthday Collisions and Approximation]]
- [[Problem — Four-Day Weather Forecast]]

---

## Related Concepts

- [[Random Variables and Probability Distributions]]
- [[Law of Total Probability and Bayes' Rule]]

---

## Source

- [[cse301/01 - Sources/Lectures/Markov_Chain.pdf]] (Slide 10)
- [[cse301/01 - Sources/Textbooks/Sheldon M. Ross book Markov Chain Chapter.pdf]] (Example 4.11, p. 212)
