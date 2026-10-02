---
type: formula
course: cse301
status: active
order: 44
---

# Likelihood and Score Equations

> 📖 **Reading Order:** Step 44 of 92 | **Module 7:** Parametric Inference  
> ◄ **Previous:** [[Maximum Likelihood Estimation]] | ► **Next:** [[Normal Distribution Parameter MLE Derivation Example]]

---

## Formula

For an independent and identically distributed (i.i.d.) sample $X_1, X_2, \dots, X_n \sim f(x; \theta)$:

### 1. Likelihood Function
$$L_n(\theta) = \prod_{i=1}^n f(X_i; \theta)$$

### 2. Log-Likelihood Function
$$\ell_n(\theta) = \log L_n(\theta) = \sum_{i=1}^n \log f(X_i; \theta)$$

### 3. Score Function and Score Equation
The **score function** $S_n(\theta)$ is the gradient of the log-likelihood:
$$S_n(\theta) = \frac{\partial \ell_n(\theta)}{\partial \theta} = \sum_{i=1}^n \frac{\partial \log f(X_i; \theta)}{\partial \theta}$$

The **score equation** sets the score function to zero:
$$S_n(\theta) = 0 \iff \sum_{i=1}^n \frac{\partial \log f(X_i; \theta)}{\partial \theta} = 0$$

### 4. Fisher Information
The **Fisher Information** in a single observation is:
$$I_1(\theta) = - E_\theta\left[\frac{\partial^2 \log f(X; \theta)}{\partial \theta^2}\right] = \text{Var}_\theta\left(\frac{\partial \log f(X; \theta)}{\partial \theta}\right)$$

For a sample of $n$ independent observations:
$$I_n(\theta) = n I_1(\theta) = - E_\theta\left[\frac{\partial^2 \ell_n(\theta)}{\partial \theta^2}\right]$$

### 5. Asymptotic Standard Error of the MLE
$$\text{se}(\hat{\theta}_n) \approx \frac{1}{\sqrt{I_n(\theta)}} = \frac{1}{\sqrt{n I_1(\theta)}}$$
The plug-in estimated standard error is:
$$\widehat{\text{se}}(\hat{\theta}_n) = \frac{1}{\sqrt{I_n(\hat{\theta}_n)}}$$

---

## Variables

| Symbol | Meaning | Dimensions |
|---|---|---|
| $L_n(\theta)$ | Joint likelihood of the observed dataset | Scalar $\ge 0$ |
| $\ell_n(\theta)$ | Logarithm of the likelihood function | Real number |
| $S_n(\theta)$ | Score function (slope of log-likelihood) | Vector / Scalar |
| $I_1(\theta)$ | Fisher Information from one observation | Positive scalar / Pos-def matrix |
| $I_n(\theta)$ | Total Fisher Information from $n$ samples | $n I_1(\theta)$ |
| $\hat{\theta}_n$ | Root of the score equation (MLE) | Parameter space $\Theta$ |

---

## Conditions

1. **Differentiability:** The density $f(x; \theta)$ must be twice continuously differentiable with respect to $\theta$.
2. **Common Support:** The support $\{x : f(x; \theta) > 0\}$ must **not** depend on the parameter $\theta$ (this allows differentiation under the integral sign).
3. **Identifiability:** Distinct parameter values must yield distinct probability distributions: $\theta_1 \ne \theta_2 \implies f(x; \theta_1) \ne f(x; \theta_2)$.

---

## Intuition

- The **score function** $S_n(\theta)$ represents the slope of the log-likelihood curve at any candidate parameter $\theta$. If $S_n(\theta) > 0$, increasing $\theta$ increases likelihood; if $S_n(\theta) < 0$, decreasing $\theta$ increases likelihood. At the optimal parameter guess $\hat{\theta}_{\text{MLE}}$, the curve reaches its peak, where the slope is flat ($S_n = 0$).
- The **Fisher Information** $I_n(\theta)$ measures the **curvature** (concavity) of the log-likelihood peak. If the log-likelihood curve is sharply curved (large second derivative, high Fisher information), the peak is narrowly defined and our estimate $\hat{\theta}$ has very small variance. If the peak is flat and broad (low Fisher information), the data provide little precision and $\hat{\theta}$ has high standard error.

---

## Derivation of Expected Score and Fisher Information Identity

### Proposition 1: The Expected Score is Always Zero
Assuming we can interchange derivative and integral:
$$E_\theta[S_1(\theta)] = E_\theta\left[\frac{\partial \log f(X; \theta)}{\partial \theta}\right] = \int \frac{\partial \log f(x; \theta)}{\partial \theta} f(x; \theta) dx$$

Using the identity $\frac{\partial \log u}{\partial \theta} = \frac{1}{u}\frac{\partial u}{\partial \theta}$:
$$= \int \frac{1}{f(x; \theta)}\frac{\partial f(x; \theta)}{\partial \theta} f(x; \theta) dx = \int \frac{\partial f(x; \theta)}{\partial \theta} dx$$

Interchanging the derivative and integral:
$$= \frac{\partial}{\partial \theta} \int f(x; \theta) dx = \frac{\partial}{\partial \theta}(1) = 0 \quad \blacksquare$$

### Proposition 2: Curvature Equals Variance of Score
Differentiate the identity $\int \frac{\partial \log f(x; \theta)}{\partial \theta} f(x; \theta) dx = 0$ with respect to $\theta$:
$$\int \left( \frac{\partial^2 \log f(x; \theta)}{\partial \theta^2} f(x; \theta) + \frac{\partial \log f(x; \theta)}{\partial \theta} \frac{\partial f(x; \theta)}{\partial \theta} \right) dx = 0$$

Substitute $\frac{\partial f(x; \theta)}{\partial \theta} = \frac{\partial \log f(x; \theta)}{\partial \theta} f(x; \theta)$:
$$\int \frac{\partial^2 \log f(x; \theta)}{\partial \theta^2} f(x; \theta) dx + \int \left(\frac{\partial \log f(x; \theta)}{\partial \theta}\right)^2 f(x; \theta) dx = 0$$

In expectation notation:
$$E_\theta\left[\frac{\partial^2 \log f(X; \theta)}{\partial \theta^2}\right] + E_\theta\left[S_1(\theta)^2\right] = 0$$

Since $E_\theta[S_1(\theta)] = 0$, $\text{Var}_\theta(S_1(\theta)) = E_\theta[S_1(\theta)^2]$. Therefore:
$$I_1(\theta) = - E_\theta\left[\frac{\partial^2 \log f(X; \theta)}{\partial \theta^2}\right] = \text{Var}_\theta(S_1(\theta)) \quad \blacksquare$$

---

## Example: Poisson Rate Parameter

Let $X_1, \dots, X_n \sim \text{Poisson}(\lambda)$, where $f(x; \lambda) = \frac{e^{-\lambda}\lambda^x}{x!}$ for $x \in \{0, 1, 2, \dots\}$.

1. **Log-Likelihood:**
   $$\ell_n(\lambda) = \sum_{i=1}^n \left( -\lambda + X_i \log \lambda - \log(X_i!) \right) = -n\lambda + \left(\sum_{i=1}^n X_i\right) \log \lambda - \sum_{i=1}^n \log(X_i!)$$
2. **Score Equation:**
   $$S_n(\lambda) = \frac{\partial \ell_n}{\partial \lambda} = -n + \frac{\sum X_i}{\lambda} = 0$$
   $$\implies \hat{\lambda}_{\text{MLE}} = \frac{1}{n}\sum_{i=1}^n X_i = \bar{X}$$
3. **Fisher Information:**
   $$\frac{\partial^2 \ell_n}{\partial \lambda^2} = -\frac{\sum X_i}{\lambda^2}$$
   $$I_n(\lambda) = -E\left[-\frac{\sum X_i}{\lambda^2}\right] = \frac{n E[X_i]}{\lambda^2} = \frac{n \lambda}{\lambda^2} = \frac{n}{\lambda}$$
4. **Asymptotic Standard Error:**
   $$\text{se}(\hat{\lambda}) = \frac{1}{\sqrt{I_n(\lambda)}} = \sqrt{\frac{\lambda}{n}} \implies \widehat{\text{se}} = \sqrt{\frac{\bar{X}}{n}}$$

---

## Common Mistakes

- Forgetting to take the negative expectation when computing Fisher information: $I(\theta) = -E[\ell'']$, not $E[\ell'']$.
- Forgetting that the score equation requires the support of the distribution to be independent of $\theta$.

---

## Related Concepts

- [[Maximum Likelihood Estimation]]
- [[Point Estimation]]
- [[Wald Test Statistic]]

---

## Sources

- [[01 - Sources/Lectures/CSE301_Maximum_Likelihood_Estimators__MLE_.pdf]]
- [[01 - Sources/Lectures/MLE.pdf]]
