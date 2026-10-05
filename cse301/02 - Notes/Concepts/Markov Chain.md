---
type: concept
course: cse301
status: active
order: 69
---

# Markov Chain

> 📖 **Reading Order:** Step 69 of 92 | **Module 10:** Stochastic Processes  
> ◄ **Previous:** [[Stochastic Process]] | ► **Next:** [[Classification of States in Markov Chains]]

---

## Building the idea

A Markov chain predicts the next state from the current state without needing additional past history, conditional on that current state. This does not say consecutive states are independent; the present usually influences the future strongly.

For a time-homogeneous discrete-time chain, $P_{ij}$ is the probability of moving from $i$ to $j$ in one step and does not change with the step number. Each row sums to one because the next state must be somewhere. With row-vector convention, an initial distribution $\nu$ evolves to $\nu P$ after one step and $\nu P^n$ after $n$ steps.

Check what the state remembers. Daily weather may be modeled by today's weather alone, but if yesterday also matters, a one-day state is insufficient. The Markov property is an assumption about the chosen state and model, not a fact created merely by writing a transition matrix.

## Definition

A **discrete-time Markov chain** is a discrete-time [[Stochastic Process]] $\{X_n, n = 0, 1, 2, \dots\}$ taking values in a finite or countable state space $S \subseteq \{0, 1, 2, \dots\}$ such that for all time steps $n \ge 0$ and all states $i_0, i_1, \dots, i_{n-1}, i, j \in S$:

$$P(X_{n+1} = j \mid X_n = i, X_{n-1} = i_{n-1}, \dots, X_0 = i_0) = P(X_{n+1} = j \mid X_n = i) = P_{ij}$$

This fundamental defining identity is known as the **Markov Property**.

The values $P_{ij}$ are called the **one-step transition probabilities** of the chain. They must satisfy two basic axiomatic requirements:
1. **Non-negativity:**
   $$P_{ij} \ge 0, \quad \forall i, j \ge 0$$
2. **Total Probability per Row (Row Stochasticity):**
   $$\sum_{j=0}^\infty P_{ij} = 1, \quad \forall i \ge 0$$

The matrix $P = [P_{ij}]$ is called the **Transition Probability Matrix** (or **stochastic matrix**):

$$P = \begin{pmatrix}
P_{00} & P_{01} & P_{02} & \cdots \\
P_{10} & P_{11} & P_{12} & \cdots \\
\vdots & \vdots & \vdots & \ddots \\
P_{i0} & P_{i1} & P_{i2} & \cdots \\
\vdots & \vdots & \vdots & \ddots
\end{pmatrix}$$

---

## How It Works

1. **Specify State Space ($S$):** Identify all distinct, mutually exclusive situations the system can occupy.
2. **Determine Transition Probabilities ($P_{ij}$):** For every pair of states $(i, j)$, determine the probability that the system moves from $i$ to $j$ in one discrete time step.
3. **Construct the Transition Probability Matrix ($P$):**
   - Each row $i$ represents the probability distribution over all destination states when starting in state $i$.
   - The row entries must sum to 1 because the process must jump to *some* state (including potentially remaining in state $i$).
4. **Define Initial Distribution ($\alpha$):**
   A probability vector $\alpha = (\alpha_0, \alpha_1, \dots)$ where $\alpha_i = P(X_0 = i)$ and $\sum_i \alpha_i = 1$.
5. **Compute Joint Probabilities via the Chain Rule:**
   The probability of observing a specific sequence of states $(i_0, i_1, \dots, i_n)$ is given by the product of the initial probability and successive transition probabilities:
   $$P(X_0 = i_0, X_1 = i_1, X_2 = i_2, \dots, X_n = i_n) = \alpha_{i_0} P_{i_0 i_1} P_{i_1 i_2} \cdots P_{i_{n-1} i_n}$$

---
### Important Properties

- **Row Sum Property:** $\sum_{j} P_{ij} = 1$ for all rows $i$. (Columns do not generally sum to 1 unless the matrix is doubly stochastic).
- **Markov Property Holds for Multi-Step Horizons:**
  $$P(X_{n+m} = j \mid X_n = i, X_{n-1} = i_{n-1}, \dots, X_0 = i_0) = P(X_{n+m} = j \mid X_n = i) = P_{ij}^m$$
- **Closure under Matrix Multiplication:** Multi-step transitions are given directly by powers of $P$ via [[Chapman-Kolmogorov Equations]].

---

## Technical Details

### Time Homogeneity (Stationary Transitions)
A Markov chain is called **time-homogeneous** if the transition probabilities do not depend on the absolute time index $n$:
$$P(X_{n+1} = j \mid X_n = i) = P(X_1 = j \mid X_0 = i) = P_{ij}, \quad \forall n \ge 0$$
Unless explicitly stated otherwise, Markov chains studied in CSE301 are assumed to be time-homogeneous.

### State Space Augmentation (Handling Higher-Order Memory)
If a system depends on the last $k$ time steps (a $k$-th order Markov chain):
$$P(X_{n+1} = j \mid X_n, X_{n-1}, \dots, X_{n-k+1})$$
it can always be reformulated as a standard first-order Markov chain by expanding the state space into $k$-tuples:
$$Y_n = (X_n, X_{n-1}, \dots, X_{n-k+1})$$
See [[Higher-Order State Weather Prediction Example]] for a concrete application.

---

## Common Mistakes

- **Confusing Row Sums with Column Sums:** Summing columns to 1 instead of rows. $P$ is row-stochastic ($\sum_j P_{ij} = 1$), not necessarily column-stochastic.
- **Assuming Symmetry ($P_{ij} = P_{ji}$):** Transition probabilities are directed. The probability of transitioning from rain to sun is rarely equal to the probability of transitioning from sun to rain.
- **Prematurely Declaring a Non-Markov Process Impossible:** Forgetting that non-Markovian processes with finite historical dependence can be converted into Markov chains via state augmentation.
- **Misapplying the Markov Property:** Forgetting that conditioning on the *present* state is required to separate past and future. Unconditioned, $X_{n+1}$ and $X_{n-1}$ are usually dependent.

---

## Exam Relevance

### 1. Two-State Weather Model
Suppose the chance of rain tomorrow depends solely on whether it rains today:
- If it rains today, it rains tomorrow with probability $\alpha$, and clears up with probability $1 - \alpha$.
- If it does not rain today, it rains tomorrow with probability $\beta$, and stays dry with probability $1 - \beta$.

Let State 0 = "Rain", State 1 = "No Rain".
The transition probability matrix is:
$$P = \begin{pmatrix}
\alpha & 1 - \alpha \\
\beta & 1 - \beta
\end{pmatrix}$$

### 2. Binary Communications Channel
A transmitter sends binary digits ($0$ or $1$) through successive transmission stages. At each stage, the bit survives intact with probability $p$, and flips with probability $1 - p$:
$$P = \begin{pmatrix}
p & 1 - p \\
1 - p & p
\end{pmatrix}$$

---
### Exam Relevance

In CSE301 examinations:
1. **Transition Matrix Formulation:** Constructing $P$ from narrative problem descriptions (e.g., weather models, genetic processes, customer brand switching).
2. **Validating Stochastic Matrices:** Verifying non-negativity and row-sum normalization.
3. **Joint Path Probability Calculations:** Computing $P(X_0 = i_0, X_1 = i_1, \dots, X_n = i_n)$ by multiplying transition entries.
4. **Higher-Order State Expansion:** Converting 2-day or multi-day weather dependencies into a valid first-order transition matrix.

---

## What to carry forward

[[Chapman-Kolmogorov Equations]] explains matrix multiplication as summing over possible intermediate states. Keep row/column conventions consistent throughout a calculation.

## Related notes

- [[Chapman-Kolmogorov Equations]]

## Sources

- [[01 - Sources/Lectures/Markov_Chain.pdf]] (Slides 2–7)
- [[01 - Sources/Textbooks/Sheldon M. Ross book Markov Chain Chapter.pdf]] (Section 4.1, pp. 193–197)
