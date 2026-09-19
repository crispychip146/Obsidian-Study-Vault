# Markov Chains — Exam Intelligence

## Overview

Discrete-Time Markov Chains (DTMC) form the core of Topic 10 (Stochastic Processes) in **CSE301: Mathematics for Computing and Data Science**. In university examinations, this topic combines analytical proofs, matrix algebra, and applied word problems.

---

## Core Question Patterns

### Pattern 1: Multi-Step Transition Probability Calculation
- **Problem Archetype:** Given a transition matrix $P$ (usually $2 \times 2$ or $3 \times 3$) and an initial state $i$, find $P_{ij}^n$ for $n \in \{2, 3, 4\}$.
- **Exam Technique:**
  - Use repeated squaring ($P^4 = (P^2)^2$) rather than sequential multiplication ($P \cdot P \cdot P \cdot P$).
  - If only a specific row or entry is requested (e.g., $P_{0j}^2$), compute only the vector-matrix product $\text{Row}_0(P) \cdot P$ rather than the full $N \times N$ product matrix.
- **Representative Problems:**
  - `[[Problem — Four-Day Weather Forecast]]`
  - `[[Problem — Rain Prediction Two Days Ahead]]`

### Pattern 2: State Space Decomposition & Classification
- **Problem Archetype:** Given an explicit transition matrix $P$:
  1. Determine accessibility between pairs of states.
  2. Partition the state space into communicating classes.
  3. Label each class as recurrent or transient.
  4. Identify absorbing states ($P_{ii} = 1$).
  5. State whether the chain is irreducible.
  6. Compute the period of states.
- **Exam Technique:**
  - Always draw the directed state transition graph first.
  - Check for self-loops ($P_{ii} > 0$): any state with a self-loop is aperiodic ($d=1$), and by class property, its entire class is aperiodic.
  - To prove accessibility $i \to j$ when $P_{ij} = 0$, explicitly write out a valid intermediate path (e.g., $P_{ik} P_{kj} > 0$).
- **Representative Problems:**
  - `[[Problem — State Communication and Irreducibility Verification]]`
  - `[[Problem — Identification of Communicating Classes and Absorbing States]]`

### Pattern 3: Stationary Distribution & Long-Run Proportions
- **Problem Archetype:** Find the stationary distribution $\pi$ and compute the long-run proportion of time spent in a specified state, or find the mean return time $\mu_{jj} = 1/\pi_j$.
- **Exam Technique:**
  - Write down $\pi P = \pi$ in scalar form.
  - **Crucial Step:** Drop one redundant balance equation and immediately substitute $\sum_j \pi_j = 1$.
  - For a $2 \times 2$ matrix $P = \begin{pmatrix} \alpha & 1-\alpha \\ \beta & 1-\beta \end{pmatrix}$, remember the quick check: $\pi_0 = \frac{\beta}{1 + \beta - \alpha}$.
- **Representative Problems & Examples:**
  - `[[Weather Forecasting Markov Chain Example]]`
  - `[[Hardy-Weinberg Law Markov Chain Example]]`

### Pattern 4: Gambler's Ruin & Absorption Probabilities
- **Problem Archetype:** A random walk with absorbing barriers at $0$ and $N$ with win probability $p$ and loss probability $q = 1-p$.
- **Exam Technique:**
  - Identify initial wealth $i$, target wealth $N$ (total bankroll of both players, not just the opponent's wealth!), and ratio $q/p$.
  - If $p = 0.5$, use $P_i = i/N$. If $p \neq 0.5$, use $P_i = \frac{1 - (q/p)^i}{1 - (q/p)^N}$.
  - For infinite adversary problems ($N \to \infty$), remember: $\lim_{N \to \infty} P_i = 0$ if $p \le 1/2$, and $1 - (q/p)^i$ if $p > 1/2$.
- **Representative Problems:**
  - `[[Problem — Patty and Max Gambler's Ruin]]`

### Pattern 5: State Augmentation (Non-Markovian Memory)
- **Problem Archetype:** Word problem where future state depends on the past 2 or 3 steps.
- **Exam Technique:**
  - Define composite states as $k$-tuples: $S = \{(R, R), (R, NR), (NR, R), (NR, NR)\}$.
  - Eliminate invalid transitions (any transition where the second element of the old state does not match the first element of the new state has probability 0).
  - Sum probabilities of all terminal states that satisfy the requested event.
- **Representative Example:**
  - `[[Higher-Order State Weather Prediction Example]]`

---

## High-Yield Theoretical Derivations

Questions in CSE301 frequently require full mathematical derivations:
1. **Derivation of Chapman-Kolmogorov Equations:**
   - Partitioning by intermediate state $X_n = k$ via Law of Total Probability.
   - Dropping $X_0 = i$ via the Markov property.
   - Using time-homogeneity to arrive at $\sum_k P_{ik}^n P_{kj}^m$.
   - Full derivation in `[[Chapman-Kolmogorov Equations]]`.
2. **Derivation of Gambler's Ruin Formula:**
   - Setting up $P_i = p P_{i+1} + q P_{i-1}$ using first-step conditioning.
   - Writing $p(P_{i+1} - P_i) = q(P_i - P_{i-1})$.
   - Telescoping geometric differences and summing the finite geometric progression.
   - Full derivation in `[[Gambler's Ruin Formula]]`.
3. **Equivalence Relation Proof of Communication:**
   - Proving reflexivity ($P_{ii}^0 = 1$), symmetry, and transitivity (via Chapman-Kolmogorov).
   - Full proof in `[[Classification of States in Markov Chains]]`.
4. **Hardy-Weinberg Invariance Proof:**
   - Showing allele frequency $P(A) = p + r/2$ is preserved after random mating.
   - Full proof in `[[Hardy-Weinberg Law Markov Chain Example]]`.

---

## Common Exam Traps & Pitfalls

1. **Stationary vs Limiting Distribution:** Stating that limiting probabilities exist for periodic chains. A periodic chain has stationary distribution and long-run time averages $\pi$, but $\lim_{n \to \infty} P_{ij}^n$ does NOT exist!
2. **Solving $\pi(P - I) = 0$ without $\sum \pi_j = 1$:** Yields only the trivial zero solution $\pi = 0$. Always replace one equation with the normalization condition.
3. **Inverting $q/p$ in Gambler's Ruin:** Putting win probability $p$ in numerator instead of loss probability $q$.
4. **Row vs Column Vector Orientation:** $\pi$ must be multiplied from the left as a row vector ($\pi P = \pi$), not from the right ($P \pi = \pi$).
