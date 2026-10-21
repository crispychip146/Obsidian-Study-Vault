---
type: example
course: cse309
status: active
order: 20
---

# SLR(1) Automaton and Parsing Table Construction Example

## Starting Point and the Problem

To master bottom-up parsing, compiler engineers must be capable of constructing an LR automaton by hand, generating the corresponding SLR(1) action and goto tables, and tracing the execution of the state stack.

We execute a complete end-to-end walkthrough on the classic augmented arithmetic expression grammar:
$$\begin{aligned}
(0)\; E' &\to E \\
(1)\; E &\to E + T \\
(2)\; E &\to T \\
(3)\; T &\to T * F \\
(4)\; T &\to F \\
(5)\; F &\to (E) \\
(6)\; F &\to \mathbf{id}
\end{aligned}$$

---

## Step 1: Construction of the Canonical Collection of LR(0) Item Sets

We compute the 12 canonical states ($I_0$ through $I_{11}$) using $\text{CLOSURE}$ and $\text{GOTO}$:

### State $I_0 = \text{CLOSURE}(\{[E' \to \cdot E]\})$
- $E' \to \cdot E$
- $E \to \cdot E + T$
- $E \to \cdot T$
- $T \to \cdot T * F$
- $T \to \cdot F$
- $F \to \cdot (E)$
- $F \to \cdot \mathbf{id}$

### Transitions from $I_0$:
- $\text{GOTO}(I_0, E) = I_1$
- $\text{GOTO}(I_0, T) = I_2$
- $\text{GOTO}(I_0, F) = I_3$
- $\text{GOTO}(I_0, () = I_4$
- $\text{GOTO}(I_0, \mathbf{id}) = I_5$

---

### Subsequent Item Sets ($I_1$ to $I_{11}$)

- **State $I_1 = \text{GOTO}(I_0, E)$:**
  - $E' \to E \cdot$ *(Accept candidate)*
  - $E \to E \cdot + T$
  - Transitions: $\text{GOTO}(I_1, +) = I_6$

- **State $I_2 = \text{GOTO}(I_0, T)$:**
  - $E \to T \cdot$ *(Reduce by rule 2)*
  - $T \to T \cdot * F$
  - Transitions: $\text{GOTO}(I_2, *) = I_7$

- **State $I_3 = \text{GOTO}(I_0, F)$:**
  - $T \to F \cdot$ *(Reduce by rule 4)*

- **State $I_4 = \text{GOTO}(I_0, () = \text{CLOSURE}(\{[F \to ( \cdot E)]\}):$**
  - $F \to ( \cdot E)$
  - $E \to \cdot E + T$
  - $E \to \cdot T$
  - $T \to \cdot T * F$
  - $T \to \cdot F$
  - $F \to \cdot (E)$
  - $F \to \cdot \mathbf{id}$
  - Transitions: $\text{GOTO}(I_4, E) = I_8$, $\text{GOTO}(I_4, T) = I_2$, $\text{GOTO}(I_4, F) = I_3$, $\text{GOTO}(I_4, () = I_4$, $\text{GOTO}(I_4, \mathbf{id}) = I_5$

- **State $I_5 = \text{GOTO}(I_0, \mathbf{id})$:**
  - $F \to \mathbf{id} \cdot$ *(Reduce by rule 6)*

- **State $I_6 = \text{GOTO}(I_1, +) = \text{CLOSURE}(\{[E \to E + \cdot T]\}):$**
  - $E \to E + \cdot T$
  - $T \to \cdot T * F$
  - $T \to \cdot F$
  - $F \to \cdot (E)$
  - $F \to \cdot \mathbf{id}$
  - Transitions: $\text{GOTO}(I_6, T) = I_9$, $\text{GOTO}(I_6, F) = I_3$, $\text{GOTO}(I_6, () = I_4$, $\text{GOTO}(I_6, \mathbf{id}) = I_5$

- **State $I_7 = \text{GOTO}(I_2, *) = \text{CLOSURE}(\{[T \to T * \cdot F]\}):$**
  - $T \to T * \cdot F$
  - $F \to \cdot (E)$
  - $F \to \cdot \mathbf{id}$
  - Transitions: $\text{GOTO}(I_7, F) = I_{10}$, $\text{GOTO}(I_7, () = I_4$, $\text{GOTO}(I_7, \mathbf{id}) = I_5$

- **State $I_8 = \text{GOTO}(I_4, E)$:**
  - $F \to (E \cdot )$
  - $E \to E \cdot + T$
  - Transitions: $\text{GOTO}(I_8, )) = I_{11}$, $\text{GOTO}(I_8, +) = I_6$

- **State $I_9 = \text{GOTO}(I_6, T)$:**
  - $E \to E + T \cdot$ *(Reduce by rule 1)*
  - $T \to T \cdot * F$
  - Transitions: $\text{GOTO}(I_9, *) = I_7$

- **State $I_{10} = \text{GOTO}(I_7, F)$:**
  - $T \to T * F \cdot$ *(Reduce by rule 3)*

- **State $I_{11} = \text{GOTO}(I_8, ))$:**
  - $F \to (E) \cdot$ *(Reduce by rule 5)*

---

## Step 2: $\text{FOLLOW}$ Sets

As computed previously:
- $\text{FOLLOW}(E) = \{+, ), \$\}$
- $\text{FOLLOW}(T) = \{+, *, ), \$\}$
- $\text{FOLLOW}(F) = \{+, *, ), \$\}$

---

## Step 3: The Complete SLR(1) Parsing Table

In the table below:
- $s_k$ denotes **Shift into State $k$**.
- $r_p$ denotes **Reduce by production rule $(p)$**.
- `acc` denotes **Accept**.

| State | $\mathbf{id}$ | $+$ | $*$ | $($ | $)$ | `$` | $E$ | $T$ | $F$ |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **0** | $s_5$ | | | $s_4$ | | | 1 | 2 | 3 |
| **1** | | $s_6$ | | | | `acc` | | | |
| **2** | | $r_2$ | $s_7$ | | $r_2$ | $r_2$ | | | |
| **3** | | $r_4$ | $r_4$ | | $r_4$ | $r_4$ | | | |
| **4** | $s_5$ | | | $s_4$ | | | 8 | 2 | 3 |
| **5** | | $r_6$ | $r_6$ | | $r_6$ | $r_6$ | | | |
| **6** | $s_5$ | | | $s_4$ | | | | 9 | 3 |
| **7** | $s_5$ | | | $s_4$ | | | | | 10 |
| **8** | | $s_6$ | | | $s_{11}$ | | | | |
| **9** | | $r_1$ | $s_7$ | | $r_1$ | $r_1$ | | | |
| **10** | | $r_3$ | $r_3$ | | $r_3$ | $r_3$ | | | |
| **11** | | $r_5$ | $r_5$ | | $r_5$ | $r_5$ | | | |

*Conflict Inspection:* Notice state 2 contains $[E \to T \cdot]$ (calling for reduce on $\text{FOLLOW}(E) = \{+, ), \$\}$) and $[T \to T \cdot * F]$ (calling for shift on $*$). Because $*$ is NOT in $\text{FOLLOW}(E)$, the shift action lands in column $*$ while the reduce actions land in columns $+$, $)$, and `$`. **There are zero collisions**, proving the grammar is **SLR(1)**!

---

## Step 4: Step-by-Step LR Parsing Simulation

Input String: $\mathbf{id} * \mathbf{id} + \mathbf{id}$ `$`  
Initial State Stack: `[0]`  
Initial Symbol Stack: `[$]`

| Step | State Stack | Symbol Stack | Input Buffer | Action Taken |
|:---:|---|---|---|---|
| **1** | `0` | `$` | `id * id + id $` | $\text{ACTION}[0, \mathbf{id}] = s_5$. Shift $\mathbf{id}$, push state 5 |
| **2** | `0 5` | `$ id` | `* id + id $` | $\text{ACTION}[5, *] = r_6$ (Reduce by $F \to \mathbf{id}$). Pop 1 state, uncover 0. $\text{GOTO}[0, F] = 3$. Push 3 |
| **3** | `0 3` | `$ F` | `* id + id $` | $\text{ACTION}[3, *] = r_4$ (Reduce by $T \to F$). Pop 1 state, uncover 0. $\text{GOTO}[0, T] = 2$. Push 2 |
| **4** | `0 2` | `$ T` | `* id + id $` | $\text{ACTION}[2, *] = s_7$. Shift $*$, push state 7 |
| **5** | `0 2 7` | `$ T *` | `id + id $` | $\text{ACTION}[7, \mathbf{id}] = s_5$. Shift $\mathbf{id}$, push state 5 |
| **6** | `0 2 7 5` | `$ T * id` | `+ id $` | $\text{ACTION}[5, +] = r_6$ (Reduce by $F \to \mathbf{id}$). Pop 1 state, uncover 7. $\text{GOTO}[7, F] = 10$. Push 10 |
| **7** | `0 2 7 10` | `$ T * F` | `+ id $` | $\text{ACTION}[10, +] = r_3$ (Reduce by $T \to T * F$). Pop 3 states, uncover 0. $\text{GOTO}[0, T] = 2$. Push 2 |
| **8** | `0 2` | `$ T` | `+ id $` | $\text{ACTION}[2, +] = r_2$ (Reduce by $E \to T$). Pop 1 state, uncover 0. $\text{GOTO}[0, E] = 1$. Push 1 |
| **9** | `0 1` | `$ E` | `+ id $` | $\text{ACTION}[1, +] = s_6$. Shift $+$, push state 6 |
| **10** | `0 1 6` | `$ E +` | `id $` | $\text{ACTION}[6, \mathbf{id}] = s_5$. Shift $\mathbf{id}$, push state 5 |
| **11** | `0 1 6 5` | `$ E + id` | `$` | $\text{ACTION}[5, \$] = r_6$ (Reduce by $F \to \mathbf{id}$). Pop 1 state, uncover 6. $\text{GOTO}[6, F] = 3$. Push 3 |
| **12** | `0 1 6 3` | `$ E + F` | `$` | $\text{ACTION}[3, \$] = r_4$ (Reduce by $T \to F$). Pop 1 state, uncover 6. $\text{GOTO}[6, T] = 9$. Push 9 |
| **13** | `0 1 6 9` | `$ E + T` | `$` | $\text{ACTION}[9, \$] = r_1$ (Reduce by $E \to E + T$). Pop 3 states, uncover 0. $\text{GOTO}[0, E] = 1$. Push 1 |
| **14** | `0 1` | `$ E` | `$` | $\text{ACTION}[1, \$] = \mathbf{accept}$! **Parsing Successfully Completed!** |

---

## Important Properties and Key Observations

1. **Operator Precedence Enforced Mechanically:** In Step 4, when the stack held `T` and lookahead was `*`, state 2 executed **shift $s_7$** rather than reducing $T$ to $E$. This delayed the addition until *after* the multiplication was reduced in Step 7!
2. **Left-to-Right Reductions:** Notice that $T * F$ was reduced to $T$ *before* the subsequent $+$ operator was shifted. The parser strictly reconstructed the canonical rightmost derivation in reverse.

---

## Related Concepts

- [[Bottom-Up Parsing, Reductions, and Handles]]
- [[Shift-Reduce Parsing and Parsing Conflicts]]
- [[LR(0) Items and the LR(0) Automaton]]
- [[SLR(1) Parsing Table Construction and LR Parser Algorithm]]
- [[Problem — SLR(1) Table Construction and Conflict Analysis]]

---

## Prerequisites

- [[LR(0) Items and the LR(0) Automaton]]
- [[SLR(1) Parsing Table Construction and LR Parser Algorithm]]

---

## Problems

- [[Problem — SLR(1) Table Construction and Conflict Analysis]]

---

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/Rumi Sir Merged.pdf|Rumi Sir Merged.pdf]], Chapter 4: Syntax Analysis (Slides 678–719).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 4.5.3 (pp. 248–254).

---

## Navigation

> 📖 **Reading Order:** Step 20 of 76 | **Module 4: Bottom-Up Parsing & LR Parsers**  
> ◄ **Previous:** [[SLR(1) Parsing Table Construction and LR Parser Algorithm]] | ► **Next:** [[Problem — SLR(1) Table Construction and Conflict Analysis]]
