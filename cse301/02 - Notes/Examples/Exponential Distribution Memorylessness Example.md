---
type: example
course: cse301
status: active
order: 15
---

# Exponential Distribution Memorylessness Example

> 📖 **Reading Order:** Step 15 of 92 | **Module 2:** Random Variables and Distributions  
> ◄ **Previous:** [[Linearity of Expectation and Indicator Random Variables Example]] | ► **Next:** [[Problem — Indicator Variables for Distinct Birthday Counts]]

---

## Problem Context & Setup

Let $T \sim \operatorname{Exp}(\lambda)$ represent the lifetime of an electronic component (or service time at a server), with rate parameter $\lambda > 0$.
The survival function is:
$$P(T > t) = e^{-\lambda t} \quad \text{for } t \ge 0$$

### What is Memorylessness?
A continuous random variable $T$ is **memoryless** if:
$$P(T > s + t \mid T > s) = P(T > t) \quad \text{for all } s, t \ge 0$$

In human terms: If you have already waited $s$ minutes for a service to complete, the probability that you must wait at least an *additional* $t$ minutes is exactly the same as if you had just arrived! The system retains **zero memory** of the elapsed time $s$.

---

## Mathematical Proof of Memorylessness

Using the definition of conditional probability:
$$P(T > s + t \mid T > s) = \frac{P(T > s + t \cap T > s)}{P(T > s)}$$

Since $s, t \ge 0$, the event $\{T > s + t\}$ is a subset of $\{T > s\}$. Therefore, their intersection is simply $\{T > s + t\}$:
$$P(T > s + t \mid T > s) = \frac{P(T > s + t)}{P(T > s)} = \frac{e^{-\lambda(s + t)}}{e^{-\lambda s}} = \frac{e^{-\lambda s} e^{-\lambda t}}{e^{-\lambda s}} = e^{-\lambda t} = P(T > t)$$
$\blacksquare$

---

## Competing Exponentials: The Two-Server Race

Suppose an incoming job is processed in parallel by two servers, Server 1 and Server 2, whose service times are independent:
$$T_1 \sim \operatorname{Exp}(\lambda_1), \quad T_2 \sim \operatorname{Exp}(\lambda_2)$$

### Question 1: Distribution of the First Completed Job
Let $M = \min(T_1, T_2)$. What is the distribution of $M$?
For any $t \ge 0$:
$$P(M > t) = P(T_1 > t \text{ and } T_2 > t) = P(T_1 > t) P(T_2 > t) = e^{-\lambda_1 t} e^{-\lambda_2 t} = e^{-(\lambda_1 + \lambda_2)t}$$

Therefore:
$$M \sim \operatorname{Exp}(\lambda_1 + \lambda_2)$$
The minimum of independent exponential random variables is itself Exponential, whose rate is the **sum of the individual rates**:
$$\lambda_{\text{total}} = \lambda_1 + \lambda_2$$

---

### Question 2: Probability that Server 1 Finishes First
What is $P(T_1 < T_2)$?
Conditioning on $T_1 = t$ via the continuous [[Law of Total Probability and Bayes' Rule]]:
$$P(T_1 < T_2) = \int_0^\infty P(T_2 > t \mid T_1 = t) f_{T_1}(t) \, dt = \int_0^\infty P(T_2 > t) f_{T_1}(t) \, dt$$
Substitute $P(T_2 > t) = e^{-\lambda_2 t}$ and $f_{T_1}(t) = \lambda_1 e^{-\lambda_1 t}$:
$$P(T_1 < T_2) = \int_0^\infty e^{-\lambda_2 t} \lambda_1 e^{-\lambda_1 t} \, dt = \lambda_1 \int_0^\infty e^{-(\lambda_1 + \lambda_2)t} \, dt$$
$$P(T_1 < T_2) = \lambda_1 \left[ \frac{-e^{-(\lambda_1 + \lambda_2)t}}{\lambda_1 + \lambda_2} \right]_0^\infty = \frac{\lambda_1}{\lambda_1 + \lambda_2}$$

---

## Concrete Numerical Example: Post Office Paradox

Alice, Bob, and Charlie walk into a post office with two clerks.
- Alice and Bob begin service simultaneously with Clerk 1 and Clerk 2, respectively.
- Charlie waits in line and will be served by whichever clerk becomes free first.
- Assume all service times are i.i.d. $\operatorname{Exp}(\lambda)$.

**Question:** What is the probability that Charlie is the **last** of the three to leave the post office?

### Solution:
1. One of Alice or Bob finishes first. By symmetry, each has probability $1/2$ of finishing first.
2. Suppose Alice finishes first. Then Charlie immediately begins service with Clerk 1.
3. At this exact moment, Bob has already been in service for some time $S > 0$.
4. **By the Memoryless Property**, Bob's remaining service time is still $\operatorname{Exp}(\lambda)$, completely resetting the clock!
5. Now Charlie and Bob are competing in an identical race, both with independent $\operatorname{Exp}(\lambda)$ remaining times.
6. By symmetry, Charlie finishes after Bob with probability $\frac{\lambda}{\lambda + \lambda} = \frac{1}{2}$.

Therefore:
$$P(\text{Charlie is last}) = \frac{1}{2}$$
Despite arriving after Alice and Bob, Charlie is the last to leave with probability **exactly $50\%$**!

---

## Related Notes

- [[Continuous Probability Distributions]] — Exponential, Gamma, and Normal distributions.
- [[M-M-1 Queue]] — Memoryless property guarantees Markovian state transitions.
- [[PASTA Property and Inspection Paradox]] — How memorylessness affects arrival averages.

---

## Sources & Traceability

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lecture 12, pages 36–39)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_7.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Section 5.3: Exponential Distribution)
