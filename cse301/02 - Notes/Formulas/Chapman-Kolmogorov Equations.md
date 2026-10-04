---
type: formula
course: cse301
status: active
order: 72
---

# Chapman-Kolmogorov Equations

> 📖 **Reading Order:** Step 72 of 92 | **Module 10:** Stochastic Processes  
> ◄ **Previous:** [[Stationary and Limiting Distributions in Markov Chains]] | ► **Next:** [[Gambler's Ruin Formula]]

---

---

## The Question and Earlier Knowledge

What analytical relationship or closed-form expectation governs Chapman-Kolmogorov Equations, and how can we compute it directly from constituent probability terms? In complex probability models, calculating probabilities or moments directly is often intractable without decomposing expectations across conditioning partitions or inequalities.

---

## Developing the Formula

By decomposing joint distributions into conditional components, expanding algebraic products, or applying geometric series sums, Chapman-Kolmogorov Equations compresses complex probabilistic reasoning into a clean, reusable formula.

---

## Formula

### Scalar Form
For any discrete-time [[Markov Chain]] and any non-negative integers $n, m \ge 0$, and states $i, j \in S$:

$$P_{ij}^{n+m} = \sum_{k=0}^\infty P_{ik}^n P_{kj}^m$$

where $P_{ij}^n = P(X_{n+k} = j \mid X_k = i)$ is the $n$-step transition probability from state $i$ to state $j$.

### Matrix Form
Let $P^{(n)}$ denote the matrix whose $(i, j)$-th entry is $P_{ij}^n$. Then:

$$P^{(n+m)} = P^{(n)} \cdot P^{(m)}$$

where $\cdot$ denotes standard matrix multiplication.

By mathematical induction, this implies:

$$P^{(n)} = P^n = \underbrace{P \cdot P \cdots P}_{n \text{ times}}$$

---

---

## Variables

| Symbol | Meaning |
|---|---|
| $P_{ij}^{n+m}$ | Probability of transitioning from state $i$ to state $j$ in exactly $n+m$ steps |
| $P_{ik}^n$ | Probability of transitioning from state $i$ to intermediate state $k$ in $n$ steps |
| $P_{kj}^m$ | Probability of transitioning from intermediate state $k$ to state $j$ in $m$ steps |
| $k$ | Intermediate state at step $n$ (summed over the entire state space $S$) |
| $P^{(n)}$ | $n$-step transition probability matrix |
| $P$ | One-step transition probability matrix $P = P^{(1)}$ |

---

---

## Conditions

1. **Discrete-Time Markov Process:** The underlying process $\{X_n, n \ge 0\}$ must satisfy the Markov property:
   $$P(X_{n+1} = j \mid X_n = i, X_{n-1} = i_{n-1}, \dots, X_0 = i_0) = P(X_{n+1} = j \mid X_n = i)$$
2. **Time Homogeneity:** The transition probabilities $P_{ij}$ are stationary (do not depend on the absolute time index $k$):
   $$P(X_{n+k} = j \mid X_k = i) = P(X_n = j \mid X_0 = i)$$
3. **Valid State Space:** The intermediate states $k$ must form a partition of the state space $S$.

---

---

## Intuition

### Intuition

To travel from city $i$ to city $j$ in $n+m$ days, you must be in *some* city $k$ at day $n$.

Because the process is Markovian, once you reach city $k$ at day $n$, your past journey from city $i$ is completely forgotten. The probability of completing the remaining journey from $k$ to $j$ in the next $m$ days depends solely on being at $k$, not on the fact that you started at $i$.

Therefore, the total probability of ending at $j$ is obtained by:
1. Multiplying the probability of reaching waypoint $k$ in $n$ steps ($P_{ik}^n$) by the probability of going from $k$ to $j$ in $m$ steps ($P_{kj}^m$).
2. Summing these disjoint path probabilities over all conceivable waypoints $k$.

In matrix terms, computing transition probabilities over multiple time steps is identical to standard matrix multiplication.

---

---

## Derivation

### Derivation

Let $X_0 = i$. We want to compute $P(X_{n+m} = j \mid X_0 = i)$.

### Step 1: Partitioning by Intermediate States (Law of Total Probability)
Condition on the state of the system at the intermediate time step $n$:
$$P_{ij}^{n+m} = P(X_{n+m} = j \mid X_0 = i) = \sum_{k=0}^\infty P(X_{n+m} = j, X_n = k \mid X_0 = i)$$

### Step 2: Applying the Definition of Conditional Probability
Using $P(A \cap B \mid C) = P(A \mid B \cap C) P(B \mid C)$:
$$P_{ij}^{n+m} = \sum_{k=0}^\infty P(X_{n+m} = j \mid X_n = k, X_0 = i) \cdot P(X_n = k \mid X_0 = i)$$

### Step 3: Invoking the Markov Property
By the Markov property, given the present state $X_n = k$, the future state $X_{n+m} = j$ is conditionally independent of the initial state $X_0 = i$:
$$P(X_{n+m} = j \mid X_n = k, X_0 = i) = P(X_{n+m} = j \mid X_n = k)$$

### Step 4: Using Time Homogeneity
Due to time homogeneity, the transition from time $n$ to $n+m$ depends only on the elapsed time $m$:
$$P(X_{n+m} = j \mid X_n = k) = P_{kj}^m$$
and by definition:
$$P(X_n = k \mid X_0 = i) = P_{ik}^n$$

### Step 5: Substitution
Substituting these two factors back into the summation yields:
$$P_{ij}^{n+m} = \sum_{k=0}^\infty P_{ik}^n P_{kj}^m \quad \blacksquare$$

---

---

## Example

### Example

Consider the two-state weather chain:
$$P = \begin{pmatrix} 0.7 & 0.3 \\ 0.4 & 0.6 \end{pmatrix}$$
where State 0 = Rain, State 1 = No Rain.

Find the probability of rain 4 days from now given rain today ($P_{00}^4$).

### Step 1: Compute $P^{(2)} = P^2$
$$P^{(2)} = \begin{pmatrix} 0.7 & 0.3 \\ 0.4 & 0.6 \end{pmatrix} \begin{pmatrix} 0.7 & 0.3 \\ 0.4 & 0.6 \end{pmatrix} = \begin{pmatrix} (0.7)(0.7) + (0.3)(0.4) & (0.7)(0.3) + (0.3)(0.6) \\ (0.4)(0.7) + (0.6)(0.4) & (0.4)(0.3) + (0.6)(0.6) \end{pmatrix}$$
$$P^{(2)} = \begin{pmatrix} 0.49 + 0.12 & 0.21 + 0.18 \\ 0.28 + 0.24 & 0.12 + 0.36 \end{pmatrix} = \begin{pmatrix} 0.61 & 0.39 \\ 0.52 & 0.48 \end{pmatrix}$$

### Step 2: Compute $P^{(4)} = (P^{(2)})^2 = P^4$
$$P^{(4)} = \begin{pmatrix} 0.61 & 0.39 \\ 0.52 & 0.48 \end{pmatrix} \begin{pmatrix} 0.61 & 0.39 \\ 0.52 & 0.48 \end{pmatrix}$$
$$P_{00}^4 = (0.61)(0.61) + (0.39)(0.52) = 0.3721 + 0.2028 = 0.5749$$
$$P_{01}^4 = (0.61)(0.39) + (0.39)(0.48) = 0.2379 + 0.1872 = 0.4251$$
$$P_{10}^4 = (0.52)(0.61) + (0.48)(0.52) = 0.3172 + 0.2496 = 0.5668$$
$$P_{11}^4 = (0.52)(0.39) + (0.48)(0.48) = 0.2028 + 0.2304 = 0.4332$$

$$P^{(4)} = \begin{pmatrix} 0.5749 & 0.4251 \\ 0.5668 & 0.4332 \end{pmatrix}$$

Thus, $P_{00}^4 = 0.5749$.

---

---

## Common Mistakes

### Common Mistakes

- **Element-wise Exponentiation:** Raising individual matrix entries to the power $n$ (i.e., $(P_{ij})^n$) instead of performing matrix multiplication $P^n$.
- **Summing over Wrong Index:** Summing over destination states $j$ instead of intermediate waypoints $k$.
- **Transposing Matrix Multiplication Order:** In general, $A B \neq B A$. While $P^n P^m = P^m P^n = P^{n+m}$ holds for powers of the same matrix, when multiplying initial probability row vectors $\alpha$, one must compute $\alpha P^n$ (row times matrix), not $P^n \alpha$.
- **Dropping the Conditioning Prematurely:** Forgetting to justify the removal of $X_0 = i$ in the third line of the derivation via the Markov property.

---

---

## Related Concepts

- [[Markov Chain]]
- [[Classification of States in Markov Chains]]
- [[Stationary and Limiting Distributions in Markov Chains]]

---

---

## Prerequisites

- [[Markov Chain]]
- [[Conditional Probability and Independence|Conditional Probability]]

---

---

## Problems

- [[Problem — Four-Day Weather Forecast]]
- [[Problem — Rain Prediction Two Days Ahead]]
- [[Problem — State Communication and Irreducibility Verification]]

---

---

## Sources

- [[01 - Sources/Lectures/Markov_Chain.pdf]] (Slides 7–10)
- [[01 - Sources/Textbooks/Sheldon M. Ross book Markov Chain Chapter.pdf]] (Section 4.2, pp. 197–202)
