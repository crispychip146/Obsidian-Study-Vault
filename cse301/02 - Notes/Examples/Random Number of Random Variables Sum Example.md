---
type: example
course: cse301
status: active
order: 34
---

# Random Number of Random Variables Sum Example

> 📖 **Reading Order:** Step 34 of 103 | **Module 3:** Conditional Probability and Conditioning  
> ◄ **Previous:** [[Ace of Spades Conditioning Paradox Example]] | ► **Next:** [[Problem — Compound Random Sum via Adam and Eve's Laws]]
---
## Problem

In computer systems, network modeling, and e-commerce, cumulative workloads often involve a **random number of random quantities**:
- $N$ = number of customer requests arriving per minute at a cloud server, where $N \sim \operatorname{Pois}(\lambda)$ with $\lambda = 100$ requests/min.
- $X_i$ = computational cost (CPU milliseconds) required by the $i$-th request, where $X_i \overset{\text{i.i.d.}}{\sim} \operatorname{Exp}(\mu)$ with rate $\mu = 0.05$ (meaning mean service time is $\mathbb{E}[X] = 1/\mu = 20\text{ ms}$).
- Assume $N$ is independent of all $X_1, X_2, \dots$.

Let the total cumulative CPU time demanded in one minute be:
$$S_N = \sum_{i=1}^N X_i \quad (\text{with } S_0 = 0)$$

**Goal:**
1. Compute the expected cumulative workload $\mathbb{E}[S_N]$.
2. Compute the variance of the cumulative workload $\operatorname{Var}(S_N)$.
3. Interpret the relative contributions of arrival randomness vs. service time randomness.
---
## Given

- Prior parameters, sample observations, state transition matrix, or probability distributions as specified.

---

## Required

- Calculate posterior distributions, point estimates, confidence intervals, or stationary distributions.

---

## Understanding the Problem and Choosing the Method

Identify the random variables, state the conditional distributions, select the appropriate probabilistic law or updating formula, and execute the algebraic substitutions step by step.

---

## Solution

### Step-by-Step Solution

### Step 1: Preliminary Distributions and Moments
- For $N \sim \operatorname{Pois}(\lambda)$:
  $$\mathbb{E}[N] = \lambda = 100$$
  $$\operatorname{Var}(N) = \lambda = 100$$

- For $X_i \sim \operatorname{Exp}(\mu)$ with $\mu = 0.05$:
  $$\mathbb{E}[X] = \frac{1}{\mu} = 20\text{ ms}$$
  $$\operatorname{Var}(X) = \frac{1}{\mu^2} = \frac{1}{0.0025} = 400\text{ ms}^2$$

---

### Step 2: Expected Cumulative Workload via Adam's Law

We condition on $N$:
Given $N = n$, $S_n$ is the sum of $n$ independent identically distributed variables:
$$\mathbb{E}[S_N \mid N = n] = \mathbb{E}\left[ \sum_{i=1}^n X_i \right] = n \mathbb{E}[X]$$
Treating $N$ as random:
$$\mathbb{E}[S_N \mid N] = N \mathbb{E}[X]$$

Applying [[Adam's Law (Law of Total Expectation)]]:
$$\mathbb{E}[S_N] = \mathbb{E}\left[ \mathbb{E}[S_N \mid N] \right] = \mathbb{E}[N \mathbb{E}[X]] = \mathbb{E}[N] \mathbb{E}[X]$$

Substituting the numerical values:
$$\mathbb{E}[S_N] = 100 \times 20 = 2000\text{ ms} = 2.0\text{ seconds}$$

---

### Step 3: Workload Variance via Eve's Law

Applying [[Eve's Law (Law of Total Variance)]]:
$$\operatorname{Var}(S_N) = \underbrace{\mathbb{E}\left[ \operatorname{Var}(S_N \mid N) \right]}_{\mathbf{EV}} + \underbrace{\operatorname{Var}\left( \mathbb{E}[S_N \mid N] \right)}_{\mathbf{VE}}$$

1. **Calculate the Conditional Variance:**
   Given $N = n$, the $X_i$ are independent:
   $$\operatorname{Var}(S_N \mid N = n) = \operatorname{Var}\left( \sum_{i=1}^n X_i \right) = n \operatorname{Var}(X)$$
   Treating $N$ as random:
   $$\operatorname{Var}(S_N \mid N) = N \operatorname{Var}(X)$$

2. **Evaluate the $\mathbf{EV}$ term (Expected Conditional Variance):**
   $$\mathbf{EV} = \mathbb{E}[N \operatorname{Var}(X)] = \operatorname{Var}(X) \mathbb{E}[N] = 400 \times 100 = 40,000\text{ ms}^2$$

3. **Evaluate the $\mathbf{VE}$ term (Variance of Conditional Expectation):**
   $$\mathbf{VE} = \operatorname{Var}(N \mathbb{E}[X]) = (\mathbb{E}[X])^2 \operatorname{Var}(N) = 20^2 \times 100 = 400 \times 100 = 40,000\text{ ms}^2$$

4. **Sum the Components:**
   $$\operatorname{Var}(S_N) = \mathbf{EV} + \mathbf{VE} = 40,000 + 40,000 = 80,000\text{ ms}^2$$

The standard deviation is:
$$\operatorname{SD}(S_N) = \sqrt{80,000} \approx 282.84\text{ ms}$$

---
### Variance Decomposition Analysis

- **$\mathbf{EV} = 40,000$ ($50\%$ of total variance):** Arises from the internal randomness of service times $X_i$ (some jobs take 2 ms, others take 80 ms).
- **$\mathbf{VE} = 40,000$ ($50\%$ of total variance):** Arises from the external arrival randomness of $N$ (some minutes see 85 requests, others see 115 requests).

Notice that if the number of requests were fixed at exactly $N = 100$ (deterministic), the total variance would be only $40,000$. The fluctuation in traffic volume $N$ doubles the system variance!
---
## Result

The mathematical derivation confirms the target probability or estimator value.

---

## Why This Works

The solution holds because every step follows directly from Bayes' rule, the law of total probability, or properties of expectation and variance.

---

## Common Mistakes

- Forgetting normalization constants when evaluating continuous posterior densities.
- Misidentifying degrees of freedom in chi-square tests.

---

## General Method

Extract the reusable problem-solving pattern: define random variables, write down the joint distribution, condition on observed data, and normalize the resulting distribution.

---

## Related Concepts

- [[Adam's Law (Law of Total Expectation)]] — Foundational expectation law.
- [[Eve's Law (Law of Total Variance)]] — General variance formula.
- [[Continuous Probability Distributions]] — Exponential distribution properties.
- [[Discrete Probability Distributions]] — Poisson distribution properties.
---
## Sources

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lecture 16, pages 50–53)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_10.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Section 9.3: Example 9.3.2)
