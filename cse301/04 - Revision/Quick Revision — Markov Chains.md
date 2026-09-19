# Quick Revision — Markov Chains

> A high-yield, exam-focused reference sheet summarizing definitions, core theorems, formulas, and solving procedures for Discrete-Time Markov Chains.

---

## 1. Core Definitions & Matrix Properties

| Term | Mathematical Definition | Key Condition / Notes |
|---|---|---|
| **Markov Property** | $P(X_{n+1}=j \mid X_n=i, \dots, X_0=i_0) = P_{ij}$ | Future is conditionally independent of past given present. |
| **Transition Matrix ($P$)** | $P = [P_{ij}]$, where $P_{ij} = P(X_1 = j \mid X_0 = i)$ | Row-stochastic: $P_{ij} \ge 0$ and $\sum_j P_{ij} = 1$. |
| **$n$-Step Probability** | $P_{ij}^n = P(X_n = j \mid X_0 = i)$ | $P^{(n)} = P^n$ (Matrix power). |
| **Chapman-Kolmogorov** | $P_{ij}^{n+m} = \sum_k P_{ik}^n P_{kj}^m$ | Matrix form: $P^{(n+m)} = P^{(n)} P^{(m)}$. |

---

## 2. Classification of States

- **Accessible ($i \to j$):** $\exists n \ge 0$ such that $P_{ij}^n > 0$.
- **Communicate ($i \leftrightarrow j$):** $i \to j$ and $j \to i$. (Equivalence relation: partitions $S$ into disjoint classes).
- **Irreducible:** Entire state space forms a single communicating class.
- **Absorbing State:** $P_{ii} = 1$ (cannot be left once entered).
- **Period ($d(i)$):** $d(i) = \gcd\{n \ge 1 : P_{ii}^n > 0\}$. If $d=1$, state is **aperiodic**. If $P_{ii} > 0$ (self-loop), state is immediately aperiodic ($d=1$). Periodicity is a class property.
- **Recurrent:** Return probability $f_i = 1 \iff \sum_{n=1}^\infty P_{ii}^n = \infty$.
- **Transient:** Return probability $f_i < 1 \iff \sum_{n=1}^\infty P_{ii}^n < \infty$.
- **Finite State Rule:** In any finite-state Markov chain, not all states can be transient; at least one recurrent state must exist.

---

## 3. Stationary vs. Limiting Distributions

### Balance Equations
$$\pi P = \pi \quad \iff \quad \pi_j = \sum_{i} \pi_i P_{ij}, \quad \forall j$$
$$\sum_{j} \pi_j = 1, \quad \pi_j \ge 0$$

### Key Distinction
- **Stationary Distribution ($\pi$):** Exists for any finite irreducible chain. Represents long-run fraction of time spent in each state.
- **Limiting Distribution ($\lim_{n\to\infty} P_{ij}^n = \pi_j$):** Requires the chain to be **irreducible AND aperiodic**.
- **Mean Recurrence Time:** $\mu_{jj} = \frac{1}{\pi_j}$.

### $2 \times 2$ Shortcut
For $P = \begin{pmatrix} \alpha & 1-\alpha \\ \beta & 1-\beta \end{pmatrix}$:
$$\pi_0 = \frac{\beta}{1 + \beta - \alpha}, \qquad \pi_1 = \frac{1 - \alpha}{1 + \beta - \alpha}$$

---

## 4. Gambler's Ruin Summary

$$P_i = \begin{cases}
\dfrac{1 - (q/p)^i}{1 - (q/p)^N}, & p \neq \frac{1}{2} \\[10pt]
\dfrac{i}{N}, & p = \frac{1}{2}
\end{cases}$$

### Ruin Probability
$$Q_i = 1 - P_i$$

### Infinite Adversary ($N \to \infty$)
$$\lim_{N \to \infty} P_i = \begin{cases}
1 - (q/p)^i, & p > \frac{1}{2} \\
0, & p \le \frac{1}{2}
\end{cases}$$

---

## 5. Quick Exam Decision Checklist

1. **Need $P^4$?** Compute $P^2 = P \cdot P$, then $P^4 = P^2 \cdot P^2$.
2. **Need $P(\text{event at step } n)$ given start state $i$?** Compute $\text{Row}_i(P^n)$ and sum entries for all states matching the event.
3. **Solving for $\pi$?** Write $\pi P = \pi$, cross out one redundant equation, and substitute $\sum \pi_j = 1$.
4. **Is state aperiodic?** Look for any self-loop ($P_{ii} > 0$) in that communicating class.
5. **Gambler's Ruin?** Check if $p = 0.5$. If yes, $P_i = i/N$. If no, calculate $q/p = (1-p)/p$ and use the geometric ratio formula.

---

## Related Notes
- [[Markov Chain]]
- [[Classification of States in Markov Chains]]
- [[Stationary and Limiting Distributions in Markov Chains]]
- [[Chapman-Kolmogorov Equations]]
- [[Gambler's Ruin Formula]]
