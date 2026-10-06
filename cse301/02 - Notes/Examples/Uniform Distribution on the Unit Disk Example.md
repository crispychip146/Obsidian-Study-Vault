---
type: example
course: cse301
status: active
order: 21
---

# Uniform Distribution on the Unit Disk Example

> 📖 **Reading Order:** Step 21 of 103 | **Module 2:** Random Variables and Distributions  
> ◄ **Previous:** [[Gaussian Normalizing Constant Polar Derivation Example]] | ► **Next:** [[Expected Absolute Distance of Random Variables Example]]

---

## Problem

A point $(X, Y)$ is selected uniformly at random from the 2D unit disk:
$$D = \{(x, y) \in \mathbb{R}^2 : x^2 + y^2 \le 1\}$$

1. Determine the joint probability density function $f_{X,Y}(x, y)$.
2. Calculate the marginal probability density function $f_X(x)$ of $X$, and determine whether $X$ and $Y$ are independent.
3. Compute the conditional density $f_{Y \mid X}(y \mid x)$ of $Y$ given $X = x$. Identify the resulting distribution and calculate $\mathbb{E}[Y \mid X = x]$ and $\operatorname{Cov}(X, Y)$.

---

## Given

- The domain $D$ is a circular disk of radius $R = 1$ centered at the origin.
- The area of the domain is $\operatorname{Area}(D) = \pi R^2 = \pi$.
- Uniform distribution over a 2D domain means the joint PDF is constant over $D$:
  $$f_{X,Y}(x, y) = c \quad \text{for } (x, y) \in D, \quad \text{and } 0 \text{ otherwise}$$

---

## Required

1. Find the normalizing constant $c$ and the joint PDF $f_{X,Y}(x, y)$.
2. Compute the marginal density $f_X(x)$ by integrating out $y$.
3. Test for independence between $X$ and $Y$.
4. Derive the conditional PDF $f_{Y \mid X}(y \mid x)$ and find the conditional expectation $\mathbb{E}[Y \mid X]$.
5. Determine whether $X$ and $Y$ are correlated.

---

## Understanding the Problem and Choosing the Method

### The Geometry of Conditioning
When a uniform distribution is defined on a non-rectangular domain (like a circle or triangle), the coordinates cannot be independent because the allowable range of one variable depends on the realized value of the other:
$$-\sqrt{1 - x^2} \le y \le \sqrt{1 - x^2}$$
If $x = 0.9$, $y$ can only lie in $[-0.436, 0.436]$. If $x = 0$, $y$ spans $[-1, 1]$. 

### Choosing the Method
We use the definition of continuous marginalization:
$$f_X(x) = \int_{-\infty}^\infty f_{X,Y}(x, y) \, dy$$
and the definition of conditional density:
$$f_{Y \mid X}(y \mid x) = \frac{f_{X,Y}(x, y)}{f_X(x)}$$

---

## Solution

### Step 1: Joint Probability Density Function
For the joint PDF to integrate to 1:
$$\iint_D c \, dx \, dy = c \iint_D 1 \, dx \, dy = c \cdot \operatorname{Area}(D) = c \pi = 1 \implies c = \frac{1}{\pi}$$

Thus, the joint PDF is:
$$f_{X,Y}(x, y) = \begin{cases} \frac{1}{\pi} & \text{if } x^2 + y^2 \le 1 \\ 0 & \text{otherwise} \end{cases}$$

---

### Step 2: Marginal Probability Density Function of $X$
For any fixed $x \in [-1, 1]$, the variable $y$ ranges between the lower and upper semicircles:
$$x^2 + y^2 \le 1 \iff -\sqrt{1 - x^2} \le y \le \sqrt{1 - x^2}$$

Integrating out $y$:
$$f_X(x) = \int_{-\sqrt{1 - x^2}}^{\sqrt{1 - x^2}} \frac{1}{\pi} \, dy = \frac{1}{\pi} \left[ y \right]_{-\sqrt{1 - x^2}}^{\sqrt{1 - x^2}} = \frac{1}{\pi} \left( \sqrt{1 - x^2} - (-\sqrt{1 - x^2}) \right)$$
$$f_X(x) = \begin{cases} \mathbf{\frac{2}{\pi} \sqrt{1 - x^2}} & \text{if } -1 \le x \le 1 \\ 0 & \text{otherwise} \end{cases}$$

*(By symmetry, the marginal density of $Y$ is identically $f_Y(y) = \frac{2}{\pi}\sqrt{1 - y^2}$ for $y \in [-1, 1]$)*.

Notice that $f_X(x)$ is **not uniform**: it forms a **semi-ellipse (semi-circular distribution)** centered at $0$. Mass is highest at $x = 0$ ($f_X(0) = 2/\pi \approx 0.637$) because the vertical chord through the center is longest ($2$), while density tapers to $0$ as $|x| \to 1$.

---

### Step 3: Test for Independence
Two continuous random variables are independent if and only if $f_{X,Y}(x, y) = f_X(x) f_Y(y)$ everywhere.

For $(x, y) \in D$:
$$f_X(x) f_Y(y) = \left(\frac{2}{\pi}\sqrt{1 - x^2}\right) \left(\frac{2}{\pi}\sqrt{1 - y^2}\right) = \frac{4}{\pi^2}\sqrt{(1 - x^2)(1 - y^2)} \ne \frac{1}{\pi}$$

**$X$ and $Y$ are NOT independent.**

*(General Rule: If the joint support is not a Cartesian product of intervals $[a, b] \times [c, d]$ (i.e., a rectangle), the variables are automatically dependent)*.

---

### Step 4: Conditional Probability Density Function $f_{Y \mid X}(y \mid x)$
Using the definition of conditional density:
$$f_{Y \mid X}(y \mid x) = \frac{f_{X,Y}(x, y)}{f_X(x)} = \frac{\frac{1}{\pi}}{\frac{2}{\pi}\sqrt{1 - x^2}} = \frac{1}{2\sqrt{1 - x^2}}$$
defined for $y \in [-\sqrt{1 - x^2}, \, +\sqrt{1 - x^2}]$.

Notice that $f_{Y \mid X}(y \mid x)$ is a **constant** with respect to $y$ over the interval $[-\sqrt{1 - x^2}, +\sqrt{1 - x^2}]$, whose total length is $2\sqrt{1 - x^2}$.

Therefore, conditional on $X = x$, $Y$ follows a **continuous uniform distribution**:
$$\boxed{Y \mid (X = x) \sim \operatorname{Unif}\left(-\sqrt{1 - x^2}, \ +\sqrt{1 - x^2}\right)}$$

---

### Step 5: Conditional Expectation and Covariance
1. **Conditional Expectation:**
   Because the conditional distribution is symmetric about $0$:
   $$\mathbb{E}[Y \mid X = x] = \frac{(-\sqrt{1 - x^2}) + (+\sqrt{1 - x^2})}{2} = \mathbf{0}$$
   By [[Adam's Law (Law of Total Expectation)]]:
   $$\mathbb{E}[Y] = \mathbb{E}[\mathbb{E}[Y \mid X]] = \mathbb{E}[0] = 0$$

2. **Covariance:**
   Using [[Adam's Law (Law of Total Expectation)|iterated expectations]] on the product $XY$:
   $$\mathbb{E}[XY] = \mathbb{E}[\mathbb{E}[XY \mid X]] = \mathbb{E}[X \cdot \mathbb{E}[Y \mid X]] = \mathbb{E}[X \cdot 0] = 0$$
   $$\operatorname{Cov}(X, Y) = \mathbb{E}[XY] - \mathbb{E}[X]\mathbb{E}[Y] = 0 - (0)(0) = \mathbf{0}$$

**$X$ and $Y$ have correlation $\rho(X, Y) = 0$ (uncorrelated), yet they are strongly dependent!**

---

## Result

1. Joint PDF: $f_{X,Y}(x, y) = \frac{1}{\pi}$ on $x^2 + y^2 \le 1$.
2. Marginal PDF: $f_X(x) = \frac{2}{\pi}\sqrt{1 - x^2}$ on $[-1, 1]$ (Semi-circular density).
3. Independence: **Dependent** (support is non-rectangular).
4. Conditional Distribution: $Y \mid (X = x) \sim \operatorname{Unif}(-\sqrt{1 - x^2}, +\sqrt{1 - x^2})$.
5. Correlation: $\operatorname{Cov}(X, Y) = 0$ (Canonical example of **uncorrelated yet dependent** continuous random variables).

---

## Why This Works

Conditioning on $X = x$ slices the 2D circular cylinder by a vertical plane at $x$. The cross-section of a cylinder of uniform height is a flat rectangular strip running from $-\sqrt{1-x^2}$ to $+\sqrt{1-x^2}$. Because the top surface is flat (uniform density), every point along that slice has equal height, making the conditional slice uniform.

---

## Common Mistakes

- **Assuming Uniform Marginals:** Expecting that a uniform distribution over a 2D disk produces uniform 1D marginals. Central slices have more area than edge slices, creating the curved $\sqrt{1 - x^2}$ density.
- **Conflating Zero Covariance with Independence:** Assuming that because $\operatorname{Cov}(X, Y) = 0$, $X$ and $Y$ must be independent. Knowing $X = 0.99$ constrains $Y$ to $[-0.14, 0.14]$, proving strong dependence.

---

## Related Concepts

- [[Joint and Marginal Distributions]] — Multivariable joint densities and integration over non-rectangular regions.
- [[Covariance and Correlation]] — Uncorrelatedness vs. independence counterexamples.
- [[Conditional Expectation]] — Iterated expectations and regression functions.

---

## Sources

- [[cse301/01 - Sources/Lectures/HT Final Note - Suchi.pdf|HT Final Note: Lecture 16 (pp. 68–69)]]
- [[cse301/01 - Sources/Lectures/ITP.pdf|Textbook: Introduction to Probability (Blitzstein & Hwang, Section 7.1: Joint Distributions and Non-rectangular Domains)]]
