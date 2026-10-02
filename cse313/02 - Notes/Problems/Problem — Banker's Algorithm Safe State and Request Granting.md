---
type: problem
course: cse313
status: active
order: 33
---

# Problem — Banker's Algorithm Safe State and Request Granting

> 📖 **Reading Order:** Step 33 of 34 | **Module 5:** Deadlocks  
> ◄ **Previous:** [[Resource Allocation Graph Cycle Detection Example]] | ► **Next:** [[Problem — Resource Allocation Graph Reduction and Cycle Detection]]

---

## Problem Statement

Consider a system with **5 processes** ($P_0, P_1, P_2, P_3, P_4$) and **4 resource types** ($A, B, C, D$).  
The total resource vector in the system is:
$$E = (A: 7, \; B: 6, \; C: 8, \; D: 5)$$

At time $t_0$, the resource allocation state is as follows:

| Process | Current Allocation ($CA$) <br/> $A \quad B \quad C \quad D$ | Maximum Requirement ($MaxReq$) <br/> $A \quad B \quad C \quad D$ |
|---|:---:|:---:|
| **$P_0$** | $0 \quad 0 \quad 1 \quad 2$ | $0 \quad 0 \quad 1 \quad 2$ |
| **$P_1$** | $1 \quad 1 \quad 0 \quad 0$ | $2 \quad 3 \quad 1 \quad 2$ |
| **$P_2$** | $1 \quad 3 \quad 5 \quad 1$ | $1 \quad 3 \quad 5 \quad 2$ |
| **$P_3$** | $0 \quad 1 \quad 1 \quad 0$ | $2 \quad 1 \quad 1 \quad 4$ |
| **$P_4$** | $3 \quad 0 \quad 1 \quad 1$ | $4 \quad 3 \quad 3 \quad 2$ |

### Tasks:
1. Calculate the **Available Resource Vector** ($A$) and the **Need Matrix** ($R = MaxReq - CA$).
2. Apply Dijkstra's Safety Algorithm to verify if the current state is **Safe**. If safe, provide a valid **Safe Sequence**.
3. **Scenario 1:** At time $t_1$, process $P_1$ issues a request:
   $$Request_1 = (1, 1, 0, 0)$$
   Can this request be granted immediately? Demonstrate the complete speculative safety verification.
4. **Scenario 2:** Suppose instead that from the initial state, process $P_4$ issues a request:
   $$Request_4 = (1, 2, 0, 1)$$
   Can this request be granted immediately? Explain step-by-step why or why not.

---

## Detailed Step-by-Step Solutions

### Part 1: Available Vector and Need Matrix

#### 1. Calculate Allocated Sum:
$$\begin{aligned}
\sum CA_A &= 0 + 1 + 1 + 0 + 3 = 5 \\
\sum CA_B &= 0 + 1 + 3 + 1 + 0 = 5 \\
\sum CA_C &= 1 + 0 + 5 + 1 + 1 = 8 \\
\sum CA_D &= 2 + 0 + 1 + 0 + 1 = 4
\end{aligned}$$
$$\text{Total Allocated} = (5, 5, 8, 4)$$

#### 2. Calculate Available Vector ($A = E - \sum CA$):
$$A = (7 - 5, \; 6 - 5, \; 8 - 8, \; 5 - 4) = \mathbf{(2, 1, 0, 1)}$$

#### 3. Calculate Need Matrix ($R = MaxReq - CA$):
| Process | $MaxReq$ | $CA$ | $\text{Need } R = MaxReq - CA$ |
|---|:---:|:---:|:---:|
| **$P_0$** | $(0, 0, 1, 2)$ | $(0, 0, 1, 2)$ | $(0, 0, 0, 0)$ |
| **$P_1$** | $(2, 3, 1, 2)$ | $(1, 1, 0, 0)$ | $(1, 2, 1, 2)$ |
| **$P_2$** | $(1, 3, 5, 2)$ | $(1, 3, 5, 1)$ | $(0, 0, 0, 1)$ |
| **$P_3$** | $(2, 1, 1, 4)$ | $(0, 1, 1, 0)$ | $(2, 0, 0, 4)$ |
| **$P_4$** | $(4, 3, 3, 2)$ | $(3, 0, 1, 1)$ | $(1, 3, 2, 1)$ |

---

### Part 2: Initial Safety Check

Initialize $Work = A = (2, 1, 0, 1)$ and $Finish = [F, F, F, F, F]$.

- **Step 1:** Compare Needs with $Work = (2, 1, 0, 1)$:
  - $P_0$: Need $(0, 0, 0, 0) \le (2, 1, 0, 1)$ $\implies$ **True**!
  - $P_0$ completes:
    $$Work = (2, 1, 0, 1) + (0, 0, 1, 2) = \mathbf{(2, 1, 1, 3)}$$
    $Finish[P_0] = \text{TRUE}$. Safe: $\langle P_0 \rangle$.

- **Step 2:** Compare remaining with $Work = (2, 1, 1, 3)$:
  - $P_2$: Need $(0, 0, 0, 1) \le (2, 1, 1, 3)$ $\implies$ **True**! ($0 \le 2, 0 \le 1, 0 \le 1, 1 \le 3$).
  - $P_2$ completes:
    $$Work = (2, 1, 1, 3) + (1, 3, 5, 1) = \mathbf{(3, 4, 6, 4)}$$
    $Finish[P_2] = \text{TRUE}$. Safe: $\langle P_0, P_2 \rangle$.

- **Step 3:** Compare remaining $\{P_1, P_3, P_4\}$ with $Work = (3, 4, 6, 4)$:
  - $P_1$: Need $(1, 2, 1, 2) \le (3, 4, 6, 4)$ $\implies$ **True**!
  - $P_1$ completes:
    $$Work = (3, 4, 6, 4) + (1, 1, 0, 0) = \mathbf{(4, 5, 6, 4)}$$
    $Finish[P_1] = \text{TRUE}$. Safe: $\langle P_0, P_2, P_1 \rangle$.

- **Step 4:** Compare remaining $\{P_3, P_4\}$ with $Work = (4, 5, 6, 4)$:
  - $P_3$: Need $(2, 0, 0, 4) \le (4, 5, 6, 4)$ $\implies$ **True**!
  - $P_3$ completes:
    $$Work = (4, 5, 6, 4) + (0, 1, 1, 0) = \mathbf{(4, 6, 7, 4)}$$
    $Finish[P_3] = \text{TRUE}$. Safe: $\langle P_0, P_2, P_1, P_3 \rangle$.

- **Step 5:** Only $P_4$ remains with $Work = (4, 6, 7, 4)$:
  - $P_4$: Need $(1, 3, 2, 1) \le (4, 6, 7, 4)$ $\implies$ **True**!
  - $P_4$ completes:
    $$Work = (4, 6, 7, 4) + (3, 0, 1, 1) = \mathbf{(7, 6, 8, 5)} = E$$
    $Finish[P_4] = \text{TRUE}$.

**Conclusion:** The initial state is **SAFE** with Safe Sequence:
$$\mathbf{\langle P_0, P_2, P_1, P_3, P_4 \rangle}$$

---

### Part 3: Scenario 1 Evaluation ($Request_1 = (1, 1, 0, 0)$)

1. **Check Condition 1:** $Request_1 \le Need_1$?
   $(1, 1, 0, 0) \le (1, 2, 1, 2) \implies$ **True**.
2. **Check Condition 2:** $Request_1 \le Available$?
   $(1, 1, 0, 0) \le (2, 1, 0, 1) \implies$ **True**.
3. **Speculative Modification:**
   $$\begin{aligned}
   A' &= (2, 1, 0, 1) - (1, 1, 0, 0) = \mathbf{(1, 0, 0, 1)} \\
   CA'_1 &= (1, 1, 0, 0) + (1, 1, 0, 0) = \mathbf{(2, 2, 0, 0)} \\
   R'_1 &= (1, 2, 1, 2) - (1, 1, 0, 0) = \mathbf{(0, 1, 1, 2)}
   \end{aligned}$$
4. **Safety Check on Speculative State ($Work = (1, 0, 0, 1)$):**
   - $P_0$: Need $(0, 0, 0, 0) \le (1, 0, 0, 1)$ $\implies$ **True**!
     $Work = (1, 0, 0, 1) + (0, 0, 1, 2) = \mathbf{(1, 0, 1, 3)}$.
   - $P_2$: Need $(0, 0, 0, 1) \le (1, 0, 1, 3)$ $\implies$ **True**!
     $Work = (1, 0, 1, 3) + (1, 3, 5, 1) = \mathbf{(2, 3, 6, 4)}$.
   - $P_1$: Need $(0, 1, 1, 2) \le (2, 3, 6, 4)$ $\implies$ **True**!
     $Work = (2, 3, 6, 4) + (2, 2, 0, 0) = \mathbf{(4, 5, 6, 4)}$.
   - $P_3$: Need $(2, 0, 0, 4) \le (4, 5, 6, 4)$ $\implies$ **True**!
     $Work = (4, 5, 6, 4) + (0, 1, 1, 0) = \mathbf{(4, 6, 7, 4)}$.
   - $P_4$: Need $(1, 3, 2, 1) \le (4, 6, 7, 4)$ $\implies$ **True**!
     $Work = (4, 6, 7, 4) + (3, 0, 1, 1) = \mathbf{(7, 6, 8, 5)}$.

**Result:** A valid safe sequence $\langle P_0, P_2, P_1, P_3, P_4 \rangle$ exists.  
**Decision: GRANT THE REQUEST IMMEDIATELY.**

---

### Part 4: Scenario 2 Evaluation ($Request_4 = (1, 2, 0, 1)$)

1. **Check Condition 1:** $Request_4 \le Need_4$?
   $(1, 2, 0, 1) \le (1, 3, 2, 1) \implies$ **True**.
2. **Check Condition 2:** $Request_4 \le Available$?
   $Request_4 = (1, 2, 0, 1)$ vs $Available = (2, 1, 0, 1)$.
   - For Resource $B$: $Request_{4, B} = 2$, but $Available_B = 1$.
   - Since $2 > 1$, **$Request_4 \not\le Available$**!

**Decision: REJECT / SUSPEND $P_4$.**  
The request **cannot be granted immediately** because the system does not possess enough free instances of Resource $B$. Process $P_4$ is placed in a waiting queue.

---

## Source Traceability & Metadata
- **Source Material:** `5. Deadlocks-week6-7-RRR.pdf` (Slides 28–31: Banker's Algorithm) and `Notes on algorithm simulation.pdf`.
- **Question ID:** `Q-CSE313-004`
- **Related Notes:**
  - Concept: [[Deadlock Fundamentals and Coffman Conditions]], [[Deadlock Prevention and Avoidance Strategies]]
  - Algorithm: [[Banker's Algorithm]]
  - Example: [[Banker's Algorithm Multi-Resource Step-by-Step Example]]
