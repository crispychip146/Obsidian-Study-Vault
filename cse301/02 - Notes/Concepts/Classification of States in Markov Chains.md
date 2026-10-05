---
type: concept
course: cse301
status: active
order: 70
---

# Classification of States in Markov Chains

> 📖 **Reading Order:** Step 70 of 92 | **Module 10:** Stochastic Processes  
> ◄ **Previous:** [[Markov Chain]] | ► **Next:** [[Stationary and Limiting Distributions in Markov Chains]]

---

## Building the idea

Think of positive-probability transitions as directed edges. State $j$ is accessible from $i$ if some finite path leads from $i$ to $j$. Communication requires paths in both directions and partitions the state space into communicating classes.

A class is closed when no transition can leave it. An absorbing state is the special case of a closed single state with $P_{ii}=1$. A recurrent state is returned to with probability one after departure; a transient state has a positive chance of never returning. In a finite chain, closed irreducible classes are recurrent, while states outside all closed classes are transient.

Period is the greatest common divisor of possible positive return times. A self-loop makes the period one, but is not necessary for aperiodicity. All states in an irreducible class share a period, so classify the class structure before studying limiting probabilities.

## Definition

In a [[Markov Chain]], states can be classified according to their reachability, mutual connectivity, recurrence behavior, and temporal periodicity.

### 1. Accessibility
A state $j$ is **accessible** from state $i$ (written $i \to j$) if there exists some integer $n \ge 0$ such that:
$$P_{ij}^n > 0$$
In words: starting from state $i$, there is a strictly positive probability that the chain will enter state $j$ at or after step $n$.

### 2. Communication
Two states $i$ and $j$ **communicate** (written $i \leftrightarrow j$) if:
$$i \to j \quad \text{and} \quad j \to i$$
That is, each state is accessible from the other.

### 3. Communicating Classes
The communication relation $\leftrightarrow$ is an **equivalence relation**:
1. **Reflexive:** $i \leftrightarrow i$, since $P_{ii}^0 = P(X_0 = i \mid X_0 = i) = 1 > 0$.
2. **Symmetric:** If $i \leftrightarrow j$, then $j \leftrightarrow i$ by definition.
3. **Transitive:** If $i \leftrightarrow j$ and $j \leftrightarrow k$, then $i \leftrightarrow k$.
   - *Proof:* There exist $n, m \ge 0$ such that $P_{ij}^n > 0$ and $P_{jk}^m > 0$. By the [[Chapman-Kolmogorov Equations]]:
     $$P_{ik}^{n+m} = \sum_{r=0}^\infty P_{ir}^n P_{rk}^m \ge P_{ij}^n P_{jk}^m > 0$$
     Hence $i \to k$. By the same logic in reverse, $k \to i$. Thus $i \leftrightarrow k$.

Because $\leftrightarrow$ is an equivalence relation, it partitions the state space $S$ into disjoint subsets called **communicating classes**. Two states belong to the same class if and only if they communicate.

### 4. Irreducibility
A Markov chain is **irreducible** if the entire state space consists of a single communicating class—that is, every state communicates with every other state ($i \leftrightarrow j$ for all $i, j \in S$). Otherwise, the chain is **reducible**.

### 5. Absorbing State
A state $i$ is **absorbing** if:
$$P_{ii} = 1$$
Once the process enters an absorbing state, it can never leave ($P_{ij} = 0$ for all $j \neq i$). An absorbing state forms a singleton communicating class $\{i\}$ from which no other state is accessible.

### 6. Periodicity
The **period** $d(i)$ of a state $i$ is defined as the greatest common divisor ($\gcd$) of all time horizons $n \ge 1$ for which return to $i$ is possible:
$$d(i) = \gcd \{n \ge 1 : P_{ii}^n > 0\}$$
- If $P_{ii}^n = 0$ for all $n \ge 1$, we define $d(i) = \infty$.
- If $d(i) = 1$, state $i$ is called **aperiodic**.
- If $d(i) > 1$, state $i$ is called **periodic** with period $d$.
- **Class Property:** Periodicity is a class property. If $i \leftrightarrow j$, then $d(i) = d(j)$.

### 7. Recurrence and Transience
Let $f_i$ denote the probability that, starting in state $i$, the process will ever re-enter state $i$:
$$f_i = P(\text{process ever returns to state } i \mid X_0 = i) = \sum_{n=1}^\infty f_{ii}^n$$
- State $i$ is **recurrent** if $f_i = 1$. (The process returns infinitely many times with probability 1).
  $$\text{Recurrent} \iff \sum_{n=1}^\infty P_{ii}^n = \infty$$
- State $i$ is **transient** if $f_i < 1$. (The process eventually leaves state $i$ never to return).
  $$\text{Transient} \iff \sum_{n=1}^\infty P_{ii}^n < \infty$$
- **Class Property:** Recurrence and transience are class properties. If $i \leftrightarrow j$, then either both are recurrent or both are transient.

---

## How It Works

### Step-by-Step Procedure to Classify States

1. **Draw the State Transition Graph:**
   - Draw a node for each state.
   - Draw a directed arrow from $i$ to $j$ if and only if $P_{ij} > 0$.
2. **Identify Reachability and Cycles:**
   - Check which nodes can reach which other nodes.
   - Group mutually reachable nodes into strongly connected components—these are the **communicating classes**.
3. **Check Irreducibility:**
   - If there is only 1 class containing all states, the chain is **irreducible**.
   - If there are 2 or more classes, the chain is **reducible**.
4. **Identify Absorbing and Open/Closed Classes:**
   - If a class has no outgoing arrows to any state outside itself, it is a **closed class**. Once entered, the process never leaves.
   - If a closed class consists of a single state ($P_{ii} = 1$), it is an **absorbing state**.
   - If a class has arrows pointing to other classes, it is **transient** (in a finite chain).
5. **Compute the Period ($d$):**
   - For each class, find cycle lengths from a representative state $i$ back to $i$.
   - Compute $\gcd$ of the cycle lengths.
   - If the state has a self-loop ($P_{ii} > 0$), then $d(i) = \gcd(1, \dots) = 1$, immediately proving the class is **aperiodic**.

---
### Important Properties

| Property | Condition | Key Consequence |
|---|---|---|
| **Accessible ($i \to j$)** | $\exists n \ge 0 : P_{ij}^n > 0$ | Reachable in $n$ steps |
| **Communicate ($i \leftrightarrow j$)** | $i \to j$ and $j \to i$ | Symmetric two-way connectivity |
| **Irreducible** | Single communicating class | Process explores all states |
| **Absorbing** | $P_{ii} = 1$ | Trap state; cannot escape |
| **Aperiodic** | $\gcd\{n : P_{ii}^n > 0\} = 1$ | Required for limiting probabilities |
| **Recurrent** | $P(\text{return}) = 1$ | Visited infinitely many times |
| **Transient** | $P(\text{return}) < 1$ | Visited only finitely many times |

---

## Technical Details

### Fundamental Theorems on Finite State Spaces
1. **At Least One Recurrent State:** In any finite-state Markov chain, not all states can be transient. At least one state (and thus at least one closed communicating class) must be recurrent.
2. **Transience of Non-Closed Classes:** In a finite-state Markov chain, any communicating class from which other states are accessible must be transient.
3. **Class Properties:**
   - Accessibility and Communication ($\leftrightarrow$)
   - Periodicity ($d$)
   - Recurrence / Transience ($f_i = 1$ vs $f_i < 1$)

---

## Common Mistakes

- **Assuming $P_{ij} = 0 \implies j$ is not accessible from $i$:** Forgetting that accessibility depends on $P_{ij}^n > 0$ for *some* $n \ge 1$ (multi-step path), not just direct one-step transitions.
- **Confusing Closed Classes with Absorbing States:** An absorbing state is a *single* state with $P_{ii} = 1$. A closed class can have multiple communicating states (e.g., $\{0, 1\}$ with transitions between each other, but no transitions escaping the set).
- **Calculating Period as Minimum Step Count Instead of GCD:** The period is the greatest common divisor of *all* return path lengths, not the shortest cycle length.
- **Assuming Reducible Chains Cannot Have Stationary Distributions:** Reducible chains can have stationary distributions, but they are generally not unique and depend on the initial state distribution.

---

## Exam Relevance

### Example 1: Verifying Irreducibility (3 States)
$$P = \begin{pmatrix}
1/2 & 1/2 & 0 \\
1/2 & 1/4 & 1/4 \\
0 & 1/3 & 2/3
\end{pmatrix}$$
- Can $0$ reach $2$? Direct transition $P_{02} = 0$, but path $0 \to 1 \to 2$ has probability:
  $$P_{01} P_{12} = \left(\frac{1}{2}\right)\left(\frac{1}{4}\right) = \frac{1}{8} > 0 \implies 0 \to 2$$
- Can $2$ reach $0$? Path $2 \to 1 \to 0$ has probability:
  $$P_{21} P_{10} = \left(\frac{1}{3}\right)\left(\frac{1}{2}\right) = \frac{1}{6} > 0 \implies 2 \to 0$$
- States $0, 1, 2$ all communicate mutually.
- **Conclusion:** There is exactly 1 communicating class $\{0, 1, 2\}$, so the chain is **irreducible**.
- Because $P_{00} = 1/2 > 0$, state 0 is aperiodic ($d=1$). By the class property, all states in the chain are **aperiodic**.

### Example 2: Decomposing a Reducible Chain (4 States)
$$P = \begin{pmatrix}
1/2 & 1/2 & 0 & 0 \\
1/2 & 1/2 & 0 & 0 \\
1/4 & 1/4 & 1/4 & 1/4 \\
0 & 0 & 0 & 1
\end{pmatrix}$$
- States $0$ and $1$ communicate ($0 \leftrightarrow 1$), but cannot reach $2$ or $3$. Class $C_1 = \{0, 1\}$ is closed.
- State $2$ can reach $0, 1, 3$, but no state can reach $2$. Class $C_2 = \{2\}$ is transient.
- State $3$ has $P_{33} = 1$. Class $C_3 = \{3\}$ is an **absorbing state**.
- **Classes:** $\{0, 1\}$, $\{2\}$, and $\{3\}$.

---
### Exam Relevance

In CSE301 examinations:
- Identifying all communicating classes from a given transition matrix $P$.
- Proving transitivity of communication using the Chapman-Kolmogorov inequality.
- Determining whether a given chain is irreducible.
- Identifying transient, recurrent, and absorbing states.
- Calculating the period of states and proving aperiodicity via self-loops ($P_{ii} > 0$).

---

## What to carry forward

[[Problem — Identification of Communicating Classes and Absorbing States]] separates reaching a state from communicating with it. Infinite chains require care: recurrence need not imply finite expected return time.

## Related notes

- [[Problem — Identification of Communicating Classes and Absorbing States]]

## Sources

- [[01 - Sources/Lectures/Markov_Chain.pdf]] (Slides 11–15, 23–24)
- [[01 - Sources/Textbooks/Sheldon M. Ross book Markov Chain Chapter.pdf]] (Section 4.3, pp. 202–211)
