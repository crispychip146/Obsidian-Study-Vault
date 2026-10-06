---
type: example
course: cse309
status: active
order: 29
---

# Display Maintenance and Non-Local Access Simulation Example

> 📖 **Reading Order:** Step 29 of 55 | **Module 3:** Run-Time Environments  
> ◄ **Previous:** [[Copying Garbage Collection Algorithm]] | ► **Next:** [[Garbage Collection Trace and Compaction Example]]

---

## Problem

Consider a program written in a block-structured language with nested procedure declarations:

```pascal
program Main;                    { Lexical Depth 1 }
    var x: integer;

    procedure P;                 { Lexical Depth 2 }
        var y: integer;

        procedure Q;             { Lexical Depth 3 }
            var z: integer;
        begin
            z := x + y;          { Non-local access! }
        end;

        procedure R;             { Lexical Depth 3 }
        begin
            Q();                 { R calls sibling Q }
        end;

    begin { Body of P }
        R();
    end;

begin { Body of Main }
    P();
end.
```

### Call Sequence Trace:
$$\text{Main} \longrightarrow P \longrightarrow R \longrightarrow Q$$

### Questions:
1. Show the state of the **Display Array** at each step of the call sequence.
2. For the statement $z := x + y$ in procedure $Q$, show how $x$ and $y$ are resolved in $O(1)$ time using the Display.
3. Show how the Display is restored as procedures return.

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

### Step-by-Step Display Tracing

The program has a maximum lexical nesting depth of $3$. We allocate a global Display array:
$$\text{Display}[1 \dots 3]$$

Each activation record contains a field `saved_display` to preserve the previous pointer at its lexical depth.

---

### Step 1: `Main` Executes (Lexical Depth 1)
- Activation record $AR_{\text{Main}}$ is created at stack address `1000`.
- $\text{Display}[1] = 1000$.

```
Display:
[1] -> 1000 (Main)
[2] -> null
[3] -> null
```

---

### Step 2: `Main` calls $P$ (Lexical Depth 2)
- Activation record $AR_{P}$ allocated at address `1040`.
- $P$ is at depth 2:
  - $AR_{P}.\text{saved\_display} = \text{Display}[2] = \text{null}$.
  - $\text{Display}[2] = 1040$.

```
Display:
[1] -> 1000 (Main)
[2] -> 1040 (P)
[3] -> null
```

---

### Step 3: $P$ calls $R$ (Lexical Depth 3)
- Activation record $AR_{R}$ allocated at address `1080`.
- $R$ is at depth 3:
  - $AR_{R}.\text{saved\_display} = \text{Display}[3] = \text{null}$.
  - $\text{Display}[3] = 1080$.

```
Display:
[1] -> 1000 (Main)
[2] -> 1040 (P)
[3] -> 1080 (R)
```

---

### Step 4: $R$ calls $Q$ (Lexical Depth 3)
- Activation record $AR_{Q}$ allocated at address `1120`.
- $Q$ is at depth 3:
  - $AR_{Q}.\text{saved\_display} = \text{Display}[3] = 1080$ (saving $R$'s pointer!).
  - $\text{Display}[3] = 1120$.

```
Display (While Q is executing):
[1] -> 1000 (Main)
[2] -> 1040 (P)
[3] -> 1120 (Q)
```

---
### Resolving Non-Local Access in Procedure $Q$: $z := x + y$

Inside $Q$:
- $z$ is local to $Q$ (depth 3):
  $$\text{Address}(z) = \text{Display}[3] + \text{offset}(z) = 1120 + \text{offset}(z)$$
- $y$ is declared in $P$ (depth 2):
  $$\text{Address}(y) = \text{Display}[2] + \text{offset}(y) = 1040 + \text{offset}(y)$$
- $x$ is declared in `Main` (depth 1):
  $$\text{Address}(x) = \text{Display}[1] + \text{offset}(x) = 1000 + \text{offset}(x)$$

**Observation:** Notice that although $Q$ was called by $R$, the Display correctly bypasses $R$'s frame and points directly to $P$'s frame at depth 2 in strictly **one pointer dereference**!

---
### Return Sequence and Display Restoration

1. **$Q$ returns to $R$:**
   - Callee epilogue restores: $\text{Display}[3] = AR_{Q}.\text{saved\_display} = 1080$.
   - Stack frame `1120` is popped.
   - Display returns to: `[1]->1000, [2]->1040, [3]->1080` (correctly pointing back to $R$).
2. **$R$ returns to $P$:**
   - Epilogue restores: $\text{Display}[3] = AR_{R}.\text{saved\_display} = \text{null}$.
   - Display returns to: `[1]->1000, [2]->1040, [3]->null`.
3. **$P$ returns to `Main`:**
   - Epilogue restores: $\text{Display}[2] = AR_{P}.\text{saved\_display} = \text{null}$.
   - Display returns to: `[1]->1000, [2]->null, [3]->null`.

The Display mechanism maintains perfect $O(1)$ access invariant across arbitrary dynamic call trees!

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

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Chapter 7 (Slides 223–229).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 7.3.2.
