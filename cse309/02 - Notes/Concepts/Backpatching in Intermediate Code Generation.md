---
type: concept
course: cse309
status: active
order: 16
---

# Backpatching in Intermediate Code Generation

> 📖 **Reading Order:** Step 16 of 55 | **Module 2:** Intermediate Code Generation  
> ◄ **Previous:** [[Control Flow Translation and Boolean Expressions]] | ► **Next:** [[Backpatching Control-Flow Code Generation Algorithm]]

---

## Building the idea

When emitting a forward jump, the compiler may know **why** it jumps before knowing **where** its target will be placed. Backpatching records that unfinished connection instead of delaying all code generation.

A true list contains instruction positions whose targets must be the expression's true destination. A false list records the analogous false exits; a next list records a statement's unresolved continuations. These lists contain places to repair, not the target instructions themselves.

For `B1 && B2`, patch B1's true exits to B2's start. Combine their false exits because either can make the conjunction false. For OR, patch B1's false exits to B2 and combine true exits. [[Control Flow Translation and Boolean Expressions]] provides the semantic reason for each connection.

A marker captures `nextquad` at the precise boundary where a target begins. This replaces an unknown future address with a known instruction position as soon as the necessary code location exists.

## How It Works

### The Three Synthesized Attributes

Unlike the inherited label scheme described in [[Control Flow Translation and Boolean Expressions]], Backpatching operates **strictly using Synthesized Attributes**. Because information flows exclusively from children to parents, backpatching integrates seamlessly with standard bottom-up shift-reduce LR parsers (Yacc, Bison).

| Synthesized Attribute | Definition | Physical Role |
| :--- | :--- | :--- |
| `B.truelist` | A list of instruction indices (quads) | Jump instructions that must transfer control to the target when boolean expression $B$ evaluates to **true**. |
| `B.falselist` | A list of instruction indices (quads) | Jump instructions that must transfer control to the target when boolean expression $B$ evaluates to **false**. |
| `S.nextlist` | A list of instruction indices (quads) | Jump instructions that must transfer control to the statement immediately following statement $S$. |

---
### The Three Fundamental List Operations

The compiler maintains linked lists of instruction indices using three primitive operations:

1. **`makelist(i)`:**
   - Creates a new linked list containing exactly one instruction index: $[i]$.
   - Returns a pointer to this list.
   - Cost: $O(1)$.
2. **`merge(p1, p2)`:**
   - Concatenates the lists pointed to by $p_1$ and $p_2$.
   - Returns a pointer to the combined list.
   - Cost: $O(1)$ by maintaining pointers to both head and tail of each linked list.
3. **`backpatch(p, i)`:**
   - Traverses the entire linked list pointed to by $p$.
   - For every instruction index $k$ in the list, writes integer $i$ into the destination address field of intermediate instruction array `quad[k]`.
   - Cost: $O(|p|)$ where $|p|$ is the number of patched jumps.

> [!TIP] The In-Place Silicon Implementation
> How did early compilers implement backpatch lists without allocating heap memory? They used the uncompleted target fields in the instruction array itself as the linked list pointers!
> If instructions $100, 104, 108$ are all on `truelist`, `quad[108].target` holds $104$, and `quad[104].target` holds $100$. Backpatching simply walked this chain through the instruction memory!

---
### Marker Non-Terminals: Capturing Real-Time Instruction Indices

Because code emission happens in real time, the compiler must record the instruction index where a new sub-clause begins *before* the sub-clause is parsed.

This is accomplished by inserting **Marker Non-Terminals** into the context-free grammar:

### 5.1 The Address Marker $M$
$$M \longrightarrow \epsilon \quad \{ M.quad = nextquad; \}$$
- `nextquad` is the global counter indicating the index of the next instruction about to be generated.
- The $\epsilon$-reduction of $M$ snapshots the current instruction counter into synthesized attribute $M.quad$.
- In an LR parser, $M$ reduces the exact instant all preceding tokens have been consumed and before the succeeding tokens are shifted.

### 5.2 The Unconditional Jump Marker $N$
$$N \longrightarrow \epsilon \quad \{ N.nextlist = \text{makelist}(nextquad); \; \text{emit}(\text{'goto _'}); \}$$
- Used in `if-then-else` constructs.
- Emits an unconditional jump over the `else` block with an unfulfilled target, and packages that instruction index into `N.nextlist` so it can be merged with the statement exit list.

---
### Architectural Comparison: Multi-Pass vs. One-Pass Backpatching

| Dimension | Multi-Pass (Inherited Labels) | One-Pass Backpatching |
| :--- | :--- | :--- |
| **Parsing Model** | Requires tree-building and top-down attribute flow | Operates purely bottom-up with synthesized attributes |
| **Yacc/Bison Integration** | Complex; needs global state or stack manipulation | **Native fit**; standard semantic actions `\$\$.truelist` |
| **Memory Requirement** | $O(\text{Program AST Size})$ in RAM | **$O(1)$ tree memory**; streams instructions linearly |
| **Instruction Output** | Emits symbolic labels (`L1:`, `L2:`) requiring an assembler pass | Emits direct numeric instruction indices (`quads`) |

---

### Theorem:
*For any well-formed control flow program, the Backpatching algorithm guarantees that:*
1. *Every conditional and unconditional branch emitted with a blank destination is backpatched to a valid instruction quad before program code generation terminates.*
2. *Control transfers strictly to the instruction quads dictated by the programming language semantics.*

### Proof:
1. **Base Case (Relational Expressions):**
   - For $B \to E_1 \text{ relop } E_2$, two instructions are emitted at indices $q$ and $q+1$:
     $$q: \text{if } E_1 \text{ relop } E_2 \text{ goto } \underline{\quad} \quad (q \in B.truelist)$$
     $$q+1: \text{goto } \underline{\quad} \quad (q+1 \in B.falselist)$$
   - Neither instruction has a target. Both are added to $B$'s lists.
2. **Inductive Step for Expressions:**
   - For $B \to B_1 \text{ || } M B_2$:
     - $B_1.falselist$ is backpatched with $M.quad$ (the starting quad of $B_2$).
     - The true jumps of $B_1$ and $B_2$ are preserved and merged into $B.truelist$.
     - Any instruction in $B_1.falselist$ is resolved to the exact start of $B_2$, satisfying short-circuit OR semantics.
   - For $B \to B_1 \text{ \&\& } M B_2$:
     - $B_1.truelist$ is backpatched with $M.quad$ (the starting quad of $B_2$).
     - The false jumps of $B_1$ and $B_2$ are merged into $B.falselist$.
     - Any instruction in $B_1.truelist$ is resolved to the start of $B_2$, satisfying short-circuit AND semantics.
3. **Inductive Step for Statements:**
   - In $S \to \mathbf{if} ( B ) M S_1$:
     - $B.truelist$ is backpatched to $M.quad$ (the start of $S_1$).
     - $B.falselist$ and $S_1.nextlist$ are merged into $S.nextlist$.
   - In sequence $S \to S_1 M S_2$:
     - $S_1.nextlist$ is backpatched to $M.quad$ (the start of $S_2$).
     - $S.nextlist = S_2.nextlist$.
4. **Program Termination:**
   - At the root program level $P \to S$, $S.nextlist$ is backpatched to the final terminating instruction (or program exit quad $nextquad$).
   - Since every sublist is either:
     a) Immediately backpatched via an $M$ marker, or
     b) Merged into a parent list that is inductively backpatched by an enclosing statement,
     no instruction remains on an unresolved list.
5. Therefore, all forward branches are completely and correctly patched. $\blacksquare$

---

## What to carry forward

[[Backpatching Control-Flow Code Generation Algorithm]] turns these meanings into semantic actions. Maintain list ownership so each pending jump receives its intended target exactly once; patching the wrong list can produce valid-looking but incorrect control flow.

## Related notes

- [[Control Flow Translation and Boolean Expressions]]
- [[Backpatching Control-Flow Code Generation Algorithm]]

## Sources

In the early decades of computing (such as on the PDP-11 with 64 kilobytes of main memory) and continuing into modern ultra-fast Just-In-Time (JIT) compilers (like Google V8, LuaJIT, or WebAssembly baselines), memory is precious and throughput is paramount:
- A multi-pass compiler builds a giant Abstract Syntax Tree (AST) of the entire program in RAM, runs multiple analysis passes, and then decorates nodes with target labels.
- In contrast, a **single-pass compiler** reads source tokens, parses them, and streams intermediate instructions directly into a flat code array or disk buffer in real time—retaining almost zero tree state in RAM.

However, a single-pass compiler encounters a seemingly fatal roadblock when generating conditional branches: **The Forward-Reference Problem**.

Consider the simple construct:
```c
if (x < 100 || x > 200) {
    y = 1;
} else {
    y = 2;
}
```
Trace what happens as the parser scans left-to-right:
1. It sees `if x < 100`. If this is true, the CPU must jump to the `then` block (`y = 1`). But at this instant, the instruction address where `y = 1` will be placed **does not exist yet**!
2. If `x < 100` is false, it must jump to test `x > 200`.
3. If `x > 200` is false, it must jump to the `else` block (`y = 2`). Again, that instruction address is completely unknown!
4. At the end of the `then` block, the CPU must execute an unconditional jump bypassing the `else` block to reach the statement following the construct. That exit address is also unknown!

How can a single-pass compiler emit a branch instruction when it has no idea where that branch will lead?

---
- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Chapter 6 (Slides 186–193).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 6.7 (Backpatching).
