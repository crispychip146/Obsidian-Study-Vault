---
type: problem
course: cse301
status: active
order: 79
---

# Problem — Rain Prediction Two Days Ahead

> 📖 **Reading Order:** Step 79 of 92 | **Module 10:** Stochastic Processes  
> ◄ **Previous:** [[Problem — Four-Day Weather Forecast]] | ► **Next:** [[Problem — State Communication and Irreducibility Verification]]

---

## Problem

Suppose whether it rains on any given day depends on the weather conditions of the previous two days:
- If it rained both yesterday and today, it will rain tomorrow with probability $0.7$.
- If it rained today but not yesterday, it will rain tomorrow with probability $0.5$.
- If it rained yesterday but not today, it will rain tomorrow with probability $0.4$.
- If it did not rain either day, it will rain tomorrow with probability $0.2$.

The system is formulated as a four-state [[Markov Chain]]:
- State 0: Rained both today and yesterday
- State 1: Rained today but not yesterday
- State 2: Rained yesterday but not today
- State 3: Did not rain either day

with transition probability matrix:
$$P = \begin{pmatrix}
0.7 & 0 & 0.3 & 0 \\
0.5 & 0 & 0.5 & 0 \\
0 & 0.4 & 0 & 0.6 \\
0 & 0.2 & 0 & 0.8
\end{pmatrix}$$

**Question:** Given that it rained both yesterday and today (so the system starts in State 0), what is the probability that it rains the day after tomorrow?

---

## Given

- Initial state: $X_0 = 0$ (State 0: rained yesterday and today)
- Transition probability matrix $P$ as given above.
- Target event: It rains on day 2 (the day after tomorrow).

---

## Required

1. Compute row 0 of the two-step transition matrix $P^{(2)}$.
2. Identify all states at time step $n = 2$ in which it rains on that day.
3. Compute the conditional probability of the target event.

---

## Concepts Tested

- [[Markov Chain]] (Higher-Order State Representation)
- [[Chapman-Kolmogorov Equations]]
- Law of Total Probability

---

## Question Type

Numerical / Matrix Multiplication

---

## Solution

The question asks about rain, while the matrix evolves a richer two-day state. Keep those levels distinct. The initial row vector is concentrated on state zero, and multiplying by $P^2$ gives the distribution of the pair two days later.

Under the stated (today, yesterday) convention, states zero and one both mean rain today. Sum their two-step probabilities, $0.49+0.12=0.61$. Looking only at state zero would demand rain on both relevant days and answer a narrower question.

You can check the same answer by conditioning on tomorrow: $0.7(0.7)+0.3(0.4)$. The first product is two successive rainy days; the second is a dry tomorrow followed by rain. This check makes the target event visible and detects a mistaken state convention.

### Step 1: Compute Row 0 of $P^{(2)} = P \cdot P$
By the [[Chapman-Kolmogorov Equations]], the two-step transition probabilities from State 0 are given by the dot product of Row 0 of $P$ with each column of $P$:

Row 0 of $P$: $\begin{pmatrix} 0.7 & 0 & 0.3 & 0 \end{pmatrix}$

$$\begin{aligned}
P_{00}^{(2)} &= 0.7(0.7) + 0(0.5) + 0.3(0) + 0(0) = 0.49 \\
P_{01}^{(2)} &= 0.7(0) + 0(0) + 0.3(0.4) + 0(0.2) = 0.12 \\
P_{02}^{(2)} &= 0.7(0.3) + 0(0.5) + 0.3(0) + 0(0) = 0.21 \\
P_{03}^{(2)} &= 0.7(0) + 0(0) + 0.3(0.6) + 0(0.8) = 0.18
\end{aligned}$$

Check row sum:
$$0.49 + 0.12 + 0.21 + 0.18 = 1.00$$

---

### Step 2: Identify Favorable Destination States
The state at step $n = 2$ encodes the weather on day 2 and day 1:
- **State 0:** $(R_2, R_1)$ — rained on day 2
- **State 1:** $(R_2, NR_1)$ — rained on day 2
- **State 2:** $(NR_2, R_1)$ — did not rain on day 2
- **State 3:** $(NR_2, NR_1)$ — did not rain on day 2

Therefore, raining on day 2 corresponds to the event $X_2 \in \{0, 1\}$.

---

### Step 3: Compute Total Probability
$$P(\text{Rain on day 2} \mid X_0 = 0) = P_{00}^{(2)} + P_{01}^{(2)}$$
$$P(\text{Rain on day 2}) = 0.49 + 0.12 = 0.61 \quad (61.0\%)$$

---
### Key Idea

When predicting an event in an augmented-state Markov chain, the physical event of interest typically corresponds to a **subset of states** rather than a single state. The overall probability is the sum of transition probabilities into all constituent states belonging to that event.

---
### Exam Pattern

Standard exam question testing:
1. Efficient row-by-matrix multiplication ($1 \times 4$ row vector times $4 \times 4$ matrix).
2. Proper mapping from composite state definitions back to underlying real-world events.

---
### Related Problems

- [[Problem — Four-Day Weather Forecast]]
- [[Problem — Patty and Max Gambler's Ruin]]

---
### Related Concepts

- [[Higher-Order State Weather Prediction Example]]
- [[Chapman-Kolmogorov Equations]]
- [[Markov Chain]]

---

## Common Mistakes

- **Computing Entire $4 \times 4$ Matrix Unnecessarily:** In an exam with strict time limits, calculating all 16 entries of $P^2$ wastes valuable time. Only **Row 0** is needed since $X_0 = 0$.
- **Omitting State 1:** Mistakenly concluding the answer is just $P_{00}^{(2)} = 0.49$ by equating "rain" solely with State 0. State 1 also represents rain on that day!
- **Index Alignment Slip:** Mixing up the 0-indexed states when taking column dot products.

---

## What to carry forward

[[Higher-Order State Weather Prediction Example]] explains why the pair is necessary. Always translate a verbal event into a set of states before summing a matrix-power row.

## Related notes

- [[Higher-Order State Weather Prediction Example]]

## Source

- [[01 - Sources/Lectures/Markov_Chain.pdf]] (Slides 16–18)
- [[01 - Sources/Textbooks/Sheldon M. Ross book Markov Chain Chapter.pdf]] (Example 4.12, pp. 212–213)
