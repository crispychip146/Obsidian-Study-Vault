---
type: problem
course: cse301
status: active
order: 34
---

# Problem — CLT Implications for the Weak Law of Large Numbers

> 📖 **Reading Order:** Step 34 of 92 | **Module 5:** Convergence of Random Variables and Asymptotics  
> ◄ **Previous:** [[Normal Approximation to Binomial and Poisson Example]] | ► **Next:** [[Point Estimation]]

---

## Problem

A polling agency wants to estimate the true proportion $p$ of users who prefer a new user interface over the old one. They survey $n$ independent users, modeling their responses as $X_1, X_2, \dots, X_n \overset{\text{i.i.d.}}{\sim} \operatorname{Bern}(p)$.
The agency estimates $p$ using the sample proportion:
$$\hat{p}_n = \bar{X}_n = \frac{1}{n} \sum_{i=1}^n X_i$$
The agency requires that their estimate $\hat{p}_n$ is within $\pm 0.03$ (3 percentage points) of the true value $p$ with at least $95\%$ confidence:
$$P\left( \lvert \hat{p}_n - p \rvert \le 0.03 \right) \ge 0.95 \iff P\left( \lvert \hat{p}_n - p \rvert > 0.03 \right) \le 0.05$$

1. **Worst-Case Variance:** Since the true value $p$ is unknown, find the maximum possible value of $\operatorname{Var}(X_i) = p(1 - p)$ for $p \in [0, 1]$.
2. **Chebyshev Sample Size:** Determine the minimum sample size $n_{\text{Cheb}}$ guaranteed to satisfy the requirement using the [[Chebyshev Inequality]].
3. **CLT Sample Size:** Determine the minimum sample size $n_{\text{CLT}}$ required using the [[Central Limit Theorem]] normal approximation (taking $z_{0.025} = 1.96$).
4. **Comparison & Theoretical Connection:**
   - Compare the sample sizes $n_{\text{Cheb}}$ and $n_{\text{CLT}}$ and discuss the practical cost implications for engineering telemetry.
   - Prove mathematically that the Central Limit Theorem implies the Weak Law of Large Numbers.

---

## Solution

The normalized fluctuation $Z_n=\sqrt n(\bar X_n-\mu)/\sigma$ converges to a normal distribution. We need to turn that statement into $P(|\bar X_n-\mu|>\epsilon)\to0$.

Do not simply substitute an increasing threshold into a limit theorem stated for fixed thresholds. Instead choose a fixed $K$ large enough that the normal tail outside $[-K,K]$ is small. Convergence in distribution makes $P(|Z_n|>K)$ close to that tail for sufficiently large $n$. Eventually $\epsilon\sqrt n/\sigma>K$, so $P(|\bar X_n-\mu|>\epsilon)\le P(|Z_n|>K)$. Since the fixed-tail target can be made arbitrarily small, the desired probability tends to zero.

This is a tightness argument: normalized fluctuations stay on a bounded probabilistic scale while the threshold grows. A CLT-based finite sample calculation is an approximation; a [[Chebyshev Inequality|Chebyshev]] sample-size calculation gives a distribution-free guarantee under its variance assumptions.

### Part 1: Worst-Case Variance
For $X_i \sim \operatorname{Bern}(p)$:
$$\sigma^2 = \operatorname{Var}(X_i) = p(1 - p) = p - p^2$$
To find the maximum, differentiate with respect to $p$:
$$\frac{d}{dp}[p - p^2] = 1 - 2p = 0 \implies p = \frac{1}{2}$$
$$\sigma_{\max}^2 = \left(\frac{1}{2}\right)\left(1 - \frac{1}{2}\right) = \frac{1}{4} = 0.25$$

Thus, without knowing $p$, we can safely bound $\operatorname{Var}(X_i) \le 0.25$.

---

### Part 2: Chebyshev Sample Size Determination
By Chebyshev's inequality applied to $\bar{X}_n$:
$$P\left( \lvert \bar{X}_n - p \rvert > \epsilon \right) \le \frac{\operatorname{Var}(\bar{X}_n)}{\epsilon^2} = \frac{\sigma^2}{n\epsilon^2} \le \frac{0.25}{n\epsilon^2}$$

We require this upper bound to be at most $\alpha = 0.05$ with $\epsilon = 0.03$:
$$\frac{0.25}{n (0.03)^2} \le 0.05$$
$$n \ge \frac{0.25}{0.05 \times (0.03)^2} = \frac{5}{(0.03)^2} = \frac{5}{0.0009} \approx 5,555.56$$

Rounding up:
$$n_{\text{Cheb}} = 5,556 \text{ users}$$

---

### Part 3: CLT Sample Size Determination
By the Central Limit Theorem:
$$Z_n = \frac{\bar{X}_n - p}{\sqrt{\frac{p(1 - p)}{n}}} \xrightarrow{d} \mathcal{N}(0, 1)$$

We want:
$$P\left( \lvert \bar{X}_n - p \rvert \le 0.03 \right) = P\left( \left\lvert \frac{\bar{X}_n - p}{\sqrt{\sigma^2 / n}} \right\rvert \le \frac{0.03}{\sqrt{\sigma^2 / n}} \right) \ge 0.95$$

For a Standard Normal variable, $P(\lvert Z \rvert \le z^*) = 0.95$ when $z^* = 1.96$ (since $\Phi(1.96) \approx 0.975$).
Therefore, we require:
$$\frac{0.03}{\sqrt{\sigma^2 / n}} \ge 1.96 \implies \frac{0.03 \sqrt{n}}{\sigma} \ge 1.96$$
Using the worst-case standard deviation $\sigma \le \sqrt{0.25} = 0.5$:
$$\sqrt{n} \ge \frac{1.96 \times 0.5}{0.03} = \frac{0.98}{0.03} \approx 32.667$$
$$n \ge (32.667)^2 \approx 1067.1$$

Rounding up:
$$n_{\text{CLT}} = 1,068 \text{ users}$$

---

### Part 4: Comparison & Theoretical Connection

#### 1. Practical Comparison:
- $n_{\text{Cheb}} = 5,556$
- $n_{\text{CLT}} = 1,068$
- The Chebyshev bound demands more than **$5\times$ as much data** as the CLT normal approximation.
- Chebyshev uses only the variance and gives a rigorous but conservative guarantee. The CLT calculation uses a normal approximation and yields an approximate sample size. For these Bernoulli data, an exact binomial calculation can assess finite-sample coverage.

#### 2. Proof that the CLT implies the weak law

Let $Z_n=\sqrt n(\bar X_n-\mu)/\sigma\Rightarrow Z\sim N(0,1)$, with $0<\sigma<\infty$. Fix an error tolerance $\epsilon>0$ and a probability target $\delta>0$.

Choose a fixed $K$ such that $P(|Z|\ge K)<\delta/2$. Since the normal distribution has no atoms at $\pm K$, convergence in distribution gives $P(|Z_n|\ge K)<\delta$ for all sufficiently large $n$. Also, $\epsilon\sqrt n/\sigma$ eventually exceeds $K$. Consequently,

$$P(|\bar X_n-\mu|\ge\epsilon)=P(|Z_n|\ge\epsilon\sqrt n/\sigma)\le P(|Z_n|\ge K)<\delta.$$

Since $\delta$ was arbitrary, this proves convergence in probability. The proof uses a fixed continuity threshold before comparing it with the increasing threshold; an informal normal approximation at a moving threshold is not a rigorous limit argument.

---

## Common Mistakes

1. **Forgetting $\sqrt{n}$ in the denominator:** The standard error of the sample mean is $\frac{\sigma}{\sqrt{n}}$, not $\frac{\sigma}{n}$.
2. **Confusing 1-sided and 2-sided tail critical values:** For $95\%$ two-sided coverage, each tail receives $2.5\%$, which corresponds to $z_{0.025} = 1.96$, not $z_{0.05} = 1.645$.

---

## What to carry forward

The CLT implies the weak law under these assumptions, but their finite-sample promises differ. Keep approximate confidence and guaranteed probability bounds separate when interpreting a sample-size result.

## Related notes

- [[Chebyshev Inequality|Chebyshev]]

## Source

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lectures 17, 20, 21, pages 54–63)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_11.pdf` (Problem 3 & 4)
- **Question ID:** `Q-CSE301-017`
