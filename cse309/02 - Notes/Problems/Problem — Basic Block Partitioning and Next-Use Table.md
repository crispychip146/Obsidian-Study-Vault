---
type: problem
course: cse309
status: active
order: 41
---

# Problem — Basic Block Partitioning and Next-Use Table

> 📖 **Reading Order:** Step 41 of 55 | **Module 4:** Code Generation  
> ◄ **Previous:** [[DAG-Based Basic Block Optimization Example]] | ► **Next:** [[Problem — DAG Optimization of Basic Block with Array Store]]

---

## Problem

Given the following intermediate code fragment:

```
(1)  x = 1
(2)  y = 2
(3)  z = x + y
(4)  if z > 10 goto (8)
(5)  p = x * 2
(6)  q = y * 2
(7)  goto (10)
(8)  p = x + 2
(9)  q = y + 2
(10) w = p + q
(11) if w < 100 goto (1)
(12) halt
```

### Tasks:
1. Identify all Leaders in this program using the 3 formal rules, stating which rule applies to each leader.
2. Partition the code into Basic Blocks and construct the Control Flow Graph (CFG).
3. Compute the backward liveness and next-use information for each statement in the block containing statements `(5), (6), (7)`. Assume variables `p, q, w, x, y` are live at exit of this block.

---

## Given

- Source program code, SDD grammar rules, TAC instructions, or flow graph.

---

## Required

- Formal step-by-step derivation, intermediate code generation, and optimization proofs.

---

## Concepts Tested

- [[Syntax-Directed Definitions and Translation Schemes]]
- [[Intermediate Representations and Three-Address Code]]

---

## Prerequisites

- [[Syntax-Directed Definitions and Translation Schemes]]

---

## Question Type

Compiler Analysis / SDD Construction / Code Generation

---

## Solution

### Understanding the Situation
Interpret the given grammar productions, program constructs, and optimization objectives.

### Developing the Key Idea
Apply the appropriate compiler technique (e.g. S-attributed bottom-up evaluation, leader identification, DAG value numbering, or Kempe's graph coloring heuristic).

### Working Through the Solution
### Step-by-Step Solution

### Part 1: Identifying Leaders

1. **Rule 1 (First Statement):**
   - Instruction `(1)` is a Leader.
2. **Rule 2 (Jump Targets):**
   - Statement (4) has `goto (8)` $\implies$ Instruction `(8)` is a Leader.
   - Statement (7) has `goto (10)` $\implies$ Instruction `(10)` is a Leader.
   - Statement (11) has `goto (1)` $\implies$ Instruction `(1)` is a Leader (already marked).
3. **Rule 3 (Following Jumps):**
   - Statement (4) is a branch $\implies$ Instruction `(5)` is a Leader.
   - Statement (7) is a branch $\implies$ Instruction `(8)` is a Leader (already marked).
   - Statement (11) is a branch $\implies$ Instruction `(12)` is a Leader.

**Complete Leader Set:** $\{ (1), (5), (8), (10), (12) \}$

---

### Part 2: Partitioning into Basic Blocks and CFG

- **Block $B_1$:** Instructions `(1) .. (4)`
  ```
  (1) x = 1
  (2) y = 2
  (3) z = x + y
  (4) if z > 10 goto (8)
  ```
- **Block $B_2$:** Instructions `(5) .. (7)`
  ```
  (5) p = x * 2
  (6) q = y * 2
  (7) goto (10)
  ```
- **Block $B_3$:** Instructions `(8) .. (9)`
  ```
  (8) p = x + 2
  (9) q = y + 2
  ```
- **Block $B_4$:** Instructions `(10) .. (11)`
  ```
  (10) w = p + q
  (11) if w < 100 goto (1)
  ```
- **Block $B_5$:** Instruction `(12)`
  ```
  (12) halt
  ```

#### Control Flow Edges:
- $B_1 \to B_3$ (branch at 4) and $B_1 \to B_2$ (fallthrough).
- $B_2 \to B_4$ (unconditional jump at 7 to 10).
- $B_3 \to B_4$ (fallthrough from 9 to 10).
- $B_4 \to B_1$ (branch at 11 to 1) and $B_4 \to B_5$ (fallthrough to 12).

---

### Part 3: Backward Next-Use Analysis for Block $B_2$

Block $B_2$ contains:
```
(5) p = x * 2
(6) q = y * 2
(7) goto (10)
```
- Initial symbol table at exit of $B_2$ (from problem statement: `p, q, x, y` live, no next use inside $B_2$):
  - `p`: (live, none)
  - `q`: (live, none)
  - `x`: (live, none)
  - `y`: (live, none)

#### Backward Scan:
1. **Instruction (7): `goto (10)`**
   - No variables defined or used. Table unchanged.
2. **Instruction (6): `q = y * 2`**
   - Attach info: `q`: (live, none), `y`: (live, none)
   - Update: `q` defined $\implies$ `q`: **(dead, none)**; `y` used $\implies$ `y`: **(live, 6)**
3. **Instruction (5): `p = x * 2`**
   - Attach info: `p`: (live, none), `x`: (live, none)
   - Update: `p` defined $\implies$ `p`: **(dead, none)**; `x` used $\implies$ `x`: **(live, 5)**

#### Next-Use Table for Block $B_2$:
| Inst # | Statement | Attached Variable Status |
| :---: | :---: | :--- |
| **(5)** | `p = x * 2` | $p$: live, none; &nbsp; $x$: live, none; &nbsp; constant `2` |
| **(6)** | `q = y * 2` | $q$: live, none; &nbsp; $y$: live, none; &nbsp; constant `2` |
| **(7)** | `goto (10)` | None |

---

### Result and Interpretation
The final annotated tree, TAC sequence, or optimized basic block is rigorously verified.

---

## Reusable Insight

Always follow compiler phase invariants: parse bottom-up or top-down according to attribute classes, build dependency graphs to verify evaluation order, and track next-use pointers backwards.

---

## Common Mistakes

- Prematurely evaluating expressions before operand definitions are processed.
- Neglecting array store kill rules in basic block DAGs.

---

## Exam Pattern

Standard BUET CSE 309 final examination problem testing syllabus Chapter 5, 6, 7, 8, or 9.

---

## Related Problems

- [[Problem — Desk Calculator SDD and Annotated Parse Tree]]
- [[Problem — Array Reference Three-Address Code Generation]]

---

## Related Concepts

- [[Syntax-Directed Definitions and Translation Schemes]]
- [[Basic Blocks and Control Flow Graphs]]

---

## Source

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Chapter 8 (Slides 323–334).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 8.4.
