# CSE301 — Topic Map

This map organizes the topics covered in CSE301 (Mathematics for Computing and Data Science), linking syllabus areas to knowledge notes.

---

## 10. Stochastic Processes

### 10.1 Foundations of Stochastic Processes
- **Core Concept:** [[Stochastic Process]]
  - Time parameter: Discrete ($T = \{0, 1, \dots\}$) vs. Continuous ($T = [0, \infty)$)
  - State space: Discrete ($S \subseteq \mathbb{Z}$) vs. Continuous ($S \subseteq \mathbb{R}$)
  - Trajectories and sample paths

### 10.2 Discrete-Time Markov Chains
- **Core Concept:** [[Markov Chain]]
  - The Markov Property (Memorylessness)
  - Transition Probability Matrix (Stochastic Matrix)
  - Time-homogeneity
  - State space augmentation (modeling higher-order history)

### 10.3 State Space Analysis & Classification
- **Core Concept:** [[Classification of States in Markov Chains]]
  - Accessibility ($i \to j$)
  - Mutual Communication ($i \leftrightarrow j$) as an Equivalence Relation
  - Communicating Classes
  - Irreducibility (Single class vs. Reducible chains)
  - Absorbing States ($P_{ii} = 1$)
  - Periodicity ($d(i) = \gcd \{n \ge 1 : P_{ii}^n > 0\}$) and Aperiodicity ($d = 1$)
  - Recurrence ($\sum P_{ii}^n = \infty$) vs. Transience ($\sum P_{ii}^n < \infty$)

### 10.4 Multi-Step Transitions & Matrix Powers
- **Formula:** [[Chapman-Kolmogorov Equations]]
  - Scalar form: $P_{ij}^{n+m} = \sum_k P_{ik}^n P_{kj}^m$
  - Matrix form: $P^{(n)} = P^n$
  - Repeated squaring for multi-step forecasts

### 10.5 Long-Run Equilibrium & Steady-State Distributions
- **Core Concept:** [[Stationary and Limiting Distributions in Markov Chains]]
  - Global Balance Equations: $\pi_j = \sum_i \pi_i P_{ij} \iff \pi P = \pi$
  - Normalization constraint: $\sum \pi_j = 1$
  - Limiting distribution: $\pi_j = \lim_{n\to\infty} P_{ij}^n$
  - Conditions for convergence: Irreducible + Aperiodic (Ergodic theorem)
  - Mean return time: $\mu_{jj} = 1/\pi_j$
  - Periodic counterexample (Stationary distribution exists without limiting distribution)

### 10.6 Random Walks & Absorption Problems
- **Formula:** [[Gambler's Ruin Formula]]
  - Difference equation recursion: $P_i = p P_{i+1} + q P_{i-1}$
  - Telescoping geometric progression of differences
  - Win probability: $P_i = \frac{1 - (q/p)^i}{1 - (q/p)^N}$ for $p \neq 1/2$; $P_i = i/N$ for $p = 1/2$
  - Asymptotic behavior against infinite bankroll ($N \to \infty$)

---

## Worked Examples & Applications
- [[Weather Forecasting Markov Chain Example]] (2-state weather forecasting, $P^4$, long-run proportions)
- [[Higher-Order State Weather Prediction Example]] (Augmented 4-state Markov chain for 2-day historical dependence)
- [[Hardy-Weinberg Law Markov Chain Example]] (Population genetics allele invariance, descendant lineage Markov chain, stationary distribution verification)

---

## Practice & Exam Problems
- [[Problem — Patty and Max Gambler's Ruin]] (Biased random walk, odds ratio evaluation, probability of ruin)
- [[Problem — Four-Day Weather Forecast]] (Two-step and four-step transition matrix powers, convergence comparison)
- [[Problem — Rain Prediction Two Days Ahead]] (Row dot product, composite destination state summation)
- [[Problem — State Communication and Irreducibility Verification]] (Proving reachability via 2-step paths, irreducibility, aperiodicity via self-loops)
- [[Problem — Identification of Communicating Classes and Absorbing States]] (Equivalence class decomposition, transient states, absorbing states)

---

## Revision & Intelligence
- [[03 - Maps/Exam Intelligence/Markov Chains Exam Intelligence|Markov Chains Exam Intelligence]]
- [[04 - Revision/Quick Revision — Markov Chains|Quick Revision — Markov Chains]]
