---
type: example
course: cse301
status: active
---

# Tandem Two-Server Queue Performance Example

## Problem

An e-commerce order processing pipeline consists of two sequential processing stages in series:
- **Stage 1 (Order Validation):** Staffed by Server 1, which processes orders exponentially at mean rate $\mu_1 = 12$ orders per minute.
- **Stage 2 (Payment & Inventory Allocation):** Staffed by Server 2, which processes orders exponentially at mean rate $\mu_2 = 10$ orders per minute.
- Orders arrive from outside into Stage 1 according to a Poisson process with rate $\lambda = 8$ orders per minute.
- Upon completing Stage 1, an order immediately enters the queue for Stage 2. Both stages have unlimited waiting room buffers.

1. Determine whether both queues in the pipeline are stable. Compute the traffic intensity at each stage.
2. Calculate the joint probability that there are exactly $2$ orders at Stage 1 and $1$ order at Stage 2 in steady state: $P(N_1 = 2, N_2 = 1)$.
3. Compute the average number of orders in Stage 1 ($L_1$), Stage 2 ($L_2$), and in the entire pipeline ($L$).
4. Compute the average time an order spends in Stage 1 ($W_1$), Stage 2 ($W_2$), and the total end-to-end pipeline latency ($W$).
5. Verify that Little's Law holds for the entire network.

---

## Given

- Pipeline structure: Tandem queue ($Q_1 \to Q_2$)
- External arrival rate: $\lambda = 8$ orders/min
- Server 1 processing rate: $\mu_1 = 12$ orders/min
- Server 2 processing rate: $\mu_2 = 10$ orders/min

---

## Required

1. $\rho_1, \rho_2$ and stability assessment.
2. Joint probability $P(N_1 = 2, N_2 = 1)$.
3. Pipeline inventory metrics: $L_1, L_2, L$.
4. Pipeline latency metrics: $W_1, W_2, W$.
5. Network Little's Law verification ($L = \lambda W$).

---

## Concepts Used

- [[Jackson Networks and Tandem Queues]]
- [[M-M-1 Queue]]
- [[M-M-1 Performance Formulas]]
- [[Little's Law]]
- Burke's Theorem

---

## Solution

### Step 1: Stability and Traffic Intensities
By Burke's Theorem, the departure process from Stage 1 is a Poisson process with rate $\lambda = 8$. Therefore, Stage 2 receives a Poisson arrival stream with rate $\lambda_2 = \lambda = 8$ orders/min.

1. **Stage 1 Utilization:**
   $$\rho_1 = \frac{\lambda}{\mu_1} = \frac{8}{12} = \frac{2}{3} \approx 0.6667 \quad (66.67\%)$$
   Since $\rho_1 = 2/3 < 1$, Stage 1 is **stable**.
2. **Stage 2 Utilization:**
   $$\rho_2 = \frac{\lambda}{\mu_2} = \frac{8}{10} = \frac{4}{5} = 0.8000 \quad (80.00\%)$$
   Since $\rho_2 = 0.80 < 1$, Stage 2 is **stable**.

Because both $\rho_1 < 1$ and $\rho_2 < 1$, the entire tandem network is **stable**.

---

### Step 2: Joint Probability $P(N_1 = 2, N_2 = 1)$
By [[Jackson Networks and Tandem Queues|Jackson's Theorem]], the joint steady-state distribution factors into independent M/M/1 marginal distributions:
$$P(n_1, n_2) = P_1(n_1) \cdot P_2(n_2) = (1 - \rho_1)\rho_1^{n_1} \cdot (1 - \rho_2)\rho_2^{n_2}$$

For $n_1 = 2$ and $n_2 = 1$:
$$P_1(2) = \left(1 - \frac{2}{3}\right)\left(\frac{2}{3}\right)^2 = \frac{1}{3} \times \frac{4}{9} = \frac{4}{27} \approx 0.1481$$
$$P_2(1) = (1 - 0.80)(0.80)^1 = 0.20 \times 0.80 = 0.1600$$

Multiplying:
$$P(N_1 = 2, N_2 = 1) = \frac{4}{27} \times 0.16 = \frac{0.64}{27} \approx 0.0237 \quad (2.37\%)$$

---

### Step 3: Average Inventory in Pipeline ($L_1, L_2, L$)
Using the M/M/1 formula $L = \frac{\rho}{1 - \rho}$:

1. **Stage 1 Average Orders:**
   $$L_1 = \frac{\rho_1}{1 - \rho_1} = \frac{2/3}{1 - 2/3} = \frac{2/3}{1/3} = 2 \text{ orders}$$
2. **Stage 2 Average Orders:**
   $$L_2 = \frac{\rho_2}{1 - \rho_2} = \frac{0.80}{1 - 0.80} = \frac{0.80}{0.20} = 4 \text{ orders}$$
3. **Total Orders in Pipeline:**
   $$L = L_1 + L_2 = 2 + 4 = \mathbf{6 \text{ orders}}$$

---

### Step 4: Average Latency in Pipeline ($W_1, W_2, W$)
Using the M/M/1 formula $W = \frac{1}{\mu - \lambda}$:

1. **Stage 1 Latency:**
   $$W_1 = \frac{1}{\mu_1 - \lambda} = \frac{1}{12 - 8} = \frac{1}{4} \text{ min} = 0.25 \text{ min} = 15 \text{ seconds}$$
2. **Stage 2 Latency:**
   $$W_2 = \frac{1}{\mu_2 - \lambda} = \frac{1}{10 - 8} = \frac{1}{2} \text{ min} = 0.50 \text{ min} = 30 \text{ seconds}$$
3. **Total End-to-End Pipeline Latency:**
   $$W = W_1 + W_2 = 0.25 + 0.50 = \mathbf{0.75 \text{ min} = 45 \text{ seconds}}$$

---

### Step 5: Verification of Network Little's Law
We verify whether $L = \lambda W$ across the entire pipeline:
$$\lambda \times W = 8 \text{ orders/min} \times 0.75 \text{ min} = 6.0 \text{ orders}$$
This matches our derived inventory $L = 6$ exactly:
$$\mathbf{L = \lambda W \quad \checkmark}$$

---

## Result Summary Table

| Metric | Stage 1 (Validation) | Stage 2 (Payment) | Total Pipeline |
|---|---|---|---|
| Arrival Rate ($\lambda$) | $8$ orders/min | $8$ orders/min | $8$ orders/min |
| Service Rate ($\mu$) | $12$ orders/min | $10$ orders/min | — |
| Utilization ($\rho$) | $66.67\%$ | $80.00\%$ | Bottleneck: Stage 2 |
| Average In Queue ($L_Q$) | $1.33$ orders | $3.20$ orders | $4.53$ orders |
| Average Total ($L$) | $2.00$ orders | $4.00$ orders | **$6.00$ orders** |
| Wait in Queue ($W_Q$) | $0.167$ min ($10$ s) | $0.400$ min ($24$ s) | $0.567$ min ($34$ s) |
| Total Residence ($W$) | $0.250$ min ($15$ s) | $0.500$ min ($30$ s) | **$0.750$ min ($45$ s)** |

---

## Related Concepts

- [[Jackson Networks and Tandem Queues]]
- [[M-M-1 Queue]]
- [[M-M-1 Performance Formulas]]
- [[Little's Law]]

---

## Sources

- [[01 - Sources/Lectures/CSE301_Queueing_Theory.pdf]]
