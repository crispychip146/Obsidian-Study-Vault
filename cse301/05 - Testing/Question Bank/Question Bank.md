# CSE301 — Question Bank

This file catalogs all practice, exam, tutorial, and lecture problems for **CSE301: Mathematics for Computing and Data Science**, tracking metadata, concepts tested, difficulty, and solution availability.

---

## Question Master Table

| ID | Title | Source | Topic | Question Type | Difficulty | Solution Status | Note Link |
|---|---|---|---|---|---|---|---|
| Q-CSE301-001 | Patty and Max Gambler's Ruin | [[cse301/01 - Sources/Lectures/Markov_Chain.pdf\|Markov_Chain.pdf]] (Slide 36) | Stochastic Processes / Random Walks | Numerical / Applied Probability | Medium | Solved | [[Problem — Patty and Max Gambler's Ruin]] |
| Q-CSE301-002 | Four-Day Weather Forecast | [[cse301/01 - Sources/Lectures/Markov_Chain.pdf\|Markov_Chain.pdf]] (Slide 10) | Discrete-Time Markov Chains | Numerical / Matrix Power | Medium | Solved | [[Problem — Four-Day Weather Forecast]] |
| Q-CSE301-003 | Rain Prediction Two Days Ahead | [[cse301/01 - Sources/Lectures/Markov_Chain.pdf\|Markov_Chain.pdf]] (Slides 16–18) | State Augmentation / Multi-Step Transition | Numerical / Matrix Dot Product | Medium | Solved | [[Problem — Rain Prediction Two Days Ahead]] |
| Q-CSE301-004 | State Communication & Irreducibility | [[cse301/01 - Sources/Lectures/Markov_Chain.pdf\|Markov_Chain.pdf]] (Slide 14) | State Classification | Proof / Conceptual Verification | Easy-Medium | Solved | [[Problem — State Communication and Irreducibility Verification]] |
| Q-CSE301-005 | Communicating Classes & Absorbing States | [[cse301/01 - Sources/Lectures/Markov_Chain.pdf\|Markov_Chain.pdf]] (Slide 15) | State Classification / Decomposition | Conceptual / Decomposition | Medium | Solved | [[Problem — Identification of Communicating Classes and Absorbing States]] |
| Q-CSE301-006 | Unbiased yet Inconsistent Estimator Analysis | [[cse301/01 - Sources/Lectures/Point Estimation and Confidence Intervals.pdf\|Point Estimation.pdf]] (Slides 14–15) | Statistical Inference / Point Estimation | Analytical / Asymptotics | Medium | Solved | [[Problem — Unbiased yet Inconsistent Estimator Analysis]] |
| Q-CSE301-007 | Sample Variance Bias and Bessel's Correction | [[cse301/01 - Sources/Lectures/CSE301_Maximum_Likelihood_Estimators__MLE_.pdf\|MLE.pdf]] (Slides 9–13) | Maximum Likelihood Estimation | Mathematical Proof / Derivation | Medium-Hard | Solved | [[Problem — Sample Variance Bias and Bessel's Correction Derivation]] |
| Q-CSE301-008 | Laplace Rule of Succession and Bayesian Updating | [[cse301/01 - Sources/Lectures/Bayesian_Inference.pdf\|Bayesian_Inference.pdf]] (Slides 14–16) | Bayesian Inference / Beta-Binomial | Applied Probability / Predictive | Medium | Solved | [[Problem — Laplace Rule of Succession and Bayesian Updating]] |
| Q-CSE301-009 | Comparing Prediction Algorithms via Paired Wald Test | [[cse301/01 - Sources/Lectures/CSE301_Hypothesis_Test.pdf\|Hypothesis_Test.pdf]] (Slides 13–14) | Hypothesis Testing / Wald Test | Numerical / Model Comparison | Medium | Solved | [[Problem — Comparing Prediction Algorithms via Paired Wald Test]] |
| Q-CSE301-010 | Multiple Testing with Bonferroni and Benjamini-Hochberg | [[cse301/01 - Sources/Lectures/CSE301_Hypothesis_Test.pdf\|Hypothesis_Test.pdf]] (Slides 32–38) | Multiple Testing / FDR Control | Algorithmic / Step-Up Comparison | Medium | Solved | [[Problem — Multiple Testing Correction with Bonferroni and Benjamini-Hochberg]] |
| Q-CSE301-011 | M-M-1 Queue Performance Metrics Calculation | [[cse301/01 - Sources/Lectures/CSE301_Queueing_Theory.pdf\|Queueing_Theory.pdf]] (Slides 13–21) | Queueing Theory / M/M/1 Systems | Numerical / Performance Modeling | Medium | Solved | [[Problem — M-M-1 Queue Performance Metrics Calculation]] |
| Q-CSE301-012 | Finite Capacity Queue Loss and Effective Throughput | [[cse301/01 - Sources/Lectures/CSE301_Queueing_Theory.pdf\|Queueing_Theory.pdf]] (Slides 22–25) | Queueing Theory / M/M/1/N Systems | Numerical / Loss & Throughput | Medium-Hard | Solved | [[Problem — Finite Capacity Queue Loss and Effective Throughput]] |
| Q-CSE301-013 | Birthday Collisions and Approximation | [[cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf\|Lecture Notes Complete]] (Lec 2) | Combinatorics / Discrete Probability | Analytical / Taylor Approximation | Medium | Solved | [[Problem — Birthday Collisions and Approximation]] |
| Q-CSE301-014 | Indicator Variables for Distinct Birthday Counts | [[cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf\|Lecture Notes Complete]] (Lec 8 & 13) | Random Variables / Indicator Methods | Analytical / Variance Derivation | Medium-Hard | Solved | [[Problem — Indicator Variables for Distinct Birthday Counts]] |
| Q-CSE301-015 | Compound Random Sum via Adam and Eve's Laws | [[cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf\|Lecture Notes Complete]] (Lec 15 & 16) | Conditioning / Total Expectation & Variance | Analytical / Compound Processes | Medium-Hard | Solved | [[Problem — Compound Random Sum via Adam and Eve's Laws]] |
| Q-CSE301-016 | Bounding Tail Probabilities with Chebyshev and Chernoff | [[cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf\|Lecture Notes Complete]] (Lec 17 & 18) | Probability Bounds / Inequalities | Numerical / Optimization | Medium | Solved | [[Problem — Bounding Tail Probabilities with Chebyshev and Chernoff]] |
| Q-CSE301-017 | CLT Implications for the Weak Law of Large Numbers | [[cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf\|Lecture Notes Complete]] (Lec 17, 20, 21) | Asymptotics / Limit Theorems | Analytical / Sample Sizing | Medium | Solved | [[Problem — CLT Implications for the Weak Law of Large Numbers]] |
| Q-CSE301-018 | Expected Local Maxima in Random Permutations | [[cse301/01 - Sources/Lectures/HT Final Note - Suchi.pdf\|HT Final Note - Suchi.pdf]] (Lec 10) | Random Variables / Indicator Methods | Analytical / Linearity of Expectation | Medium | Solved | [[Problem — Expected Number of Local Maxima in Random Permutations]] |



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

### Q-CSE301-006: Unbiased yet Inconsistent Estimator Analysis
- **Problem Summary:** Evaluate estimator $\hat{\mu} = X_1$ for sample of size $n$. Compute bias ($0$), variance ($\sigma^2$), MSE ($\sigma^2$), and prove inconsistency via failure of convergence in probability.
- **Concepts Tested:** [[Point Estimation]], [[Estimator Consistency and Convergence]], [[Bias-Variance Decomposition]]
- **Key Insight:** Unbiasedness does not imply consistency; estimators must pool data across $n$ for variance to vanish.
- **Detailed Note:** [[Problem — Unbiased yet Inconsistent Estimator Analysis]]

### Q-CSE301-007: Sample Variance Bias and Bessel's Correction
- **Problem Summary:** Rigorous algebraic expansion proving that $E[\frac{1}{n}\sum (X_i - \bar{X})^2] = \frac{n-1}{n}\sigma^2$, calculating downward bias $-\sigma^2/n$, and establishing Bessel's unbiased correction $S^2 = \frac{n}{n-1}\hat{\sigma}^2$.
- **Concepts Tested:** [[Maximum Likelihood Estimation]], [[Point Estimation]], [[Normal Distribution Parameter MLE Derivation Example]]
- **Key Insight:** Deviations around $\bar{X}$ are strictly smaller than around true mean $\mu$, consuming 1 degree of freedom.
- **Detailed Note:** [[Problem — Sample Variance Bias and Bessel's Correction Derivation]]

### Q-CSE301-008: Laplace Rule of Succession and Bayesian Updating
- **Problem Summary:** Safety testing of autonomous vehicle software over 5 flawless runs. Derive predictive probability of success on 6th run $\frac{s+1}{n+2} = 6/7$, proving equivalence to posterior mean and contrasting with overfitted MLE.
- **Concepts Tested:** [[Bayesian Inference]], [[Beta-Binomial Conjugate Updating Formula]], [[Maximum Likelihood Estimation]]
- **Key Insight:** Continuous law of total probability shows predictive success probability equals parameter posterior mean.
- **Detailed Note:** [[Problem — Laplace Rule of Succession and Bayesian Updating]]

### Q-CSE301-009: Comparing Prediction Algorithms via Paired Wald Test
- **Problem Summary:** Evaluating two classifiers on $n = 500$ identical images. Calculate paired differences $D_i = X_i - Y_i$, empirical standard error, Wald statistic $W = -3.41$, and $p = 0.00065$, explaining why unpaired testing fails.
- **Concepts Tested:** [[Wald Test Statistic]], [[Hypothesis Testing Framework]], [[p-Values and Significance]]
- **Key Insight:** Positive correlation on identical test instances reduces variance of paired difference, boosting test power.
- **Detailed Note:** [[Problem — Comparing Prediction Algorithms via Paired Wald Test]]

### Q-CSE301-010: Multiple Testing with Bonferroni and Benjamini-Hochberg
- **Problem Summary:** Processing 10 genomic test $p$-values. Contrast unadjusted testing (5 rejections, $40.1\%$ FWER) with Bonferroni (2 rejections, over-conservative) and Benjamini-Hochberg step-up procedure (5 rejections with $\text{FDR} \le 5\%$).
- **Concepts Tested:** [[Multiple Testing and False Discovery Rate]], [[Benjamini-Hochberg Procedure Algorithm]], [[p-Values and Significance]]
- **Key Insight:** Adaptive linear rank threshold $\ell_i = \frac{i}{m}q$ safely maximizes discoveries while bounding false discovery rate.
- **Detailed Note:** [[Problem — Multiple Testing Correction with Bonferroni and Benjamini-Hochberg]]

### Q-CSE301-011: M-M-1 Queue Performance Metrics Calculation
- **Problem Summary:** Edge router handling $\lambda = 800$, $\mu = 1000$. Compute utilization ($0.80$), idle probability ($0.20$), $L=4$, $W=5$ ms, $L_Q=3.2$, $W_Q=4$ ms, and demonstrate $5\times$ latency surge under $20\%$ traffic increase.
- **Concepts Tested:** [[M-M-1 Queue]], [[M-M-1 Performance Formulas]], [[Little's Law]]
- **Key Insight:** Hyperbolic $(1-\rho)^{-1}$ congestion curve produces explosive delay increases as utilization approaches 1.
- **Detailed Note:** [[Problem — M-M-1 Queue Performance Metrics Calculation]]

### Q-CSE301-012: Finite Capacity Queue Loss and Effective Throughput
- **Problem Summary:** Microservice with buffer $N = 3$, $\lambda = 6$, $\mu = 4$ ($\rho = 1.5 > 1$). Prove stability, compute state probabilities ($P_0 = 8/65$), drop probability ($P_3 = 41.5\%$), effective throughput ($3.508$ req/s), and response time $W = 566$ ms.
- **Concepts Tested:** [[Finite Capacity M-M-1-N Queue]], [[PASTA Property and Inspection Paradox]], [[Little's Law]]
- **Key Insight:** Finite capacity queues are stable for any arrival rate; Little's Law requires dividing by effective rate $\lambda_{\text{eff}}$.
- **Detailed Note:** [[Problem — Finite Capacity Queue Loss and Effective Throughput]]

### Q-CSE301-013: Birthday Collisions and Approximation
- **Problem Summary:** Exact hash collision probability for $k$ keys into $m$ buckets, closed-form exponential lower bound $1 - e^{-k(k-1)/(2m)}$, 32-bit hash threshold derivation ($k \approx 77,163$), and pair indicator expectation $\mathbb{E}[C] = \binom{k}{2}\frac{1}{m}$.
- **Concepts Tested:** [[Combinatorics and Counting Principles]], [[Probability Axioms and Naive Probability]], [[Birthday Problem and Collisions Example]]
- **Key Insight:** Birthday collisions scale with the number of pairs $\binom{k}{2} \approx k^2/2$, not the number of keys $k$.
- **Detailed Note:** [[Problem — Birthday Collisions and Approximation]]

### Q-CSE301-014: Indicator Variables for Distinct Birthday Counts
- **Problem Summary:** Group of $k$ people across $n = 365$ days. Express distinct days $D = \sum I_i$, compute expectation $n[1 - (1-1/n)^k]$, prove pairwise covariance $\operatorname{Cov}(I_i, I_j) < 0$ due to competition for people, and derive exact variance.
- **Concepts Tested:** [[Linearity of Expectation and Indicator Random Variables Example]], [[Covariance and Correlation]], [[Discrete Probability Distributions]]
- **Key Insight:** Summing indicators bypasses dependencies for expectation, but requires computing all $n(n-1)$ pairwise covariances for variance.
- **Detailed Note:** [[Problem — Indicator Variables for Distinct Birthday Counts]]

### Q-CSE301-015: Compound Random Sum via Adam and Eve's Laws
- **Problem Summary:** Database write transactions $N \sim \operatorname{Bin}(200, 0.4)$ with log sizes $X_i \sim \operatorname{Gamma}(3, 0.5)$. Compute mean disk write volume ($480$ MB), variance via Eve's Law ($2688$ MB$^2$), variance attribution ($64.3\%$ from transaction count fluctuation), and compound MGF $M_N(\ln M_X(t))$.
- **Concepts Tested:** [[Adam's Law (Law of Total Expectation)]], [[Eve's Law (Law of Total Variance)]], [[Moment Generating Functions]]
- **Key Insight:** Total variance decomposes into within-group (payload spread) and between-group (arrival rate fluctuation) components.
- **Detailed Note:** [[Problem — Compound Random Sum via Adam and Eve's Laws]]

### Q-CSE301-016: Bounding Tail Probabilities with Chebyshev and Chernoff
- **Problem Summary:** Network buffer with arrivals $X \sim \operatorname{Pois}(20)$ and capacity 40. Contrast Markov bound ($50.0\%$), two-sided Chebyshev ($5.0\%$), one-sided Cantelli ($4.76\%$), and derive optimal Chernoff exponent $t^* = \ln 2$ to prove overflow probability is $\le 0.0441\%$.
- **Concepts Tested:** [[Markov Inequality]], [[Chebyshev Inequality]], [[Chernoff Bound]]
- **Key Insight:** Exploiting higher moments via MGF transforms polynomial bounds into exponentially decaying bounds.
- **Detailed Note:** [[Problem — Bounding Tail Probabilities with Chebyshev and Chernoff]]

### Q-CSE301-017: CLT Implications for the Weak Law of Large Numbers
- **Problem Summary:** User polling sample sizing for $3\%$ margin of error at $95\%$ confidence. Contrast conservative Chebyshev sample size ($n = 5,556$) with CLT normal approximation ($n = 1,068$), and provide a rigorous proof that the CLT implies the WLLN.
- **Concepts Tested:** [[Central Limit Theorem]], [[Law of Large Numbers]], [[Chebyshev Inequality]]
- **Key Insight:** Normal approximation reduces required sample size by over $5\times$ compared to distribution-free Chebyshev bounds.
- **Detailed Note:** [[Problem — CLT Implications for the Weak Law of Large Numbers]]

### Q-CSE301-018: Expected Number of Local Maxima in Random Permutations
- **Problem Summary:** For a uniform random permutation of $\{1, 2, \dots, n\}$ ($n \ge 2$), find the expected number of local maxima using indicator variables across boundary elements ($P = 1/2$) and interior elements ($P = 1/3$). Yields exact expected value $\mathbb{E}[X] = \frac{n+1}{3}$, verified against all 6 permutations for $n = 3$.
- **Concepts Tested:** [[Linearity of Expectation and Indicator Random Variables Example]], [[Random Variables and Probability Distributions]]
- **Key Insight:** Linearity of expectation bypasses complex dependencies between adjacent peaks, while symmetry makes any element in an interior triple equally likely ($1/3$) to be the maximum.
- **Detailed Note:** [[Problem — Expected Number of Local Maxima in Random Permutations]]


