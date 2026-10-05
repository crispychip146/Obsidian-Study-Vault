---
type: algorithm
course: cse309
status: active
order: 37
---

# A Simple Code Generator Algorithm

> 📖 **Reading Order:** Step 37 of 55 | **Module 4:** Code Generation  
> ◄ **Previous:** [[DAG Construction and Local Optimization of Basic Blocks]] | ► **Next:** [[Peephole Optimization Techniques]]

---

## Building the idea

The simple generator tracks where current values are stored. A **register descriptor** records values held by a register; an **address descriptor** records locations holding a variable's current value. The descriptors must describe the emitted code, not what the compiler hopes is still present.

For `x=y+z`, obtain the operands, select a destination register, emit the operation, and update the descriptors. If a register's old content will be overwritten and contains the only copy of a needed value, store that value first. If it is dead or available elsewhere, the store is unnecessary.

[[Liveness and Next-Use Analysis within Basic Blocks]] explains the “needed” test. At block exit, preserve live-out values required by successor code; a simple memory-based boundary convention stores dirty live values not already current in memory.

Trace an illustrative operand through load, use, overwrite, and possible store. The physical register may change ownership several times, but each live value must retain an accessible correct copy.

## How It Works

### End of Basic Block Actions

At the end of a basic block:
- If variable $x$ is **live at block exit**, and the current value of $x$ resides only in a register $R_x$ (not in memory):
  - Emit:
    $$\text{ST } x, R_x$$
- Any temporary variable whose lifetime does not extend beyond the block is simply discarded without storing.

---

## Pseudocode

### The Code Generation Algorithm for $x = y + z$

1. Consult address descriptor of $y$ to determine its current location $L_y$:
   - Prefer a register holding $y$. If $y$ is only in memory, $L_y = y$.
2. Consult address descriptor of $z$ to determine its location $L_z$.
3. Call `getReg(x = y + z)` to choose a destination register $R_x$.
4. If $y$ is not already in register $R_x$:
   - Emit instruction: $\text{LD } R_x, L_y$.
5. Emit instruction: $\text{ADD } R_x, R_x, L_z$.
6. **Update Descriptors:**
   - Set register descriptor of $R_x$ to $\{ x \}$.
   - Set address descriptor of $x$ to $\{ R_x \}$ (and remove memory $x$, since memory is now stale!).
   - Remove $R_x$ from the address descriptors of any other variables that previously occupied it.
7. **Free Dead Registers:**
   - If $y$ has no next use and is in register $R_y \ne R_x$, remove $y$ from $R_y$'s descriptor.
   - If $z$ has no next use and is in register $R_z$, remove $z$ from $R_z$'s descriptor.

---

## Complexity

- **Time Complexity:** $O(N \cdot K)$ where $N$ is the number of TAC statements in the basic block and $K$ is the number of hardware registers (scanning register descriptors takes $O(K)$ time).
- **Space Complexity:** $O(V + K)$ storing descriptors for $V$ variables and $K$ registers.

---

## Exam Relevance

---

### Exam Relevance

Frequently tested on final examinations via hand-simulation of A Simple Code Generator Algorithm on given code fragments or graphs.

---

## What to carry forward

Descriptors are invariants maintained after every instruction. [[Live Ranges and Live Intervals in Register Allocation]] extends the allocation problem beyond this local generator's choices. Calls and target register constraints require additional preservation rules.

## Related notes

- [[Liveness and Next-Use Analysis within Basic Blocks]]
- [[Live Ranges and Live Intervals in Register Allocation]]

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Chapter 8 (Slides 348–358).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 8.6.
