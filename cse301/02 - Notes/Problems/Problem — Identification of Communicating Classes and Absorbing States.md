---
type: problem
course: cse301
status: active
order: 81
---

# Problem — Identification of Communicating Classes and Absorbing States

> 📖 **Reading Order:** Step 81 of 92 | **Module 10:** Stochastic Processes  
> ◄ **Previous:** [[Problem — State Communication and Irreducibility Verification]] | ► **Next:** [[Queueing Systems and Kendall Notation]]

---

---

## Problem

Consider a discrete-time [[Markov Chain]] with four states $S = \{0, 1, 2, 3\}$ and transition probability matrix:

$$P = \begin{pmatrix}
\frac{1}{2} & \frac{1}{2} & 0 & 0 \\[6pt]
\frac{1}{2} & \frac{1}{2} & 0 & 0 \\[6pt]
\frac{1}{4} & \frac{1}{4} & \frac{1}{4} & \frac{1}{4} \\[6pt]
0 & 0 & 0 & 1
\end{pmatrix}$$

1. Identify all communicating classes of the Markov chain.
2. For each communicating class, determine whether it is **recurrent** or **transient**.
3. Identify any **absorbing states**.
4. Explain why state $2$ does not communicate with state $0$, even though state $0$ is accessible from state $2$.
5. Is this Markov chain irreducible?

---

---

## Given

- State space $S = \{0, 1, 2, 3\}$
- Transition matrix $P$ as given above.

---

---

## Required

1. Complete partition of state space into communicating classes $C_1, C_2, \dots$
2. Classification of each class as recurrent (closed) or transient (open).
3. Identification of absorbing state(s).
4. Rigorous explanation of one-way accessibility $2 \to 0$ without communication ($2 \not\leftrightarrow 0$).
5. Determination of irreducibility.

---

---

## Concepts Tested

- [[Classification of States in Markov Chains]]
- [[Markov Chain]]
- Absorbing states, recurrence, transience, irreducibility

---

---

## Prerequisites

- [[Markov Chain]]
- [[Classification of States in Markov Chains]]

---

---

## Question Type

Conceptual / State Space Decomposition

---

---

## Solution

### Understanding the Situation
Interpret the given sample space, random variables, and event conditions.

### Developing the Key Idea
Select the governing probabilistic principle (e.g. Chapman-Kolmogorov, Adam's Law, Central Limit Theorem, or likelihood maximization) and verify that conditions hold.

### Working Through the Solution
### Solution

### Step 1: Analyze State Reachability
Examine the row transitions of $P$:
- **From State 0:** $P_{00} = 1/2$, $P_{01} = 1/2$. The process only transitions to $\{0, 1\}$. It cannot reach $\{2, 3\}$.
- **From State 1:** $P_{10} = 1/2$, $P_{11} = 1/2$. The process only transitions to $\{0, 1\}$. It cannot reach $\{2, 3\}$.
- **From State 2:** $P_{20} = 1/4, P_{21} = 1/4, P_{22} = 1/4, P_{23} = 1/4$. State 2 can directly reach all four states.
- **From State 3:** $P_{33} = 1$. The process remains in state 3 forever.

---

### Step 2: Form Communicating Classes
Recall that $i \leftrightarrow j$ requires $i \to j$ AND $j \to i$.

1. **States 0 and 1:**
   - $P_{01} = 1/2 > 0 \implies 0 \to 1$
   - $P_{10} = 1/2 > 0 \implies 1 \to 0$
   - Thus, $0 \leftrightarrow 1$. They belong to the same communicating class.
   - Neither $0$ nor $1$ can ever reach $2$ or $3$ (all outgoing probabilities to $2$ and $3$ are zero, so $P_{02}^n = P_{03}^n = P_{12}^n = P_{13}^n = 0$ for all $n \ge 1$).
   - Therefore, no other states can belong to this class:
     $$C_1 = \{0, 1\}$$

2. **State 2:**
   - State 2 can reach states 0, 1, and 3 ($P_{20} = 1/4 > 0, P_{21} = 1/4 > 0, P_{23} = 1/4 > 0$).
   - However, once the process leaves state 2, it can **never return** to state 2, because neither class $\{0, 1\}$ nor state $3$ has any transition into state 2 ($P_{02} = P_{12} = P_{32} = 0$).
   - Thus, for any $j \neq 2$, $2 \to j$ but $j \not\to 2$.
   - Consequently, state 2 communicates only with itself ($P_{22}^0 = 1$):
     $$C_2 = \{2\}$$

3. **State 3:**
   - $P_{33} = 1$. Once in state 3, the process never leaves.
   - It cannot reach $0, 1$, or $2$.
   - Thus, state 3 communicates only with itself:
     $$C_3 = \{3\}$$

**Communicating Classes:**
$$\{0, 1\}, \quad \{2\}, \quad \{3\}$$

---

### Step 3: Classify Recurrence, Transience, and Absorbing States
- **Class $C_1 = \{0, 1\}$:** This class is **closed** (no probability mass can escape). In a finite Markov chain, every closed communicating class is **recurrent**. Once entered, the chain stays in $\{0, 1\}$ forever and visits both states infinitely often.
- **Class $C_2 = \{2\}$:** This class is **not closed** because there is a non-zero probability of leaving it ($P_{20} + P_{21} + P_{23} = 3/4 > 0$) with zero probability of ever returning. Hence, state 2 is **transient**.
- **Class $C_3 = \{3\}$:** Because $P_{33} = 1$, state 3 is an **absorbing state**. By definition, an absorbing state is a closed singleton communicating class and is **recurrent**.

---

### Step 4: Explain Why $2 \not\leftrightarrow 0$ Despite $2 \to 0$
Communication is defined strictly as a symmetric, two-way relationship:
$$2 \leftrightarrow 0 \iff (2 \to 0) \land (0 \to 2)$$
- $2 \to 0$ holds because $P_{20} = 1/4 > 0$ (state 0 is accessible from state 2 in 1 step).
- However, $0 \to 2$ is **false**: starting from state 0, the process transitions only to 0 or 1 with probability 1. By induction via Chapman-Kolmogorov, $P_{02}^n = 0$ for all $n \ge 1$.
- Because accessibility is strictly one-way (a "one-way street"), states 2 and 0 **do not communicate**.

---

### Step 5: Check Irreducibility
A Markov chain is irreducible if and only if it consists of a single communicating class containing all states.
Because the state space is partitioned into three distinct classes $\{0, 1\}, \{2\}, \{3\}$, the chain is **reducible** (not irreducible).

---
### Key Idea

Accessibility ($i \to j$) is a directed reachability property, whereas communication ($i \leftrightarrow j$) is an equivalence relation requiring mutually reachable paths. In transition graphs, any state with outgoing transitions to a closed class from which there is no return path is transient.

---
### Exam Pattern

Exam questions frequently provide a $3 \times 3$ or $4 \times 4$ matrix and ask students to:
1. List all equivalence classes.
2. Label each class as recurrent or transient.
3. Identify absorbing states.
4. State whether the chain is irreducible.

---
### Related Problems

- [[Problem — State Communication and Irreducibility Verification]]

---
### Related Concepts

- [[Classification of States in Markov Chains]]
- [[Markov Chain]]
- [[Stationary and Limiting Distributions in Markov Chains]]

---

### Result and Interpretation
The final analytical solution and numerical metrics are rigorously verified against probability axioms.

---

## Reusable Insight

Always decompose complex event probabilities by conditioning on a partition of the sample space (Law of Total Probability), or by writing indicator random variables to exploit linearity of expectation.

---

## Common Mistakes

- **Confusing Accessibility with Communication:** Grouping states $0, 1, 2$ into one class simply because state 2 can jump to states 0 and 1.
- **Overlooking Absorbing State Definition:** Forgetting to check diagonal entries $P_{ii} = 1$ to immediately spot absorbing states.
- **Calling the Whole Chain Transient or Recurrent:** Markov chains with multiple classes are not uniformly recurrent or transient; classification applies to individual states and communicating classes.

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

- [[01 - Sources/Lectures/Markov_Chain.pdf]] (Slide 15)
- [[01 - Sources/Textbooks/Sheldon M. Ross book Markov Chain Chapter.pdf]] (Section 4.3, pp. 202–206)
