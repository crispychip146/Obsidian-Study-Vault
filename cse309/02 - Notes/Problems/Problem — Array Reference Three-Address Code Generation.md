---
type: problem
course: cse309
status: active
order: 19
---

# Problem — Array Reference Three-Address Code Generation

> 📖 **Reading Order:** Step 19 of 55 | **Module 2:** Intermediate Code Generation  
> ◄ **Previous:** [[Array Reference and Boolean Control-Flow TAC Generation Example]] | ► **Next:** [[Problem — Backpatching Boolean Expression Translation]]

---

## Problem

A high-performance computing library defines a 3-dimensional data cube in C syntax as:
```c
int A[10][20][30];
```
The compiler targets a 32-bit architecture with the following system characteristics:
- Primitive integer width is 4 bytes ($w = 4$).
- Contiguous **Row-Major** memory layout.
- The base memory address of the array is labeled `base(A)`.
- Array dimensions use standard 0-based indexing ($0 \le i < 10$, $0 \le j < 20$, $0 \le k < 30$).

### Your Objectives:
1. **Geometric Derivation:** From first physical principles of computer memory, geometrically derive the exact byte address equation for element $A[i][j][k]$.
2. **Horner's Factorization:** Factor the multi-dimensional offset formula into an iterative recurrence using Horner's polynomial rule.
3. **Three-Address Code Generation:** Write the exact sequence of TAC instructions emitted by the compiler's Syntax-Directed Translation scheme for the statement:
   $$x = A[i][j][k] + y$$
4. **Intermediate Representation Tables:** Provide the full representation of the generated instructions as:
   - A **Quadruples Table**
   - A **Triples Table**
5. **Architectural Analysis:** Formally explain why the first dimension bound ($n_1 = 10$) **never appears** in the address calculation formula.

---

## Solution

For A[10][20][30], the last index counts individual integers. One step in j skips 30 integers; one step in i skips 20·30 integers. Therefore A[i][j][k] lies `(i*600+j*30+k)` elements after the base.

Each integer occupies four bytes, so the byte displacement is four times that element count. [[Multi-Dimensional Array Addressing Formulas]] compresses the reasoning to `4*((i*20+j)*30+k)`. The outer dimension 10 constrains i's range; it is not i's stride.

Translate the factored calculation into one operation per TAC instruction, then add the base or use the specified base-plus-offset array instruction. [[Intermediate Representations and Three-Address Code]] distinguishes a temporary name from its eventual physical storage.

For the quadruples table, preserve which temporary each operation produces and which later instruction consumes it. A numerically correct formula can still be translated incorrectly if a use points to the wrong temporary.

### Part 1: Geometric Physical Derivation

Computer memory is a flat, 1-dimensional array of bytes. A 3D array $A[n_1][n_2][n_3]$ is conceptually a book:
- $n_1 = 10$: Number of **pages (2D planes)**.
- $n_2 = 20$: Number of **rows** on each page.
- $n_3 = 30$: Number of **columns (elements)** per row.

```
       3D Array Geometric Layout: A[10][20][30]
       
          Plane i
        ┌──────────────┐
       /              /│
      /   Row j      / │
     ┌──────────────┐  │
     │ ■ ■ ■ ... ■  │  │  <-- 30 elements (k-th element)
     │ ■ ■ ■ ... ■  │  │
     │ ...          │  │
     │ 20 rows      │ /
     └──────────────┘/
```

To locate element $A[i][j][k]$ in memory relative to `base(A)`:
1. **Skip $i$ entire planes:**  
   Each plane contains $n_2 \times n_3 = 20 \times 30 = 600$ integer elements.  
   Skipped elements $= i \times 600$.
2. **Skip $j$ rows on plane $i$:**  
   Each row contains $n_3 = 30$ integer elements.  
   Skipped elements $= j \times 30$.
3. **Skip $k$ elements in row $j$:**  
   Offset within row $= k$ elements.

Summing all elements preceding $A[i][j][k]$:
$$\text{Element Offset}(i, j, k) = i \times (20 \times 30) + j \times 30 + k = 600i + 30j + k$$

Multiplying by the size of each element ($w = 4$ bytes):
$$\mathbf{\text{Byte Offset} = (600i + 30j + k) \times 4 = 2400i + 120j + 4k}$$
$$\mathbf{\text{Physical Address}(A[i][j][k]) = \text{base}(A) + 2400i + 120j + 4k}$$

---

### Part 2: Horner's Rule Factorization

Evaluating $2400i + 120j + 4k$ directly requires multiple large multiplications. Compilers factor this polynomial using Horner's nested form:
$$\text{Offset} = ((i \times n_2 + j) \times n_3 + k) \times w$$
$$\text{Offset} = ((i \times 20 + j) \times 30 + k) \times 4$$

This yields an iterative, inductive recurrence:
$$\begin{aligned}
\text{dim}_1: \quad t_1 &= i \\
\text{dim}_2: \quad t_2 &= t_1 \times 20 + j \\
\text{dim}_3: \quad t_3 &= t_2 \times 30 + k \\
\text{bytes}: \quad \text{offset} &= t_3 \times 4
\end{aligned}$$

---

### Part 3: Emitted Three-Address Code (TAC)

Applying the SDT from [[Translation of Expressions and Array References]]:

```text
t1 = i * 20        // Plane offset: i * n2
t2 = t1 + j        // Add row index j
t3 = t2 * 30       // Row offset: (i * 20 + j) * n3
t4 = t3 + k        // Add column index k (Total element count)
t5 = t4 * 4        // Multiply by element byte width w = 4
t6 = A[t5]         // Indexed memory load: R-value load of A[i][j][k]
t7 = t6 + y        // Arithmetic addition: A[i][j][k] + y
x = t7             // Variable store
```

---

### Part 4: Intermediate Representation Tables

#### A. Quadruples Table Representation
Every quadruple has 4 explicit fields: `(op, arg1, arg2, result)`:

| Quad # | Operator (`op`) | Argument 1 (`arg1`) | Argument 2 (`arg2`) | Result (`result`) |
| :---: | :---: | :---: | :---: | :---: |
| **(1)** | `*` | `i` | `20` | `t1` |
| **(2)** | `+` | `t1` | `j` | `t2` |
| **(3)** | `*` | `t2` | `30` | `t3` |
| **(4)** | `+` | `t3` | `k` | `t4` |
| **(5)** | `*` | `t4` | `4` | `t5` |
| **(6)** | `[]=` | `A` | `t5` | `t6` |
| **(7)** | `+` | `t6` | `y` | `t7` |
| **(8)** | `=` | `t7` | - | `x` |

#### B. Triples Table Representation
Triples avoid creating temporary names (`t1`, `t2`, $\dots$). Previous computations are referenced by instruction position `(k)`:

| Position | Operator (`op`) | Argument 1 (`arg1`) | Argument 2 (`arg2`) |
| :---: | :---: | :---: | :---: |
| **(0)** | `*` | `i` | `20` |
| **(1)** | `+` | `(0)` | `j` |
| **(2)** | `*` | `(1)` | `30` |
| **(3)** | `+` | `(2)` | `k` |
| **(4)** | `*` | `(3)` | `4` |
| **(5)** | `[]=` | `A` | `(4)` |
| **(6)** | `+` | `(5)` | `y` |
| **(7)** | `=` | `x` | `(6)` |

---

### Part 5: Architectural Insight: Why $n_1 = 10$ Never Appears

#### Formal Justification:
1. Let the array be $A[d_1][d_2]\dots[d_k]$.
2. The index $i_1$ denotes which sub-block along the first dimension is selected.
3. Once $i_1$ is chosen, how many elements are skipped? To jump past $i_1$ sub-blocks, we only need to know **how large each sub-block is**:
   $$\text{Size of Sub-block} = d_2 \times d_3 \times \dots \times d_k \times w$$
4. The total number of sub-blocks available ($d_1$) only dictates when an index is out of bounds ($0 \le i_1 < d_1$). It has **zero effect** on the spacing or byte offset between sub-blocks!
5. This is why in C function declarations, the first dimension may be omitted, but all subsequent dimensions MUST be specified:
   ```c
   void process_cube(int A[][20][30]); // VALID! The compiler can calculate all offsets.
   void invalid_cube(int A[10][][30]); // COMPILE ERROR! Cannot calculate row jumps.
   ```

---

## Common Mistakes

> [!CAUTION] The 1-Based Indexing Pitfall
> If an exam question specifies 1-based indexing ($1 \le i \le 10, 1 \le j \le 20, 1 \le k \le 30$) or custom bounds ($l_m \le i_m \le u_m$):
> - You MUST normalize each index by subtracting its lower bound: $(i - l_1), (j - l_2), (k - l_3)$.
> - Failing to subtract the lower bound shifts every memory access by an invalid constant base offset, resulting in total loss of marks!

---

## What to carry forward

Check A[0][0][0] at base, successive k values four bytes apart, and the j stride 120 bytes. Bounds checks and address calculation answer separate questions; an address formula alone does not validate indices.

## Related notes

- [[Multi-Dimensional Array Addressing Formulas]]
- [[Intermediate Representations and Three-Address Code]]

## Source

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Chapter 6 (Slides 146–152).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 6.4.3 (Addresses of Array Elements) & Section 6.2 (Quadruples/Triples).
