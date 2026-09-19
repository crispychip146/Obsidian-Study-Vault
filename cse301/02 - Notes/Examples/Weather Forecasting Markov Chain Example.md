---
type: example
course: cse301
status: active
---

# Weather Forecasting Markov Chain Example

## Problem

Suppose that the chance of rain tomorrow depends only on whether it is raining today and not on previous days' weather:
- If it rains today, it rains tomorrow with probability $\alpha = 0.7$.
- If it does not rain today, it rains tomorrow with probability $\beta = 0.4$.

1. Formulate the system as a two-state [[Markov Chain]] and write its transition probability matrix $P$.
2. Given that it is raining today, calculate the probability that it rains four days from now.
3. Compute the long-run proportion of days that are rainy.

---

## Given

- State $0$: "Rain"
- State $1$: "No Rain"
- $P(\text{Rain tomorrow} \mid \text{Rain today}) = P_{00} = \alpha = 0.7$
- $P(\text{No Rain tomorrow} \mid \text{Rain today}) = P_{01} = 1 - \alpha = 0.3$
- $P(\text{Rain tomorrow} \mid \text{No Rain today}) = P_{10} = \beta = 0.4$
- $P(\text{No Rain tomorrow} \mid \text{No Rain today}) = P_{11} = 1 - \beta = 0.6$
- Initial state: $X_0 = 0$ (raining today)

---

## Required

1. One-step transition probability matrix $P$.
2. Four-step transition probability $P_{00}^4 = P(X_4 = 0 \mid X_0 = 0)$.
3. Long-run stationary distribution $\pi = (\pi_0, \pi_1)$, specifically $\pi_0$.

---

## Concepts Used

- [[Markov Chain]]
- [[Chapman-Kolmogorov Equations]]
- [[Stationary and Limiting Distributions in Markov Chains]]

---

## Solution

### Step 1: Formulate the Transition Probability Matrix
Using states $\{0, 1\}$:
$$P = \begin{pmatrix}
P_{00} & P_{01} \\
P_{10} & P_{11}
\end{pmatrix} = \begin{pmatrix}
0.7 & 0.3 \\
0.4 & 0.6
\end{pmatrix}$$
Check row sums:
- Row 0: $0.7 + 0.3 = 1.0$
- Row 1: $0.4 + 0.6 = 1.0$
Both rows sum to 1, confirming a valid stochastic matrix.

---

### Step 2: Compute the 4-Step Transition Probability $P_{00}^4$
By the [[Chapman-Kolmogorov Equations]], the 4-step transition matrix is given by $P^{(4)} = P^4$.
We can compute this efficiently by squaring $P$ to obtain $P^2$, and squaring $P^2$ to obtain $P^4$.

#### Calculate $P^{(2)} = P \cdot P$:
$$P^{(2)} = \begin{pmatrix} 0.7 & 0.3 \\ 0.4 & 0.6 \end{pmatrix} \begin{pmatrix} 0.7 & 0.3 \\ 0.4 & 0.6 \end{pmatrix}$$
- $P_{00}^{(2)} = (0.7)(0.7) + (0.3)(0.4) = 0.49 + 0.12 = 0.61$
- $P_{01}^{(2)} = (0.7)(0.3) + (0.3)(0.6) = 0.21 + 0.18 = 0.39$
- $P_{10}^{(2)} = (0.4)(0.7) + (0.6)(0.4) = 0.28 + 0.24 = 0.52$
- $P_{11}^{(2)} = (0.4)(0.3) + (0.6)(0.6) = 0.12 + 0.36 = 0.48$

$$P^{(2)} = \begin{pmatrix} 0.61 & 0.39 \\ 0.52 & 0.48 \end{pmatrix}$$

#### Calculate $P^{(4)} = P^{(2)} \cdot P^{(2)}$:
$$P^{(4)} = \begin{pmatrix} 0.61 & 0.39 \\ 0.52 & 0.48 \end{pmatrix} \begin{pmatrix} 0.61 & 0.39 \\ 0.52 & 0.48 \end{pmatrix}$$
- $P_{00}^{(4)} = (0.61)(0.61) + (0.39)(0.52) = 0.3721 + 0.2028 = 0.5749$
- $P_{01}^{(4)} = (0.61)(0.39) + (0.39)(0.48) = 0.2379 + 0.1872 = 0.4251$
- $P_{10}^{(4)} = (0.52)(0.61) + (0.48)(0.52) = 0.3172 + 0.2496 = 0.5668$
- $P_{11}^{(4)} = (0.52)(0.39) + (0.48)(0.48) = 0.2028 + 0.2304 = 0.4332$

$$P^{(4)} = \begin{pmatrix} 0.5749 & 0.4251 \\ 0.5668 & 0.4332 \end{pmatrix}$$

Therefore, $P_{00}^4 = 0.5749$.

---

### Step 3: Compute the Long-Run Proportion of Rainy Days
To find the steady-state proportion of rainy days, solve the balance equations $\pi P = \pi$ with $\pi_0 + \pi_1 = 1$:
$$\begin{pmatrix} \pi_0 & \pi_1 \end{pmatrix} \begin{pmatrix} 0.7 & 0.3 \\ 0.4 & 0.6 \end{pmatrix} = \begin{pmatrix} \pi_0 & \pi_1 \end{pmatrix}$$

This gives:
1. $0.7 \pi_0 + 0.4 \pi_1 = \pi_0 \implies 0.4 \pi_1 = 0.3 \pi_0 \implies \pi_1 = \frac{3}{4} \pi_0$
2. Normalization condition: $\pi_0 + \pi_1 = 1$

Substitute $\pi_1 = 0.75 \pi_0$:
$$\pi_0 + 0.75 \pi_0 = 1 \implies 1.75 \pi_0 = 1$$
$$\pi_0 = \frac{1}{1.75} = \frac{4}{7} \approx 0.571428$$
$$\pi_1 = 1 - \pi_0 = \frac{3}{7} \approx 0.428571$$

General formula check:
$$\pi_0 = \frac{\beta}{1 + \beta - \alpha} = \frac{0.4}{1 + 0.4 - 0.7} = \frac{0.4}{0.7} = \frac{4}{7}$$

---

## Result

1. Transition matrix:
   $$P = \begin{pmatrix} 0.7 & 0.3 \\ 0.4 & 0.6 \end{pmatrix}$$
2. Probability of rain 4 days from now given rain today:
   $$P_{00}^4 = 0.5749 \quad (57.49\%)$$
3. Long-run proportion of rainy days:
   $$\pi_0 = \frac{4}{7} \approx 57.14\%$$

Notice how close $P_{00}^4 = 0.5749$ is to the limiting value $\pi_0 \approx 0.5714$, showing how rapidly the Markov chain converges toward its stationary distribution.

---

## Why This Works

- The Chapman-Kolmogorov equations guarantee that multi-step probabilities correspond to powers of the transition matrix. Computing $P^4 = (P^2)^2$ reduces computational complexity from 3 matrix multiplications to 2.
- Because all entries of $P$ are strictly positive ($P_{ij} > 0$), the chain is irreducible and aperiodic (primitive), guaranteeing geometric convergence of $P^n$ to a rank-1 matrix where every row equals $\pi = (4/7, 3/7)$.

---

## Common Mistakes

- **Squaring Individual Elements:** Calculating $(0.7)^4 = 0.2401$ instead of matrix power $P^4$.
- **Ignoring Normalization:** Trying to solve $\pi(P - I) = 0$ without using $\pi_0 + \pi_1 = 1$, leading to infinite trivial solutions or $\pi = 0$.
- **Arithmetic Inversion:** Inverting $\pi_1 = (3/4)\pi_0$ as $\pi_0 = (3/4)\pi_1$.

---

## General Method

For any 2-state Markov chain $P = \begin{pmatrix} \alpha & 1-\alpha \\ \beta & 1-\beta \end{pmatrix}$:
1. Multi-step transition matrix: Use repeated squaring $P^{(2k)} = (P^{(k)})^2$.
2. Stationary distribution closed-form solution:
   $$\pi_0 = \frac{\beta}{1 - \alpha + \beta}, \qquad \pi_1 = \frac{1 - \alpha}{1 - \alpha + \beta}$$

---

## Related Concepts

- [[Markov Chain]]
- [[Chapman-Kolmogorov Equations]]
- [[Stationary and Limiting Distributions in Markov Chains]]
- [[Higher-Order State Weather Prediction Example]]

---

## Sources

- [[01 - Sources/Lectures/Markov_Chain.pdf]] (Slides 5, 10, 21–22)
- [[01 - Sources/Textbooks/Sheldon M. Ross book Markov Chain Chapter.pdf]] (Example 4.1, p. 194; Example 4.11, p. 212)
