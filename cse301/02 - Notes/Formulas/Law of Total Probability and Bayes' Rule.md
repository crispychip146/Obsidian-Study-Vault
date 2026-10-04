---
type: formula
course: cse301
status: active
order: 19
---

# Law of Total Probability and Bayes' Rule

> 📖 **Reading Order:** Step 19 of 92 | **Module 3:** Conditional Probability and Conditioning  
> ◄ **Previous:** [[Conditional Expectation]] | ► **Next:** [[Adam's Law (Law of Total Expectation)]]

---

---

## The Question and Earlier Knowledge

What analytical relationship or closed-form expectation governs Law of Total Probability and Bayes' Rule, and how can we compute it directly from constituent probability terms? In complex probability models, calculating probabilities or moments directly is often intractable without decomposing expectations across conditioning partitions or inequalities.

---

## Developing the Formula

By decomposing joint distributions into conditional components, expanding algebraic products, or applying geometric series sums, Law of Total Probability and Bayes' Rule compresses complex probabilistic reasoning into a clean, reusable formula.

---

## Formula

### 1. Law of Total Probability (LTP)
Let $B_1, B_2, \dots, B_n$ form a **partition** of the sample space $S$ (i.e., they are mutually disjoint, $B_i \cap B_j = \emptyset$ for $i \ne j$, and $\bigcup_{i=1}^n B_i = S$, with $P(B_i) > 0$).
For any event $A$:
$$P(A) = \sum_{i=1}^n P(A \cap B_i) = \sum_{i=1}^n P(A \mid B_i) P(B_i)$$

#### Continuous Version:
If $X$ and $Y$ are continuous random variables with joint density $f_{X, Y}(x, y)$:
$$P(A) = \int_{-\infty}^\infty P(A \mid X = x) f_X(x) \, dx$$

---

### 2. Bayes' Rule (Bayes' Theorem)
Bayes' Rule provides the mathematical mechanism to invert cause and effect—calculating the posterior probability of a hypothesis $B_k$ after observing evidence $A$:

$$P(B_k \mid A) = \frac{P(A \cap B_k)}{P(A)} = \frac{P(A \mid B_k) P(B_k)}{\sum_{i=1}^n P(A \mid B_i) P(B_i)}$$

#### Continuous Parameter Version (Bayesian Inference):
For a continuous parameter $\theta$ and observed data $x$:
$$f(\theta \mid x) = \frac{f(x \mid \theta) f(\theta)}{\int f(x \mid \theta') f(\theta') \, d\theta'} \propto \mathcal{L}(\theta) f(\theta)$$
where:
- $f(\theta)$ is the **prior distribution**.
- $f(x \mid \theta) = \mathcal{L}(\theta)$ is the **likelihood**.
- $f(\theta \mid x)$ is the **posterior distribution**.

---

---

## Variables

| Symbol | Meaning |
|---|---|
| $X, Y$ | Random variables governed by underlying probability distributions |
| $\mathbb{E}[\cdot]$ | Expected value operator |
| $\text{Var}(\cdot)$ | Variance operator |

---

## Conditions

- Random variables must possess finite first and second moments (well-defined expectations).
- Probability distributions must satisfy standard non-negativity and total probability integration axioms.

---

## Intuition

### Component Breakdown & Terminology

- **Prior Probability $P(B_k)$:** The baseline belief in hypothesis $B_k$ prior to seeing any experimental data.
- **Likelihood $P(A \mid B_k)$:** The probability that evidence $A$ would be generated if hypothesis $B_k$ were true.
- **Marginal Likelihood (Evidence) $P(A) = \sum_i P(A \mid B_i)P(B_i)$:** The total probability of observing evidence $A$ across all possible states of the world. Acts as a normalizing constant.
- **Posterior Probability $P(B_k \mid A)$:** The updated belief in hypothesis $B_k$ after incorporating evidence $A$.

$$\text{Posterior} = \frac{\text{Likelihood} \times \text{Prior}}{\text{Evidence}}$$

---

---

## Derivation

Derived by applying definition of expectation, interchanging summation/integrals via Fubini's theorem, and collecting terms.

---

## Example

### Classic Application Example: Rare Disease Testing

Suppose a rare disease affects $0.1\%$ of the population ($P(D) = 0.001$).
A diagnostic test has:
- Sensitivity (True Positive Rate): $P(+ \mid D) = 0.99$
- Specificity (True Negative Rate): $P(- \mid D^c) = 0.95 \implies \text{False Positive Rate } P(+ \mid D^c) = 0.05$.

A randomly selected individual tests positive ($+$). What is the probability they actually have the disease ($P(D \mid +)$)?

### Step 1: Compute Evidence $P(+)$ using LTP
$$P(+) = P(+ \mid D) P(D) + P(+ \mid D^c) P(D^c)$$
$$P(+) = (0.99)(0.001) + (0.05)(0.999) = 0.00099 + 0.04995 = 0.05094$$

### Step 2: Apply Bayes' Rule
$$P(D \mid +) = \frac{P(+ \mid D) P(D)}{P(+)} = \frac{0.00099}{0.05094} \approx 0.0194 \approx 1.94\%$$

**Surprising Insight (Base Rate Fallacy):** Even though the test is $99\%$ accurate on sick patients, because the disease is extremely rare, over $98\%$ of positive test results are false positives!

---

---

## Common Mistakes

- Confusing conditional variance with the variance of conditional expectation (Eve's Law components).
- Forgetting that linearity of expectation holds unconditionally, whereas $\mathbb{E}[XY] = \mathbb{E}[X]\mathbb{E}[Y]$ requires independence.

---

## Related Concepts

- [[Conditional Probability and Independence]] — Definition of conditioning and multiplication rule.
- [[Bayesian Inference]] — Statistical inference paradigm built on Bayes' Rule.
- [[Monty Hall Problem Example]] — Bayesian solution to the famous game show puzzle.

---

---

## Prerequisites

- [[Random Variables and Probability Distributions]]

---

## Problems

- [[Problem — Birthday Collisions and Approximation]]

---

## Sources

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lectures 4 & 5, pages 10–16)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_3.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Section 2.3 & 2.4)
