---
type: example
course: cse301
status: active
order: 22
---

# Expected Absolute Distance of Random Variables Example

> 📖 **Reading Order:** Step 22 of 103 | **Module 2:** Random Variables and Distributions  
> ◄ **Previous:** [[Uniform Distribution on the Unit Disk Example]] | ► **Next:** [[Exponential Distribution Memorylessness Example]]

---

## Problem

1. **Uniform Case:** Let $X, Y \overset{\text{i.i.d.}}{\sim} \operatorname{Unif}(0, 1)$ be two independent standard uniform random variables. Calculate the expected absolute difference $\mathbb{E}[|X - Y|]$.
2. **Gaussian Case:** Let $Z_1, Z_2 \overset{\text{i.i.d.}}{\sim} \mathcal{N}(0, 1)$ be two independent standard normal random variables. Calculate the expected absolute difference $\mathbb{E}[|Z_1 - Z_2|]$.

---

## Given

- **Part 1:** $X, Y$ are independent continuous random variables on $[0, 1]$ with marginal PDFs $f_X(x) = 1$ and $f_Y(y) = 1$. The joint PDF is $f_{X,Y}(x, y) = 1$ on the unit square $[0, 1]^2$.
- **Part 2:** $Z_1, Z_2$ are independent standard normal random variables with mean $0$ and variance $1$.

---

## Required

1. Compute $\mathbb{E}[|X - Y|]$ via two distinct techniques:
   - **Method A:** 2D [[Law of the Unconscious Statistician (LOTUS)|LOTUS]] integration splitting the domain across the diagonal.
   - **Method B:** Order statistics min/max decomposition $|X - Y| = \max(X, Y) - \min(X, Y)$.
2. Compute $\mathbb{E}[|Z_1 - Z_2|]$ using linear combinations of normal random variables and the folded normal expectation.

---

## Understanding the Problem and Choosing the Method

The absolute value function $|u - v|$ is non-linear and non-differentiable at $u = v$. 
- In multivariable calculus, evaluating $\mathbb{E}[|X - Y|]$ requires eliminating the absolute value sign by splitting the domain of integration along the line of symmetry $x = y$.
- In order statistics, the distance between two points on a line is identically the difference between the larger and the smaller point: $|X - Y| = X_{(2)} - X_{(1)}$.
- For normal random variables, the difference of independent Gaussians is itself Gaussian, allowing the 2D problem to reduce to a 1D folded normal expectation.

---

## Solution

### Part 1: Uniform Random Variables ($X, Y \overset{\text{i.i.d.}}{\sim} \operatorname{Unif}(0, 1)$)

#### Method A: 2D LOTUS Double Integration
By the 2D Law of the Unconscious Statistician:
$$\mathbb{E}[|X - Y|] = \int_0^1 \int_0^1 |x - y| f_{X,Y}(x, y) \, dy \, dx = \int_0^1 \int_0^1 |x - y| \, dy \, dx$$

1. **Split Across the Diagonal $x = y$:**
   The unit square is partitioned into two symmetric regions: $x > y$ and $y > x$.
   By symmetry:
   $$\mathbb{E}[|X - Y|] = 2 \iint_{x > y} (x - y) \, dy \, dx$$

2. **Set the Integration Limits:**
   In the lower triangular region $x > y$, $x$ ranges from $0$ to $1$, and for each $x$, $y$ ranges from $0$ to $x$:
   $$\mathbb{E}[|X - Y|] = 2 \int_{x = 0}^1 \left( \int_{y = 0}^x (x - y) \, dy \right) dx$$

3. **Evaluate the Inner Integral:**
   $$\int_0^x (x - y) \, dy = \left[ xy - \frac{y^2}{2} \right]_{y=0}^{y=x} = x^2 - \frac{x^2}{2} = \frac{x^2}{2}$$

4. **Evaluate the Outer Integral:**
   $$\mathbb{E}[|X - Y|] = 2 \int_0^1 \frac{x^2}{2} \, dx = \int_0^1 x^2 \, dx = \left[ \frac{x^3}{3} \right]_0^1 = \mathbf{\frac{1}{3}}$$

---

#### Method B: Min/Max Order Statistics Decomposition
Let $M = \max(X, Y)$ and $L = \min(X, Y)$.

1. **Algebraic Identity:**
   $$|X - Y| = M - L$$
   and:
   $$M + L = X + Y$$

2. **Sum of Expectations:**
   By linearity of expectation:
   $$\mathbb{E}[M + L] = \mathbb{E}[X + Y] = \mathbb{E}[X] + \mathbb{E}[Y] = \frac{1}{2} + \frac{1}{2} = 1$$

3. **Order Statistics Expectation:**
   When $n = 2$ points are dropped independently and uniformly into the interval $[0, 1]$, they divide the unit interval into $n + 1 = 3$ sub-intervals of equal expected length $\frac{1}{n + 1} = \frac{1}{3}$:
   $$\mathbb{E}[L] = \mathbb{E}[X_{(1)}] = \frac{1}{3}$$
   $$\mathbb{E}[M] = \mathbb{E}[X_{(2)}] = \frac{2}{3}$$

4. **Difference of Expectations:**
   $$\mathbb{E}[|X - Y|] = \mathbb{E}[M - L] = \mathbb{E}[M] - \mathbb{E}[L] = \frac{2}{3} - \frac{1}{3} = \mathbf{\frac{1}{3}}$$

Both methods confirm that the expected distance between two uniform points on the unit interval is exactly $\frac{1}{3}$.

---

### Part 2: Gaussian Random Variables ($Z_1, Z_2 \overset{\text{i.i.d.}}{\sim} \mathcal{N}(0, 1)$)

1. **Distribution of the Difference:**
   Let $D = Z_1 - Z_2 = Z_1 + (-Z_2)$.
   Because any linear combination of independent normal random variables is normal:
   $$\mathbb{E}[D] = \mathbb{E}[Z_1] - \mathbb{E}[Z_2] = 0 - 0 = 0$$
   $$\operatorname{Var}(D) = \operatorname{Var}(Z_1) + (-1)^2 \operatorname{Var}(Z_2) = 1 + 1 = 2$$
   $$\sigma_D = \sqrt{2}$$

   Therefore:
   $$D \sim \mathcal{N}(0, 2)$$

2. **Scaling to Standard Normal:**
   We can express $D$ as $D = \sqrt{2} Z$, where $Z \sim \mathcal{N}(0, 1)$.
   Then:
   $$\mathbb{E}[|Z_1 - Z_2|] = \mathbb{E}[|D|] = \mathbb{E}[|\sqrt{2} Z|] = \sqrt{2} \, \mathbb{E}[|Z|]$$

3. **Expectation of the Absolute Standard Normal $|Z|$:**
   By LOTUS on the standard normal density $\phi(z) = \frac{1}{\sqrt{2\pi}} e^{-z^2/2}$:
   $$\mathbb{E}[|Z|] = \int_{-\infty}^\infty |z| \frac{1}{\sqrt{2\pi}} e^{-z^2/2} \, dz$$
   Since $|z| e^{-z^2/2}$ is an even function:
   $$\mathbb{E}[|Z|] = 2 \int_0^\infty z \frac{1}{\sqrt{2\pi}} e^{-z^2/2} \, dz = \frac{2}{\sqrt{2\pi}} \int_0^\infty z e^{-z^2/2} \, dz$$
   Using the substitution $u = z^2/2$, $du = z \, dz$:
   $$\int_0^\infty z e^{-z^2/2} \, dz = \left[ -e^{-z^2/2} \right]_0^\infty = 0 - (-1) = 1$$
   $$\mathbb{E}[|Z|] = \frac{2}{\sqrt{2\pi}} \times 1 = \sqrt{\frac{2}{\pi}}$$

4. **Final Calculation:**
   $$\mathbb{E}[|Z_1 - Z_2|] = \sqrt{2} \times \sqrt{\frac{2}{\pi}} = \sqrt{\frac{4}{\pi}} = \mathbf{\frac{2}{\sqrt{\pi}}} \approx 1.128379$$

---

## Result

$$\boxed{\mathbb{E}[|X - Y|] = \frac{1}{3} \quad \text{for } X, Y \overset{\text{i.i.d.}}{\sim} \operatorname{Unif}(0, 1)}$$
$$\boxed{\mathbb{E}[|Z_1 - Z_2|] = \frac{2}{\sqrt{\pi}} \approx 1.1284 \quad \text{for } Z_1, Z_2 \overset{\text{i.i.d.}}{\sim} \mathcal{N}(0, 1)}$$

---

## Why This Works

- In the uniform case, even though points can be as far apart as $1$ or as close as $0$, the triangular density of the difference $D = X - Y$ concentrates near $0$, pulling the average distance down to $\frac{1}{3}$ (substantially less than the midpoint $0.5$).
- In the Gaussian case, standardizing the linear combination $Z_1 - Z_2$ converts an intractable bivariate integral into the well-known folded standard normal mean $\sqrt{2/\pi}$.

---

## Common Mistakes

- **Integrating Without Splitting:** Attempting to evaluate $\int (x - y) dy dx$ without splitting the absolute value, which yields $0$ by antisymmetry rather than the positive expected distance.
- **Adding Standard Deviations:** Writing $\sigma_{Z_1 - Z_2} = \sigma_1 + \sigma_2 = 1 + 1 = 2$. Variances add ($\sigma^2 = 1 + 1 = 2$), so the standard deviation is $\sqrt{2}$, not $2$.

---

## Related Concepts

- [[Law of the Unconscious Statistician (LOTUS)]] — Expectation of continuous functions of random vectors.
- [[Continuous Probability Distributions]] — Standard Normal and Uniform distributions.
- [[Gaussian Normalizing Constant Polar Derivation Example]] — Gaussian integral evaluation.

---

## Sources

- [[cse301/01 - Sources/Lectures/HT Final Note - Suchi.pdf|HT Final Note: Lecture 17 (pp. 70–71)]]
- [[cse301/01 - Sources/Lectures/ITP.pdf|Textbook: Introduction to Probability (Blitzstein & Hwang, Section 7.2: LOTUS for Several Variables)]]
