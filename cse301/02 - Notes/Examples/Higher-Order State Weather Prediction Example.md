---
type: example
course: cse301
status: active
order: 75
---

# Higher-Order State Weather Prediction Example

> 📖 **Reading Order:** Step 75 of 92 | **Module 10:** Stochastic Processes  
> ◄ **Previous:** [[Weather Forecasting Markov Chain Example]] | ► **Next:** [[Hardy-Weinberg Law Markov Chain Example]]

---

---

## Problem

Suppose whether it rains today depends on the weather conditions of the **past two days**:
- If it rained both yesterday and today, it will rain tomorrow with probability $0.7$.
- If it rained today but not yesterday, it will rain tomorrow with probability $0.5$.
- If it rained yesterday but not today, it will rain tomorrow with probability $0.4$.
- If it did not rain either day, it will rain tomorrow with probability $0.2$.

Given that it rained both yesterday and today, what is the probability that it rains the day after tomorrow?

---

---

## Given

Because the probability of tomorrow's weather depends on two preceding days, the process $\{X_n\}$ where $X_n \in \{\text{Rain}, \text{No Rain}\}$ is a second-order Markov process (not a first-order Markov chain in its native state space).

To transform this into a standard first-order [[Markov Chain]], we augment the state space into 2-day historical pairs:

| State Index | Meaning | Notation |
|---|---|---|
| **0** | Rained today, rained yesterday | $(R, R)$ |
| **1** | Rained today, did not rain yesterday | $(R, NR)$ |
| **2** | Did not rain today, rained yesterday | $(NR, R)$ |
| **3** | Did not rain today, did not rain yesterday | $(NR, NR)$ |

Initial condition: We start in **State 0** ($X_0 = 0$, meaning it rained both yesterday and today).

---

---

## Required

1. Formulate the $4 \times 4$ one-step transition probability matrix $P$.
2. Compute the two-step transition probabilities from State 0: $P_{0j}^{(2)}$ for $j \in \{0, 1, 2, 3\}$.
3. Calculate the probability that it rains the day after tomorrow (at time step $n = 2$).

---

---

## Understanding the Problem and Choosing the Method

Identify the random variables, state the conditional distributions, select the appropriate probabilistic law or updating formula, and execute the algebraic substitutions step by step.

---

## Solution

### Concepts Used

- [[Markov Chain]] (State Space Augmentation)
- [[Chapman-Kolmogorov Equations]]
- [[Stochastic Process]]

---
### Solution

### Step 1: Construct the $4 \times 4$ Transition Probability Matrix
Notice the temporal shift rule:
If today's state is $(A, B)$ (meaning today $= A$, yesterday $= B$), then tomorrow's state must be $(\text{Tomorrow}, A)$.
- If tomorrow is Rain ($R$), the new state is $(R, A)$.
- If tomorrow is No Rain ($NR$), the new state is $(NR, A)$.
Any transition to a state whose "yesterday" does not match today's "today" has probability $0$.

#### Transition Probabilities from each state:
1. **From State 0: $(R, R)$**
   - If tomorrow rains ($R$): new state is $(R, R)$ (State 0), with probability $0.7$.
   - If tomorrow does not rain ($NR$): new state is $(NR, R)$ (State 2), with probability $1 - 0.7 = 0.3$.
   - Transitions to State 1 $(R, NR)$ and State 3 $(NR, NR)$ are impossible ($0$).
   - Row 0: $\begin{pmatrix} 0.7 & 0 & 0.3 & 0 \end{pmatrix}$

2. **From State 1: $(R, NR)$**
   - If tomorrow rains: new state is $(R, R)$ (State 0), with probability $0.5$.
   - If tomorrow does not rain: new state is $(NR, R)$ (State 2), with probability $1 - 0.5 = 0.5$.
   - Row 1: $\begin{pmatrix} 0.5 & 0 & 0.5 & 0 \end{pmatrix}$

3. **From State 2: $(NR, R)$**
   - If tomorrow rains: new state is $(R, NR)$ (State 1), with probability $0.4$.
   - If tomorrow does not rain: new state is $(NR, NR)$ (State 3), with probability $1 - 0.4 = 0.6$.
   - Row 2: $\begin{pmatrix} 0 & 0.4 & 0 & 0.6 \end{pmatrix}$

4. **From State 3: $(NR, NR)$**
   - If tomorrow rains: new state is $(R, NR)$ (State 1), with probability $0.2$.
   - If tomorrow does not rain: new state is $(NR, NR)$ (State 3), with probability $1 - 0.2 = 0.8$.
   - Row 3: $\begin{pmatrix} 0 & 0.2 & 0 & 0.8 \end{pmatrix}$

Thus, the transition probability matrix is:
$$P = \begin{pmatrix}
0.7 & 0 & 0.3 & 0 \\
0.5 & 0 & 0.5 & 0 \\
0 & 0.4 & 0 & 0.6 \\
0 & 0.2 & 0 & 0.8
\end{pmatrix}$$

---

### Step 2: Compute Two-Step Matrix $P^{(2)} = P \cdot P$
"The day after tomorrow" corresponds to $n = 2$ transitions from time $0$.
By the [[Chapman-Kolmogorov Equations]], $P^{(2)} = P^2$.

Let us compute row 0 of $P^{(2)}$ by taking the dot product of Row 0 of $P$ with each column of $P$:
- Row 0 of $P$: $\begin{pmatrix} 0.7 & 0 & 0.3 & 0 \end{pmatrix}$

$$\begin{aligned}
P_{00}^{(2)} &= 0.7(0.7) + 0(0.5) + 0.3(0) + 0(0) = 0.49 + 0 + 0 + 0 = 0.49 \\
P_{01}^{(2)} &= 0.7(0) + 0(0) + 0.3(0.4) + 0(0.2) = 0 + 0 + 0.12 + 0 = 0.12 \\
P_{02}^{(2)} &= 0.7(0.3) + 0(0.5) + 0.3(0) + 0(0) = 0.21 + 0 + 0 + 0 = 0.21 \\
P_{03}^{(2)} &= 0.7(0) + 0(0) + 0.3(0.6) + 0(0.8) = 0 + 0 + 0.18 + 0 = 0.18
\end{aligned}$$

For completeness, the entire matrix $P^{(2)}$ is:
$$P^{(2)} = \begin{pmatrix}
0.49 & 0.12 & 0.21 & 0.18 \\
0.35 & 0.20 & 0.15 & 0.30 \\
0.20 & 0.12 & 0.20 & 0.48 \\
0.10 & 0.16 & 0.10 & 0.64
\end{pmatrix}$$

Check: $0.49 + 0.12 + 0.21 + 0.18 = 1.00$.

---

### Step 3: Identify States Corresponding to Rain and Compute Probability
The system starts in State 0 at $n = 0$.
At step $n = 2$ (the day after tomorrow), the weather is rainy if the system is in any state where "today" is Rain ($R$):
- **State 0:** $(R, R)$ — rained on day 2
- **State 1:** $(R, NR)$ — rained on day 2

States 2 $(NR, R)$ and 3 $(NR, NR)$ represent no rain on day 2.

Therefore, the total probability of rain the day after tomorrow is:
$$P(\text{Rain at } n = 2 \mid X_0 = 0) = P_{00}^{(2)} + P_{01}^{(2)}$$
$$P(\text{Rain}) = 0.49 + 0.12 = 0.61$$

---
### General Method

1. **State Expansion:** For memory of depth $k$ over alphabet $\mathcal{A}$, define states as $k$-tuples $\mathbf{s} \in \mathcal{A}^k$.
2. **Transition Matrix Structure:** $P_{(a_1, \dots, a_k), (b_1, \dots, b_k)} = 0$ unless $b_2 = a_1, b_3 = a_2, \dots, b_k = a_{k-1}$.
3. **Multi-Step Forecasting:** Multiply $P^n$ and sum over all terminal states matching the event of interest.

---

---

## Result

- Two-step transition probabilities from State 0:
  $$\begin{pmatrix} P_{00}^{(2)} & P_{01}^{(2)} & P_{02}^{(2)} & P_{03}^{(2)} \end{pmatrix} = \begin{pmatrix} 0.49 & 0.12 & 0.21 & 0.18 \end{pmatrix}$$
- Probability that it rains the day after tomorrow:
  $$0.61 \quad (61\%)$$

---

---

## Why This Works

- Systems whose dynamics depend on a finite window of past history of length $k$ can always be modeled as a first-order Markov chain by defining the state as a $k$-tuple of consecutive values.
- In this expanded state space, each transition automatically preserves continuity (the second element of the past tuple becomes the first element of the next tuple), ensuring the Markov property holds strictly.

---

---

## Common Mistakes

- **Forgetting Impossible Transitions:** Placing non-zero probabilities on transitions like $(R, R) \to (R, NR)$. If today is $R$, tomorrow's "yesterday" must be $R$; it cannot magically become $NR$.
- **Adding the Wrong Entries:** Only reporting $P_{00}^{(2)} = 0.49$ and forgetting that State 1 $(R, NR)$ also represents rain on that day.
- **Inverting the State Tuple Ordering:** Mixing up $(R, NR)$ with $(NR, R)$, which leads to transposed column associations.

---

---

## General Method

Extract the reusable problem-solving pattern: define random variables, write down the joint distribution, condition on observed data, and normalize the resulting distribution.

---

## Related Concepts

- [[Markov Chain]]
- [[Chapman-Kolmogorov Equations]]
- [[Weather Forecasting Markov Chain Example]]

---

---

## Sources

- [[01 - Sources/Lectures/Markov_Chain.pdf]] (Slides 16–18)
- [[01 - Sources/Textbooks/Sheldon M. Ross book Markov Chain Chapter.pdf]] (Example 4.4, pp. 195–196; Example 4.12, pp. 212–213)
