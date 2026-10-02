---
type: formula
course: cse301
status: active
---

# Little's Law

## Formula

**Little's Law** is the most celebrated and fundamental theorem in queueing theory and operations research. It establishes an exact, invariant relationship between inventory (concurrency), throughput (arrival rate), and delay (residence time):

$$\mathbf{L = \lambda_a W}$$

where:
- $L$: The long-run average number of customers in the system.
- $\lambda_a$: The average arrival rate of customers who actually enter the system.
- $W$: The average time a customer spends in the system.

### Subsidiary Forms of Little's Law

1. **Queue Length and Waiting Time:**
   $$L_Q = \lambda_a W_Q$$
   where $L_Q$ is the average number waiting in the queue, and $W_Q$ is the average wait time in queue before service begins.
2. **Server Utilization:**
   $$L_{\text{server}} = \rho = \lambda_a E[S] = \frac{\lambda_a}{\mu}$$
   where $E[S] = 1/\mu$ is the mean service time.
3. **Decomposition:**
   $$W = W_Q + \frac{1}{\mu} \iff L = L_Q + \frac{\lambda_a}{\mu}$$

---

## Variables

| Symbol | Meaning | Dimensions |
|---|---|---|
| $L$ | Time-average customer count in system | Customers / Items |
| $L_Q$ | Time-average customer count waiting in line | Customers / Items |
| $\lambda_a$ | Long-run effective arrival rate | Customers / Time |
| $W$ | Expected residence time in system | Time |
| $W_Q$ | Expected waiting time in queue | Time |
| $\mu$ | Server processing rate | Customers / Time |
| $\rho$ | Server utilization | Dimensionless $\in [0, 1)$ |

---

## Universality: Why Little's Law Is Extraordinary

Little's Law requires **almost no restrictive assumptions**:
- It does **not** assume Poisson arrivals (works for ANY arrival distribution).
- It does **not** assume exponential service times (works for ANY service distribution).
- It does **not** assume a single server (works for 1 server, 100 servers, or infinite servers).
- It does **not** assume FIFO queue discipline (holds for LIFO, Priority, Random, Processor Sharing).
- It holds for a single queue, a subcomponent of a queue, a network of 1,000 servers, a software pipeline, or an entire manufacturing warehouse.

The only requirements are:
1. The system must reach a stationary stochastic steady state.
2. Customers must eventually depart (no customers trapped forever).
3. The limits defining long-run averages must exist.

---

## Derivation via the Fundamental Cost Identity

Little's Law can be derived with mathematical elegance using the **Fundamental Cost Identity** introduced by Sheldon Ross:

Imagine that entering customers are forced to pay money to the system operator according to some predefined accounting cost rule:
- Let $g(W_i)$ be the total fee paid by customer $i$ who spends residence time $W_i$ in the system.
- Let $G = E[g(W)] = \lim_{A \to \infty} \frac{1}{A}\sum_{i=1}^A g(W_i)$ be the average payment per entering customer.
- Let $r(t)$ be the instantaneous rate at which the system operator earns money at continuous time $t$.
- Let $R = \lim_{t \to \infty} \frac{1}{t}\int_0^t r(s) ds$ be the long-run time-average revenue rate.

### The Fundamental Cost Identity
$$\mathbf{R = \lambda_a G}$$
In words:
$$\text{Long-run Revenue Rate} = (\text{Arrival Rate}) \times (\text{Average Payment per Customer})$$

---

### Special Case 1: Deriving $L = \lambda_a W$
Choose the cost rule: **"Every customer pays \$1 per unit of time while present in the system."**
1. **Customer's Total Payment:**
   A customer who spends $W_i$ time units in the system pays:
   $$g(W_i) = 1 \times W_i = W_i \implies G = E[W] = W$$
2. **System Earning Rate at Time $t$:**
   Since every customer currently in the facility pays \$1 per unit time, if there are $X(t)$ customers present at time $t$, the system earns money at rate:
   $$r(t) = X(t) \cdot \$1 = X(t)$$
   The long-run time-average earning rate is:
   $$R = \lim_{t \to \infty} \frac{1}{t}\int_0^t X(s) ds = L$$
3. **Equating via the Cost Identity:**
   $$R = \lambda_a G \implies \mathbf{L = \lambda_a W} \quad \blacksquare$$

---

### Special Case 2: Deriving $L_Q = \lambda_a W_Q$
Choose the cost rule: **"Every customer pays \$1 per unit of time while waiting in the queue (stopping payment when service begins)."**
- $g(W_i) = W_{Q, i} \implies G = W_Q$
- $r(t) = X_Q(t) \implies R = L_Q$
- Substituting into $R = \lambda_a G$:
  $$\mathbf{L_Q = \lambda_a W_Q} \quad \blacksquare$$

---

### Special Case 3: Deriving Server Utilization $\rho = \frac{\lambda_a}{\mu}$
Choose the cost rule: **"Every customer pays \$1 per unit of time while in service."**
- $g(W_i) = S_i$ (service time) $\implies G = E[S] = \frac{1}{\mu}$
- $r(t) = 1$ if the server is busy, and $0$ if idle $\implies R = P(\text{server is busy}) = \rho$
- Substituting into $R = \lambda_a G$:
  $$\rho = \lambda_a \left(\frac{1}{\mu}\right) = \mathbf{\frac{\lambda_a}{\mu}} \quad \blacksquare$$

---

## Example: Fast-Food Drive-Through

A drive-through lane observes that cars arrive at an average rate of $\lambda = 2$ cars per minute.
On average, a car spends $W = 3$ minutes from entering the driveway until leaving with their food.

By Little's Law:
$$L = \lambda W = 2 \text{ cars/min} \times 3 \text{ min} = 6 \text{ cars}$$
At any random instant, an overhead drone will count an average of **6 cars** in the drive-through lane.

---

## Common Mistakes

- **Unit mismatch:** Mixing hours and minutes (e.g., $\lambda$ in customers/hour and $W$ in minutes). Always convert to identical time units!
- **Gross vs. Effective Arrivals:** Using gross arrival rate $\lambda$ instead of effective arrival rate $\lambda_a = \lambda(1 - P_{\text{blocked}})$ in finite capacity loss systems.

---

## Related Concepts

- [[Queueing Systems and Kendall Notation]]
- [[M-M-1 Queue]]
- [[Finite Capacity M-M-1-N Queue]]
- [[M-M-1 Performance Formulas]]

---

## Sources

- [[01 - Sources/Lectures/CSE301_Queueing_Theory.pdf]]
