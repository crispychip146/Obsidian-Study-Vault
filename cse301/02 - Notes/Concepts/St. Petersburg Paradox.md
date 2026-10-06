---
type: concept
course: cse301
status: active
order: 13
---

# St. Petersburg Paradox

> 📖 **Reading Order:** Step 13 of 103 | **Module 2:** Random Variables and Distributions  
> ◄ **Previous:** [[Cauchy and Student-t Distributions]] | ► **Next:** [[Joint and Marginal Distributions]]

---

## Starting Point and the Problem

Classical decision theory and expected value calculations suggest that a rational decision-maker should be willing to pay any entrance fee strictly less than the expected payoff of a game:
$$\text{Fair Price} = \mathbb{E}[\text{Payoff}]$$

However, consider the following gambling proposition introduced by Nicolas Bernoulli in 1713:
A fair coin is flipped repeatedly until the first Head appears. If the first Head appears on flip $k$, the player wins $2^k$ dollars.

When we compute the expected monetary payoff of this game:
$$\mathbb{E}[Y] = \sum_{k=1}^\infty 2^k \cdot P(X = k) = \sum_{k=1}^\infty 2^k \left(\frac{1}{2}\right)^k = \sum_{k=1}^\infty 1 = 1 + 1 + 1 + \dots = \infty$$

The expected payoff is **infinite**. Under naive expected value theory, a rational player should be willing to pay *any finite sum*—even $\$1,000,000$ or their entire life savings—for a single ticket to play this game. Yet in empirical experiments, almost nobody is willing to pay more than $\$20$ to $\$40$. 

This discrepancy between mathematical expectation and rational human behavior is the **St. Petersburg Paradox**.

---

## Developing the Idea

To understand why expected value fails here, let us follow what actually happens during the game:

### The Probability and Payoff Structure

Let $X \sim \operatorname{FS}(1/2)$ be the number of tosses until the first Head (First Success distribution):

| Trial of First Head ($k$) | Probability $P(X = k) = (1/2)^k$ | Payout $Y = 2^k$ | Expected Contribution $2^k \cdot (1/2)^k$ | Cumulative Win Probability |
|---|---|---|---|---|
| $k = 1$ | $1/2 = 0.5000$ | $\$2$ | $\$1.00$ | $50.0\%$ |
| $k = 2$ | $1/4 = 0.2500$ | $\$4$ | $\$1.00$ | $75.0\%$ |
| $k = 3$ | $1/8 = 0.1250$ | $\$8$ | $\$1.00$ | $87.5\%$ |
| $k = 4$ | $1/16 = 0.0625$ | $\$16$ | $\$1.00$ | $93.75\%$ |
| $k = 5$ | $1/32 = 0.03125$ | $\$32$ | $\$1.00$ | $96.875\%$ |
| $k = 6$ | $1/64 = 0.015625$ | $\$64$ | $\$1.00$ | $98.4375\%$ |

Notice that **every single possible round contributes exactly $\$1.00$ to the expected value**, regardless of how large $k$ is. Because the number of possible flips is unbounded, the sum of these $\$1$ increments diverges to $\infty$.

### The Real Obstacle

Now look at the cumulative probabilities:
- With probability $50\%$, you win only $\$2$.
- With probability $75\%$, you win $\$4$ or less.
- With probability $87.5\%$, you win $\$8$ or less.
- With probability $98.4\%$, you win $\$64$ or less.

If an individual pays $\$50$ to play, they have an **overwhelming $96.875\%$ chance of losing money** ($k \le 4$, payoff $\le \$16$). The infinite expectation is driven entirely by astronomical payouts occurring with near-zero probabilities (e.g., winning $\$1,048,576$ on flip $k=20$, which occurs only once in a million games).

---

## Resolutions to the Paradox

There are two primary mathematical and economic resolutions to the paradox:

### 1. The Finite Casino Bankroll Resolution (Practical Constraint)

No real-world gambling house or financial institution possesses infinite capital. Suppose the casino has a maximum bankroll of $W = \$2^{30} \approx \$1,073,741,824$ (approximately $\$1\text{ Billion}$). 

If the first Head occurs at flip $k \le 30$, the casino pays $2^k$. If $k > 30$, the casino can pay at most its total bankroll of $2^{30}$.

The expected payoff with this realistic upper bound is:
$$\mathbb{E}[Y_{\text{capped}}] = \sum_{k=1}^{30} 2^k \left(\frac{1}{2}\right)^k + \sum_{k=31}^\infty 2^{30} \left(\frac{1}{2}\right)^k$$

Evaluating the two terms:
$$\sum_{k=1}^{30} 1 = 30$$
$$\sum_{k=31}^\infty 2^{30} \left(\frac{1}{2}\right)^k = 2^{30} \cdot \frac{(1/2)^{31}}{1 - 1/2} = 2^{30} \cdot (1/2)^{30} = 1$$

$$\mathbb{E}[Y_{\text{capped}}] = 30 + 1 = \$31.00$$

Even with a massive **one-billion-dollar bankroll**, the mathematically fair price of the ticket is only **$\$31.00$**! The infinite expectation in the pure problem is a mathematical artifact of assuming infinite counterparty wealth.

---

### 2. Expected Utility Theory (Bernoulli's Resolution)

In 1738, Daniel Bernoulli resolved the paradox by recognizing that people do not evaluate decisions based on *expected monetary value*, but on **expected utility** $\mathbb{E}[U(W)]$. 

Because money has **diminishing marginal utility** (an additional $\$1,000$ matters far more to an impoverished student than to a billionaire), the utility function is strictly concave, commonly modeled as the natural logarithm:
$$U(w) = \log(w)$$

Let the player have initial wealth $w$ and pay entrance fee $c$. The player accepts the game if the expected utility with the game exceeds the utility of keeping their wealth:
$$\mathbb{E}[U(w - c + Y)] > U(w)$$

If the player has wealth $w = 0$ (evaluating the payoff in isolation with $U(y) = \log_2(y)$):
$$\mathbb{E}[U(Y)] = \sum_{k=1}^\infty \log_2(2^k) \cdot \left(\frac{1}{2}\right)^k = \sum_{k=1}^\infty k \cdot \left(\frac{1}{2}\right)^k$$

Using the identity for the expectation of a $\operatorname{Geom}(1/2)$ / $\operatorname{FS}(1/2)$ random variable:
$$\sum_{k=1}^\infty k \left(\frac{1}{2}\right)^k = 2$$

The certainty equivalent $C$ satisfying $\log_2(C) = 2$ is:
$$C = 2^2 = \$4.00$$

Under logarithmic utility, the subjective value of the game is merely **$\$4.00$**!

---

## Technical Details

### Divergence of Moments
Let $Y = 2^X$ where $X \sim \operatorname{FS}(p)$.
- If $p = 1/2$: $\mathbb{E}[Y^r] = \sum_{k=1}^\infty (2^r)^k (1/2)^k = \sum_{k=1}^\infty (2^{r-1})^k$.
- For any $r \ge 1$, the ratio $2^{r-1} \ge 1$, so all integer moments $\mathbb{E}[Y], \mathbb{E}[Y^2], \dots$ diverge to $\infty$.
- However, for fractional power utilities $U(y) = y^\alpha$ with $0 < \alpha < 1$:
  $$\mathbb{E}[Y^\alpha] = \sum_{k=1}^\infty (2^\alpha)^k \left(\frac{1}{2}\right)^k = \sum_{k=1}^\infty (2^{\alpha - 1})^k$$
  Since $\alpha < 1$, the common ratio $2^{\alpha - 1} < 1$, and the series converges to $\frac{2^{\alpha - 1}}{1 - 2^{\alpha - 1}} < \infty$.

---

## Important Properties and Why They Hold

- **Heavy Tails:** The St. Petersburg distribution is an extreme example of a **heavy-tailed distribution** where tail probabilities decay like $P(Y \ge y) \sim 1/y$.
- **Failure of the Law of Large Numbers (LLN):** Because $\mathbb{E}[Y] = \infty$, the sample average $\bar{Y}_n = \frac{1}{n} \sum_{i=1}^n Y_i$ does not converge to a finite constant. Instead, $\frac{\bar{Y}_n}{\log_2 n} \xrightarrow{P} 1$ (generalized weak law of large numbers).

---

## Common Mistakes

- **Conflating Expected Value with Most Likely Outcome:** Believing that an infinite expected value implies high probability of winning big. The mode of the distribution is $\$2$ (with $50\%$ probability).
- **Ignoring Counterparty Risk / Solvency:** Assuming infinite expectation applies to physical lotteries without accounting for the casino's liquidity limits.

---

## Exam Relevance

In CSE 301, questions involving the St. Petersburg Paradox typically assess:
1. **Geometric Series Moment Calculation:** Demonstrating the step-by-step divergence of $\mathbb{E}[2^X]$ when $X \sim \operatorname{FS}(1/2)$.
2. **Capped Expectation Problems:** Given a maximum payout $M = 2^N$, calculating the finite truncated expected payoff $\sum_{k=1}^N 1 + 2^N P(X > N) = N + 1$.
3. **Logarithmic Utility Evaluation:** Computing expected utility $\mathbb{E}[\log_2(Y)] = \sum k (1/2)^k = 2$.

---

## Related Concepts

- [[Discrete Probability Distributions]] — Geometric and First Success distributions.
- [[Random Variables and Probability Distributions]] — Definition of expectation and existence of moments.
- [[Cauchy and Student-t Distributions]] — Distributions with heavy tails and undefined expectations.

---

## Prerequisites

- [[Discrete Probability Distributions]] — First Success distribution $\operatorname{FS}(p)$.
- [[Random Variables and Probability Distributions]] — Expected value formula $\mathbb{E}[g(X)] = \sum g(x) P(X=x)$.

---

## Sources

- [[cse301/01 - Sources/Lectures/HT Final Note - Suchi.pdf|HT Final Note: Lecture 10–11 (pp. 42–43)]]
- [[cse301/01 - Sources/Lectures/ITP.pdf|Textbook: Introduction to Probability (Blitzstein & Hwang, Section 4.3: St. Petersburg Paradox)]]
