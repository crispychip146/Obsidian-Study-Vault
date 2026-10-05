---
type: problem
course: cse309
status: active
order: 42
---

# Problem — DAG Optimization of Basic Block with Array Store

> 📖 **Reading Order:** Step 42 of 55 | **Module 4:** Code Generation  
> ◄ **Previous:** [[Problem — Basic Block Partitioning and Next-Use Table]] | ► **Next:** [[Live Ranges and Live Intervals in Register Allocation]]

---

## Problem

Consider the following basic block containing array operations:

```
(1) x = a[i]
(2) a[j] = y
(3) z = a[i]
```

### Questions:
1. Explain why it is **incorrect** in general to eliminate statement (3) by reusing the value of `x` computed in statement (1).
2. Construct the DAG representation of this basic block adhering to the compiler rules for array load (`=[]`) and array store (`[]=`) operations.
3. Under what specific condition can the compiler optimize away statement (3)? Show the resulting simplified DAG under that condition.

---

## Solution

The two reads spell `a[i]` identically, but the store between them can change the answer. If i==j, `x=a[i]` gets the old element and `z=a[i]` gets y after the store. Replacing z's load with x would preserve the wrong memory version.

[[DAG Construction and Local Optimization of Basic Blocks]] therefore treats the store as a dependency barrier for potentially aliased loads. One representation creates a new version of the array; another invalidates affected load nodes for future reuse. Neither means the already computed scalar x changes retroactively.

Reuse becomes possible if the compiler proves the store cannot affect the loaded location, the relevant index and base values are unchanged, and no other observable behavior prevents elimination. Proving i≠j is the central condition in this simple example.

This is the difference between syntactic equality and semantic value availability. The program's memory state is an input to the load even when it is not printed as an ordinary operand.

### Part 1: Why Reusing `x` is Semantically Unsound
In statement (2), the assignment `a[j] = y` modifies an element of array `a`.
- If the runtime indices are equal ($i == j$), statement (2) overwrites the value of `a[i]` with $y$.
- When statement (3) `z = a[i]` executes, the true value of `z` must be $y$, **NOT** the old value of `x`!
- Therefore, without proving $i \ne j$, reusing `x` introduces a fatal data-corruption bug.

---

### Part 2: DAG Construction with Array Store Rules

In a compiler DAG:
1. **Statement (1) `x = a[i]`:**
   - Create leaf nodes $a_0$ and $i_0$.
   - Create load node $N_1 = \text{'=[]'}(a_0, i_0)$.
   - Attach label `x` to $N_1$.
2. **Statement (2) `a[j] = y`:**
   - Create leaf nodes $j_0$ and $y_0$.
   - Create store node $N_2 = \text{'[]='}(a_0, j_0, y_0)$.
   - Node $N_2$ has 3 children: array $a_0$, index $j_0$, and value $y_0$.
   - **The Kill Effect:** $N_2$ represents a new version of array $a$. Any subsequent access to $a$ must depend on $N_2$!
3. **Statement (3) `z = a[i]`:**
   - The array operand is now the modified array represented by node $N_2$.
   - Create new load node $N_3 = \text{'=[]'}(N_2, i_0)$.
   - Attach label `z` to $N_3$.

```mermaid
graph TD
    a0["a0"]
    i0["i0"]
    j0["j0"]
    y0["y0"]

    N1["N1: =[] (x)"] --- a0
    N1 --- i0

    N2["N2: []= (Store)"] --- a0
    N2 --- j0
    N2 --- y0

    N3["N3: =[] (z)"] --- N2
    N3 --- i0
```

Notice that $N_3$ takes $N_2$ as its array parent, perfectly preserving the sequential dependence!

---

### Part 3: Compile-Time Index Disambiguation

If the compiler can prove at compile time that $i$ and $j$ refer to distinct constants (e.g., `i = 4` and `j = 8`), then $i \ne j$ is guaranteed.

Under this condition:
- The write `a[j] = y` cannot alter `a[i]`.
- The load `a[i]` at statement (3) is an identical subexpression to statement (1).
- Node $N_3$ is eliminated, and label `z` is attached directly to Node $N_1$:

```mermaid
graph TD
    N1_opt["N1: =[] (Labels: { x, z })"] --- a0
    N1_opt --- i0
    N2_opt["N2: []="] --- a0
    N2_opt --- j0
    N2_opt --- y0
```

### Reassembled Optimized Code (when $i \ne j$ is guaranteed):
```
x = a[i]
a[j] = y
z = x
```

---

## What to carry forward

Give the equal-index counterexample before the optimization. It establishes why the general replacement fails. Then state the non-aliasing proof needed for the restricted case, rather than assuming different variable names imply different addresses.

## Related notes

- [[DAG Construction and Local Optimization of Basic Blocks]]

## Source

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Chapter 8 (Slides 341–343).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 8.5.2.
