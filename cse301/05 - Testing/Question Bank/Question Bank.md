# CSE301 — Question Bank

This file catalogs all practice, exam, tutorial, and lecture problems for **CSE301: Mathematics for Computing and Data Science**, tracking metadata, concepts tested, difficulty, and solution availability.

---

## Question Master Table

| ID | Title | Source | Topic | Question Type | Difficulty | Solution Status | Note Link |
|---|---|---|---|---|---|---|---|
| Q-CSE301-001 | Patty and Max Gambler's Ruin | [[01 - Sources/Lectures/Markov_Chain.pdf\|Markov_Chain.pdf]] (Slide 36) | Stochastic Processes / Random Walks | Numerical / Applied Probability | Medium | Solved | [[Problem — Patty and Max Gambler's Ruin]] |
| Q-CSE301-002 | Four-Day Weather Forecast | [[01 - Sources/Lectures/Markov_Chain.pdf\|Markov_Chain.pdf]] (Slide 10) | Discrete-Time Markov Chains | Numerical / Matrix Power | Medium | Solved | [[Problem — Four-Day Weather Forecast]] |
| Q-CSE301-003 | Rain Prediction Two Days Ahead | [[01 - Sources/Lectures/Markov_Chain.pdf\|Markov_Chain.pdf]] (Slides 16–18) | State Augmentation / Multi-Step Transition | Numerical / Matrix Dot Product | Medium | Solved | [[Problem — Rain Prediction Two Days Ahead]] |
| Q-CSE301-004 | State Communication & Irreducibility | [[01 - Sources/Lectures/Markov_Chain.pdf\|Markov_Chain.pdf]] (Slide 14) | State Classification | Proof / Conceptual Verification | Easy-Medium | Solved | [[Problem — State Communication and Irreducibility Verification]] |
| Q-CSE301-005 | Communicating Classes & Absorbing States | [[01 - Sources/Lectures/Markov_Chain.pdf\|Markov_Chain.pdf]] (Slide 15) | State Classification / Decomposition | Conceptual / Decomposition | Medium | Solved | [[Problem — Identification of Communicating Classes and Absorbing States]] |

---

## Detailed Question Summaries

### Q-CSE301-001: Patty and Max Gambler's Ruin
- **Problem Summary:** Patty starts with 5 pennies, Max starts with 10 ($N=15$). Probability of winning each flip is $p = 0.6$. Find probability Patty wipes Max out and probability of ruin.
- **Concepts Tested:** [[Gambler's Ruin Formula]], [[Markov Chain]], [[Classification of States in Markov Chains]]
- **Key Insight:** Asymmetric Gambler's Ruin formula with $q/p = 2/3$.
- **Detailed Note:** [[Problem — Patty and Max Gambler's Ruin]]

### Q-CSE301-002: Four-Day Weather Forecast
- **Problem Summary:** Weather chain with $\alpha = 0.7, \beta = 0.4$. Given rain today, calculate probability of rain 2 days and 4 days from now, and compare with steady-state limit $\pi_0 = 4/7$.
- **Concepts Tested:** [[Chapman-Kolmogorov Equations]], [[Stationary and Limiting Distributions in Markov Chains]], [[Weather Forecasting Markov Chain Example]]
- **Key Insight:** Repeated squaring $P^4 = (P^2)^2$.
- **Detailed Note:** [[Problem — Four-Day Weather Forecast]]

### Q-CSE301-003: Rain Prediction Two Days Ahead
- **Problem Summary:** 2-day weather memory formulated as 4-state Markov chain. Compute probability of rain 2 days ahead starting from state $(R, R)$.
- **Concepts Tested:** [[Higher-Order State Weather Prediction Example]], [[Chapman-Kolmogorov Equations]], [[Markov Chain]]
- **Key Insight:** Summing row 0 transition entries for terminal states 0 and 1: $P_{00}^{(2)} + P_{01}^{(2)} = 0.49 + 0.12 = 0.61$.
- **Detailed Note:** [[Problem — Rain Prediction Two Days Ahead]]

### Q-CSE301-004: State Communication & Irreducibility Verification
- **Problem Summary:** 3-state chain with $P_{02} = 0$. Prove state 2 is accessible from 0 in 2 steps. Prove mutual communication, irreducibility, and aperiodicity via self-loops.
- **Concepts Tested:** [[Classification of States in Markov Chains]], [[Chapman-Kolmogorov Equations]]
- **Key Insight:** $P_{01} P_{12} > 0$ establishes accessibility; self-loop $P_{00} > 0$ proves aperiodicity of entire irreducible class.
- **Detailed Note:** [[Problem — State Communication and Irreducibility Verification]]

### Q-CSE301-005: Communicating Classes & Absorbing States
- **Problem Summary:** 4-state reducible chain. Identify all communicating classes ($\{0, 1\}, \{2\}, \{3\}$), classify as recurrent/transient, identify absorbing state ($3$), and explain why $2 \not\leftrightarrow 0$.
- **Concepts Tested:** [[Classification of States in Markov Chains]]
- **Key Insight:** Accessibility is directed; communication requires bidirectional accessibility.
- **Detailed Note:** [[Problem — Identification of Communicating Classes and Absorbing States]]
