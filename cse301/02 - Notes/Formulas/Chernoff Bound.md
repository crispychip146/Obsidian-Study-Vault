---
type: formula
course: cse301
status: active
---

# Chernoff Bound

## Mathematical Statement

Let $X$ be a random variable whose [[Moment Generating Functions|Moment Generating Function]] $M_X(t) = \mathbb{E}[e^{tX}]$ exists for $t$ in an open interval containing zero.
For any real threshold $a$:

- **Upper Tail Bound:**
  $$P(X \ge a) \le \inf_{t > 0} e^{-ta} M_X(t)$$

- **Lower Tail Bound:**
  $$P(X \le a) \le \inf_{t < 0} e^{-ta} M_X(t)$$

---

## Derivation via Markov's Inequality

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

## Convex Duality & The Cumulant Generating Function

Let $\psi_X(t) = \ln M_X(t)$ be the **cumulant generating function** (log-MGF). The bound can be rewritten as:
$$P(X \ge a) \le \exp\left( -\sup_{t > 0} [ta - \psi_X(t)] \right)$$

The quantity $I(a) = \sup_{t > 0} [ta - \psi_X(t)]$ is the **Fenchel-Legendre transform** (or rate function) of $X$, and governs large deviation theory:
$$P(X \ge a) \le e^{-I(a)}$$

---

## Classic Example: Standard Normal Tail Bound

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

## Comparison of Tail Decays

| Inequality | Bound Order for Tail $P(X \ge a)$ | Decay Rate | Information Required |
|---|---|---|---|
| **Markov** | $O(1/a)$ | Polynomial (very slow) | Mean $\mathbb{E}[X]$ only |
| **Chebyshev** | $O(1/a^2)$ | Polynomial | Mean and Variance |
| **Chernoff** | $O(e^{-c a^2})$ | **Exponential (extremely fast)** | Entire MGF $M_X(t)$ |

---

## Related Notes

- [[Markov Inequality]] — Base inequality used in the proof.
- [[Chebyshev Inequality]] — Second moment polynomial bound.
- [[Moment Generating Functions]] — Provides $M_X(t)$.
- [[Comparison of Probability Bounds Example]] — Concrete numerical benchmark.

---

## Sources & Traceability

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lecture 18, pages 57–59)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_11.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Section 10.2: Chernoff Bounds)
