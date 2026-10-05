---
type: formula
course: cse301
status: active
order: 86
---

# M-M-1 Performance Formulas

> 📖 **Reading Order:** Step 86 of 92 | **Module 11:** Queuing Theory  
> ◄ **Previous:** [[M-M-1 Queue]] | ► **Next:** [[Finite Capacity M-M-1-N Queue]]

---

## Building the idea

The formula table has a short chain of reasoning. First normalize the geometric state probabilities to get $\pi_n=(1-\rho)\rho^n$. Summing their weighted counts gives $L=\rho/(1-\rho)$.

An arriving customer is admitted in the infinite-buffer stable model, so [[Little's Law]] gives $W=L/\lambda=1/(\mu-\lambda)$. Subtract the mean service time $1/\mu$ to obtain queue waiting time $W_Q$. Multiplying that wait by $\lambda$ gives $L_Q$. The tail sum of the geometric distribution gives $P(N\ge k)=\rho^k$.

These are stationary averages under the stated M/M/1 assumptions, not deadlines for individual customers. If $\lambda$ rises toward $\mu$, the denominator $\mu-\lambda$ shrinks and delay grows rapidly. Keep units consistent; a service rate of 1000 per second corresponds to a mean service time of one millisecond.

## Formula

For an M/M/1 queue with Poisson arrival rate $\lambda$, exponential service rate $\mu$, and stability condition $\rho = \frac{\lambda}{\mu} < 1$:

| Quantity | Symbol | Formula | Alternative ($\rho$-form) |
|---|---|---|---|
| **Traffic Intensity / Utilization** | $\rho$ | $\frac{\lambda}{\mu}$ | $\rho$ |
| **Probability of Empty System** | $P_0$ | $1 - \frac{\lambda}{\mu}$ | $1 - \rho$ |
| **Steady-State Probability of $n$ Customers** | $P_n$ | $\left(1 - \frac{\lambda}{\mu}\right)\left(\frac{\lambda}{\mu}\right)^n$ | $(1 - \rho)\rho^n$ |
| **Probability of at least $k$ Customers** | $P(N \ge k)$ | $\left(\frac{\lambda}{\mu}\right)^k$ | $\rho^k$ |
| **Average Number of Customers in System** | $L$ | $\frac{\lambda}{\mu - \lambda}$ | $\frac{\rho}{1 - \rho}$ |
| **Average Number of Customers in Queue** | $L_Q$ | $\frac{\lambda^2}{\mu(\mu - \lambda)}$ | $\frac{\rho^2}{1 - \rho}$ |
| **Average Time Spent in System** | $W$ | $\frac{1}{\mu - \lambda}$ | $\frac{1}{\mu(1 - \rho)}$ |
| **Average Time Spent Waiting in Queue** | $W_Q$ | $\frac{\lambda}{\mu(\mu - \lambda)}$ | $\frac{\rho}{\mu(1 - \rho)}$ |

---

## Intuition

### Continuous Residence Time Distributions

Beyond average values, the M/M/1 queue permits exact closed-form probability distributions for individual customer wait times:

### 1. Total Time in System ($T$)
The total residence time $T$ (queue wait + service) of an entering customer follows an **Exponential distribution** with parameter $\mu - \lambda$:
$$f_T(t) = (\mu - \lambda) e^{-(\mu - \lambda)t}, \quad t \ge 0$$
The tail probability that a customer spends more than $t$ time units in the system is:
$$\mathbf{P(T > t) = e^{-(\mu - \lambda)t} = e^{-\mu(1 - \rho)t}}$$

### 2. Time Waiting in Queue ($T_Q$)
Because an arriving customer finds an empty system with probability $1 - \rho$, there is a discrete mass at zero wait time:
$$P(T_Q = 0) = P_0 = 1 - \rho$$
For $t > 0$, the tail probability is:
$$\mathbf{P(T_Q > t) = \rho e^{-(\mu - \lambda)t}}$$

---
### Quick Reference Identities

$$\begin{aligned}
L &= L_Q + \rho \\
W &= W_Q + \frac{1}{\mu} \\
L &= \lambda W \quad (\text{Little's Law}) \\
L_Q &= \lambda W_Q \quad (\text{Little's Law}) \\
L_Q &= \rho L \\
W_Q &= \rho W
\end{aligned}$$

---

## Derivation

### Derivation of $P(N \ge k)$

To find the probability that the system holds at least $k$ customers:
$$P(N \ge k) = \sum_{n=k}^\infty P_n = \sum_{n=k}^\infty (1 - \rho)\rho^n$$
Factor out $\rho^k$:
$$= (1 - \rho)\rho^k \sum_{m=0}^\infty \rho^m$$
Since $\sum_{m=0}^\infty \rho^m = \frac{1}{1 - \rho}$:
$$P(N \ge k) = (1 - \rho)\rho^k \left(\frac{1}{1 - \rho}\right) = \mathbf{\rho^k} \quad \blacksquare$$

---

## Example

### Example: Quick Parameter Calculation

A web server handles $\lambda = 40$ requests/sec with capacity $\mu = 50$ requests/sec.
1. **Utilization:** $\rho = \frac{40}{50} = 0.80$ ($80\%$ busy).
2. **Idle Probability:** $P_0 = 1 - 0.80 = 0.20$ ($20\%$ idle).
3. **Average requests in server:** $L = \frac{0.80}{1 - 0.80} = \frac{0.80}{0.20} = 4$ requests.
4. **Average latency:** $W = \frac{1}{50 - 40} = \frac{1}{10} = 0.10 \text{ sec} = 100 \text{ ms}$.
5. **Average wait in buffer:** $W_Q = \frac{0.80}{50(1 - 0.80)} = \frac{0.80}{10} = 0.08 \text{ sec} = 80 \text{ ms}$.
6. **Average in buffer:** $L_Q = 40 \times 0.08 = 3.2$ requests.
7. **Probability of queue backlog $> 3$ requests:** $P(N \ge 4) = (0.80)^4 = 0.4096 \approx 41\%$.

---

## What to carry forward

Use $L-L_Q=\rho$ and $W-W_Q=1/\mu$ as consistency checks. Apply the table only after checking $\rho<1$ and the system boundary.

## Related notes

- [[Little's Law]]

## Sources

- [[01 - Sources/Lectures/CSE301_Queueing_Theory.pdf]]
