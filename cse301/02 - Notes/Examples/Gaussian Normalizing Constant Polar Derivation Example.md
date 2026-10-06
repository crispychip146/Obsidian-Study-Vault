---
type: example
course: cse301
status: active
order: 20
---

# Gaussian Normalizing Constant Polar Derivation Example

> 📖 **Reading Order:** Step 20 of 103 | **Module 2:** Random Variables and Distributions  
> ◄ **Previous:** [[Poisson Triplet Birthday Collisions Example]] | ► **Next:** [[Uniform Distribution on the Unit Disk Example]]

---

## Problem

Prove that the total area under the Gaussian bell curve kernel $e^{-z^2/2}$ over the entire real line is $\sqrt{2\pi}$:
$$I = \int_{-\infty}^\infty e^{-z^2/2} \, dz = \sqrt{2\pi}$$

Consequently, derive the normalizing constant $c = \frac{1}{\sqrt{2\pi}}$ required for the Standard Normal Probability Density Function:
$$f_Z(z) = \frac{1}{\sqrt{2\pi}} e^{-z^2/2}, \quad z \in \mathbb{R}$$

---

## Given

- The Gaussian function $g(z) = e^{-z^2/2}$ is continuous, positive, and symmetric ($g(-z) = g(z)$) on $\mathbb{R}$.
- By Liouville's theorem in calculus, the indefinite integral $\int e^{-z^2/2} dz$ has no elementary antiderivative.
- For $f_Z(z) = c e^{-z^2/2}$ to represent a valid probability density function:
  $$\int_{-\infty}^\infty f_Z(z) \, dz = 1 \iff c \int_{-\infty}^\infty e^{-z^2/2} \, dz = 1 \iff c = \frac{1}{I}$$

---

## Required

Evaluate $I = \int_{-\infty}^\infty e^{-z^2/2} dz$ using multivariable calculus and coordinate transformations, and verify that $c = \frac{1}{\sqrt{2\pi}}$.

---

## Understanding the Problem and Choosing the Method

### The 1D Obstacle
In single-variable calculus, standard integration techniques fail:
- $u$-substitution ($u = z^2/2$) requires a $z \, dz$ term in the integrand, which is absent.
- Integration by parts does not simplify the exponent.

### The 2D Strategy (Poisson–Gauss Method)
Rather than computing $I$ directly, we compute its **square**:
$$I^2 = \left( \int_{-\infty}^\infty e^{-x^2/2} \, dx \right) \left( \int_{-\infty}^\infty e^{-y^2/2} \, dy \right)$$

Because $x$ and $y$ are independent dummy variables of integration, we can merge the product into a **double integral over the entire 2D Cartesian plane** $\mathbb{R}^2$:
$$I^2 = \int_{-\infty}^\infty \int_{-\infty}^\infty e^{-\frac{x^2 + y^2}{2}} \, dx \, dy$$

The term $x^2 + y^2$ suggests a transformation to **polar coordinates** $(r, \theta)$, where $r^2 = x^2 + y^2$.

---

## Solution

### Step 1: Define the Coordinate Transformation
Let:
$$x = r \cos\theta$$
$$y = r \sin\theta$$
where the radius spans $r \in [0, \infty)$ and the angle spans $\theta \in [0, 2\pi)$.

---

### Step 2: Compute the Jacobian Determinant
To convert the differential area element $dx \, dy$ into polar coordinates, compute the Jacobian matrix:
$$J = \begin{pmatrix} \frac{\partial x}{\partial r} & \frac{\partial x}{\partial \theta} \\[6pt] \frac{\partial y}{\partial r} & \frac{\partial y}{\partial \theta} \end{pmatrix} = \begin{pmatrix} \cos\theta & -r\sin\theta \\ \sin\theta & r\cos\theta \end{pmatrix}$$

The determinant of the Jacobian is:
$$|J| = (\cos\theta)(r\cos\theta) - (-r\sin\theta)(\sin\theta) = r\cos^2\theta + r\sin^2\theta = r(\cos^2\theta + \sin^2\theta) = r$$

Since $r \ge 0$, the area element is:
$$dx \, dy = |J| \, dr \, d\theta = r \, dr \, d\theta$$

Notice the remarkable effect of this transformation: **the extra factor of $r$ provides the missing derivative needed to integrate $e^{-r^2/2}$!**

---

### Step 3: Evaluate the Polar Double Integral
Substitute $x^2 + y^2 = r^2$ and $dx \, dy = r \, dr \, d\theta$ into the double integral:
$$I^2 = \int_{\theta = 0}^{2\pi} \int_{r = 0}^\infty e^{-r^2/2} \, r \, dr \, d\theta$$

Because the integrand does not depend on $\theta$, the double integral factors cleanly:
$$I^2 = \left( \int_0^{2\pi} d\theta \right) \left( \int_0^\infty r e^{-r^2/2} \, dr \right)$$

1. **Angular Integral:**
   $$\int_0^{2\pi} d\theta = 2\pi$$

2. **Radial Integral:**
   Use the substitution $u = \frac{r^2}{2}$, so $du = r \, dr$:
   - When $r = 0$, $u = 0$.
   - When $r \to \infty$, $u \to \infty$.
   $$\int_0^\infty r e^{-r^2/2} \, dr = \int_0^\infty e^{-u} \, du = \left[ -e^{-u} \right]_0^\infty = 0 - (-1) = 1$$

3. **Product:**
   $$I^2 = 2\pi \times 1 = 2\pi$$

---

### Step 4: Extract the 1D Normalizing Constant
Taking the square root (since $e^{-z^2/2} > 0$, the integral must be positive):
$$I = \int_{-\infty}^\infty e^{-z^2/2} \, dz = \sqrt{2\pi}$$

The normalizing constant $c$ required for $\int_{-\infty}^\infty c e^{-z^2/2} dz = 1$ is:
$$c = \frac{1}{\sqrt{2\pi}}$$

---

### Step 5: Extension to General Normal $\mathcal{N}(\mu, \sigma^2)$
For a general normal random variable $X \sim \mathcal{N}(\mu, \sigma^2)$, substitute $z = \frac{x - \mu}{\sigma}$, so $dx = \sigma \, dz$:
$$\int_{-\infty}^\infty e^{-\frac{(x - \mu)^2}{2\sigma^2}} \, dx = \int_{-\infty}^\infty e^{-z^2/2} \, (\sigma \, dz) = \sigma \sqrt{2\pi}$$

Therefore, the general Gaussian PDF is:
$$f_X(x) = \frac{1}{\sigma \sqrt{2\pi}} \exp\left( -\frac{(x - \mu)^2}{2\sigma^2} \right)$$

---

## Result

$$\boxed{\int_{-\infty}^\infty e^{-z^2/2} \, dz = \sqrt{2\pi}}$$
$$\boxed{c = \frac{1}{\sqrt{2\pi}} \approx 0.398942}$$

---

## Why This Works

The difficulty in 1D is that $e^{-z^2/2}$ has rotational symmetry in the 2D plane that is invisible along a single axis. Moving to 2D exposes the circular symmetry $x^2 + y^2 = r^2$. The Jacobian $|J| = r$ arises because the circumference of a circle of radius $r$ is $2\pi r$, which grows linearly with $r$, supplying the exact differential weight needed to undo the chain rule for $e^{-r^2/2}$.

---

## Common Mistakes

- **Omitting the Jacobian $r$:** Writing $dx \, dy = dr \, d\theta$, which violates the multivariable change-of-variables theorem and produces an unsolvable integral.
- **Incorrect Angular Limits:** Integrating $\theta$ from $0$ to $\pi$ instead of $0$ to $2\pi$ (covering only a half-plane).
- **Confusing Kernel with PDF:** Forgetting that $\sqrt{2\pi}$ is in the *denominator* of the PDF $f_Z(z) = \frac{1}{\sqrt{2\pi}} e^{-z^2/2}$, but in the *numerator* of the integral value $\int e^{-z^2/2} dz = \sqrt{2\pi}$.

---

## Related Concepts

- [[Continuous Probability Distributions]] — Standard Normal and general Gaussian distributions.
- [[Central Limit Theorem]] — Why the Gaussian distribution emerges as the universal limit of sums.
- [[Joint and Marginal Distributions]] — Multivariable change of variables and Jacobian transformations.

---

## Sources

- [[cse301/01 - Sources/Lectures/HT Final Note - Suchi.pdf|HT Final Note: Lecture 13 (pp. 53–54)]]
- [[cse301/01 - Sources/Lectures/ITP.pdf|Textbook: Introduction to Probability (Blitzstein & Hwang, Section 5.4: Normal Distribution)]]
