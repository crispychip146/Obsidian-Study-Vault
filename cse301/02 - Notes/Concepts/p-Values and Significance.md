---
type: concept
course: cse301
status: active
order: 58
---

# p-Values and Significance

> 📖 **Reading Order:** Step 58 of 92 | **Module 9:** Hypothesis Testing  
> ◄ **Previous:** [[Hypothesis Testing Framework]] | ► **Next:** [[Wald Test Statistic]]

---

## Building the idea

A $p$-value is calculated in a world where the null model is assumed true. It is the probability, under that model, of a statistic at least as extreme as the observed one, using the test's specified direction or discrepancy measure.

It is not the probability that the null is true. That latter question reverses the conditioning and would require a model for hypotheses, such as a Bayesian prior. Nor is $1-p$ the chance that a finding will replicate.

For a two-sided normal statistic, extremeness is distance from zero, giving $2[1-\Phi(|w|)]$. For a chi-square discrepancy, larger values alone are more extreme. The reference distribution and tail definition must match the statistic. Discrete tests can yield conservative $p$-values because not every significance level is attainable exactly.

## Definition

The **$p$-value** is the probability, computed assuming the null hypothesis $H_0$ is true, of observing a test statistic at least as extreme as (or more extreme than) the value actually observed in the sample data.

Formally, let $T(\mathbf{X})$ be a test statistic where large values provide evidence against $H_0$, and let $t_{\text{obs}} = T(\mathbf{x})$ denote the realized value calculated from the observed dataset $\mathbf{x}$. The $p$-value is defined as:

$$p = \sup_{\theta \in \Theta_0} P_\theta\left(T(\mathbf{X}) \ge t_{\text{obs}}\right)$$

### Alternative Operational Definition
The $p$-value is the **smallest significance level $\alpha$** at which a hypothesis test would reject the null hypothesis $H_0$:
$$p = \inf\big\{\alpha \in (0, 1) : T(\mathbf{x}) \in R_\alpha\big\}$$

---

## How It Works

### Standard Interpretation Scale

While modern statistics encourages reporting exact numerical $p$-values rather than binary thresholds, the following scientific scale is widely recognized:

| $p$-value Range | Weight of Evidence Against $H_0$ |
|---|---|
| $p < 0.01$ | **Very Strong Evidence** against $H_0$ |
| $0.01 \le p < 0.05$ | **Strong Evidence** against $H_0$ |
| $0.05 \le p < 0.10$ | **Weak / Marginal Evidence** against $H_0$ |
| $p \ge 0.10$ | **Little to No Evidence** against $H_0$ (consistent with chance) |

---
### 1. One-Sided Right-Tail Test
For $H_0: \theta \le \theta_0$ vs $H_1: \theta > \theta_0$:
$$p = P_{\theta_0}\left(T(\mathbf{X}) \ge t_{\text{obs}}\right) = 1 - F_{T}(t_{\text{obs}})$$

### 2. One-Sided Left-Tail Test
For $H_0: \theta \ge \theta_0$ vs $H_1: \theta < \theta_0$:
$$p = P_{\theta_0}\left(T(\mathbf{X}) \le t_{\text{obs}}\right) = F_{T}(t_{\text{obs}})$$

### 3. Two-Sided Symmetric Test (e.g., Wald Test)
For $H_0: \theta = \theta_0$ vs $H_1: \theta \ne \theta_0$, where the null distribution of $W$ is standard normal $N(0, 1)$ and $w_{\text{obs}} = \frac{\hat{\theta} - \theta_0}{\widehat{\text{se}}}$:
$$p = P(\lvert Z \rvert \ge \lvert w_{\text{obs}} \rvert) = 2 \cdot P(Z \le -\lvert w_{\text{obs}} \rvert) = 2\Phi(-\lvert w_{\text{obs}} \rvert)$$

---
### Crucial Fallacies and Misinterpretations

The American Statistical Association (ASA) highlighted that the $p$-value is one of the most frequently misunderstood concepts in all of science.

### ❌ Fallacy 1: "The $p$-value is the probability that $H_0$ is true."
**Reality:** 
$$p \ne P(H_0 \mid \text{data})$$
The $p$-value is computed **assuming $H_0$ is $100\%$ true from the beginning**: $P(\text{data} \mid H_0)$. It is a statement about the data, not about the hypothesis. Finding $P(H_0 \mid \text{data})$ requires [[Bayesian Inference]] with a prior distribution.

### ❌ Fallacy 2: "$1 - p$ is the probability that the alternative $H_1$ is true."
**Reality:** 
$$1 - p \ne P(H_1 \mid \text{data})$$
A tiny $p$-value ($p = 0.001$) does **not** mean there is a $99.9\%$ chance that your alternative discovery is correct.

### ❌ Fallacy 3: "$p > 0.05$ proves that the null hypothesis is true."
**Reality:** 
A large $p$-value simply means the sample size was too small or the variance was too large to detect a difference. It indicates **insufficient evidence**, never proof of equality.

---
### The Null Distribution of the $p$-Value

An extraordinary mathematical property of the $p$-value:
> **Theorem:** 
> When the null hypothesis $H_0$ is true and the test statistic $T$ has a continuous distribution, the random variable $P$ is **uniformly distributed on the interval $(0, 1)$**:
> $$P \sim \text{Uniform}(0, 1)$$

### Proof
Under $H_0$, let $F$ be the continuous CDF of $T$. 
The $p$-value is $P = 1 - F(T)$.
For any $u \in (0, 1)$:
$$P_{H_0}(P \le u) = P_{H_0}(1 - F(T) \le u) = P_{H_0}(F(T) \ge 1 - u)$$
By the Probability Integral Transform, $F(T) \sim \text{Uniform}(0, 1)$, so:
$$P_{H_0}(F(T) \ge 1 - u) = 1 - (1 - u) = u$$
Since $P(P \le u) = u$, $P \sim \text{Uniform}(0, 1) \quad \blacksquare$.

This beautiful result explains why setting a threshold $\alpha = 0.05$ guarantees that the Type I error rate is exactly $5\%$: under $H_0$, $P(P \le 0.05) = 0.05$.

---

## What to carry forward

The significance threshold controls an error rate across repetitions when the procedure is valid. [[Multiple Testing and False Discovery Rate]] shows why repeatedly applying it creates an additional problem.

## Related notes

- [[Multiple Testing and False Discovery Rate]]

## Sources

- [[01 - Sources/Lectures/CSE301_Hypothesis_Test.pdf]]
