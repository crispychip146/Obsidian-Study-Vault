---
type: problem
course: cse301
status: active
order: 80
---

# Problem — State Communication and Irreducibility Verification

> 📖 **Reading Order:** Step 80 of 92 | **Module 10:** Stochastic Processes  
> ◄ **Previous:** [[Problem — Rain Prediction Two Days Ahead]] | ► **Next:** [[Problem — Identification of Communicating Classes and Absorbing States]]

---

---

## Problem

Consider a three-state [[Markov Chain]] with state space $S = \{0, 1, 2\}$ and transition probability matrix:

$$P = \begin{pmatrix}
\frac{1}{2} & \frac{1}{2} & 0 \\[6pt]
\frac{1}{2} & \frac{1}{4} & \frac{1}{4} \\[6pt]
0 & \frac{1}{3} & \frac{2}{3}
\end{pmatrix}$$

1. Determine whether state $2$ is accessible from state $0$, despite $P_{02} = 0$.
2. Prove that all pairs of states communicate with each other ($i \leftrightarrow j$ for all $i, j \in \{0, 1, 2\}$).
3. Determine whether the Markov chain is irreducible.
4. Find the period of each state.

---

---

## Given

- State space $S = \{0, 1, 2\}$
- Transition matrix $P$ as specified above.

---

---

## Required

1. Verification of accessibility $0 \to 2$.
2. Proof of mutual communication for all pairs.
3. Determination of irreducibility.
4. Periodicity $d(i)$ for $i \in \{0, 1, 2\}$.

---

---

## Concepts Tested

- [[Classification of States in Markov Chains]] (Accessibility, Communication, Communicating Classes, Irreducibility, Periodicity)
- [[Chapman-Kolmogorov Equations]]
- [[Markov Chain]]

---

---

## Prerequisites

- [[Markov Chain]]
- [[Classification of States in Markov Chains]]

---

---

## Question Type

Proof / Conceptual Verification

---

---

## Solution

### Understanding the Situation
Interpret the given sample space, random variables, and event conditions.

### Developing the Key Idea
Select the governing probabilistic principle (e.g. Chapman-Kolmogorov, Adam's Law, Central Limit Theorem, or likelihood maximization) and verify that conditions hold.

### Working Through the Solution
### Solution

### Step 1: Verify Accessibility $0 \to 2$
State $2$ is accessible from state $0$ ($0 \to 2$) if there exists an integer $n \ge 1$ such that $P_{02}^n > 0$.
Although direct transition $P_{02} = 0$, consider the two-step path $0 \to 1 \to 2$:
By the [[Chapman-Kolmogorov Equations]]:
$$P_{02}^{(2)} = \sum_{k=0}^2 P_{0k} P_{k2} \ge P_{01} P_{12} = \left(\frac{1}{2}\right)\left(\frac{1}{4}\right) = \frac{1}{8} > 0$$

Since $P_{02}^{(2)} \ge \frac{1}{8} > 0$, state $2$ is **accessible from state 0** in $n = 2$ steps.

---

### Step 2: Prove Mutual Communication ($i \leftrightarrow j$)
We verify the communication of all pairs:

1. **Between states 0 and 1:**
   - $P_{01} = \frac{1}{2} > 0 \implies 0 \to 1$
   - $P_{10} = \frac{1}{2} > 0 \implies 1 \to 0$
   - Therefore, $0 \leftrightarrow 1$.

2. **Between states 1 and 2:**
   - $P_{12} = \frac{1}{4} > 0 \implies 1 \to 2$
   - $P_{21} = \frac{1}{3} > 0 \implies 2 \to 1$
   - Therefore, $1 \leftrightarrow 2$.

3. **Between states 0 and 2:**
   - In Step 1, we proved $0 \to 2$ via $0 \to 1 \to 2$ ($P_{02}^{(2)} \ge 1/8 > 0$).
   - For the reverse direction $2 \to 0$, consider path $2 \to 1 \to 0$:
     $$P_{20}^{(2)} \ge P_{21} P_{10} = \left(\frac{1}{3}\right)\left(\frac{1}{2}\right) = \frac{1}{6} > 0 \implies 2 \to 0$$
   - Alternatively, by the **transitivity** of communication:
     $$0 \leftrightarrow 1 \quad \text{and} \quad 1 \leftrightarrow 2 \implies 0 \leftrightarrow 2$$
   - Therefore, $0 \leftrightarrow 2$.

---

### Step 3: Check Irreducibility
Since every state communicates with every other state, the equivalence relation $\leftrightarrow$ partitions the state space into a **single communicating class**:
$$C = \{0, 1, 2\}$$

Because the state space contains only one class, the Markov chain is **irreducible**.

---

### Step 4: Determine Periodicity of Each State
The period of state $i$ is $d(i) = \gcd \{n \ge 1 : P_{ii}^n > 0\}$.

Inspect the diagonal entries of $P$:
$$P_{00} = \frac{1}{2} > 0$$
Because the chain can return to state 0 in a single step ($n = 1$):
$$d(0) = \gcd(1, \dots) = 1$$
Thus, state 0 is **aperiodic**.

Because **periodicity is a class property**, all states in the same communicating class share identical periods. Since $\{0, 1, 2\}$ forms a single communicating class:
$$d(0) = d(1) = d(2) = 1$$
All states are **aperiodic**.

---
### Key Idea

A zero entry $P_{ij} = 0$ in the one-step transition matrix does *not* imply that state $j$ cannot be reached from state $i$. Reachability only requires the existence of at least one path of non-zero probability in the transition graph. If any state in an irreducible class has a self-loop ($P_{ii} > 0$), the entire class is automatically aperiodic.

---
### Exam Pattern

A staple exam question testing:
1. Definition of accessibility vs direct transition.
2. Formal proof of communication via intermediate paths.
3. Definition and verification of irreducibility.
4. Exploiting self-loops ($P_{ii} > 0$) to establish aperiodicity instantly.

---
### Related Problems

- [[Problem — Identification of Communicating Classes and Absorbing States]]
- [[Problem — Four-Day Weather Forecast]]

---
### Related Concepts

- [[Classification of States in Markov Chains]]
- [[Stationary and Limiting Distributions in Markov Chains]]
- [[Chapman-Kolmogorov Equations]]

---

### Result and Interpretation
The final analytical solution and numerical metrics are rigorously verified against probability axioms.

---

## Reusable Insight

Always decompose complex event probabilities by conditioning on a partition of the sample space (Law of Total Probability), or by writing indicator random variables to exploit linearity of expectation.

---

## Common Mistakes

- **Equating $P_{ij} = 0$ with Inaccessibility:** Claiming that $2$ is not accessible from $0$ simply because $P_{02} = 0$.
- **Recomputing the Period for Every State Independently:** Manually calculating cycle combinations for states 1 and 2 instead of invoking the class property of periodicity once $d(0) = 1$ is established.
- **Forgetting Transitivity Proof:** Claiming $0 \leftrightarrow 2$ without either citing the transitivity theorem or providing the two-step Chapman-Kolmogorov calculation.

---

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

- [[01 - Sources/Lectures/Markov_Chain.pdf]] (Slide 14)
- [[01 - Sources/Textbooks/Sheldon M. Ross book Markov Chain Chapter.pdf]] (Section 4.3, pp. 202–204)
