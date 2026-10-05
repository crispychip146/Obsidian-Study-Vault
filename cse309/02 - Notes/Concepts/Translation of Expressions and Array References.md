---
type: concept
course: cse309
status: active
order: 14
---

# Translation of Expressions and Array References

> 📖 **Reading Order:** Step 14 of 55 | **Module 2:** Intermediate Code Generation  
> ◄ **Previous:** [[Multi-Dimensional Array Addressing Formulas]] | ► **Next:** [[Control Flow Translation and Boolean Expressions]]

---

## Building the idea

Expression translation answers where the result will be available. For a scalar addition, translate both operands, create a temporary for the sum, and emit an instruction referring to their resulting locations.

An array reference adds a second task: compute the element's location. [[Multi-Dimensional Array Addressing Formulas]] supplies the offset calculation. Reading `x=a[i]` then loads from that location; writing `a[i]=x` stores to it. The address calculation can be shared, but a value and an address are different results.

In an illustrative four-byte integer array, `t1=4*i` counts byte displacement. `t2=a[t1]` in the classroom TAC notation loads the element at that displacement. Do not multiply by four again if the operand already represents bytes.

[[Intermediate Representations and Three-Address Code]] makes these steps explicit. The attribute named `.addr` can denote an IR result name rather than a physical pointer, so read its definition before interpreting the generated code.

## How It Works

### SDD for Arithmetic Expressions

For pure arithmetic, translation synthesizes temporaries bottom-up:

| Production | Semantic Rules | Physical Operation |
| :--- | :--- | :--- |
| $S \longrightarrow \mathbf{id} = E;$ | $\text{gen}(\mathbf{id}.entry \text{ '=' } E.addr);$ | Writes computed temporary into variable's storage. |
| $E \longrightarrow E_1 + E_2$ | $E.addr = \text{newtemp}();$<br>$\text{gen}(E.addr \text{ '=' } E_1.addr \text{ '+' } E_2.addr);$ | Allocates temporary, emits binary addition. |
| $E \longrightarrow - E_1$ | $E.addr = \text{newtemp}();$<br>$\text{gen}(E.addr \text{ '= minus ' } E_1.addr);$ | Emits unary negation. |
| $E \longrightarrow ( E_1 )$ | $E.addr = E_1.addr;$ | Passes through inner address (zero code emitted). |
| $E \longrightarrow \mathbf{id}$ | $E.addr = \mathbf{id}.entry;$ | Returns variable's symbol table address. |
| $E \longrightarrow \mathbf{num}$ | $E.addr = \mathbf{num}.val;$ | Returns literal constant. |

---
### SDD for Multi-Dimensional Array Addressing

To correctly implement the Row-Major addressing formula proved in [[Multi-Dimensional Array Addressing Formulas]], the compiler uses a dedicated non-terminal $L$ with three synthesized attributes:
- `L.array`: Base identifier or symbol table entry of the array.
- `L.type`: The type of the subarray remaining to be indexed.
- `L.addr`: A temporary variable holding the accumulated **byte offset** computed so far.

```
Production                   Semantic Rules
-----------------------------------------------------------------------------------------------------------------------
S -> L = E ;                 /* L-Value Context (Array Store): L[offset] = E */
                             gen(L.array.base '[' L.addr ']' '=' E.addr);

S -> id = E ;                /* Simple Variable Assignment */
                             gen(id.entry '=' E.addr);

E -> L                       /* R-Value Context (Array Load): temp = L[offset] */
                             E.addr = newtemp();
                             gen(E.addr '=' L.array.base '[' L.addr ']');

L -> id [ E ]                /* Base 1st Dimension Index: A[i1] */
                             L.array = id.entry;
                             L.type  = id.type.elem;             /* Elem type after 1st dim */
                             L.addr  = newtemp();
                             gen(L.addr '=' E.addr '*' L.type.width);

L -> L1 [ E ]                /* Inductive Multi-Dimensional Step: A[i1]...[ik] */
                             L.array = L1.array;
                             L.type  = L1.type.elem;            /* Elem type after next dim */
                             t       = newtemp();
                             L.addr  = newtemp();
                             gen(t '=' E.addr '*' L.type.width);
                             gen(L.addr '=' L1.addr '+' t);
```

---
### Type Coercions in Expressions: Widening and Narrowing

Real-world languages permit mixed-mode arithmetic (e.g., `float x; int y; x = x + y;`). The CPU ALU cannot add an integer directly to a floating-point register without hardware conversion.

Compilers define a **Type Hierarchy (Lattice)**:
$$\mathbf{char} \subset \mathbf{int} \subset \mathbf{long} \subset \mathbf{float} \subset \mathbf{double}$$

```mermaid
graph BT
    Char["char (1 byte)"] --> Int["int (4 bytes)"]
    Int --> Long["long (8 bytes)"]
    Long --> Float["float (4 bytes IEEE)"]
    Float --> Double["double (8 bytes IEEE)"]
```

### Widening vs. Narrowing:
1. **Widening Conversions (Coercions):**
   - Movement **up** the hierarchy (e.g., $\mathbf{int} \to \mathbf{float}$).
   - Safe and value-preserving. Compilers insert these conversions automatically:
     ```text
     t1 = intToFloat y
     t2 = x +float t1
     ```
2. **Narrowing Conversions:**
   - Movement **down** the hierarchy (e.g., $\mathbf{float} \to \mathbf{int}$).
   - Potentially lossy (truncates fractional digits or causes integer overflow). Languages typically require an explicit programmer cast (`(int)x`).

---

### Mathematical Equivalence: How the SDD Implements Horner's Rule

Why do the rules for $L \to L_1 [ E ]$ generate the exact row-major byte offset?

### The Recurrence Proof:
Recall from [[Multi-Dimensional Array Addressing Formulas]] that for an array $A[d_1][d_2]\dots[d_k]$ of elements of width $w$:
$$\text{Offset}(i_1, i_2, \dots, i_k) = \sum_{m=1}^k \left( i_m \times \prod_{j=m+1}^k d_j \right) \times w$$
Notice what the type system gives us at each level of grammar reduction:
1. When indexing dimension $m$, the remaining type is $T_m = \mathbf{array}(d_{m+1}, \mathbf{array}(\dots, \mathbf{elem}))$.
2. The width of an element of this type is:
   $$\text{width}(T_m) = d_{m+1} \times d_{m+2} \times \dots \times d_k \times w = \left( \prod_{j=m+1}^k d_j \right) \times w$$
3. When the grammar executes $L \to L_1 [ E ]$:
   - $E.addr = i_m$.
   - It computes: $t = i_m \times \text{width}(T_m) = i_m \times \left( \prod_{j=m+1}^k d_j \right) \times w$.
   - It accumulates: $L.addr = L_1.addr + t$.
4. By finite induction over dimensions $1 \le m \le k$:
   $$L.addr = \sum_{m=1}^k \left( i_m \times \text{width}(T_m) \right) = \text{Exact Row-Major Byte Offset!}$$
The grammar calculates Horner's polynomial accumulation incrementally during bottom-up parsing without needing a loop!

---

## Exam Relevance

---

### Comprehensive Trace: $x = a[i][j]$ vs. $a[i][j] = x$

Assume array `a` is declared as: `int a[10][20];` (where $\text{width}(\mathbf{int}) = 4$).
- Type of `a`: $\mathbf{array}(10, \mathbf{array}(20, \mathbf{int}))$.
- Subarray type after 1st index: $\mathbf{array}(20, \mathbf{int})$, width $= 20 \times 4 = 80$ bytes.
- Element type after 2nd index: $\mathbf{int}$, width $= 4$ bytes.

### Scenario A: R-Value Load ($x = a[i][j]$)

```mermaid
sequenceDiagram
    autonumber
    participant Parser as Parser Actions
    participant TAC as Emitted Three-Address Code

    Parser->>TAC: Parse a[i] (L1 -> id[E])
    Note over TAC: L1.type.width = 80 bytes
    TAC-->>TAC: t1 = i * 80
    Parser->>TAC: Parse a[i][j] (L -> L1[E])
    Note over TAC: L.type.width = 4 bytes
    TAC-->>TAC: t2 = j * 4
    TAC-->>TAC: t3 = t1 + t2
    Parser->>TAC: Context is RHS (E -> L)
    TAC-->>TAC: t4 = a[t3] (MEMORY LOAD)
    Parser->>TAC: Statement Assignment (S -> x = E)
    TAC-->>TAC: x = t4
```

### Scenario B: L-Value Store ($a[i][j] = x$)
Notice what happens when the exact same expression $a[i][j]$ appears on the **left-hand side**:
1. $L_1 \to a[i]$ emits: `t1 = i * 80`.
2. $L \to L_1[j]$ emits:
   ```text
   t2 = j * 4
   t3 = t1 + t2
   ```
3. Now, the statement production matches $S \to L = E;$:
   Instead of emitting a load, it emits an **indexed memory store**:
   ```text
   a[t3] = x
   ```
Look at the elegance: The exact same sub-productions $L$ compute the byte offset `t3`. The enclosing context ($E \to L$ vs $S \to L = E$) determines whether the hardware issues an indexed load or an indexed store!

---

## What to carry forward

Track temporaries by purpose: operand value, byte offset, element address, or loaded element. [[Control Flow Translation and Boolean Expressions]] adds expressions whose useful result may be a branch destination rather than a stored Boolean.

## Related notes

- [[Multi-Dimensional Array Addressing Formulas]]
- [[Intermediate Representations and Three-Address Code]]
- [[Control Flow Translation and Boolean Expressions]]

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Chapter 6 (Slides 138–153).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 6.4 (Expressions) & Section 6.5 (Type Checking).
