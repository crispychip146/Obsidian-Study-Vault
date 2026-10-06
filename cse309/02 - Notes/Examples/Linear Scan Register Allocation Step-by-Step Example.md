---
type: example
course: cse309
status: active
order: 47
---

# Linear Scan Register Allocation Step-by-Step Example

> 📖 **Reading Order:** Step 47 of 55 | **Module 5:** Register Allocation  
> ◄ **Previous:** [[Chaitin's Graph Coloring Register Allocation Algorithm]] | ► **Next:** [[Chaitin's Graph Coloring Register Allocation Example]]

---

## Problem

We trace the exact register allocation example presented in the KMS lecture slides (Slides 368–425).

Consider the 10-instruction program fragment:

```
(0) e = d + a
(1) f = b + c
(2) f = f + b
(3) ifZ e goto _L0
(4) d = e + f
(5) goto _L1
(6) _L0:
(7) e = e + c
(8) f = d + e
(9) _L1:
(10) g = e + f
```

### Derived Live Intervals:
Liveness analysis computes the following 1-dimensional live intervals for the 7 variables:

```
Variable a: [0, 1]
Variable b: [0, 3]
Variable c: [0, 7]
Variable d: [0, 8]
Variable e: [1, 9]
Variable f: [2, 9]
Variable g: [9, 10]
```

Sorted by starting point: `[a, b, c, d, e, f, g]`.

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

### Allocation Simulation with $R = 4$ Registers: $\{ R_0, R_1, R_2, R_3 \}$

Let us trace `linear_scan` with 4 physical registers:

```
Step | Interval Processed | Action Taken                          | Active Set (sorted by end) | Free Registers
----------------------------------------------------------------------------------------------------------------
1    | a: [0, 1]          | Assign R0                             | [a:1]                      | {R1, R2, R3}
2    | b: [0, 3]          | Assign R1                             | [a:1, b:3]                 | {R2, R3}
3    | c: [0, 7]          | Assign R2                             | [a:1, b:3, c:7]            | {R3}
4    | d: [0, 8]          | Assign R3                             | [a:1, b:3, c:7, d:8]       | {}
5    | e: [1, 9]          | Expire a (end 1 <= 1). Free R0!       |                            |
     |                    | Assign R0 to e                        | [b:3, c:7, d:8, e:9]       | {}
6    | f: [2, 9]          | No interval has end < 2.              |                            |
     |                    | Active is full (4 items)!             |                            |
     |                    | Spill check: candidate is e:9 (or f:9)|                            |
```
Notice: At point 2, 5 variables (`b, c, d, e, f`) are simultaneously live! With only 4 registers, a spill must occur.

---
### Allocation Simulation with $R = 2$ Registers (Demonstrating Spilling)

Now let us trace Linear Scan with only $R = 2$ registers: $\{ R_0, R_1 \}$.

### Step 1: Interval `a: [0, 1]`
- `active` is empty. Assign $R_0$.
- `active = [ a:1 ]`, `free = { R1 }`.

### Step 2: Interval `b: [0, 3]`
- No expired intervals. Assign $R_1$.
- `active = [ a:1, b:3 ]`, `free = { }`.

### Step 3: Interval `c: [0, 7]`
- No expired intervals (`a.end = 1 > 0`).
- `active` is full ($len = 2$).
- **Spill Decision:** Compare `candidate = active[-1]` (`b:3`) with `current` (`c:7`).
  - $c.end = 7 > b.end = 3$.
  - Current interval $c$ extends farther! **Spill $c$!**
- `active = [ a:1, b:3 ]`, allocation: `c -> SPILLED`.

### Step 4: Interval `d: [0, 8]`
- `active` is full. Candidate is `b:3`.
- $d.end = 8 > b.end = 3 \implies$ **Spill $d$!**
- allocation: `d -> SPILLED`.

### Step 5: Interval `e: [1, 9]`
- Check expiration: `a.end = 1 <= 1` $\implies$ **$a$ has expired!**
- Reclaim $R_0$.
- `active = [ b:3 ]`, `free = { R0 }`.
- Assign $R_0$ to $e$.
- `active = [ b:3, e:9 ]`, `free = { }`.

### Step 6: Interval `f: [2, 9]`
- No expired intervals ($b.end = 3 > 2$).
- `active` is full. Candidate is $e:9$.
- $e.end = 9 \ge f.end = 9 \implies$ **Spill $e$ or $f$!**
- Candidate $e$ lives to 9; spill $e$, assign $R_0$ to $f$.
- `active = [ b:3, f:9 ]`.

### Step 7: Interval `g: [9, 10]`
- Check expiration at 9: both $b$ and $f$ expired!
- Reclaim $R_0, R_1$.
- Assign $R_0$ to $g$.

---

## Result

| Variable | Live Interval | Allocation Status | Assigned Physical Register |
| :---: | :---: | :---: | :---: |
| **$a$** | `[0, 1]` | In Register | **$R_0$** |
| **$b$** | `[0, 3]` | In Register | **$R_1$** |
| **$c$** | `[0, 7]` | **Spilled** | Memory Stack Slot |
| **$d$** | `[0, 8]` | **Spilled** | Memory Stack Slot |
| **$e$** | `[1, 9]` | **Spilled** | Memory Stack Slot |
| **$f$** | `[2, 9]` | In Register | **$R_0$** |
| **$g$** | `[9, 10]` | In Register | **$R_0$** |

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

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Register Allocation (Slides 368–425).
- **Textbook / Papers:** Poletto & Sarkar, "Linear Scan Register Allocation", ACM TOPLAS 1999.
