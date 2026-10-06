---
type: problem
course: cse301
status: active
order: 88
---

# Problem — Patty and Max Gambler's Ruin

> 📖 **Reading Order:** Step 88 of 103 | **Module 10:** Stochastic Processes  
> ◄ **Previous:** [[Hardy-Weinberg Law Markov Chain Example]] | ► **Next:** [[Problem — Four-Day Weather Forecast]]
---
## Problem

Patty and Max flip pennies in successive independent rounds. On each flip, Patty wins with probability $p = 0.6$ and loses with probability $q = 1 - p = 0.4$. 

Patty starts with $5$ pennies, and Max starts with $10$ pennies. The game continues until one player wins all the pennies (i.e., until either Patty reaches $15$ pennies or is reduced to $0$ pennies).

1. What is the probability that Patty wipes Max out (wins all 15 pennies)?
2. What is the probability that Patty is ruined?
3. Compare Patty's initial fraction of the total wealth with her winning probability, and interpret the result.
---
## Given

- Initial fortune of Patty: $i = 5$
- Initial fortune of Max: $10$
- Total capital in the system: $N = 5 + 10 = 15$
- Probability of winning $1$ penny on each flip: $p = 0.6$
- Probability of losing $1$ penny on each flip: $q = 0.4$
- Stopping states: $0$ (Patty ruined) and $N = 15$ (Max ruined)
---
## Required

1. Win probability $P_5 = P(\text{fortune reaches } 15 \mid X_0 = 5)$.
2. Ruin probability $Q_5 = 1 - P_5$.
3. Comparison between initial stake proportion $i/N$ and win probability $P_5$.
---
## Concepts Tested

- [[Markov Chain]]
- [[Classification of States in Markov Chains]] (Absorbing states, transient states)
- [[Gambler's Ruin Formula]]
---
## Prerequisites

- [[Markov Chain]]
- [[Gambler's Ruin Formula]]
---
## Question Type

Numerical / Applied Probability
---
## Solution

### Understanding the Situation
Interpret the given sample space, random variables, and event conditions.

### Developing the Key Idea
Select the governing probabilistic principle (e.g. Chapman-Kolmogorov, Adam's Law, Central Limit Theorem, or likelihood maximization) and verify that conditions hold.

### Working Through the Solution
### Solution

### Step 1: Identify the Parameter Ratio $q/p$
$$\frac{q}{p} = \frac{0.4}{0.6} = \frac{4}{6} = \frac{2}{3}$$
Since $p \neq 1/2$ ($q/p = 2/3 \neq 1$), we use the asymmetric Gambler's Ruin formula.

---

### Step 2: Apply the Gambler's Ruin Formula
The probability of reaching $N$ starting from $i$ is:
$$P_i = \frac{1 - (q/p)^i}{1 - (q/p)^N}$$

Substitute $i = 5$, $N = 15$, and $q/p = 2/3$:
$$P_5 = \frac{1 - (2/3)^5}{1 - (2/3)^{15}}$$

---

### Step 3: Numerical Computation
Evaluate $(2/3)^5$:
$$\left(\frac{2}{3}\right)^5 = \frac{32}{243} \approx 0.131687$$
$$1 - \left(\frac{2}{3}\right)^5 = 1 - 0.131687 = 0.868313$$

Evaluate $(2/3)^{15}$:
$$\left(\frac{2}{3}\right)^{15} = \frac{32768}{14348907} \approx 0.00228366$$
$$1 - \left(\frac{2}{3}\right)^{15} = 1 - 0.00228366 = 0.99771634$$

Divide the numerator by the denominator:
$$P_5 = \frac{0.868313}{0.997716} \approx 0.8703008 \approx 0.8703 \quad (87.03\%)$$

---

### Step 4: Compute the Probability of Ruin
$$Q_5 = 1 - P_5 = 1 - 0.8703 = 0.1297 \quad (12.97\%)$$

---

### Step 5: Interpretation and Comparison with Initial Wealth Share
Patty's initial share of total capital was:
$$\frac{i}{N} = \frac{5}{15} = \frac{1}{3} \approx 33.33\%$$

If the game were fair ($p = 0.5$), Patty's chance of winning would be exactly $33.33\%$.
However, because Patty possesses a $60\%$ to $40\%$ single-trial advantage, her probability of wiping Max out surges to **$87.03\%$**.

---
### Key Idea

In a repeated random walk with absorbing barriers, win probability does not scale linearly with initial capital unless $p = 1/2$. A small per-trial edge ($p > 1/2$) compounds exponentially over a multi-round horizon because $(q/p)^i$ decays rapidly with $i$.

---
### Exam Pattern

This is a classic examination problem testing:
1. Recognizing a Gambler's Ruin scenario from a narrative description.
2. Correctly identifying $i$, $N$, $p$, and $q$.
3. Evaluating the ratio $(q/p)$ and substituting into the closed-form formula without arithmetic slips.
4. Explaining the qualitative behavior of biased vs fair walks.

---
### Related Problems

- [[Problem — Rain Prediction Two Days Ahead]]

---
### Related Concepts

- [[Gambler's Ruin Formula]]
- [[Markov Chain]]
- [[Classification of States in Markov Chains]]

---

### Result and Interpretation
The final analytical solution and numerical metrics are rigorously verified against probability axioms.

---

## Reusable Insight

Always decompose complex event probabilities by conditioning on a partition of the sample space (Law of Total Probability), or by writing indicator random variables to exploit linearity of expectation.

---

## Common Mistakes

- **Incorrect Boundary Value ($N$):** Setting $N = 10$ (Max's pennies) instead of the total pennies in play $N = 5 + 10 = 15$.
- **Inverted Odds Ratio:** Calculating $p/q = 1.5$ instead of $q/p = 2/3$. Remember that $q$ (loss probability) is in the numerator.
- **Using the Fair Game Formula:** Applying $P_i = i/N = 5/15 = 0.3333$, ignoring the fact that $p = 0.6 \neq 0.5$.
---
## Exam Pattern

Standard BUET CSE 301 final exam question testing probability bounds, Markov chain stationarity, or statistical parameter estimation.

---

## Related Problems

- [[Problem — Birthday Collisions and Approximation]]
- [[Problem — Four-Day Weather Forecast]]

---

## Related Concepts

- [[Random Variables and Probability Distributions]]
- [[Law of Total Probability and Bayes' Rule]]

---

## Source

- [[cse301/01 - Sources/Lectures/Markov_Chain.pdf]] (Slide 36)
- [[cse301/01 - Sources/Textbooks/Sheldon M. Ross book Markov Chain Chapter.pdf]] (Example 4.22, pp. 233–234)
