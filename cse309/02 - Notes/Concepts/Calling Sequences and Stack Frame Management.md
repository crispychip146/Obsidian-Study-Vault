---
type: concept
course: cse309
status: active
order: 22
---

# Calling Sequences and Stack Frame Management

> 📖 **Reading Order:** Step 22 of 55 | **Module 3: Run-Time Environments**  
> ◄ **Previous:** [[Run-Time Storage Organization and Activation Records]] | ► **Next:** [[Non-Local Variable Access in Static and Dynamic Scopes]]

---

## Starting Point and the Problem

In any software ecosystem, a function written in C must be able to call a function compiled from Rust, C++, or raw assembly language. How can two independent pieces of code communicate reliably without clobbering each other's hardware registers or misinterpreting stack data?

The answer is the **Application Binary Interface (ABI)** (such as the System V AMD64 ABI or the classic x86 `cdecl` convention).

A calling sequence is a meticulously synchronized protocol between the **Caller** (the procedure issuing the call) and the **Callee** (the procedure being invoked). The compiler must divide the responsibilities:
- Who evaluates arguments?
- Who allocates the stack frame?
- Who preserves CPU registers?
- Who cleans up memory upon return?

```
                     The Calling Protocol Contract
           CALLER                                CALLEE
     ┌──────────────────────┐              ┌──────────────────────┐
     │ Evaluates arguments  │ ──CALL──>    │ Pushes old $fp       │
     │ Saves caller-saved   │              │ Sets new $fp = $sp   │
     │ registers            │              │ Saves callee-saved   │
     │ Pushes return addr   │              │ Allocates locals     │
     └──────────────────────┘              └──────────────────────┘
                 ▲                                     │
                 │                                   EXECUTE
                 │                                     │
     ┌──────────────────────┐              ┌──────────────────────┐
     │ Retrieves return val │ <──RET────── │ Places return value  │
     │ Restores caller-saved│              │ Restores callee-saved│
     │ Cleans up arguments  │              │ Pops frame pointer   │
     └──────────────────────┘              └──────────────────────┘
```

---

## Developing the Idea

Modern CPUs have limited general-purpose hardware registers (e.g., 16 in x86-64, 32 in ARM64). Saving all registers to RAM on every function call would destroy CPU performance, as memory writes take dozens of clock cycles.

To maximize execution speed, ABIs divide hardware registers into two economic categories:

| Category | Typical Registers (x86-64) | Ownership & Invariant Rule | Compiler Strategy |
| :--- | :--- | :--- | :--- |
| **Caller-Saved** *(Volatile / Scratch)* | `%rax`, `%rcx`, `%rdx`, `%rsi`, `%rdi`, `%r8`–`%r11` | **The Caller owns them.** The Callee is free to overwrite and destroy them without saving! | Used for short-lived, transient expressions. If the Caller needs the value across a call, the **Caller** must push it to the stack before `CALL`. |
| **Callee-Saved** *(Non-Volatile / Preserved)* | `%rbx`, `%rsp`, `%rbp`, `%r12`–`%r15` | **The Callee must preserve them.** When the Callee returns, these registers **must** contain their exact original values! | Used for long-lived loop counters and persistent variables. If the Callee wants to use `%r12`, the **Callee** must push `%r12` in its prologue and pop it in its epilogue. |

> [!TIP] Why This Split Saves Thousands of Instructions
> Consider a function $A$ that calls 10 tiny leaf functions in a loop.
> - If all registers were *caller-saved*, function $A$ would have to execute 10 separate save-and-restore memory operations around every single call!
> - By placing $A$'s loop counter in a *callee-saved* register (e.g., `%r12`), the 10 leaf functions simply use caller-saved registers and never touch `%r12`. Function $A$'s variable remains untouched in silicon without a single RAM spill!

---

## Definition

**Calling Sequences and Stack Frame Management** is a formal compiler mechanism that structures syntax-directed translation, intermediate representations, runtime environments, or code generation.

---

## How It Works

### Concrete Silicon Execution: Prologue, Body, and Epilogue

Let us trace the physical assembly instructions executed on a downward-growing stack:

### 3.1 The Caller Sequence (Before `CALL`)
1. **Pass Arguments:**
   - In modern 64-bit systems, the first 6 arguments are placed directly into registers: `%rdi`, `%rsi`, `%rdx`, `%rcx`, `%r8`, `%r9`.
   - Arguments beyond the 6th are pushed onto the memory stack in right-to-left order.
2. **Save Live Caller-Saved Registers:**
   - Any scratch registers currently holding live variables are pushed onto the stack.
3. **Issue `CALL` Instruction:**
   - The hardware pushes the Program Counter (Return Address) onto the stack and jumps to the callee's label:
     ```assembly
     call callee_function
     ```

---

### 3.2 The Callee Prologue (Entering the Function)
The very first instructions executed inside the callee establish its activation record:
```assembly
pushq   %rbp            # 1. Save caller's frame pointer (Dynamic Link!)
movq    %rsp, %rbp      # 2. Anchor new Frame Pointer at current stack top
pushq   %r12            # 3. Save any callee-saved registers to be used
pushq   %r13
subq    $32, %rsp       # 4. Allocate 32 bytes for local variables and temporaries
```

At this moment, the new stack frame is established:
- Locals are accessed as negative offsets: `[rbp - 8]`, `[rbp - 16]`.
- Stack arguments (if $>6$) are accessed as positive offsets: `[rbp + 16]`, `[rbp + 24]`.

---

### 3.3 The Callee Epilogue (Exiting the Function)
When the function finishes, it dismantles its stack frame cleanly:
```assembly
movq    %rdi, %rax      # 1. Place return value into designated register (%rax)
addq    $32, %rsp       # 2. Deallocate local variable space (or movq %rbp, %rsp)
popq    %r13            # 3. Restore preserved callee-saved registers
popq    %r12
popq    %rbp            # 4. Restore caller's original Frame Pointer!
ret                     # 5. Pop return address into PC and return to caller!
```

> [!NOTE] The x86 `leave` Instruction
> The two operations `movq %rbp, %rsp` followed by `popq %rbp` are so ubiquitous that the x86 instruction set provides a dedicated single-byte hardware instruction: `leave`.

---

### 3.4 The Caller Cleanup (After `RET`)
1. **Retrieve Result:** The caller reads the function's return value directly from `%rax`.
2. **Pop Overflow Arguments:** In caller-cleanup conventions (like C `cdecl`), the caller increments `%rsp` to discard parameters pushed for the call.
3. **Restore Caller-Saved Registers:** Saved registers are popped back into the CPU.

---

### Technical Details

### When LIFO Fails: Closures and Escaping Frames

The entire stack allocation model relies on a strict invariant: **A function's activation record is never accessed after the function returns.**

What happens in modern functional and scripting languages (JavaScript, Python, Swift, Go, Scheme) that support **Closures (Upward Funargs)**?

```javascript
function makeCounter() {
    let count = 0; // Local variable in makeCounter's activation record
    return function() {
        count++;   // Inner closure references enclosing local variable!
        return count;
    };
}

let counter = makeCounter(); // makeCounter returns!
counter(); // Returns 1
counter(); // Returns 2
```

### Formal Proof: Impossibility of Pure Stack Allocation for Closures

#### Theorem:
*In a language supporting first-class functions that capture lexical variables (closures), allocating all activation records on a LIFO stack leads to memory unsafety (dangling pointer dereference).*

#### Proof:
1. Let procedure $P$ declare local variable $x$ within its activation record $AR_P$.
2. Let $P$ define an inner function $F$ that references $x$, and let $P$ return $F$ as a first-class value to caller $C$.
3. When $P$ returns to $C$, if activation records adhere strictly to LIFO stack discipline, $AR_P$ is deallocated, and the stack pointer is adjusted:
   $$\text{Active Stack} = \text{Active Stack} \setminus \{ AR_P \}$$
4. Caller $C$ subsequently invokes the closure $F$.
5. The execution of $F$ attempts to read/write variable $x$ via its captured lexical environment pointer (which references memory within $AR_P$).
6. However, memory region $AR_P$ has been deallocated and may be overwritten by subsequent function calls (e.g., $C$ calling procedure $Q$).
7. Dereferencing this address is an undefined memory access (dangling pointer) resulting in memory corruption or arbitrary code execution.
8. Therefore, any activation record containing variables captured by an escaping closure **cannot** be deallocated upon function return and cannot reside on a pure LIFO stack. $\blacksquare$

### Language Implementation Solutions:
1. **Heap Allocation of Activation Records:** Languages like Scheme, Python, and JavaScript allocate activation records directly on the **garbage-collected heap**. Frames persist as long as reachable from any active closure.
2. **Variable Capture Promotion (Go, Swift, Rust):** The compiler performs **Escape Analysis**. If a local variable does not escape, it is kept on the fast CPU stack. If it is captured by an escaping closure, only that specific variable is "promoted" to a heap-allocated box!

---

## Example

Detailed walkthroughs and traces are provided in the corresponding example and problem notes.

---

## Technical Details

Target architecture and ABI specifications govern low-level alignment and register assignments.

---

## Important Properties and Why They Hold

- **Semantic Soundness:** Preserves program execution equivalence.
- **Algorithmic Efficiency:** Operates in low polynomial or linear time over the program structure.

---

## Common Mistakes

- Confusing syntactic validity with semantic correctness.
- Overlooking variable scoping or memory aliasing side effects.

---

## Exam Relevance

Tested regularly in compiler examinations via syntax-directed translation proofs, activation record diagrams, and control flow optimization problems.

---

## Related Concepts

- [[Run-Time Storage Organization and Activation Records]]
- [[Non-Local Variable Access in Static and Dynamic Scopes]]

---

## Prerequisites

- [[Run-Time Storage Organization and Activation Records]]

---

## Problems

- [[Problem — Activation Record and Display Table Tracing]]

---

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Chapter 7 (Slides 222–230).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 7.2.2–7.2.4 (Calling Sequences).
