---
type: concept
course: cse301
status: active
order: 8
---

# Discrete Probability Distributions

> 📖 **Reading Order:** Step 08 of 92 | **Module 2:** Random Variables and Distributions  
> ◄ **Previous:** [[Random Variables and Probability Distributions]] | ► **Next:** [[Continuous Probability Distributions]]

---

## Overview

In probability, many real-world phenomena share underlying structures known as **distribution stories**. Recognizing the story behind a problem immediately identifies the distribution, its Probability Mass Function (PMF), mean, and variance without tedious re-derivation.

---

## 1. Bernoulli Distribution: $\operatorname{Bern}(p)$

- **Story:** A single trial with two possible outcomes: "Success" (1) with probability $p$, or "Failure" (0) with probability $q = 1 - p$.
- **Support:** $k \in \{0, 1\}$
- **PMF:**
  $$P(X = k) = p^k (1 - p)^{1 - k}$$
- **Mean & Variance:**
  $$\mathbb{E}[X] = p, \quad \operatorname{Var}(X) = p(1 - p) = pq$$
- **Role:** The foundational building block for Binomial, Geometric, and Negative Binomial distributions.

---

## 2. Binomial Distribution: $\operatorname{Bin}(n, p)$

- **Story:** Number of successes in $n$ independent and identically distributed (i.i.d.) $\operatorname{Bern}(p)$ trials.
- **Support:** $k \in \{0, 1, 2, \dots, n\}$
- **PMF:**
  $$P(X = k) = \binom{n}{k} p^k (1 - p)^{n - k}$$
- **Mean & Variance:**
  $$\mathbb{E}[X] = np, \quad \operatorname{Var}(X) = np(1 - p)$$
  *Derivation via Indicators:* Let $X = \sum_{i=1}^n I_i$, where $I_i \sim \operatorname{Bern}(p)$ are independent. By linearity, $\mathbb{E}[X] = \sum \mathbb{E}[I_i] = np$. By independence, $\operatorname{Var}(X) = \sum \operatorname{Var}(I_i) = npq$.

---

## 3. Hypergeometric Distribution: $\operatorname{HGeom}(w, b, n)$

- **Story:** Sampling **without replacement** from a finite population of $w$ white (success) balls and $b$ black (failure) balls, drawing a sample of size $n$. $X$ is the number of white balls in the sample.
- **Support:** $\max(0, n - b) \le k \le \min(n, w)$
- **PMF:**
  $$P(X = k) = \frac{\binom{w}{k} \binom{b}{n - k}}{\binom{w + b}{n}}$$
- **Mean & Variance:** Let $N = w + b$ and $p = \frac{w}{N}$.
  $$\mathbb{E}[X] = n p = n \frac{w}{w + b}$$
  $$\operatorname{Var}(X) = n p (1 - p) \left( \frac{N - n}{N - 1} \right)$$
- **Finite Population Correction (FPC):** The factor $\frac{N - n}{N - 1}$ reflects reduced variance due to sampling without replacement. As $N \to \infty$ with $p$ fixed, $\frac{N - n}{N - 1} \to 1$, and $\operatorname{HGeom} \to \operatorname{Bin}(n, p)$.

---

## 4. Geometric Distribution: $\operatorname{Geom}(p)$

- **Story:** Number of **failures before the first success** in a sequence of independent $\operatorname{Bern}(p)$ trials. *(Note: Some conventions count total trials until first success; the Harvard Stat 110 convention defines $\operatorname{Geom}(p)$ as failures before first success, with $X \in \{0, 1, 2, \dots\}$)*.
- **Support:** $k \in \{0, 1, 2, \dots\}$
- **PMF:**
  $$P(X = k) = (1 - p)^k p = q^k p$$
- **CDF:**
  $$P(X \le k) = 1 - P(X > k) = 1 - P(\text{first } k+1 \text{ trials are failures}) = 1 - (1 - p)^{k + 1}$$
- **Mean & Variance:**
  $$\mathbb{E}[X] = \frac{1 - p}{p} = \frac{q}{p}, \quad \operatorname{Var}(X) = \frac{1 - p}{p^2} = \frac{q}{p^2}$$
- **First Success Distribution $\operatorname{FS}(p)$:** If $Y$ is the trial number of the first success, then $Y = X + 1$, so $Y \in \{1, 2, \dots\}$, $\mathbb{E}[Y] = \frac{1}{p}$, $\operatorname{Var}(Y) = \frac{q}{p^2}$.
- **Memoryless Property (Discrete):**
  $$P(X \ge s + t \mid X \ge s) = P(X \ge t)$$
  The Geometric distribution is the **only** discrete distribution with the memoryless property!

---

## 5. Negative Binomial Distribution: $\operatorname{NBin}(r, p)$

- **Story:** Number of failures observed before achieving the $r$-th success in independent $\operatorname{Bern}(p)$ trials.
- **Support:** $k \in \{0, 1, 2, \dots\}$
- **PMF:**
  $$P(X = k) = \binom{k + r - 1}{r - 1} p^r (1 - p)^k = \binom{k + r - 1}{k} p^r (1 - p)^k$$
  *(Logic: The $r$-th success must occur on trial $k + r$. Among the preceding $k + r - 1$ trials, exactly $r - 1$ must be successes)*.
- **Mean & Variance:** Since $X = \sum_{j=1}^r G_j$ where $G_j \overset{\text{i.i.d.}}{\sim} \operatorname{Geom}(p)$:
  $$\mathbb{E}[X] = \frac{r(1 - p)}{p}, \quad \operatorname{Var}(X) = \frac{r(1 - p)}{p^2}$$

---

## 6. Poisson Distribution: $\operatorname{Pois}(\lambda)$

- **Story:** Counts occurrences of rare events over a fixed interval of time or space, where events occur at a constant average rate $\lambda > 0$ independently of the time since the last event.
- **Support:** $k \in \{0, 1, 2, \dots\}$
- **PMF:**
  $$P(X = k) = \frac{e^{-\lambda} \lambda^k}{k!}$$
- **Mean & Variance:**
  $$\mathbb{E}[X] = \lambda, \quad \operatorname{Var}(X) = \lambda$$
  *(Equidispersion: Mean equals variance)*.
- **Poisson Approximation to Binomial (Law of Rare Events):**
  If $n \to \infty$ and $p \to 0$ such that $np \to \lambda$ (constant):
  $$\binom{n}{k} p^k (1 - p)^{n - k} \xrightarrow{n \to \infty} \frac{e^{-\lambda} \lambda^k}{k!}$$
  *Rule of thumb:* A good approximation when $n \ge 20$ and $p \le 0.05$, or $n \ge 100$ and $np \le 10$.
- **Sum of Independent Poissons:**
  If $X_1 \sim \operatorname{Pois}(\lambda_1)$ and $X_2 \sim \operatorname{Pois}(\lambda_2)$ are independent:
  $$X_1 + X_2 \sim \operatorname{Pois}(\lambda_1 + \lambda_2)$$

---

## Summary Reference Table

| Distribution | Notation | Parameters | PMF $P(X = k)$ | Mean $\mathbb{E}[X]$ | Variance $\operatorname{Var}(X)$ |
|---|---|---|---|---|---|
| **Bernoulli** | $\operatorname{Bern}(p)$ | $p \in [0, 1]$ | $p^k (1-p)^{1-k},\; k \in \{0, 1\}$ | $p$ | $p(1-p)$ |
| **Binomial** | $\operatorname{Bin}(n, p)$ | $n \in \mathbb{N}, p \in [0, 1]$ | $\binom{n}{k} p^k (1-p)^{n-k},\; k \in \{0..n\}$ | $np$ | $np(1-p)$ |
| **Hypergeometric** | $\operatorname{HGeom}(w, b, n)$ | $w, b, n \in \mathbb{N}$ | $\frac{\binom{w}{k}\binom{b}{n-k}}{\binom{w+b}{n}}$ | $n \frac{w}{w+b}$ | $n \frac{w}{w+b} \frac{b}{w+b} \frac{w+b-n}{w+b-1}$ |
| **Geometric** | $\operatorname{Geom}(p)$ | $p \in (0, 1]$ | $(1-p)^k p,\; k \ge 0$ | $\frac{1-p}{p}$ | $\frac{1-p}{p^2}$ |
| **Negative Binomial** | $\operatorname{NBin}(r, p)$ | $r \in \mathbb{N}, p \in (0, 1]$ | $\binom{k+r-1}{r-1} p^r (1-p)^k,\; k \ge 0$ | $\frac{r(1-p)}{p}$ | $\frac{r(1-p)}{p^2}$ |
| **Poisson** | $\operatorname{Pois}(\lambda)$ | $\lambda > 0$ | $\frac{e^{-\lambda}\lambda^k}{k!},\; k \ge 0$ | $\lambda$ | $\lambda$ |

---

## Cross-Topic Connections / Exam Relevance

- **Queueing Theory:** Poisson arrivals directly define Markovian arrival streams in the [[M-M-1 Queue]].
- **Estimation:** Binomial and Poisson parameter estimation are central paradigms in [[Maximum Likelihood Estimation]] and [[Beta-Binomial Conjugate Updating Formula]].
- **Indicator Variables:** Expectation and variance proofs for these distributions are standard exam questions relying on indicator decomposition.

---

## Sources & Traceability

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lectures 6–9, pages 17–29)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_4.pdf` & `5.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Chapters 3 & 4)
