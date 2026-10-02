---
type: problem
course: cse301
status: active
---

# Problem — M-M-1 Queue Performance Metrics Calculation

## Problem

An internet edge router receives incoming network packets according to a Poisson process at an average arrival rate of $\lambda = 800$ packets per second. The router's transmission interface processes packets with exponentially distributed transmission times at an average service rate of $\mu = 1000$ packets per second. The buffer capacity is effectively unlimited.

1. Compute the router's traffic intensity $\rho$ and the probability that the router is completely idle ($P_0$).
2. Calculate the probability that there are strictly more than $3$ packets in the router: $P(N > 3)$.
3. Compute the average number of packets in the router ($L$) and the average number of packets waiting in the buffer ($L_Q$).
4. Compute the average total time a packet spends in the router ($W$) and the average time spent waiting in the buffer ($W_Q$).
5. Calculate the probability that a packet's total residence time in the router exceeds $5$ milliseconds ($0.005$ seconds).
6. Suppose packet arrival traffic increases by $20\%$ (from $\lambda = 800$ to $\lambda = 960$ packets/sec). Compute the new average packet latency $W_{\text{new}}$ and discuss the non-linear "hockey stick" congestion effect.

---

## Given

- Model: M/M/1
- Arrival rate: $\lambda = 800$ packets/sec
- Service rate: $\mu = 1000$ packets/sec
- Capacity: $\infty$

---

## Required

1. $\rho$ and $P_0$.
2. Tail queue probability $P(N > 3) = P(N \ge 4)$.
3. $L$ and $L_Q$.
4. $W$ and $W_Q$.
5. $P(T > 0.005 \text{ s})$.
6. Sensitivity analysis under $20\%$ traffic surge.

---

## Concepts Tested

- [[M-M-1 Queue]]
- [[M-M-1 Performance Formulas]]
- [[Little's Law]]
- Exponential residence time distribution

---

## Solution

### 1. Traffic Intensity and Idle Probability
$$\rho = \frac{\lambda}{\mu} = \frac{800}{1000} = 0.80 \quad (80\% \text{ utilization})$$
Since $\rho = 0.80 < 1$, the router is **stable**.

The probability that the router is completely idle (empty) is:
$$P_0 = 1 - \rho = 1 - 0.80 = 0.20 \quad (20\%)$$

---

### 2. Probability of More Than 3 Packets in Router
$P(N > 3)$ is the probability that $N \ge 4$.
Applying the [[M-M-1 Performance Formulas|M/M/1 tail formula]]:
$$P(N \ge k) = \rho^k$$
For $k = 4$:
$$P(N > 3) = P(N \ge 4) = \rho^4 = (0.80)^4 = 0.4096 \approx 40.96\%$$

---

### 3. Average Number of Packets ($L$ and $L_Q$)
- **Total packets in router (service + buffer):**
  $$L = \frac{\rho}{1 - \rho} = \frac{0.80}{1 - 0.80} = \frac{0.80}{0.20} = \mathbf{4 \text{ packets}}$$
- **Packets waiting in buffer (queue):**
  $$L_Q = \frac{\rho^2}{1 - \rho} = \frac{(0.80)^2}{0.20} = \frac{0.64}{0.20} = \mathbf{3.2 \text{ packets}}$$
Notice that $L - L_Q = 4 - 3.2 = 0.8 = \rho$ (the average number of packets in transmission).

---

### 4. Average Residence and Wait Times ($W$ and $W_Q$)
- **Average total time in router ($W$):**
  $$W = \frac{1}{\mu - \lambda} = \frac{1}{1000 - 800} = \frac{1}{200} \text{ seconds} = 0.005 \text{ seconds} = \mathbf{5 \text{ milliseconds}}$$
- **Average time waiting in buffer ($W_Q$):**
  $$W_Q = W - \frac{1}{\mu} = \frac{1}{200} - \frac{1}{1000} = 0.005 - 0.001 = 0.004 \text{ seconds} = \mathbf{4 \text{ milliseconds}}$$
Verify with Little's Law: $L = \lambda W = 800 \times 0.005 = 4 \quad \checkmark$ and $L_Q = \lambda W_Q = 800 \times 0.004 = 3.2 \quad \checkmark$.

---

### 5. Probability That Residence Time Exceeds 5 ms
Recall that the total residence time $T$ follows an exponential distribution with parameter $\mu - \lambda = 1000 - 800 = 200$:
$$P(T > t) = e^{-(\mu - \lambda)t}$$
For $t = 0.005$ seconds:
$$P(T > 0.005) = e^{-200 \times 0.005} = e^{-1} \approx \mathbf{0.3679 \quad (36.79\%)}$$

---

### 6. Sensitivity Analysis: 20% Surge in Traffic
Now suppose $\lambda_{\text{new}} = 800 \times 1.20 = 960$ packets/second:
1. **New Utilization:**
   $$\rho_{\text{new}} = \frac{960}{1000} = 0.96 \quad (96\% \text{ utilization})$$
2. **New Latency:**
   $$W_{\text{new}} = \frac{1}{\mu - \lambda_{\text{new}}} = \frac{1}{1000 - 960} = \frac{1}{40} \text{ seconds} = 0.025 \text{ seconds} = \mathbf{25 \text{ milliseconds}}$$
3. **New Buffer Inventory:**
   $$L_{\text{new}} = \frac{0.96}{1 - 0.96} = \frac{0.96}{0.04} = \mathbf{24 \text{ packets}}$$

### Comparison Analysis
- Traffic increase: $\frac{960 - 800}{800} = \mathbf{+20\%}$
- Latency increase: $\frac{25 \text{ ms} - 5 \text{ ms}}{5 \text{ ms}} = \mathbf{+400\% \quad (5\times \text{ increase!})}$
- Queue length increase: $\frac{24 - 4}{4} = \mathbf{+500\% \quad (6\times \text{ increase!})}$

**Engineering Conclusion:**
Because queueing delay is hyperbolic in $(1 - \rho)^{-1}$, a modest $20\%$ increase in traffic pushes the system from an $80\%$ load to a $96\%$ load, causing packet delay to quintuple from $5$ ms to $25$ ms. This non-linear explosion is why queueing analysis is indispensable for network capacity planning.

---

## Related Concepts

- [[M-M-1 Queue]]
- [[M-M-1 Performance Formulas]]
- [[Little's Law]]

---

## Source

- [[01 - Sources/Lectures/CSE301_Queueing_Theory.pdf]]
