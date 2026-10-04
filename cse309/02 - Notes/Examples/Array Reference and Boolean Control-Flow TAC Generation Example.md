---
type: example
course: cse309
status: active
order: 18
---

# Array Reference and Boolean Control-Flow TAC Generation Example

> 📖 **Reading Order:** Step 18 of 55 | **Module 2:** Intermediate Code Generation  
> ◄ **Previous:** [[Backpatching Control-Flow Code Generation Algorithm]] | ► **Next:** [[Problem — Array Reference Three-Address Code Generation]]

---

---

## Problem

Consider the classic searching and filtering loop found in systems programming:
```c
while (i < 10 && a[i] > max) {
    a[i] = a[i] - 1;
    i = i + 1;
}
```

### Why Short-Circuiting Is a Matter of Life and Death
Look at the array boundary: `a` is allocated with 10 elements (indices $0$ to $9$).
- If `i` reaches $10$, the condition `i < 10` evaluates to **false**.
- If the compiler did **not** short-circuit and eagerly evaluated `a[i] > max`, the CPU would attempt to read `a[10]`. That is a **buffer over-read**! It might read garbage memory, or if `a` is at the end of a memory page, trigger a fatal hardware Segmentation Fault (`SIGSEGV`).
- Because short-circuiting mandates that the second operand of `&&` must never execute if the first operand is false, the compiler's generated Three-Address Code **must** jump past the memory load whenever `i < 10` fails!

---

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

### Compilation Mechanics: Step-by-Step Backpatching Trace

Let us trace the execution of the Backpatching algorithm from [[Backpatching Control-Flow Code Generation Algorithm]] on this while-loop:

$$\text{Production: } S \longrightarrow \mathbf{while} \; M_1 \; ( B ) \; M_2 \; S_1$$

### Step 1: Loop Header Marker $M_1$
- The loop begins at instruction quad 100.
- Marker $M_1 \to \epsilon$ fires:
  $$M_1.quad = nextquad = \mathbf{100}$$

---

### Step 2: Translating the Short-Circuit Condition ($B_1 \text{ \&\& } M_{and} B_2$)
The condition is $B = B_1 \text{ \&\& } M_{and} B_2$:

1. **Sub-condition $B_1$ (`i < 10`):**
   - Emits quad 100: `if i < 10 goto _`
   - Emits quad 101: `goto _`
   - $B_1.truelist = [100]$, $B_1.falselist = [101]$.
2. **Marker $M_{and}$ between $B_1$ and $B_2$:**
   - Fires at quad 102: $M_{and}.quad = \mathbf{102}$.
   - The semantic rule for `&&` executes:
     $$\text{backpatch}(B_1.truelist, M_{and}.quad) \implies \text{backpatch}([100], 102)$$
   - Instruction 100 becomes: `100: if i < 10 goto 102`.
3. **Sub-condition $B_2$ (`a[i] > max`):**
   - Translates array reference $a[i]$ (R-value load):
     ```text
     102: t1 = i * 4
     103: t2 = a[t1]
     ```
   - Emits relational jump for `t2 > max`:
     ```text
     104: if t2 > max goto _
     105: goto _
     ```
   - $B_2.truelist = [104]$, $B_2.falselist = [105]$.
4. **Finalizing $B \to B_1 \text{ \&\& } M_{and} B_2$:**
   - $B.truelist = B_2.truelist = [104]$.
   - $B.falselist = \text{merge}(B_1.falselist, B_2.falselist) = \text{merge}([101], [105]) = \mathbf{[101, 105]}$.

---

### Step 3: Loop Body Marker $M_2$
- Marker $M_2 \to \epsilon$ fires before statement body $S_1$:
  $$M_2.quad = nextquad = \mathbf{106}$$
- The while-loop semantic rule executes:
  $$\text{backpatch}(B.truelist, M_2.quad) \implies \text{backpatch}([104], 106)$$
- Instruction 104 becomes: `104: if t2 > max goto 106`.

---

### Step 4: Translating Loop Body $S_1$

1. **Statement 1: `a[i] = a[i] - 1;`:**
   - Read RHS `a[i]`:
     ```text
     106: t3 = i * 4
     107: t4 = a[t3]
     108: t5 = t4 - 1
     ```
   - Write to LHS `a[i]`:
     ```text
     109: t6 = i * 4
     110: a[t6] = t5
     ```
2. **Statement 2: `i = i + 1;`:**
   ```text
   111: t7 = i + 1
   112: i = t7
   ```
   $S_1.nextlist = []$.

---

### Step 5: Finalizing the While Loop
The while loop rule emits the unconditional loop back-edge:
- `emit('goto ' M1.quad)` $\implies$ emits quad 113: `goto 100`.
- Statement exit list:
  $$S.nextlist = B.falselist = \mathbf{[101, 105]}$$
- When the statement succeeding the while-loop is reached at quad **114**, the compiler backpatches:
  $$\text{backpatch}([101, 105], 114)$$
- Instructions 101 and 105 become:
  ```text
  101: goto 114
  105: goto 114
  ```

---
### Complete Emitted Three-Address Code (TAC)

```text
100: if i < 10 goto 102      // B1 test: if true, test B2
101: goto 114                // Short-circuit exit: i >= 10
102: t1 = i * 4              // B2 array offset
103: t2 = a[t1]              // B2 array load
104: if t2 > max goto 106    // B2 test: if true, enter loop body
105: goto 114                // Condition failed: exit loop
106: t3 = i * 4              // Loop body: a[i] = a[i] - 1
107: t4 = a[t3]
108: t5 = t4 - 1
109: t6 = i * 4
110: a[t6] = t5              // Indexed memory store
111: t7 = i + 1              // i = i + 1
112: i = t7
113: goto 100                // Loop back to test
114: ...                     // Loop exit target
```

---
### Quadruples vs. Triples vs. Indirect Triples

In intermediate representations, TAC is physically stored in compiler memory using one of three classic data structures:

```
                    Comparison of IR Storage Schemes
  ┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐
  │   Quadruples    │       │     Triples     │       │ Indirect Triples│
  ├─────────────────┤       ├─────────────────┤       ├─────────────────┤
  │ Explicit temp   │       │ Implicit temp   │       │ Pointers to     │
  │ names (t1, t2). │       │ by array index. │       │ triples. Easy   │
  │ Heavy memory.   │       │ Hard to move.   │       │ to reorder!     │
  └─────────────────┘       └─────────────────┘       └─────────────────┘
```

### 4.1 Quadruple Table Representation
Quadruples use 4 fields: `(op, arg1, arg2, result)`. Temporaries are explicitly named.

| Quad # | Operator (`op`) | Argument 1 (`arg1`) | Argument 2 (`arg2`) | Result (`result`) |
| :---: | :---: | :---: | :---: | :---: |
| **100** | `if <` | `i` | `10` | `102` |
| **101** | `goto` | - | - | `114` |
| **102** | `*` | `i` | `4` | `t1` |
| **103** | `[]=` (load) | `a` | `t1` | `t2` |
| **104** | `if >` | `t2` | `max` | `106` |
| **105** | `goto` | - | - | `114` |
| **106** | `*` | `i` | `4` | `t3` |
| **107** | `[]=` (load) | `a` | `t3` | `t4` |
| **108** | `-` | `t4` | `1` | `t5` |
| **109** | `*` | `i` | `4` | `t6` |
| **110** | `=[]` (store) | `t5` | `t6` | `a` |
| **111** | `+` | `i` | `1` | `t7` |
| **112** | `=` | `t7` | - | `i` |
| **113** | `goto` | - | - | `100` |

---

### 4.2 Triples Representation
Triples avoid creating temporary names (`t1`, `t2`, $\dots$). Instead, references to previous computations use the **position index** (in parentheses `(k)`):

| Position | Operator (`op`) | Argument 1 (`arg1`) | Argument 2 (`arg2`) |
| :---: | :---: | :---: | :---: |
| **(0)** | `*` | `i` | `4` |
| **(1)** | `[]=` | `a` | `(0)` |
| **(2)** | `-` | `(1)` | `1` |
| **(3)** | `*` | `i` | `4` |
| **(4)** | `=[]` | `(2)` | `(3)` |

> [!WARNING] The Triple Reordering Defect
> When an optimizing compiler moves an instruction during Code Motion or Instruction Scheduling, every subsequent triple referring to that instruction by its absolute index `(k)` must be hunted down and updated!

---

### 4.3 Indirect Triples Representation
To overcome the reordering defect of Triples, compilers use **Indirect Triples**:
- Triples are stored in an array without changing their position.
- An auxiliary **Pointer Array** lists the order of execution:
  $$\text{Execution Order: } [p_0, p_1, p_2, \dots]$$
- If the compiler wants to reorder instructions, it simply swaps pointers in the execution array! No triple arguments need to be updated.

---

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

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Chapter 6 (Slides 102–117, 146–175).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 6.2 (Representations of Intermediate Code), Section 6.7 (Backpatching).
