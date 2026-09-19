---
type: formula
course: cse301
status: active
---

# Gambler's Ruin Formula

## Formula

Let a gambler start with initial fortune $i$ and play successive independent games against an opponent, where the gambler wins 1 unit with probability $p$ and loses 1 unit with probability $q = 1 - p$. The contest terminates when the gambler's fortune reaches either $0$ (ruin) or $N$ (success / bankrupting the opponent).

The probability $P_i$ that the gambler reaches fortune $N$ before being ruined (reaching $0$) is:

$$P_i = \begin{cases}
\dfrac{1 - (q/p)^i}{1 - (q/p)^N}, & \text{if } p \neq \dfrac{1}{2} \\[12pt]
\dfrac{i}{N}, & \text{if } p = \dfrac{1}{2}
\end{cases}$$

### Probability of Ruin
The probability of ruin (hitting $0$ before $N$) is the complementary probability:

$$Q_i = 1 - P_i = \begin{cases}
\dfrac{(q/p)^i - (q/p)^N}{1 - (q/p)^N}, & \text{if } p \neq \dfrac{1}{2} \\[12pt]
1 - \dfrac{i}{N}, & \text{if } p = \dfrac{1}{2}
\end{cases}$$

### Asymptotic Limit Against an Infinite Adversary ($N \to \infty$)
If playing against an adversary with unlimited capital ($N \to \infty$):

$$\lim_{N \to \infty} P_i = \begin{cases}
1 - \left(\dfrac{q}{p}\right)^i, & \text{if } p > \dfrac{1}{2} \\[10pt]
0, & \text{if } p \le \dfrac{1}{2}
\end{cases}$$

---

## Variables

| Symbol | Meaning |
|---|---|
| $i$ | Initial fortune of the gambler ($0 \le i \le N$) |
| $N$ | Target fortune / absorbing boundary (total capital of both players) |
| $p$ | Probability of winning $1$ unit on each independent play |
| $q$ | Probability of losing $1$ unit on each play ($q = 1 - p$) |
| $q/p$ | Ratio of loss to win probability (odds ratio) |
| $P_i$ | Probability of reaching $N$ before $0$, given initial fortune $i$ |
| $Q_i$ | Probability of ruin (hitting $0$ before $N$), given initial fortune $i$ |

---

## Conditions

1. **Absorbing Boundaries:** States $0$ and $N$ are absorbing: $P_{00} = 1$ and $P_{NN} = 1$. Once reached, the game halts immediately.
2. **Transient Intermediate States:** For $1 \le i \le N - 1$, transitions only occur to adjacent states:
   $$P_{i, i+1} = p, \quad P_{i, i-1} = q = 1 - p$$
3. **Step Size Exactly One:** Each play increments or decrements the fortune by exactly $1$ unit (no multi-unit bets or ties).
4. **Independent and Identically Distributed (i.i.d.) Trials:** The outcome of each play is independent of all past plays.

---

## Intuition

- **Fair Game ($p = 1/2$):** In a fair coin-toss game, win probability is strictly proportional to capital share: $P_i = i/N$. If you bring $20\%$ of the total bankroll to the table, your chance of taking all the money is exactly $20\%$.
- **Compounding Geometric Advantage ($p \neq 1/2$):** When $p > 1/2$, the ratio $q/p < 1$. As fortune $i$ increases, $(q/p)^i$ decays exponentially toward 0, making your win probability surge rapidly toward 1. A tiny statistical edge compounds powerfully over time.
- **The Tragedy of Fair Gambling Against the House ($N \to \infty$):** If an individual plays a fair game ($p = 0.5$) against a casino with effectively infinite wealth ($N \to \infty$), the probability of avoiding ruin is:
  $$\lim_{N \to \infty} \frac{i}{N} = 0$$
  **Ruin is certain with probability 1**, even in an unbiased game! Only an strictly favorable game ($p > 1/2$) provides a non-zero probability of surviving forever.

---

## Derivation

Let $P_i = P(\text{fortune reaches } N \mid X_0 = i)$.

### Step 1: Conditioning on the First Play
By the law of total probability and the [[Markov Chain]] property, condition on whether the gambler wins or loses the first round:
$$P_i = p P_{i+1} + q P_{i-1}, \quad \text{for } i = 1, 2, \dots, N-1$$
with boundary conditions:
$$P_0 = 0 \quad (\text{already ruined}), \qquad P_N = 1 \quad (\text{already reached target})$$

### Step 2: Transforming into a Geometric Difference
Because $p + q = 1$, we write $P_i$ as $(p + q) P_i = p P_i + q P_i$:
$$p P_i + q P_i = p P_{i+1} + q P_{i-1}$$
Rearranging terms by grouping $p$ and $q$:
$$p(P_{i+1} - P_i) = q(P_i - P_{i-1})$$
$$P_{i+1} - P_i = \frac{q}{p}(P_i - P_{i-1}), \quad \text{for } i = 1, 2, \dots, N-1$$

### Step 3: Telescoping the Differences
Notice that each consecutive step difference is simply the preceding difference scaled by the constant ratio $q/p$.
Since $P_0 = 0$, the base difference is $P_1 - P_0 = P_1$:
$$\begin{aligned}
P_2 - P_1 &= \frac{q}{p} P_1 \\
P_3 - P_2 &= \frac{q}{p} (P_2 - P_1) = \left(\frac{q}{p}\right)^2 P_1 \\
&\;\;\vdots \\
P_i - P_{i-1} &= \left(\frac{q}{p}\right)^{i-1} P_1 \\
&\;\;\vdots \\
P_N - P_{N-1} &= \left(\frac{q}{p}\right)^{N-1} P_1
\end{aligned}$$

### Step 4: Summing to Express $P_i$ in Terms of $P_1$
Summing the first $i - 1$ difference equations:
$$\sum_{k=1}^{i-1} (P_{k+1} - P_k) = P_i - P_1 = P_1 \sum_{k=1}^{i-1} \left(\frac{q}{p}\right)^k$$
Adding $P_1$ to both sides:
$$P_i = P_1 \left[ 1 + \frac{q}{p} + \left(\frac{q}{p}\right)^2 + \dots + \left(\frac{q}{p}\right)^{i-1} \right] = P_1 \sum_{k=0}^{i-1} \left(\frac{q}{p}\right)^k$$

### Step 5: Evaluating the Finite Geometric Series
**Case 1: $p \neq 1/2$ ($q/p \neq 1$):**
Using the finite geometric sum formula $\sum_{k=0}^{i-1} r^k = \frac{1 - r^i}{1 - r}$:
$$P_i = P_1 \frac{1 - (q/p)^i}{1 - (q/p)}$$

**Case 2: $p = 1/2$ ($q/p = 1$):**
The sum has $i$ terms of $1$:
$$P_i = i P_1$$

### Step 6: Determining $P_1$ Using Boundary Condition $P_N = 1$
At $i = N$, $P_N = 1$:
- For $p \neq 1/2$:
  $$1 = P_1 \frac{1 - (q/p)^N}{1 - (q/p)} \implies P_1 = \frac{1 - (q/p)}{1 - (q/p)^N}$$
- For $p = 1/2$:
  $$1 = N P_1 \implies P_1 = \frac{1}{N}$$

### Step 7: Final Closed-Form Expression
Substituting $P_1$ back into the formula for $P_i$:
- For $p \neq 1/2$:
  $$P_i = \left( \frac{1 - (q/p)}{1 - (q/p)^N} \right) \left( \frac{1 - (q/p)^i}{1 - (q/p)} \right) = \frac{1 - (q/p)^i}{1 - (q/p)^N}$$
- For $p = 1/2$:
  $$P_i = i \left(\frac{1}{N}\right) = \frac{i}{N} \quad \blacksquare$$

---

## Example

Patty and Max play a coin-tossing game.
- Patty wins each flip with probability $p = 0.6$ (hence $q = 0.4$).
- Patty starts with $5$ pennies ($i = 5$).
- Max starts with $10$ pennies (so Patty needs $5 + 10 = 15$ pennies to win: $N = 15$).

Calculate the probability that Patty wins all of Max's pennies:
1. Ratio $q/p = 0.4 / 0.6 = 2/3$.
2. Compute powers:
   $$(2/3)^5 = \frac{32}{243} \approx 0.131687$$
   $$(2/3)^{15} \approx 0.002284$$
3. Apply formula:
   $$P_5 = \frac{1 - (2/3)^5}{1 - (2/3)^{15}} = \frac{1 - 0.131687}{1 - 0.002284} = \frac{0.868313}{0.997716} \approx 0.8703 \quad (87.03\%)$$

Even though Patty starts with only $33.3\%$ of the money ($5/15$), her $60\%$ edge elevates her winning probability to over $87\%$.

---

## Common Mistakes

- **Confusing Total Fortune ($N$) with Opponent's Fortune ($M$):** $N$ is the total sum of money in play ($N = i_{\text{player}} + i_{\text{opponent}}$). If the opponent has $10$ coins and you have $5$, then $N = 15$, not $10$.
- **Inverting the Ratio $q/p$:** Using $p/q$ instead of $q/p$. Remember: $q$ (loss probability) is in the numerator!
- **Using Non-Fair Formula when $p = 0.5$:** Substituting $p = 0.5$ into $\frac{1 - (q/p)^i}{1 - (q/p)^N}$ produces $0/0$. When $p = 0.5$, use L'Hôpital's rule or directly apply $P_i = i/N$.
- **Misinterpreting $N \to \infty$ for Fair Games:** Believing that in a fair game ($p = 0.5$), you have a $50\%$ chance of never going bankrupt against an infinite bankroll. The true probability is exactly $0$.

---

## Related Concepts

- [[Markov Chain]]
- [[Classification of States in Markov Chains]]
- [[Stochastic Process]]

---

## Prerequisites

- [[Markov Chain]]
- [[Conditional Probability]]

---

## Problems

- [[Problem — Patty and Max Gambler's Ruin]]

---

## Sources

- [[01 - Sources/Lectures/Markov_Chain.pdf]] (Slides 30–36)
- [[01 - Sources/Textbooks/Sheldon M. Ross book Markov Chain Chapter.pdf]] (Section 4.5.1, pp. 230–234)
