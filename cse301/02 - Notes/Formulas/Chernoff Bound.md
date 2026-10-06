---
type: formula
course: cse301
status: active
order: 27
---

# Chernoff Bound

> 📖 **Reading Order:** Step 27 of 92 | **Module 4:** Probability Bounds and Inequalities  
> ◄ **Previous:** [[Chebyshev Inequality]] | ► **Next:** [[Cauchy-Schwarz and Jensen Inequalities]]

---

---

## The Question and Earlier Knowledge

What analytical relationship or closed-form expectation governs Chernoff Bound, and how can we compute it directly from constituent probability terms? In complex probability models, calculating probabilities or moments directly is often intractable without decomposing expectations across conditioning partitions or inequalities.

---

## Developing the Formula

By decomposing joint distributions into conditional components, expanding algebraic products, or applying geometric series sums, Chernoff Bound compresses complex probabilistic reasoning into a clean, reusable formula.

---

## Formula

Let $X$ be a random variable whose [[Moment Generating Functions|Moment Generating Function]] $M_X(t) = \mathbb{E}[e^{tX}]$ exists for $t$ in an open interval containing zero.
For any real threshold $a$:

- **Upper Tail Bound:**
  $$P(X \ge a) \le \inf_{t > 0} e^{-ta} M_X(t)$$

- **Lower Tail Bound:**
  $$P(X \le a) \le \inf_{t < 0} e^{-ta} M_X(t)$$

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

### Convex Duality & The Cumulant Generating Function

Let $\psi_X(t) = \ln M_X(t)$ be the **cumulant generating function** (log-MGF). The bound can be rewritten as:
$$P(X \ge a) \le \exp\left( -\sup_{t > 0} [ta - \psi_X(t)] \right)$$

The quantity $I(a) = \sup_{t > 0} [ta - \psi_X(t)]$ is the **Fenchel-Legendre transform** (or rate function) of $X$, and governs large deviation theory:
$$P(X \ge a) \le e^{-I(a)}$$

---
### Comparison of Tail Decays

| Inequality | Bound Order for Tail $P(X \ge a)$ | Decay Rate | Information Required |
|---|---|---|---|
| **Markov** | $O(1/a)$ | Polynomial (very slow) | Mean $\mathbb{E}[X]$ only |
| **Chebyshev** | $O(1/a^2)$ | Polynomial | Mean and Variance |
| **Chernoff** | $O(e^{-c a^2})$ | **Exponential (extremely fast)** | Entire MGF $M_X(t)$ |

---

---

## Derivation

### Derivation via Markov's Inequality

For any positive parameter $t > 0$, the function $g(x) = e^{tx}$ is strictly monotonically increasing and positive everywhere ($e^{tx} > 0$).
Therefore, the events $\{X \ge a\}$ and $\{e^{tX} \ge e^{ta}\}$ are identical:
$$P(X \ge a) = P\left( e^{tX} \ge e^{ta} \right)$$

Applying [[Markov Inequality]] to the non-negative random variable $Y = e^{tX}$ with threshold $e^{ta}$:
$$P\left( e^{tX} \ge e^{ta} \right) \le \frac{\mathbb{E}[e^{tX}]}{e^{ta}} = e^{-ta} M_X(t)$$

Because this upper bound holds validly for **every** $t > 0$, we can choose the optimal $t^*$ that minimizes the right-hand side to obtain the tightest possible bound:
$$P(X \ge a) \le \inf_{t > 0} e^{-ta} M_X(t)$$
$\blacksquare$

Similarly, for the lower tail ($a \le \mathbb{E}[X]$), multiplying by $t < 0$ reverses the inequality ($X \le a \iff tx \ge ta \iff e^{tX} \ge e^{ta}$), yielding:
$$P(X \le a) \le \inf_{t < 0} e^{-ta} M_X(t)$$

---

---

## Example

### Classic Example: Standard Normal Tail Bound

Let $Z \sim \mathcal{N}(0, 1)$. Its MGF is $M_Z(t) = e^{t^2 / 2}$.
For any $c > 0$:
$$P(Z \ge c) \le e^{-tc} e^{t^2 / 2} = e^{-tc + t^2 / 2}$$

To find the optimal $t > 0$, minimize the exponent $h(t) = \frac{t^2}{2} - tc$:
$$h'(t) = t - c = 0 \implies t^* = c$$
Substitute $t^* = c$ back into the bound:
$$h(c) = \frac{c^2}{2} - c^2 = -\frac{c^2}{2}$$

**Result:**
$$P(Z \ge c) \le e^{-c^2 / 2}$$
By symmetry, the two-sided tail is bounded by:
$$P(\lvert Z \rvert \ge c) \le 2e^{-c^2 / 2}$$

---

---

## Common Mistakes

- **Sign of Parameter $t$:** Minimizing over unconstrained $t \in \mathbb{R}$. For upper-tail bounds $P(X \ge a)$, optimization must be restricted to $t > 0$; for lower-tail bounds $P(X \le a)$, optimization must use $t < 0$.
- **Non-existent MGF:** Attempting to apply Chernoff bounds to heavy-tailed distributions whose MGF diverges for all $t > 0$ (e.g., Cauchy, Log-Normal, or Pareto).
- **Sub-optimal $t$ Choice:** Failing to differentiate the exponent to find $t^* = \arg\min_t \{M_X(t)e^{-ta}\}$, resulting in loose bounds.

---

## Related Concepts

- [[Markov Inequality]] — Base inequality used in the proof.
- [[Chebyshev Inequality]] — Second moment polynomial bound.
- [[Moment Generating Functions]] — Provides $M_X(t)$.
- [[Comparison of Probability Bounds Example]] — Concrete numerical benchmark.

---

---

## Prerequisites

- [[Random Variables and Probability Distributions]]
- [[Markov Inequality]]
- [[Moment Generating Functions]]

---

## Problems

- [[Problem — Bounding Tail Probabilities with Chebyshev and Chernoff]]

---

## Sources

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lecture 18, pages 57–59)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_11.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Section 10.2: Chernoff Bounds)
