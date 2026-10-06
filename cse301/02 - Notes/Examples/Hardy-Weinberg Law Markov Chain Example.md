---
type: example
course: cse301
status: active
order: 87
---

# Hardy-Weinberg Law Markov Chain Example

> 📖 **Reading Order:** Step 87 of 103 | **Module 10:** Stochastic Processes  
> ◄ **Previous:** [[Higher-Order State Weather Prediction Example]] | ► **Next:** [[Problem — Patty and Max Gambler's Ruin]]
---
## Problem

In population genetics, consider a gene with two alleles, $A$ and $a$. An individual's genotype consists of a pair of these genes: $AA$, $aa$, or $Aa$.

In a large population, suppose the initial fractions of individuals with genotypes $AA$, $aa$, and $Aa$ are $p_0$, $q_0$, and $r_0$ respectively, where $p_0 + q_0 + r_0 = 1$. Mating is completely random, meaning each parent contributes one gene chosen uniformly at random to their offspring.

1. Determine the probability that a gene chosen at random from the population is of type $A$ or type $a$.
2. Derive the genotype proportions $p, q, r$ in the next generation.
3. Prove that the allele frequencies remain constant in all subsequent generations (**Hardy-Weinberg Law**).
4. Model the genetic lineage of a single individual across generations as a three-state [[Markov Chain]], determine its transition probability matrix $P$, and prove that the stationary distribution is $\pi = (p, q, r)$.
---
## Given

- Genotypes: $\{AA, aa, Aa\}$
- Initial generation proportions: $P(AA) = p_0, \; P(aa) = q_0, \; P(Aa) = r_0$ with $p_0 + q_0 + r_0 = 1$.
- Random mating with equal inheritance:
  - An $AA$ parent always transmits an $A$ gene (probability $1$).
  - An $aa$ parent always transmits an $a$ gene (probability $1$).
  - An $Aa$ parent transmits an $A$ gene with probability $1/2$, and an $a$ gene with probability $1/2$.
---
## Required

1. Allele frequencies $P(A)$ and $P(a)$.
2. Offspring genotype frequencies $p = P(AA)$, $q = P(aa)$, $r = P(Aa)$.
3. Algebraic invariance proof: $P_{\text{next}}(A) = P_{\text{initial}}(A)$.
4. Markov transition matrix $P$ for descendant lineage and verification of stationary distribution $\pi P = \pi$.
---
## Understanding the Problem and Choosing the Method

Identify the random variables, state the conditional distributions, select the appropriate probabilistic law or updating formula, and execute the algebraic substitutions step by step.

---

## Solution

### Concepts Used

- [[Markov Chain]]
- [[Stationary and Limiting Distributions in Markov Chains]]
- Law of Total Probability
- Conditional Probability

---
### Solution

### Step 1: Probability that a Random Gene Is Type $A$ or $a$
Selecting a gene from a randomly chosen parent is mathematically equivalent to drawing a gene uniformly at random from the population's entire gene pool.

Conditioning on the genotype of the parent:
$$\begin{aligned}
P(A) &= P(A \mid AA) P(AA) + P(A \mid aa) P(aa) + P(A \mid Aa) P(Aa) \\
&= 1 \cdot p_0 + 0 \cdot q_0 + \frac{1}{2} \cdot r_0 \\
&= p_0 + \frac{r_0}{2}
\end{aligned}$$

Similarly, for allele $a$:
$$\begin{aligned}
P(a) &= P(a \mid AA) P(AA) + P(a \mid aa) P(aa) + P(a \mid Aa) P(Aa) \\
&= 0 \cdot p_0 + 1 \cdot q_0 + \frac{1}{2} \cdot r_0 \\
&= q_0 + \frac{r_0}{2}
\end{aligned}$$

Check: $P(A) + P(a) = p_0 + q_0 + r_0 = 1$.

---

### Step 2: Genotype Proportions in the Offspring Generation
Under random mating, a child inherits two genes drawn independently from the gene pool:
- **Genotype $AA$:** Child receives gene $A$ from both parents:
  $$p = P(A) \cdot P(A) = \left(p_0 + \frac{r_0}{2}\right)^2$$
- **Genotype $aa$:** Child receives gene $a$ from both parents:
  $$q = P(a) \cdot P(a) = \left(q_0 + \frac{r_0}{2}\right)^2$$
- **Genotype $Aa$:** Child receives $A$ from mother and $a$ from father, OR $a$ from mother and $A$ from father:
  $$r = 2 \cdot P(A) \cdot P(a) = 2 \left(p_0 + \frac{r_0}{2}\right)\left(q_0 + \frac{r_0}{2}\right)$$

---

### Step 3: Proof of the Hardy-Weinberg Invariance Law
Does the proportion of $A$ genes change in the new generation?
In the new generation, an individual has genotype $AA$ with probability $p$, and $Aa$ with probability $r$.
Therefore, the probability that a gene chosen from this new generation is $A$ is:
$$P_{\text{new}}(A) = p + \frac{r}{2}$$

Substituting the formulas for $p$ and $r$ derived in Step 2:
$$\begin{aligned}
P_{\text{new}}(A) &= \left(p_0 + \frac{r_0}{2}\right)^2 + \frac{1}{2} \cdot 2 \left(p_0 + \frac{r_0}{2}\right)\left(q_0 + \frac{r_0}{2}\right) \\
&= \left(p_0 + \frac{r_0}{2}\right)^2 + \left(p_0 + \frac{r_0}{2}\right)\left(q_0 + \frac{r_0}{2}\right)
\end{aligned}$$

Factor out the common term $\left(p_0 + \frac{r_0}{2}\right)$:
$$\begin{aligned}
P_{\text{new}}(A) &= \left(p_0 + \frac{r_0}{2}\right) \left[ \left(p_0 + \frac{r_0}{2}\right) + \left(q_0 + \frac{r_0}{2}\right) \right] \\
&= \left(p_0 + \frac{r_0}{2}\right) [p_0 + q_0 + r_0]
\end{aligned}$$

Since $p_0 + q_0 + r_0 = 1$:
$$P_{\text{new}}(A) = p_0 + \frac{r_0}{2} = P_{\text{initial}}(A) \quad \blacksquare$$

Because allele frequencies $P(A)$ and $P(a)$ do not change from generation to generation, the genotype frequencies $p, q, r$ remain completely fixed in all subsequent generations:
$$p = \left(p + \frac{r}{2}\right)^2, \quad q = \left(q + \frac{r}{2}\right)^2, \quad r = 2\left(p + \frac{r}{2}\right)\left(q + \frac{r}{2}\right)$$

---

### Step 4: Lineage Markov Chain and Stationary Distribution
Now consider following the lineage of a single individual over generations:
Let $X_n$ be the genotype of the descendant at generation $n$, with state space $\{AA, aa, Aa\}$.

Each descendant mates with a partner drawn uniformly from the stabilized population (where proportions are $p, q, r$).
The partner transmits an $A$ gene with probability $p + r/2$, and an $a$ gene with probability $q + r/2$.

#### Formulate Transition Matrix $P$:
Order rows and columns as $1 = AA, \; 2 = aa, \; 3 = Aa$:

1. **Given Parent is $AA$:** Transmits $A$ with probability 1.
   - Child is $AA$ if mate transmits $A$: probability $p + r/2$.
   - Child is $aa$: probability $0$.
   - Child is $Aa$ if mate transmits $a$: probability $q + r/2$.
   - Row $AA$: $\begin{pmatrix} p + \frac{r}{2} & 0 & q + \frac{r}{2} \end{pmatrix}$

2. **Given Parent is $aa$:** Transmits $a$ with probability 1.
   - Child is $AA$: probability $0$.
   - Child is $aa$ if mate transmits $a$: probability $q + r/2$.
   - Child is $Aa$ if mate transmits $A$: probability $p + r/2$.
   - Row $aa$: $\begin{pmatrix} 0 & q + \frac{r}{2} & p + \frac{r}{2} \end{pmatrix}$

3. **Given Parent is $Aa$:** Transmits $A$ with probability $1/2$, and $a$ with probability $1/2$.
   - Child is $AA$: parent transmits $A$ ($1/2$) AND mate transmits $A$ ($p + r/2$):
     $$P(AA) = \frac{1}{2}\left(p + \frac{r}{2}\right) = \frac{p}{2} + \frac{r}{4}$$
   - Child is $aa$: parent transmits $a$ ($1/2$) AND mate transmits $a$ ($q + r/2$):
     $$P(aa) = \frac{1}{2}\left(q + \frac{r}{2}\right) = \frac{q}{2} + \frac{r}{4}$$
   - Child is $Aa$:
     $$P(Aa) = 1 - [P(AA) + P(aa)] = \frac{p}{2} + \frac{q}{2} + \frac{r}{2}$$

Thus, the transition probability matrix is:
$$P = \begin{pmatrix}
p + \frac{r}{2} & 0 & q + \frac{r}{2} \\
0 & q + \frac{r}{2} & p + \frac{r}{2} \\
\frac{p}{2} + \frac{r}{4} & \frac{q}{2} + \frac{r}{4} & \frac{p}{2} + \frac{q}{2} + \frac{r}{2}
\end{pmatrix}$$

#### Verify Stationary Distribution $\pi = \begin{pmatrix} p & q & r \end{pmatrix}$:
We check $\pi P = \pi$ by multiplying $\pi$ with each column of $P$:

- **Column 1 ($AA$):**
  $$\begin{aligned}
  (\pi P)_1 &= p \left(p + \frac{r}{2}\right) + q(0) + r \left(\frac{p}{2} + \frac{r}{4}\right) \\
  &= p \left(p + \frac{r}{2}\right) + \frac{r}{2} \left(p + \frac{r}{2}\right) \\
  &= \left(p + \frac{r}{2}\right)\left(p + \frac{r}{2}\right) = \left(p + \frac{r}{2}\right)^2
  \end{aligned}$$
  By the Hardy-Weinberg law proven in Step 3, $\left(p + \frac{r}{2}\right)^2 = p = \pi_1$.

- **Column 2 ($aa$):**
  $$\begin{aligned}
  (\pi P)_2 &= p(0) + q \left(q + \frac{r}{2}\right) + r \left(\frac{q}{2} + \frac{r}{4}\right) \\
  &= q \left(q + \frac{r}{2}\right) + \frac{r}{2} \left(q + \frac{r}{2}\right) \\
  &= \left(q + \frac{r}{2}\right)^2 = q = \pi_2
  \end{aligned}$$

- **Column 3 ($Aa$):**
  Since probabilities sum to 1:
  $$(\pi P)_3 = 1 - [(\pi P)_1 + (\pi P)_2] = 1 - [p + q] = r = \pi_3$$

Thus, $\pi P = \pi$, confirming that $\pi = (p, q, r)$ is indeed the stationary distribution of the descendant Markov chain. $\blacksquare$
---
## Result

1. Allele frequencies: $P(A) = p_0 + r_0/2$, $P(a) = q_0 + r_0/2$.
2. Offspring genotype frequencies: $p = P(A)^2, \; q = P(a)^2, \; r = 2P(A)P(a)$.
3. Allele frequencies remain invariant across generations ($P_{\text{new}}(A) = P_{\text{old}}(A)$).
4. Stationary distribution of the lineage Markov chain matches the population frequencies: $\pi = (p, q, r)$.
---
## Why This Works

The stability of the gene pool mirrors the convergence of a Markov chain to its stationary distribution: once the population reaches random-mating equilibrium, the probability distribution of an individual descendant's genotype matches the macroscopic composition of the entire population.
---
## Common Mistakes

- **Forgetting the Factor of 2 in $r$:** Writing $r = P(A)P(a)$ instead of $2P(A)P(a)$. The genotype $Aa$ can be formed in two mutually exclusive ways (mother $A$ / father $a$ OR mother $a$ / father $A$).
- **Failing to Factor Out $(p + r/2)$ in the Invariance Proof:** Trying to expand everything into polynomials and getting lost in algebra rather than using the identity $p_0 + q_0 + r_0 = 1$.
- **Assuming One Generation Changes Allele Frequencies:** Assuming that random mating changes allele frequencies. Random mating only redistributes alleles into genotypes; it never alters allele frequencies.
---
## General Method

Extract the reusable problem-solving pattern: define random variables, write down the joint distribution, condition on observed data, and normalize the resulting distribution.

---

## Related Concepts

- [[Markov Chain]]
- [[Stationary and Limiting Distributions in Markov Chains]]
- [[Stochastic Process]]
---
## Sources

- [[cse301/01 - Sources/Lectures/Markov_Chain.pdf]] (Slides 25–29)
- [[cse301/01 - Sources/Textbooks/Sheldon M. Ross book Markov Chain Chapter.pdf]] (Example 4.14, pp. 215–217)
