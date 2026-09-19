# CSE301 — Dependency Map

This map outlines conceptual prerequisites, core dependencies, and learning pathways for CSE301 topics.

---

## Visual Dependency Graph

```mermaid
flowchart TD
    %% Foundational Probability
    RV["Random Variable"]
    CP["Conditional Probability"]
    
    %% Foundations of Stochastic Processes
    SP["Stochastic Process"]
    RV --> SP
    CP --> SP
    
    %% Core Markov Chain Concepts
    MC["Markov Chain"]
    SP --> MC
    CP --> MC
    
    %% Multi-step & Classification
    CK["Chapman-Kolmogorov Equations"]
    MC --> CK
    
    CS["Classification of States in Markov Chains"]
    MC --> CS
    CK -.->|Transitivity Proof| CS
    
    %% Long-Run Behavior
    SLD["Stationary & Limiting Distributions"]
    CS --> SLD
    MC --> SLD
    
    %% Applications & Formulas
    GRF["Gambler's Ruin Formula"]
    MC --> GRF
    CS -.->|Absorbing & Transient| GRF
    
    %% Examples
    WF["Weather Forecasting Example"]
    MC --> WF
    CK --> WF
    SLD --> WF
    
    HOS["Higher-Order State Weather Prediction"]
    MC --> HOS
    CK --> HOS
    
    HW["Hardy-Weinberg Law Example"]
    MC --> HW
    SLD --> HW
    
    %% Problems
    P_GR["Problem: Patty & Max Gambler's Ruin"]
    GRF --> P_GR
    
    P_WF["Problem: Four-Day Weather Forecast"]
    CK --> P_WF
    WF --> P_WF
    
    P_RP["Problem: Rain Prediction Two Days Ahead"]
    HOS --> P_RP
    CK --> P_RP
    
    P_SC["Problem: State Communication & Irreducibility"]
    CS --> P_SC
    
    P_CC["Problem: Communicating Classes & Absorbing States"]
    CS --> P_CC
```

---

## Detailed Prerequisite Chains

### 1. Markov Chains Pathway
1. `[[Random Variable]]` + `[[Conditional Probability]]`
   $$\downarrow$$
2. `[[Stochastic Process]]`
   $$\downarrow$$
3. `[[Markov Chain]]`
   - Defines one-step transition matrix $P$ and row-stochasticity.
   $$\downarrow$$
4. `[[Chapman-Kolmogorov Equations]]`
   - Proves $P^{(n)} = P^n$ and establishes transitivity of reachability.
   $$\downarrow$$
5. `[[Classification of States in Markov Chains]]`
   - Establishes accessibility, communicating classes, irreducibility, periodicity, recurrence, and transience.
   $$\downarrow$$
6. `[[Stationary and Limiting Distributions in Markov Chains]]`
   - Balance equations $\pi P = \pi$, ergodic theorem conditions, difference between stationary and limiting distributions.

### 2. Random Walk and Gambler's Ruin Pathway
1. `[[Markov Chain]]` (Absorbing barriers at $0$ and $N$)
   $$\downarrow$$
2. `[[Classification of States in Markov Chains]]` (States $0, N$ are absorbing; $\{1, \dots, N-1\}$ are transient)
   $$\downarrow$$
3. `[[Gambler's Ruin Formula]]` (Second-order difference equation solved via telescoping geometric sequence)
   $$\downarrow$$
4. `[[Problem — Patty and Max Gambler's Ruin]]`

---

## Application and Problem Mapping

| Knowledge Note | Directly Tested / Applied By |
|---|---|
| `[[Markov Chain]]` | `[[Weather Forecasting Markov Chain Example]]`, `[[Problem — Four-Day Weather Forecast]]` |
| `[[Chapman-Kolmogorov Equations]]` | `[[Problem — Four-Day Weather Forecast]]`, `[[Problem — Rain Prediction Two Days Ahead]]` |
| `[[Classification of States in Markov Chains]]` | `[[Problem — State Communication and Irreducibility Verification]]`, `[[Problem — Identification of Communicating Classes and Absorbing States]]` |
| `[[Stationary and Limiting Distributions in Markov Chains]]` | `[[Hardy-Weinberg Law Markov Chain Example]]`, `[[Weather Forecasting Markov Chain Example]]` |
| `[[Gambler's Ruin Formula]]` | `[[Problem — Patty and Max Gambler's Ruin]]` |
