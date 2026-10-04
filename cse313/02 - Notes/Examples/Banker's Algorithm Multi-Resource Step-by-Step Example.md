---
type: example
course: cse313
status: active
order: 31
---

# Banker's Algorithm Multi-Resource Step-by-Step Example

> 📖 **Reading Order:** Step 31 of 34 | **Module 5:** Deadlocks  
> ◄ **Previous:** [[Deadlock Detection and Recovery Algorithms]] | ► **Next:** [[Resource Allocation Graph Cycle Detection Example]]

---

---

## Problem

*(Directly derived from course simulation lecture notes: `Notes on algorithm simulation.pdf`)*

Consider a system with **4 processes** ($P_1, P_2, P_3, P_4$) and **3 resource types**:
- **Resource $A$:** 9 total units
- **Resource $B$:** 3 total units
- **Resource $C$:** 6 total units
$$E = (9, 3, 6)$$

### Current Allocation Matrix ($CA$) & Maximum Requirement Matrix ($MaxReq$):

| Process | Current Allocation ($CA$) <br/> $A \quad B \quad C$ | Maximum Requirements ($MaxReq$) <br/> $A \quad B \quad C$ |
|---|:---:|:---:|
| **$P_1$** | $1 \quad 0 \quad 0$ | $3 \quad 2 \quad 2$ |
| **$P_2$** | $6 \quad 1 \quad 2$ | $6 \quad 1 \quad 3$ |
| **$P_3$** | $2 \quad 1 \quad 1$ | $3 \quad 1 \quad 4$ |
| **$P_4$** | $0 \quad 0 \quad 2$ | $4 \quad 2 \quad 2$ |
| **Total Allocated** | $\mathbf{9 \quad 2 \quad 5}$ | — |

**Question:** Determine whether the current state of this system is **Safe** or **Unsafe**. If safe, find a valid safe sequence.

---

---

## Given

- System state matrices, resource vectors, and process workload parameters as specified in problem setup.

---

## Required

- Determine step-by-step state transitions, verify system invariants, and calculate resulting performance metrics.

---

## Understanding the Problem and Choosing the Method

Analyze initial conditions, verify prerequisite invariants, track state changes iteratively, and check final consistency against theoretical rules.

---

## Solution

### 2. Step 1: Compute Available Vector ($A$) and Need Matrix ($R$)

### 1. Available Vector ($A$):
The available resources are calculated by subtracting the total allocated resources from the total system capacity:
$$A = E - \sum_{i=1}^4 CA_i = (9, 3, 6) - (9, 2, 5) = \mathbf{(0, 1, 1)}$$

### 2. Request / Need Matrix ($R = MaxReq - CA$):
$$R_{i, j} = MaxReq_{i, j} - CA_{i, j}$$

$$\begin{aligned}
R(P_1) &= (3 - 1, 2 - 0, 2 - 0) = \mathbf{(2, 2, 2)} \\
R(P_2) &= (6 - 6, 1 - 1, 3 - 2) = \mathbf{(0, 0, 1)} \\
R(P_3) &= (3 - 2, 1 - 1, 4 - 1) = \mathbf{(1, 0, 3)} \\
R(P_4) &= (4 - 0, 2 - 0, 2 - 2) = \mathbf{(4, 2, 0)}
\end{aligned}$$

Summary Table of Remaining Needs:
$$R = \begin{pmatrix} 2 & 2 & 2 \\ 0 & 0 & 1 \\ 1 & 0 & 3 \\ 4 & 2 & 0 \end{pmatrix}$$

---

---

### 3. Step 2: Safety Simulation Algorithm

We initialize $Work = A = (0, 1, 1)$ and $Finish = [F, F, F, F]$.

### Iteration 1:
Compare the remaining needs with $Work = (0, 1, 1)$:
- For $P_1$: Need $(2, 2, 2) \le (0, 1, 1)$? $\implies$ **False** ($2 > 0$ for resource A).
- For $P_2$: Need $(0, 0, 1) \le (0, 1, 1)$? $\implies$ **True** ($0 \le 0, 0 \le 1, 1 \le 1$).
- For $P_3$: Need $(1, 0, 3) \le (0, 1, 1)$? $\implies$ **False** ($1 > 0$).
- For $P_4$: Need $(4, 2, 0) \le (0, 1, 1)$? $\implies$ **False** ($4 > 0$).

> Only process **$P_2$** can be served first!

- **Process $P_2$ runs to completion** and releases all its allocated resources back to the pool:
$$Work_{\text{new}} = Work_{\text{old}} + CA(P_2) = (0, 1, 1) + (6, 1, 2) = \mathbf{(6, 2, 3)}$$
- Set $Finish[P_2] = \text{TRUE}$.
- **Safe Sequence so far:** $\langle P_2 \rangle$.

---

### Iteration 2:
Available pool is now $Work = (6, 2, 3)$. Evaluate remaining processes $\{P_1, P_3, P_4\}$:
- For $P_1$: Need $(2, 2, 2) \le (6, 2, 3)$? $\implies$ **True** ($2 \le 6, 2 \le 2, 2 \le 3$).
- For $P_3$: Need $(1, 0, 3) \le (6, 2, 3)$? $\implies$ **True** ($1 \le 6, 0 \le 2, 3 \le 3$).
- For $P_4$: Need $(4, 2, 0) \le (6, 2, 3)$? $\implies$ **True** ($4 \le 6, 2 \le 2, 0 \le 3$).

> *Course Note:* When multiple candidate processes satisfy the condition, any candidate may be legally selected. Following the instructor's trace, we select **$P_1$**.

- **Process $P_1$ runs to completion** and releases its allocated resources:
$$Work_{\text{new}} = Work_{\text{old}} + CA(P_1) = (6, 2, 3) + (1, 0, 0) = \mathbf{(7, 2, 3)}$$
- Set $Finish[P_1] = \text{TRUE}$.
- **Safe Sequence so far:** $\langle P_2, P_1 \rangle$.

---

### Iteration 3:
Available pool is now $Work = (7, 2, 3)$. Evaluate remaining processes $\{P_3, P_4\}$:
- For $P_3$: Need $(1, 0, 3) \le (7, 2, 3)$? $\implies$ **True** ($1 \le 7, 0 \le 2, 3 \le 3$).
- For $P_4$: Need $(4, 2, 0) \le (7, 2, 3)$? $\implies$ **True** ($4 \le 7, 2 \le 2, 0 \le 3$).

We select **$P_3$**:
- **Process $P_3$ runs to completion** and releases its allocated resources:
$$Work_{\text{new}} = Work_{\text{old}} + CA(P_3) = (7, 2, 3) + (2, 1, 1) = \mathbf{(9, 3, 4)}$$
- Set $Finish[P_3] = \text{TRUE}$.
- **Safe Sequence so far:** $\langle P_2, P_1, P_3 \rangle$.

---

### Iteration 4:
Available pool is now $Work = (9, 3, 4)$. Only $P_4$ remains:
- For $P_4$: Need $(4, 2, 0) \le (9, 3, 4)$? $\implies$ **True** ($4 \le 9, 2 \le 3, 0 \le 4$).
- **Process $P_4$ runs to completion** and releases its allocated resources:
$$Work_{\text{new}} = Work_{\text{old}} + CA(P_4) = (9, 3, 4) + (0, 0, 2) = \mathbf{(9, 3, 6)}$$
- Notice that final $Work$ equals the total system capacity vector $E = (9, 3, 6)$!
- Set $Finish[P_4] = \text{TRUE}$.

---

---

## Result

Since $Finish[i] = \text{TRUE}$ for all $i \in \{1, 2, 3, 4\}$, the system is in a **SAFE STATE**.

### Primary Safe Sequence:
$$\mathbf{\langle P_2 \to P_1 \to P_3 \to P_4 \rangle}$$

*(Alternative valid safe sequences include $\langle P_2, P_3, P_1, P_4 \rangle$ and $\langle P_2, P_4, P_1, P_3 \rangle$.)*

---

---

## Why This Works

Each state transformation follows the operational semantics of kernel execution, ensuring mutual exclusion, safe scheduling, or deadlock freedom.

---

## Common Mistakes

- Overlooking state changes between execution phases.
- Incorrectly calculating intermediate residual capacities or queue offsets.

---

## General Method

Extract the generic algorithmic pattern: initialize tracking vectors, simulate execution step by step, verify invariant conditions, and calculate final summary metrics.

---

## Related Concepts

- [[Operating System Structures and Functions]]
- [[Process Concepts and Memory Layout]]

---

## Sources

- Andrew S. Tanenbaum & Herbert Bos, *Modern Operating Systems* (4th Edition)
- Silberschatz et al., *Operating System Concepts* (10th Edition)
