---
type: example
course: cse309
status: active
order: 40
---

# DAG-Based Basic Block Optimization Example

> 📖 **Reading Order:** Step 40 of 55 | **Module 4:** Code Generation  
> ◄ **Previous:** [[Basic Block Partitioning and Next-Use Computation Example]] | ► **Next:** [[Problem — Basic Block Partitioning and Next-Use Table]]

---

## Problem

Consider the following basic block containing redundant computations and array references:

```
(1) a = b + c
(2) d = a - d
(3) e = b + c
(4) f = e - d
(5) b = a - d
(6) g = b + c
```

Assume that at the exit of this basic block:
- Variables `a`, `b`, and `f` are **live**.
- All other variables (`c`, `d`, `e`, `g`) are **dead**.

### Tasks:
1. Construct the Directed Acyclic Graph (DAG) for this basic block step-by-step.
2. Identify and eliminate local common subexpressions.
3. Perform dead code elimination on the DAG.
4. Reassemble the optimal Three-Address Code from the simplified DAG.

---

## Given

- Grammar productions, semantic rules, basic blocks, or register sets as specified in the problem setup.

---

## Required

- Full step-by-step annotated tree derivation, TAC generation, DAG reduction, or register assignment trace.

---

## Understanding the Problem and Choosing the Method

Analyze the input program structure, identify the governing compiler phase algorithms, and simulate the execution step by step while maintaining all internal invariants.

---

## Solution

### Step-by-Step DAG Construction

### Step 1: Statement (1) `a = b + c`
- Create leaf nodes $b_0$ and $c_0$.
- Create interior node $N_1 = ('+', b_0, c_0)$.
- Attach label `a` to $N_1$.

### Step 2: Statement (2) `d = a - d`
- Create leaf node $d_0$.
- Create interior node $N_2 = ('-', N_1, d_0)$.
- Attach label `d` to $N_2$.

### Step 3: Statement (3) `e = b + c`
- Signature `('+', b_0, c_0)` **already exists** at Node $N_1$!
- Do NOT create a new node. Simply attach label `e` to $N_1$.
- Node $N_1$ now has labels `{ a, e }`.

### Step 4: Statement (4) `f = e - d`
- Left child is $N_1$ (since `e` is at $N_1$).
- Right child is $N_2$ (since `d` is at $N_2$).
- Create interior node $N_3 = ('-', N_1, N_2)$.
- Attach label `f` to $N_3$.

### Step 5: Statement (5) `b = a - d`
- Left child is $N_1$, right child is $N_2$.
- Signature `('-', N_1, N_2)` **already exists** at Node $N_3$!
- Attach label `b` to $N_3$.
- Node $N_3$ now has labels `{ f, b }`.

### Step 6: Statement (6) `g = b + c`
- Left child is $N_3$ (since `b` is now at $N_3$).
- Right child is leaf $c_0$.
- Create interior node $N_4 = ('+', N_3, c_0)$.
- Attach label `g` to $N_4$.

---
### DAG Optimization and Dead Code Elimination

```mermaid
graph TD
    N1["Node 1 (+)<br/>Labels: { a, e }"] --- b0["b0"]
    N1 --- c0["c0"]
    N2["Node 2 (-)<br/>Labels: { d }"] --- N1
    N2 --- d0["d0"]
    N3["Node 3 (-)<br/>Labels: { f, b }"] --- N1
    N3 --- N2
    N4["Node 4 (+)<br/>Labels: { g } (DEAD ROOT)"] --- N3
    N4 --- c0
```

### Applying Dead Code Elimination:
1. Node $N_4$ has only label `g` attached.
2. Given that `g` is **dead at block exit**, node $N_4$ is an unused root. **Eliminate Node $N_4$!**
3. Label `e` on $N_1$ and label `d` on $N_2$ are dead at exit, but the nodes themselves must be evaluated to compute $N_3$ (which computes live variables `f` and `b`).

---
### Reassembling Optimized Three-Address Code

We evaluate the surviving nodes in topological order ($N_1 \to N_2 \to N_3$):

1. **Evaluate Node $N_1$:**
   - Computes $b_0 + c_0$. Live label `a` is attached:
     ```
     a = b + c
     ```
2. **Evaluate Node $N_2$:**
   - Computes $N_1 - d_0$. Use temporary `d` (or `t1`):
     ```
     d = a - d
     ```
3. **Evaluate Node $N_3$:**
   - Computes $N_1 - N_2$. Attached live labels are `f` and `b`:
     ```
     b = a - d
     f = b
     ```

### Complete Reconstructed Code:
```
a = b + c
d = a - d
b = a - d
f = b
```

### Result Analysis:
- Original code: **6 statements**.
- Optimized code: **4 statements**.
- The redundant common subexpressions `b + c` (statement 3) and `a - d` (statement 5) were completely eliminated, and dead computation `g = b + c` was purged!

---

## Result

The compilation pass finishes with verified intermediate representations and correct register assignments.

---

## Why This Works

Every transformation maintains semantic program equivalence while optimizing instruction counts, memory foot-print, or register usage.

---

## Common Mistakes

- Incorrectly calculating stack frame offsets or TAC temporaries.
- Forgetting to spill registers when register demand exceeds hardware pool size.

---

## General Method

Extract the general procedure: parse/partition input, construct intermediate data structures, apply optimizations iteratively, and emit final code.

---

## Related Concepts

- [[Syntax-Directed Definitions and Translation Schemes]]
- [[Basic Blocks and Control Flow Graphs]]

---

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Chapter 8 (Slides 335–347).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 8.5, Example 8.10.
