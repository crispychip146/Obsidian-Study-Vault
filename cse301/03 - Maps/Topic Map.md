# CSE301 — Topic Map

This map organizes the topics covered in CSE301 (Mathematics for Computing and Data Science), linking syllabus areas to knowledge notes.

---

## 1. Counting & Discrete Probability

### 1.1 Combinatorics Principles
- **Core Concept:** [[Combinatorics and Counting Principles]]
  - Multiplication rule, permutations, combinations, binomial coefficients
  - Four sampling paradigms (with/without replacement, ordered/unordered)
  - Stars and Bars (Bose-Einstein allocation)
- **Core Concept:** [[Probability Axioms and Naive Probability]]
  - Kolmogorov axioms (non-negativity, normalization, countable additivity)
  - Complement rule, monotonicity, Boole's union bound, continuity of probability
- **Formula:** [[Inclusion-Exclusion Principle]]
  - General $n$-set union formula, Bonferroni truncation bounds, indicator variable derivation
- **Worked Examples:**
  - [[Birthday Problem and Collisions Example]] (23-person 50% threshold, Taylor series approximation, hash collisions)
  - [[Derangements and Card Matching Example]] (Montmort matching, $1/e$ asymptotic derangement limit)
- **Practice Problem:** [[Problem — Birthday Collisions and Approximation]] (Q-CSE301-013)

---

## 2. Random Variables & Probability Distributions

### 2.1 Foundational Variables & Distributions
- **Core Concept:** [[Random Variables and Probability Distributions]]
  - RVs as functions from sample space to reals, CDF definition & properties
  - Discrete vs continuous mechanics (PMF vs PDF)
  - Expectation, linearity, variance, and standard deviation
- **Core Concept:** [[Discrete Probability Distributions]]
  - Distribution stories, PMFs, means, variances: $\operatorname{Bern}, \operatorname{Bin}, \operatorname{HGeom}, \operatorname{Geom}, \operatorname{FS}, \operatorname{NBin}, \operatorname{Pois}$
- **Core Concept:** [[Continuous Probability Distributions]]
  - Continuous families: $\operatorname{Unif}, \mathcal{N}, \operatorname{Exp}, \operatorname{Gamma}, \operatorname{Beta}$
  - Universality of the Uniform (Probability Integral Transform)
  - Empirical 68-95-99.7 rule for Gaussians
- **Core Concept:** [[Joint and Marginal Distributions]]
  - Joint PMF/PDF, marginalization, conditional distributions, independence criteria
  - The support dependency trap
- **Core Concept:** [[Covariance and Correlation]]
  - $\operatorname{Cov}(X, Y)$, bilinearity, variance of sums, correlation range $[-1, 1]$
  - Uncorrelatedness vs independence
- **Formulas:**
  - [[Law of the Unconscious Statistician (LOTUS)]]
  - [[Moment Generating Functions]]
- **Worked Examples:**
  - [[Linearity of Expectation and Indicator Random Variables Example]]
  - [[Exponential Distribution Memorylessness Example]]
- **Practice Problem:** [[Problem — Indicator Variables for Distinct Birthday Counts]] (Q-CSE301-014)

---

## 3. Conditional Probability & Conditioning

### 3.1 Conditioning & Laws of Total Probability / Expectation
- **Core Concept:** [[Conditional Probability and Independence]]
  - Definition $P(A \mid B)$, chain rule, pairwise vs mutual independence, conditional independence
- **Core Concept:** [[Conditional Expectation]]
  - Number $\mathbb{E}[Y \mid X = x]$ vs random variable $\mathbb{E}[Y \mid X]$
  - Best MSE predictor (orthogonal projection theorem), pulling out known factors
- **Formulas:**
  - [[Law of Total Probability and Bayes' Rule]] (discrete & continuous partitions, base rate fallacy)
  - [[Adam's Law (Law of Total Expectation)]] ($\mathbb{E}[Y] = \mathbb{E}[\mathbb{E}[Y \mid X]]$)
  - [[Eve's Law (Law of Total Variance)]] (EVVE within/between-group variance decomposition)
- **Worked Examples:**
  - [[Monty Hall Problem Example]]
  - [[Random Number of Random Variables Sum Example]]
- **Practice Problem:** [[Problem — Compound Random Sum via Adam and Eve's Laws]] (Q-CSE301-015)

---

## 4. Probability Bounds & Inequalities

### 4.1 Concentration & Moment Inequalities
- **Formulas:**
  - [[Markov Inequality]] (first-moment non-negative tail bound)
  - [[Chebyshev Inequality]] (variance concentration, Cantelli one-sided bound)
  - [[Chernoff Bound]] (exponential moment optimization, rate function)
  - [[Cauchy-Schwarz and Jensen Inequalities]] (quadratic bounds, convexity, AM-GM)
- **Worked Example:**
  - [[Comparison of Probability Bounds Example]] (side-by-side numerical comparison: Markov, Chebyshev, Chernoff, exact)
- **Practice Problem:** [[Problem — Bounding Tail Probabilities with Chebyshev and Chernoff]] (Q-CSE301-016)

---

## 5. Convergence of Random Variables & Asymptotics

### 5.1 Limit Theorems
- **Core Concept:** [[Law of Large Numbers]]
  - Weak vs Strong Law of Large Numbers (convergence in probability vs almost surely)
  - Gambler's fallacy and dilution mechanism
- **Core Concept:** [[Central Limit Theorem]]
  - Lindeberg-Lévy CLT, MGF convergence proof sketch
  - Continuity correction for discrete approximations
- **Worked Example:**
  - [[Normal Approximation to Binomial and Poisson Example]]
- **Practice Problem:** [[Problem — CLT Implications for the Weak Law of Large Numbers]] (Q-CSE301-017)

---

## 6. Statistical Inference & Point Estimation


### 6.1 Point Estimation Principles
- **Core Concept:** [[Point Estimation]]
  - Estimators as random variables vs. fixed unknown parameters
  - Sampling distribution, standard error, and estimated standard error $\widehat{\text{se}}$
  - Bias: $\text{bias}(\hat{\theta}_n) = E_\theta[\hat{\theta}_n] - \theta$
- **Formula:** [[Bias-Variance Decomposition]]
  - $\text{MSE}(\hat{\theta}_n) = \text{bias}^2(\hat{\theta}_n) + \text{Var}_\theta(\hat{\theta}_n)$
  - Bias-variance trade-off in statistical learning

### 6.2 Asymptotics and Consistency
- **Core Concept:** [[Estimator Consistency and Convergence]]
  - Convergence in probability ($X_n \xrightarrow{P} X$) vs. quadratic mean ($X_n \xrightarrow{qm} X$)
  - Consistency via MSE: $\text{bias} \to 0$ and $\text{Var} \to 0 \implies \hat{\theta}_n \xrightarrow{P} \theta$
  - Disconnect between unbiasedness and consistency (unbiased yet inconsistent vs. biased yet consistent)
- **Practice Problem:** [[Problem — Unbiased yet Inconsistent Estimator Analysis]]

### 6.3 Confidence Intervals and Sets
- **Core Concept:** [[Confidence Intervals and Confidence Sets]]
  - Definition: $P_\theta(\theta \in C_n) \ge 1 - \alpha$
  - Frequentist coverage vs. subjective probability fallacy
  - The Berger-Wolpert confidence set puzzle
- **Formula:** [[Normal-Based Large-Sample Confidence Interval]]
  - $C_n = \hat{\theta}_n \pm z_{\alpha/2}\widehat{\text{se}}$
- **Worked Examples:**
  - [[Bernoulli Parameter Estimation and Confidence Interval Example]]
  - [[Berger-Wolpert Confidence Set Puzzle Example]]

---

## 7. Parametric Inference & Maximum Likelihood Estimation

### 7.1 Likelihood and Score Principles
- **Core Concept:** [[Maximum Likelihood Estimation]]
  - Likelihood $L_n(\theta)$ and log-likelihood $\ell_n(\theta)$
  - Score function $S_n(\theta) = 0$
  - Equivariance / functional invariance under transformations: $\widehat{g(\theta)} = g(\hat{\theta})$
  - Asymptotic properties: Consistency, asymptotic normality, efficiency
- **Formula:** [[Likelihood and Score Equations]]
  - Fisher Information $I_n(\theta) = -E[\ell''_n(\theta)] = \text{Var}(S_n(\theta))$
  - Asymptotic standard error $\text{se}(\hat{\theta}) \approx 1/\sqrt{I_n(\theta)}$

### 7.2 Regular and Non-Regular MLEs
- **Worked Examples:**
  - [[Normal Distribution Parameter MLE Derivation Example]] (Gaussian joint MLE, bias of $\hat{\sigma}^2$, Bessel's correction)
  - [[Uniform Distribution Non-Regular MLE Example]] (Boundary likelihood maximization, order statistics, $X_{(n)}$)
  - [[Discrete and Continuous Parameter MLE Reference Examples]] (Bernoulli, Binomial, Poisson, Geometric, Exponential, Uniform)
- **Practice Problem:** [[Problem — Sample Variance Bias and Bessel's Correction Derivation]]

---

## 8. Bayesian Inference & MAP Estimation

### 8.1 Bayesian Foundations
- **Core Concept:** [[Bayesian Inference]]
  - Frequentist vs. Bayesian philosophy
  - Parameters as random variables; prior distribution $f(\theta)$, likelihood $L_n(\theta)$, marginal likelihood $m(\mathbf{x})$, posterior $f(\theta \mid \mathbf{x})$
  - Point estimates: Posterior Mean (Bayes), Posterior Median, Mode (MAP)
  - Conjugacy principle and prior types (subjective, flat, improper, Jeffreys)
- **Core Concept:** [[Maximum A Posteriori (MAP) Estimation]]
  - $\hat{\theta}_{\text{MAP}} = \arg\max_\theta [\ell_n(\theta) + \log f(\theta)]$
  - Equivalence to MLE under uniform flat prior
  - Log-prior as regularizer ($L_2$ Ridge penalty for Gaussian, $L_1$ Lasso penalty for Laplace)
- **Core Concept:** [[Credible Intervals]]
  - $P(\theta \in C \mid \mathbf{x}) = 1 - \alpha$
  - Direct subjective probability vs. frequentist confidence intervals
  - Equal-tailed intervals vs. Highest Posterior Density (HPD) regions

### 8.2 Conjugate Updating Models
- **Formula:** [[Beta-Binomial Conjugate Updating Formula]]
  - $\text{Beta}(\alpha, \beta) + \text{Binomial}(n, p) \to \text{Beta}(\alpha + s, \beta + n - s)$
  - Posterior mean as weighted average of prior and sample mean
  - Pseudocounts interpretation
- **Formula:** [[Normal-Normal Conjugate Updating Formula]]
  - $N(a, b^2) + N(\theta, \sigma^2) \to N(\bar{\theta}, \tau^2)$
  - Precision addition: $1/\tau^2 = n/\sigma^2 + 1/b^2$
- **Worked Examples & Problems:**
  - [[Bernoulli Bayesian Inference with Beta Prior Example]]
  - [[Two Binomial Distributions Comparison via Bayesian Simulation Example]]
  - [[Problem — Laplace Rule of Succession and Bayesian Updating]]

---

## 9. Hypothesis Testing

### 9.1 The Testing Framework
- **Core Concept:** [[Hypothesis Testing Framework]]
  - Null hypothesis $H_0$ vs. Alternative hypothesis $H_1$
  - Test statistic $T(\mathbf{X})$ and rejection region $R$
  - Type I error ($\alpha$) vs. Type II error ($\beta$), power ($1 - \beta$)
  - Legal trial analogy: Presumption of nullity
  - Statistical vs. scientific (practical) significance
  - Duality between hypothesis tests and confidence intervals
- **Core Concept:** [[p-Values and Significance]]
  - Definition: $p = P_{H_0}(T \ge t_{\text{obs}})$
  - Smallest significance level at which $H_0$ is rejected
  - Common fallacies ($p \ne P(H_0 \mid \text{data})$)
  - Null distribution: $P \sim \text{Uniform}(0, 1)$ under $H_0$

### 9.2 Test Procedures and Multiplicity
- **Formula:** [[Wald Test Statistic]]
  - $W = \frac{\hat{\theta} - \theta_0}{\widehat{\text{se}}} \xrightarrow{d} N(0, 1)$
  - Asymptotic size $\alpha$ and two-sided $p$-value $2\Phi(-\lvert w \rvert)$
- **Formula:** [[Pearson's Chi-Square Goodness-of-Fit Test]]
  - $V = \sum \frac{(O_j - E_j)^2}{E_j} \xrightarrow{d} \chi^2_{k-1}$
- **Core Concept:** [[Multiple Testing and False Discovery Rate]]
  - Family-Wise Error Rate (FWER) inflation $1 - (1 - \alpha)^m \to 1$
  - Bonferroni correction: reject if $P_i \le \alpha/m$
  - False Discovery Rate (FDR): $\text{FDR} = E[V / \max(R, 1)]$
- **Algorithms:**
  - [[Permutation Test Algorithm]] (Nonparametric exact two-sample testing via exchangeability)
  - [[Benjamini-Hochberg Procedure Algorithm]] (Step-up adaptive linear rank threshold $\ell_i = \frac{i}{m}q$)
- **Worked Examples & Problems:**
  - [[Mendel's Peas Chi-Square Goodness-of-Fit Example]]
  - [[Toy Permutation Test Example]]
  - [[Problem — Comparing Prediction Algorithms via Paired Wald Test]]
  - [[Problem — Multiple Testing Correction with Bonferroni and Benjamini-Hochberg]]

---

## 10. Stochastic Processes & Markov Chains

### 10.1 Foundations & Discrete-Time Markov Chains
- **Core Concepts:**
  - [[Stochastic Process]] (State spaces, time index, trajectories)
  - [[Markov Chain]] (Markov property, transition probability matrix, state augmentation)
  - [[Classification of States in Markov Chains]] (Communication, classes, irreducibility, periodicity, absorbing states, recurrence/transience)
  - [[Stationary and Limiting Distributions in Markov Chains]] (Balance equations $\pi P = \pi$, ergodic theorem)
- **Formulas:**
  - [[Chapman-Kolmogorov Equations]] ($P^{(n)} = P^n$)
  - [[Gambler's Ruin Formula]] ($P_i = \frac{1 - (q/p)^i}{1 - (q/p)^N}$)
- **Examples & Problems:**
  - [[Weather Forecasting Markov Chain Example]]
  - [[Higher-Order State Weather Prediction Example]]
  - [[Hardy-Weinberg Law Markov Chain Example]]
  - [[Problem — Patty and Max Gambler's Ruin]]
  - [[Problem — Four-Day Weather Forecast]]
  - [[Problem — Rain Prediction Two Days Ahead]]
  - [[Problem — State Communication and Irreducibility Verification]]
  - [[Problem — Identification of Communicating Classes and Absorbing States]]

---

## 11. Queueing Theory

### 11.1 Queueing Foundations
- **Core Concept:** [[Queueing Systems and Kendall Notation]]
  - Kendall notation: $A / S / c / K$
  - Four fundamental metrics: $L, L_Q, W, W_Q$
  - Residence time decomposition: $W = W_Q + 1/\mu \iff L = L_Q + \rho$
- **Formula:** [[Little's Law]]
  - $L = \lambda_a W, \quad L_Q = \lambda_a W_Q, \quad \rho = \lambda_a / \mu$
  - Ross's Fundamental Cost Identity derivation: $R = \lambda_a G$
- **Core Concept:** [[PASTA Property and Inspection Paradox]]
  - Time-average $P_n$, arrival-average $a_n$, departure-average $d_n$
  - Unit-jump theorem: $a_n = d_n$
  - PASTA theorem: $a_n = P_n$ for Poisson arrivals via independent increments
  - Counterexamples (deterministic arrivals) and inspection paradox

### 11.2 Single-Server Models
- **Core Concept:** [[M-M-1 Queue]]
  - Birth-Death Markov chain formulation
  - Balance equations and telescoping solution: $P_n = (1 - \rho)\rho^n$
  - Stability condition: $\rho = \lambda/\mu < 1$
  - Hyperbolic delay explosion ("hockey stick" curve)
- **Formula:** [[M-M-1 Performance Formulas]]
  - $L = \frac{\rho}{1 - \rho}$, $W = \frac{1}{\mu - \lambda}$, $L_Q = \frac{\rho^2}{1 - \rho}$, $W_Q = \frac{\rho}{\mu - \lambda}$
  - Exponential residence time distribution $P(T > t) = e^{-(\mu - \lambda)t}$
- **Core Concept:** [[Finite Capacity M-M-1-N Queue]]
  - Truncated state space $\{0, 1, \dots, N\}$
  - Normalization: $P_0 = \frac{1 - \rho}{1 - \rho^{N+1}}$
  - Stability for all finite $\lambda$ (even $\lambda > \mu$)
  - Blocking probability $P_{\text{loss}} = P_N$ and effective throughput $\lambda_{\text{eff}} = \lambda(1 - P_N)$
  - Little's Law with effective arrival rate: $W = L / \lambda_{\text{eff}}$

### 11.3 Networks of Queues
- **Core Concept:** [[Jackson Networks and Tandem Queues]]
  - Tandem queues in series: Burke's theorem (departures are Poisson with rate $\lambda$)
  - Product-form stationary distribution: $P(n_1, n_2) = P_1(n_1)P_2(n_2)$
  - General open Jackson networks: Routing matrix $P_{ij}$, external rates $r_i$
  - Traffic equations: $\boldsymbol{\lambda}^T = \mathbf{r}^T (\mathbf{I} - \mathbf{P})^{-1}$
  - Jackson's Theorem: Independent product form $P(\mathbf{n}) = \prod (1 - \rho_j)\rho_j^{n_j}$
- **Worked Examples & Problems:**
  - [[Shoe Shine Shop Queueing Model Example]] (Two-stage service, blocking state, CTMC balance equations)
  - [[Tandem Two-Server Queue Performance Example]] (Sequential processing pipeline)
  - [[Problem — M-M-1 Queue Performance Metrics Calculation]]
  - [[Problem — Finite Capacity Queue Loss and Effective Throughput]]
