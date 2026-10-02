# CSE 301 — Mathematics for Computing and Data Science

> 3 hours/week · 3 credits

## Course Description

Advanced counting and discrete probability, random variables and distributions,
probability bounds, convergence, statistical inference, Bayesian inference,
hypothesis testing, stochastic processes, and queuing theory.

---

## 🗺️ Master Sequential Reading Roadmap (Steps 01 – 92)

Read these notes in strict chronological order to build cumulative mastery from foundations of probability to stochastic processes and queueing theory:

### Module 1: Counting and Discrete Probability
- [ ] **Step 01 (Concept):** [[Combinatorics and Counting Principles]] — Multiplication rule, permutations, combinations, and sampling paradigms.
- [ ] **Step 02 (Concept):** [[Probability Axioms and Naive Probability]] — Kolmogorov axioms, non-negativity, normalization, and countable additivity.
- [ ] **Step 03 (Formula):** [[Inclusion-Exclusion Principle]] — General n-set union formula and Bonferroni truncation bounds.
- [ ] **Step 04 (Example):** [[Birthday Problem and Collisions Example]] — 23-person threshold, Taylor expansion, and hash collision bounds.
- [ ] **Step 05 (Example):** [[Derangements and Card Matching Example]] — Montmort matching problem and 1/e asymptotic limit.
- [ ] **Step 06 (Problem):** [[Problem — Birthday Collisions and Approximation]] — Collision probability derivation and Poisson approximation (Q-CSE301-013).

### Module 2: Random Variables and Distributions
- [ ] **Step 07 (Concept):** [[Random Variables and Probability Distributions]] — Mapping outcomes to reals, CDF, PMF, PDF, expectation, and variance.
- [ ] **Step 08 (Concept):** [[Discrete Probability Distributions]] — Bernoulli, Binomial, Hypergeometric, Geometric, First Success, Negative Binomial, Poisson.
- [ ] **Step 09 (Concept):** [[Continuous Probability Distributions]] — Uniform, Normal, Exponential, Gamma, Beta, and Universality of the Uniform.
- [ ] **Step 10 (Concept):** [[Joint and Marginal Distributions]] — Joint PMF/PDF, marginalization, conditional distributions, and support dependency.
- [ ] **Step 11 (Concept):** [[Covariance and Correlation]] — Bilinearity, variance of sums, correlation bounds [-1, 1], and independence vs uncorrelatedness.
- [ ] **Step 12 (Formula):** [[Law of the Unconscious Statistician (LOTUS)]] — Computing expected value of transformed variables without deriving transformed densities.
- [ ] **Step 13 (Formula):** [[Moment Generating Functions]] — Laplace-type transforms, moment generation, and distribution uniqueness.
- [ ] **Step 14 (Example):** [[Linearity of Expectation and Indicator Random Variables Example]] — Decomposing complex random counts into sums of Bernoulli indicators.
- [ ] **Step 15 (Example):** [[Exponential Distribution Memorylessness Example]] — Analytical and physical implications of P(X > s + t | X > s) = P(X > t).
- [ ] **Step 16 (Problem):** [[Problem — Indicator Variables for Distinct Birthday Counts]] — Exact mean and variance derivation via indicator variables (Q-CSE301-014).

### Module 3: Conditional Probability and Conditioning
- [ ] **Step 17 (Concept):** [[Conditional Probability and Independence]] — P(A | B), multiplication rule, pairwise vs mutual independence, conditional independence.
- [ ] **Step 18 (Concept):** [[Conditional Expectation]] — E[Y | X] as a random variable, projection property, and conditional variance.
- [ ] **Step 19 (Formula):** [[Law of Total Probability and Bayes' Rule]] — Partition theorem, prior-to-posterior updating, and base rate fallacy.
- [ ] **Step 20 (Formula):** [[Adam's Law (Law of Total Expectation)]] — Iterated expectations: E[Y] = E[E[Y | X]].
- [ ] **Step 21 (Formula):** [[Eve's Law (Law of Total Variance)]] — Variance decomposition: Var(Y) = E[Var(Y | X)] + Var(E[Y | X]).
- [ ] **Step 22 (Example):** [[Monty Hall Problem Example]] — Bayesian updating and decision analysis under unconditional switching strategy.
- [ ] **Step 23 (Example):** [[Random Number of Random Variables Sum Example]] — Applying Adam and Eve's laws to compound branching and queueing sums.
- [ ] **Step 24 (Problem):** [[Problem — Compound Random Sum via Adam and Eve's Laws]] — Calculating mean and variance of random sums with random terms (Q-CSE301-015).

### Module 4: Probability Bounds and Inequalities
- [ ] **Step 25 (Formula):** [[Markov Inequality]] — First-moment tail bound for non-negative random variables: P(X >= a) <= E[X]/a.
- [ ] **Step 26 (Formula):** [[Chebyshev Inequality]] — Second-moment concentration bound using variance: P(|X - mu| >= k*sigma) <= 1/k^2.
- [ ] **Step 27 (Formula):** [[Chernoff Bound]] — Exponential moment tail bound via MGF optimization for sharp asymptotic decay.
- [ ] **Step 28 (Formula):** [[Cauchy-Schwarz and Jensen Inequalities]] — Inner product bounds and convex transformation inequalities E[g(X)] >= g(E[X]).
- [ ] **Step 29 (Example):** [[Comparison of Probability Bounds Example]] — Comparing Markov, Chebyshev, and Chernoff bounds on Binomial tails.
- [ ] **Step 30 (Problem):** [[Problem — Bounding Tail Probabilities with Chebyshev and Chernoff]] — Comparative concentration bounds derivation and sharpness analysis (Q-CSE301-016).

### Module 5: Convergence of Random Variables and Asymptotics
- [ ] **Step 31 (Concept):** [[Law of Large Numbers]] — Weak Law (convergence in probability) and Strong Law (almost sure convergence).
- [ ] **Step 32 (Concept):** [[Central Limit Theorem]] — Convergence of standardized sample means to Standard Normal N(0, 1).
- [ ] **Step 33 (Example):** [[Normal Approximation to Binomial and Poisson Example]] — Continuity correction and large-sample Gaussian approximation.
- [ ] **Step 34 (Problem):** [[Problem — CLT Implications for the Weak Law of Large Numbers]] — Bounding tail deviations and proving WLLN as a corollary of CLT (Q-CSE301-017).

### Module 6: Statistical Inference
- [ ] **Step 35 (Concept):** [[Point Estimation]] — Sample statistics, bias, standard error, and Mean Squared Error (MSE).
- [ ] **Step 36 (Formula):** [[Bias-Variance Decomposition]] — MSE(theta_hat) = Bias(theta_hat)^2 + Var(theta_hat).
- [ ] **Step 37 (Concept):** [[Estimator Consistency and Convergence]] — Weak consistency via probability limits and Chebyshev verification.
- [ ] **Step 38 (Concept):** [[Confidence Intervals and Confidence Sets]] — Frequentist interpretation of coverage probability and confidence sets.
- [ ] **Step 39 (Formula):** [[Normal-Based Large-Sample Confidence Interval]] — Wald-type intervals: theta_hat +- z_{alpha/2} * SE(theta_hat).
- [ ] **Step 40 (Example):** [[Bernoulli Parameter Estimation and Confidence Interval Example]] — Sample proportion estimation, standard errors, and Wald confidence intervals.
- [ ] **Step 41 (Example):** [[Berger-Wolpert Confidence Set Puzzle Example]] — Two-observation uniform puzzle illustrating counter-intuitive coverage behavior.
- [ ] **Step 42 (Problem):** [[Problem — Unbiased yet Inconsistent Estimator Analysis]] — Analyzing estimators that are unbiased but fail consistency (Q-CSE301-006).

### Module 7: Parametric Inference
- [ ] **Step 43 (Concept):** [[Maximum Likelihood Estimation]] — Likelihood function, log-likelihood, MLE invariance property, and asymptotic efficiency.
- [ ] **Step 44 (Formula):** [[Likelihood and Score Equations]] — Score function S(theta) = 0 and Fisher Information matrix.
- [ ] **Step 45 (Example):** [[Normal Distribution Parameter MLE Derivation Example]] — Deriving joint MLEs for mean mu and variance sigma^2 under Gaussian assumptions.
- [ ] **Step 46 (Example):** [[Uniform Distribution Non-Regular MLE Example]] — Order statistics MLE for Uniform(0, theta) violating Cramer-Rao regularity.
- [ ] **Step 47 (Example):** [[Discrete and Continuous Parameter MLE Reference Examples]] — Canonical MLE reference derivations for Bernoulli, Poisson, Exponential, and Geometric.
- [ ] **Step 48 (Problem):** [[Problem — Sample Variance Bias and Bessel's Correction Derivation]] — Formal expectation derivation of S^2 and proof of Bessel's 1/(n-1) correction (Q-CSE301-007).

### Module 8: Bayesian Inference
- [ ] **Step 49 (Concept):** [[Bayesian Inference]] — Prior distribution, likelihood, posterior distribution, and conjugate families.
- [ ] **Step 50 (Concept):** [[Maximum A Posteriori (MAP) Estimation]] — Mode of posterior distribution as regularized point estimate.
- [ ] **Step 51 (Concept):** [[Credible Intervals]] — Direct posterior probability intervals: Equal-tailed vs Highest Posterior Density (HPD).
- [ ] **Step 52 (Formula):** [[Beta-Binomial Conjugate Updating Formula]] — Beta(alpha + k, beta + n - k) posterior update mechanics.
- [ ] **Step 53 (Formula):** [[Normal-Normal Conjugate Updating Formula]] — Posterior precision as sum of prior precision and data precision.
- [ ] **Step 54 (Example):** [[Bernoulli Bayesian Inference with Beta Prior Example]] — Prior belief shifting to posterior distribution under coin toss observations.
- [ ] **Step 55 (Example):** [[Two Binomial Distributions Comparison via Bayesian Simulation Example]] — Monte Carlo simulation of posterior differences P(p1 > p2 | data).
- [ ] **Step 56 (Problem):** [[Problem — Laplace Rule of Succession and Bayesian Updating]] — Deriving (k+1)/(n+2) prediction formula under uniform prior (Q-CSE301-008).

### Module 9: Hypothesis Testing
- [ ] **Step 57 (Concept):** [[Hypothesis Testing Framework]] — Null vs alternative hypotheses, Type I error alpha, Type II error beta, statistical power.
- [ ] **Step 58 (Concept):** [[p-Values and Significance]] — Definition, calculation, misinterpretations, and significance thresholds.
- [ ] **Step 59 (Formula):** [[Wald Test Statistic]] — W = (theta_hat - theta_0)^2 / Var(theta_hat) ~ Chi-Square(1).
- [ ] **Step 60 (Formula):** [[Pearson's Chi-Square Goodness-of-Fit Test]] — Chi-Square statistic = sum (O_i - E_i)^2 / E_i ~ Chi-Square(k - 1).
- [ ] **Step 61 (Algorithm):** [[Permutation Test Algorithm]] — Non-parametric resampling procedure for two-sample hypothesis testing.
- [ ] **Step 62 (Concept):** [[Multiple Testing and False Discovery Rate]] — Family-Wise Error Rate (FWER) vs False Discovery Rate (FDR), Bonferroni correction.
- [ ] **Step 63 (Algorithm):** [[Benjamini-Hochberg Procedure Algorithm]] — Controlling FDR at level q via ordered p-value thresholding.
- [ ] **Step 64 (Example):** [[Mendel's Peas Chi-Square Goodness-of-Fit Example]] — Testing Mendelian 9:3:3:1 phenotypic inheritance ratios.
- [ ] **Step 65 (Example):** [[Toy Permutation Test Example]] — Step-by-step exact permutation distribution calculation on small samples.
- [ ] **Step 66 (Problem):** [[Problem — Comparing Prediction Algorithms via Paired Wald Test]] — Evaluating machine learning model accuracy differences via paired testing (Q-CSE301-009).
- [ ] **Step 67 (Problem):** [[Problem — Multiple Testing Correction with Bonferroni and Benjamini-Hochberg]] — Applying Bonferroni and BH corrections to 10 genomic test p-values (Q-CSE301-010).

### Module 10: Stochastic Processes
- [ ] **Step 68 (Concept):** [[Stochastic Process]] — Families of random variables indexed by time, discrete vs continuous parameter/state spaces.
- [ ] **Step 69 (Concept):** [[Markov Chain]] — Markov memoryless property, transition probability matrix, and n-step transitions.
- [ ] **Step 70 (Concept):** [[Classification of States in Markov Chains]] — Accessible, communicating classes, irreducible, transient, recurrent, and periodic states.
- [ ] **Step 71 (Concept):** [[Stationary and Limiting Distributions in Markov Chains]] — Stationary vector pi = pi * P, ergodicity theorem, and long-run state proportions.
- [ ] **Step 72 (Formula):** [[Chapman-Kolmogorov Equations]] — P_{ij}^{n+m} = sum_k P_{ik}^n * P_{kj}^m matrix multiplication theorem.
- [ ] **Step 73 (Formula):** [[Gambler's Ruin Formula]] — Ruin probability P_i and expected duration for fair and biased random walks.
- [ ] **Step 74 (Example):** [[Weather Forecasting Markov Chain Example]] — 2-state Markov chain modeling sunny and rainy weather transitions.
- [ ] **Step 75 (Example):** [[Higher-Order State Weather Prediction Example]] — Converting a 2-day memory process into a first-order 4-state Markov chain.
- [ ] **Step 76 (Example):** [[Hardy-Weinberg Law Markov Chain Example]] — Genotype frequencies under random mating modeled as Markov chain.
- [ ] **Step 77 (Problem):** [[Problem — Patty and Max Gambler's Ruin]] — Solving ruin probabilities for asymmetric coin tosses (Q-CSE301-001).
- [ ] **Step 78 (Problem):** [[Problem — Four-Day Weather Forecast]] — 3-step Chapman-Kolmogorov transition probability calculation (Q-CSE301-002).
- [ ] **Step 79 (Problem):** [[Problem — Rain Prediction Two Days Ahead]] — Conditional probability forecasting using matrix powers (Q-CSE301-003).
- [ ] **Step 80 (Problem):** [[Problem — State Communication and Irreducibility Verification]] — Graph reachability analysis and irreducibility proof (Q-CSE301-004).
- [ ] **Step 81 (Problem):** [[Problem — Identification of Communicating Classes and Absorbing States]] — Partitioning state spaces and detecting absorption traps (Q-CSE301-005).

### Module 11: Queuing Theory
- [ ] **Step 82 (Concept):** [[Queueing Systems and Kendall Notation]] — Arrival process, service distribution, server count, buffer capacity (A/S/c/K).
- [ ] **Step 83 (Formula):** [[Little's Law]] — Fundamental conservation law: L = lambda * W and L_q = lambda * W_q.
- [ ] **Step 84 (Concept):** [[PASTA Property and Inspection Paradox]] — Poisson Arrivals See Time Averages and length-biased sampling effects.
- [ ] **Step 85 (Concept):** [[M-M-1 Queue]] — Birth-death Markov process, stability condition rho = lambda / mu < 1, state probabilities.
- [ ] **Step 86 (Formula):** [[M-M-1 Performance Formulas]] — Closed-form expressions: L = rho/(1-rho), W = 1/(mu - lambda), W_q = rho/(mu - lambda).
- [ ] **Step 87 (Concept):** [[Finite Capacity M-M-1-N Queue]] — Bounded buffer queue, blocking probability P_N, and effective arrival rate lambda_eff.
- [ ] **Step 88 (Concept):** [[Jackson Networks and Tandem Queues]] — Burke's theorem, product-form stationary distributions, and feed-forward queueing networks.
- [ ] **Step 89 (Example):** [[Shoe Shine Shop Queueing Model Example]] — 2-stage tandem queueing system without intermediate waiting room.
- [ ] **Step 90 (Example):** [[Tandem Two-Server Queue Performance Example]] — Analyzing throughput and bottleneck server in sequential queueing pipelines.
- [ ] **Step 91 (Problem):** [[Problem — M-M-1 Queue Performance Metrics Calculation]] — End-to-end metrics calculation for an IT service desk (Q-CSE301-011).
- [ ] **Step 92 (Problem):** [[Problem — Finite Capacity Queue Loss and Effective Throughput]] — Loss probability and throughput analysis for bounded buffers (Q-CSE301-012).
---

# Sources

- [[01 - Sources/Lectures/Lecture_Notes_Complete.pdf|Lecture Notes: Complete Course Notes (Tahmid Hasen, Lec 1–21)]]
- [[01 - Sources/Lectures/strategic_practice_and_homework_1.pdf|Problem Sets: Strategic Practice & Homework 1–11 (Harvard Stat 110, Joe Blitzstein)]]
- [[01 - Sources/Lectures/ITP.pdf|Textbook: Introduction to Probability (Blitzstein & Hwang)]]
- [[01 - Sources/Lectures/Point Estimation and Confidence Intervals.pdf|Lecture: Point Estimation & Confidence Sets (Md Irtiaz Kabir)]]
- [[01 - Sources/Lectures/CSE301_Maximum_Likelihood_Estimators__MLE_.pdf|Lecture: Maximum Likelihood Estimators (Md Irtiaz Kabir)]]
- [[01 - Sources/Lectures/MLE.pdf|Reference: Probability Distributions and Their MLEs]]
- [[01 - Sources/Lectures/Bayesian_Inference.pdf|Lecture: Bayesian Inference (Md Irtiaz Kabir)]]
- [[01 - Sources/Lectures/CSE301_Hypothesis_Test.pdf|Lecture: Hypothesis Test (Md Irtiaz Kabir)]]
- [[01 - Sources/Lectures/CSE301_Queueing_Theory.pdf|Lecture: Queueing Theory (Md Irtiaz Kabir)]]
- [[01 - Sources/Lectures/Markov_Chain.pdf|Lecture: Markov Chains (Md Irtiaz Kabir)]]
- [[01 - Sources/Textbooks/Sheldon M. Ross book Markov Chain Chapter.pdf|Textbook: Sheldon M. Ross — Markov Chains (Chapter 4)]]

---

# Knowledge Maps

- [[03 - Maps/Topic Map|Topic Map]]
- [[03 - Maps/Dependency Map|Dependency Map]]
- [[03 - Maps/Exam Intelligence/Markov Chains Exam Intelligence|Markov Chains Exam Intelligence]]

---

# Testing & Question Bank

- [[05 - Testing/Question Bank/Question Bank|Question Bank]] (Cataloging Problems Q-CSE301-001 through Q-CSE301-017)

---

# Revision

- [[04 - Revision/Quick Revision — Markov Chains|Quick Revision — Markov Chains]]

