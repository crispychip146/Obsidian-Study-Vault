---
type: concept
course: cse309
status: active
order: 32
---

# Code Generation Issues and Target Machine Architecture

> 📖 **Reading Order:** Step 32 of 55 | **Module 4:** Code Generation  
> ◄ **Previous:** [[Problem — Activation Record and Display Table Tracing]] | ► **Next:** [[Basic Blocks and Control Flow Graphs]]

---

## Building the idea

TAC says what values to compute. Code generation decides which target instructions implement those computations and where their operands live. A source-level addition may need loads, an arithmetic instruction, and a store depending on the target and the current register state.

Separate **instruction selection** from **register allocation** and **assignment**. Selection chooses an instruction pattern; allocation determines which live values remain in registers; assignment chooses particular registers subject to constraints. Their choices interact, but they answer different questions.

The target model below defines instructions and addressing costs for classroom reasoning. Its cost units need not be literal cycles on a modern processor. Use that model consistently when comparing two instruction sequences.

[[Intermediate Representations and Three-Address Code]] supplies input operations. [[Liveness and Next-Use Analysis within Basic Blocks]] tells us which values must survive, so the generator can avoid discarding needed operands while reusing scarce registers.

## How It Works

### A Simple Target Machine Model

To study code generation algorithms rigorously, the Dragon Book and BUET syllabus define a representative RISC-like target machine model:

### Memory and Registers:
- $n$ general-purpose hardware registers: $R_0, R_1, \dots, R_{n-1}$.
- Word-addressable memory.

### Instruction Set:
1. **Load:** `LD r, x` ($r = \text{contents of memory location } x$).
2. **Store:** `ST x, r` ($\text{memory location } x = r$).
3. **Arithmetic / Logic:** `ADD r1, r2, r3` ($r1 = r2 + r3$) or two-address format: `ADD r1, r2` ($r1 = r1 + r2$).
4. **Immediate Arithmetic:** `ADD r, r, #c` ($r = r + c$).
5. **Conditional Branch:** `Bcond r, L` (e.g., `BLTZ r, L` branches to label $L$ if $r < 0$).
6. **Unconditional Branch:** `BR L` (jumps to label $L$).

### Addressing Modes and Instruction Costs:
The compiler uses an **Instruction Cost Model**:
$$\text{Cost} = 1 + \sum (\text{Cost of Addressing Modes})$$

| Mode | Format | Address Computation | Mode Cost | Total Instruction Cost |
| :--- | :--- | :--- | :---: | :---: |
| **Register** | `R` | Operand is in register $R$ | $0$ | `ADD R0, R1` $\to$ **1** |
| **Immediate** | `#c` | Operand is constant literal $c$ | $1$ | `LD R0, #100` $\to$ **2** |
| **Direct Memory** | `M` | Operand is in memory address $M$ | $1$ | `LD R0, x` $\to$ **2** |
| **Indexed** | `c(R)` | $\text{Address} = c + \text{contents}(R)$ | $1$ | `LD R0, 4(R1)` $\to$ **2** |
| **Indirect** | `*R` | $\text{Address} = \text{contents}(R)$ | $0$ | `LD R0, *R1` $\to$ **1** |

---

## Exam Relevance

---

### Code Generation Example

Consider generating target machine code for:
$$x = y + z$$
Assuming variables $x, y, z$ reside in memory:

```
LD   R0, y        /* Cost: 2 (1 opcode + 1 memory word for y) */
ADD  R0, R0, z    /* Cost: 2 (1 opcode + 1 memory word for z) */
ST   x, R0        /* Cost: 2 (1 opcode + 1 memory word for x) */
```
**Total Instruction Cost:** $2 + 2 + 2 = 6$.

If $y$ is already in register $R_0$ from a preceding operation:
```
ADD  R0, R0, z    /* Cost: 2 */
ST   x, R0        /* Cost: 2 */
```
**Total Cost:** $4$ (one-third less cost in this instruction model; this is not a universal execution-time speedup).

---

## What to carry forward

A shorter sequence is not automatically faster on every target. Compare legal instructions, dependencies, memory accesses, and the stated cost model. [[A Simple Code Generator Algorithm]] keeps explicit descriptors to make these choices traceable.

## Related notes

- [[Intermediate Representations and Three-Address Code]]
- [[Liveness and Next-Use Analysis within Basic Blocks]]
- [[A Simple Code Generator Algorithm]]

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Chapter 8 (Slides 296–321).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 8.1 & 8.2.
