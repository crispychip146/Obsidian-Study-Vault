---
type: example
course: cse301
status: active
order: 100
---

# Shoe Shine Shop Queueing Model Example

> 📖 **Reading Order:** Step 100 of 103 | **Module 11:** Queuing Theory  
> ◄ **Previous:** [[Jackson Networks and Tandem Queues]] | ► **Next:** [[Tandem Two-Server Queue Performance Example]]
---
## Problem

A shoe shine shop has two chairs, each staffed by a dedicated server:
- An entering customer first sits in **Chair 1**, where shoes are cleaned and prepared at exponential service rate $\mu_1$.
- Upon completing service in Chair 1, the customer moves to **Chair 2** for polishing at exponential service rate $\mu_2$, provided Chair 2 is empty.
- If Chair 2 is currently occupied, the customer **remains seated in Chair 1**, blocking Chair 1 from accepting any new customers until Chair 2 becomes free.
- There is no separate waiting room. Customers arrive from outside according to a Poisson process with rate $\lambda$.
- If an arriving customer finds Chair 1 occupied (either actively being served or blocked waiting for Chair 2), the customer leaves immediately and is lost.

1. Define a state space that fully captures the physical state of the shop.
2. Draw the state transition rate diagram and formulate the steady-state balance equations.
3. Express the steady-state probabilities in terms of $P_{00}$.
4. Find the proportion of potential customers who actually enter the shop.
5. Derive the mean time $W$ that an entering customer spends in the shop.
---
## Given

- Server 1 rate: $\mu_1$ (Chair 1)
- Server 2 rate: $\mu_2$ (Chair 2)
- Arrival rate: $\lambda$ (Poisson)
- Capacity: At most 2 customers total in the shop.
- Blocking rule: When Chair 1 is done and Chair 2 is busy, Chair 1 enters blocked state $b$.
---
## Required

1. Minimal complete state space representation.
2. Rate in = rate out balance equations.
3. Closed-form stationary probabilities.
4. Entry proportion and effective throughput $\lambda_a$.
5. Average residence time $W$ via Little's Law.
---
## Understanding the Problem and Choosing the Method

Identify the random variables, state the conditional distributions, select the appropriate probabilistic law or updating formula, and execute the algebraic substitutions step by step.

---

## Solution

### Concepts Used

- [[Queueing Systems and Kendall Notation]]
- [[PASTA Property and Inspection Paradox]]
- [[Little's Law]]
- Continuous-Time Markov Chain balance equations

---
### Solution

### Step 1: Choosing the State Space
Merely counting the total number of customers $N \in \{0, 1, 2\}$ is insufficient, because when 1 customer is present, they could be in Chair 1 or Chair 2. Furthermore, when 2 customers are present, Chair 1 could be actively working or blocked.

We represent the state by the ordered pair:
$$(s_1, s_2)$$
where:
- $s_1 \in \{0, 1, b\}$: Chair 1 is empty ($0$), serving ($1$), or blocked ($b$).
- $s_2 \in \{0, 1\}$: Chair 2 is empty ($0$) or serving ($1$).

The feasible state space consists of **5 distinct states**:
$$S = \{(0, 0), \; (1, 0), \; (0, 1), \; (1, 1), \; (b, 1)\}$$

Notice that $(b, 0)$ is impossible because if Chair 2 were empty, the customer would instantly move into Chair 2.

---

### Step 2: Transition Rate Diagram and Balance Equations

```
                λ                  µ1
       (0, 0) ────> (1, 0) ───────────────────┐
         ▲            │                        │
         │ µ2         │ µ2                     ▼
         │            ▼                  ┌──> (0, 1)
       (0, 1) <──── (1, 1) ────> (b, 1) ─┘      │
         ▲            ▲    µ1      │            │ λ
         │            │            │ µ2         ▼
         └────────────┴────────────┘          (1, 1)
```

Setting **Total Rate Exiting = Total Rate Entering** for each state:

1. **State $(0, 0)$:**
   $$\lambda P_{00} = \mu_2 P_{01}$$
2. **State $(1, 0)$:**
   Exits via service at Chair 1 ($\mu_1$). Enters via arrival to empty shop ($\lambda P_{00}$) or Chair 2 completing service while Chair 1 is still busy ($\mu_2 P_{11}$):
   $$\mu_1 P_{10} = \lambda P_{00} + \mu_2 P_{11}$$
3. **State $(0, 1)$:**
   Exits via arrival ($\lambda$) or Chair 2 finishing service ($\mu_2$). Enters via Chair 1 finishing when Chair 2 was empty ($\mu_1 P_{10}$) or Chair 2 finishing while Chair 1 was blocked ($\mu_2 P_{b1}$):
   $$(\lambda + \mu_2) P_{01} = \mu_1 P_{10} + \mu_2 P_{b1}$$
4. **State $(1, 1)$:**
   Exits via Chair 1 finishing ($\mu_1$) or Chair 2 finishing ($\mu_2$). Enters via arrival when Chair 2 is busy ($\lambda P_{01}$):
   $$(\mu_1 + \mu_2) P_{11} = \lambda P_{01}$$
5. **State $(b, 1)$:**
   Exits via Chair 2 finishing ($\mu_2$). Enters via Chair 1 finishing service while Chair 2 is still occupied ($\mu_1 P_{11}$):
   $$\mu_2 P_{b1} = \mu_1 P_{11}$$

---

### Step 3: Expressing Probabilities in Terms of $P_{00}$

1. From Equation (1):
   $$P_{01} = \frac{\lambda}{\mu_2} P_{00}$$
2. From Equation (4):
   $$P_{11} = \frac{\lambda}{\mu_1 + \mu_2} P_{01} = \frac{\lambda^2}{\mu_2(\mu_1 + \mu_2)} P_{00}$$
3. From Equation (5):
   $$P_{b1} = \frac{\mu_1}{\mu_2} P_{11} = \frac{\lambda^2 \mu_1}{\mu_2^2 (\mu_1 + \mu_2)} P_{00}$$
4. From Equation (2):
   $$\mu_1 P_{10} = \lambda P_{00} + \mu_2 \left( \frac{\lambda^2}{\mu_2(\mu_1 + \mu_2)} P_{00} \right) = \lambda P_{00} \left( 1 + \frac{\lambda}{\mu_1 + \mu_2} \right) = \lambda P_{00} \left( \frac{\lambda + \mu_1 + \mu_2}{\mu_1 + \mu_2} \right)$$
   $$P_{10} = \frac{\lambda(\lambda + \mu_1 + \mu_2)}{\mu_1(\mu_1 + \mu_2)} P_{00}$$

5. **Normalization:**
   $$P_{00} + P_{10} + P_{01} + P_{11} + P_{b1} = 1$$
   $$P_{00} \left[ 1 + \frac{\lambda(\lambda + \mu_1 + \mu_2)}{\mu_1(\mu_1 + \mu_2)} + \frac{\lambda}{\mu_2} + \frac{\lambda^2}{\mu_2(\mu_1 + \mu_2)} + \frac{\lambda^2 \mu_1}{\mu_2^2(\mu_1 + \mu_2)} \right] = 1$$
   Solving yields $P_{00}$, and back-substituting gives all five probabilities.

---

### Step 4: Proportion of Customers Who Enter
An arriving customer enters if and only if Chair 1 is empty, which corresponds to the system being in state $(0, 0)$ or $(0, 1)$.
By [[PASTA Property and Inspection Paradox|PASTA]]:
$$\text{Proportion Who Enter} = P_{\text{enter}} = P_{00} + P_{01}$$

Conversely, the proportion of lost customers is:
$$P_{\text{lost}} = 1 - P_{\text{enter}} = P_{10} + P_{11} + P_{b1}$$

The effective arrival rate of entering customers is:
$$\lambda_a = \lambda (P_{00} + P_{01})$$

---

### Step 5: Mean Time in System ($W$)
The average number of customers present in the shop ($L$) is:
$$L = 0 \cdot P_{00} + 1 \cdot (P_{10} + P_{01}) + 2 \cdot (P_{11} + P_{b1}) = P_{10} + P_{01} + 2(P_{11} + P_{b1})$$

Applying [[Little's Law]]:
$$W = \frac{L}{\lambda_a} = \frac{P_{10} + P_{01} + 2(P_{11} + P_{b1})}{\lambda(P_{00} + P_{01})}$$

---
### Key Insight: The Cost of Blocking

Notice that when $\mu_1 \gg \mu_2$ (cleaning is very fast, polishing is slow), Chair 1 spends almost all its time in the blocked state $(b, 1)$. Even though Server 1 is available and sitting idle, incoming customers are turned away at the door because the previous customer has nowhere to move. This illustrates the importance of intermediate buffers in pipeline computing architectures.
---
## Result

- Feasible state space requires tracking blocked states: $S = \{(0,0), (1,0), (0,1), (1,1), (b,1)\}$.
- Entry proportion: $P_{00} + P_{01}$.
- Mean residence time: $W = \frac{L}{\lambda(P_{00} + P_{01})}$.
---
## Why This Works

The solution holds because every step follows directly from Bayes' rule, the law of total probability, or properties of expectation and variance.

---

## Common Mistakes

- Forgetting normalization constants when evaluating continuous posterior densities.
- Misidentifying degrees of freedom in chi-square tests.

---

## General Method

Extract the reusable problem-solving pattern: define random variables, write down the joint distribution, condition on observed data, and normalize the resulting distribution.

---

## Related Concepts

- [[Queueing Systems and Kendall Notation]]
- [[PASTA Property and Inspection Paradox]]
- [[Little's Law]]
- [[Finite Capacity M-M-1-N Queue]]
---
## Sources

- [[cse301/01 - Sources/Lectures/CSE301_Queueing_Theory.pdf]]
