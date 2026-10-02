---
type: problem
course: cse301
status: active
order: 24
---

# Problem — Compound Random Sum via Adam and Eve's Laws

> 📖 **Reading Order:** Step 24 of 92 | **Module 3:** Conditional Probability and Conditioning  
> ◄ **Previous:** [[Random Number of Random Variables Sum Example]] | ► **Next:** [[Markov Inequality]]

---

## Problem Statement

A distributed database cluster receives a random number $N$ of write transactions per second, where $N \sim \operatorname{Bin}(m, p)$ with $m = 200$ client threads and transmission probability $p = 0.4$.
Each committed write transaction $i$ writes $X_i$ megabytes of log data, where $X_1, X_2, \dots$ are i.i.d. continuous random variables following a Gamma distribution:
$$X_i \sim \operatorname{Gamma}(\alpha, \beta) \quad \text{with shape } \alpha = 3 \text{ and rate } \beta = 0.5 \text{ (scale } \theta = 1/\beta = 2\text{)}$$
Assume $N$ and the sequence $\{X_i\}$ are mutually independent. Let $S_N = \sum_{i=1}^N X_i$ be the total volume of log data written to disk per second (with $S_0 = 0$).

1. **Mean Log Volume:** Compute the exact expected total log volume $\mathbb{E}[S_N]$ written per second.
2. **Variance Decomposition:** Use [[Eve's Law (Law of Total Variance)]] to determine the exact variance $\operatorname{Var}(S_N)$ and the standard deviation $\operatorname{SD}(S_N)$.
3. **Relative Variance Attribution:** What percentage of $\operatorname{Var}(S_N)$ is attributable to client transaction randomness vs. individual payload size randomness?
4. **Compound Moment Generating Function:** Derive an analytical expression for the MGF $M_{S_N}(t)$ in terms of the MGFs $M_N(t)$ and $M_X(t)$.

---

## Prerequisites & Relevant Concepts

- [[Discrete Probability Distributions]] — Binomial distribution properties.
- [[Continuous Probability Distributions]] — Gamma distribution properties.
- [[Adam's Law (Law of Total Expectation)]] & [[Eve's Law (Law of Total Variance)]]
- [[Moment Generating Functions]] — MGF conditioning.

---

## Full Step-by-Step Solution

### Part 1: Expected Value $\mathbb{E}[S_N]$
First, compute the parameters of the underlying distributions:
- **For $N \sim \operatorname{Bin}(m, p)$:**
  $$\mathbb{E}[N] = mp = 200 \times 0.4 = 80$$
  $$\operatorname{Var}(N) = mp(1 - p) = 200 \times 0.4 \times 0.6 = 48$$

- **For $X_i \sim \operatorname{Gamma}(\alpha, \beta)$:**
  $$\mathbb{E}[X] = \frac{\alpha}{\beta} = \frac{3}{0.5} = 6\text{ MB}$$
  $$\operatorname{Var}(X) = \frac{\alpha}{\beta^2} = \frac{3}{(0.5)^2} = \frac{3}{0.25} = 12\text{ MB}^2$$

Applying [[Adam's Law (Law of Total Expectation)]]:
$$\mathbb{E}[S_N \mid N] = N \mathbb{E}[X] = 6N$$
$$\mathbb{E}[S_N] = \mathbb{E}[6N] = 6 \mathbb{E}[N] = 6 \times 80 = 480\text{ MB}$$

---

### Part 2: Exact Variance via Eve's Law
By Eve's Law:
$$\operatorname{Var}(S_N) = \mathbb{E}\left[ \operatorname{Var}(S_N \mid N) \right] + \operatorname{Var}\left( \mathbb{E}[S_N \mid N] \right)$$

1. **Conditional Variance:**
   Given $N = n$, the $n$ terms are i.i.d.:
   $$\operatorname{Var}(S_N \mid N = n) = n \operatorname{Var}(X) = 12n$$
   Treating $N$ as random:
   $$\operatorname{Var}(S_N \mid N) = 12N$$

2. **Term 1 ($\mathbf{EV}$):**
   $$\mathbb{E}[\operatorname{Var}(S_N \mid N)] = \mathbb{E}[12N] = 12 \mathbb{E}[N] = 12 \times 80 = 960\text{ MB}^2$$

3. **Term 2 ($\mathbf{VE}$):**
   $$\operatorname{Var}(\mathbb{E}[S_N \mid N]) = \operatorname{Var}(6N) = 6^2 \operatorname{Var}(N) = 36 \times 48 = 1728\text{ MB}^2$$

4. **Total Variance:**
   $$\operatorname{Var}(S_N) = 960 + 1728 = 2688\text{ MB}^2$$
   The standard deviation is:
   $$\operatorname{SD}(S_N) = \sqrt{2688} \approx 51.85\text{ MB}$$

---

### Part 3: Relative Variance Attribution
- **Payload size variability ($\mathbf{EV}$):**
  $$\frac{960}{2688} \times 100\% \approx 35.71\%$$
- **Transaction count variability ($\mathbf{VE}$):**
  $$\frac{1728}{2688} \times 100\% \approx 64.29\%$$

Over $64\%$ of the total fluctuation in disk write volume comes from the fluctuation in how many clients submit transactions, rather than payload sizes!

---

### Part 4: Compound MGF Derivation
By definition of MGF:
$$M_{S_N}(t) = \mathbb{E}[e^{t S_N}]$$

Conditioning on $N$ via Adam's Law:
$$\mathbb{E}[e^{t S_N}] = \mathbb{E}\left[ \mathbb{E}\left[ e^{t \sum_{i=1}^N X_i} \;\middle|\; N \right] \right]$$

For fixed $N = n$, the $X_i$ are independent, so the expectation of the product is the product of expectations:
$$\mathbb{E}\left[ e^{t \sum_{i=1}^n X_i} \right] = \prod_{i=1}^n \mathbb{E}[e^{t X_i}] = [M_X(t)]^n$$
Treating $N$ as random:
$$\mathbb{E}[e^{t S_N} \mid N] = [M_X(t)]^N = e^{N \ln M_X(t)}$$

Now take the expectation over $N$:
$$\mathbb{E}[e^{N \ln M_X(t)}] = M_N(\ln M_X(t))$$

**General Theorem for Compound MGFs:**
$$M_{S_N}(t) = M_N\left( \ln M_X(t) \right)$$

Substituting the specific distributions:
- $M_N(s) = (1 - p + p e^s)^m$
- Therefore, $M_N(\ln M_X(t)) = \left( 1 - p + p e^{\ln M_X(t)} \right)^m = \left( 1 - p + p M_X(t) \right)^m$
- With $M_X(t) = \left(1 - \frac{t}{\beta}\right)^{-\alpha} = \left(1 - 2t\right)^{-3}$:
$$M_{S_N}(t) = \left[ 0.6 + 0.4(1 - 2t)^{-3} \right]^{200} \quad \text{for } t < \frac{1}{2}$$

---

## Common Pitfalls

1. **Forgetting the Squared Mean in VE:** Writing $\operatorname{Var}(N \mathbb{E}[X]) = \mathbb{E}[X]\operatorname{Var}(N)$ instead of $(\mathbb{E}[X])^2 \operatorname{Var}(N)$. Constants pull out squared from variance!
2. **Confusing MGF composition:** The compound MGF is $M_N(\ln M_X(t))$, which equals the Probability Generating Function (PGF) of $N$ evaluated at $M_X(t)$: $G_N(M_X(t))$.

---

## Sources & Traceability

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lecture 15 & 16, pages 47–53)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_10.pdf`
- **Question ID:** `Q-CSE301-015`
