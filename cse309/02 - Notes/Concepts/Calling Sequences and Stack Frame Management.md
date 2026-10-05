---
type: concept
course: cse309
status: active
order: 22
---

# Calling Sequences and Stack Frame Management

> 📖 **Reading Order:** Step 22 of 55 | **Module 3:** Run-Time Environments  
> ◄ **Previous:** [[Run-Time Storage Organization and Activation Records]] | ► **Next:** [[Non-Local Variable Access in Static and Dynamic Scopes]]

---

## Building the idea

A call must let the callee begin without losing what the caller needs afterward. The **calling sequence** is the agreement about arguments, return values, saved registers, frame layout, and control transfer.

Follow an illustrative stack-based call. The caller supplies arguments and preserves any live caller-saved registers. The callee establishes its frame, saves registers it must preserve, and reserves local storage. On return it restores those obligations, removes its frame, and transfers control to the saved return location.

The stack pointer tracks changing stack extent; an optional frame pointer provides a stable reference for frame fields. [[Run-Time Storage Organization and Activation Records]] explains why those fields belong to one activation rather than one source function.

Specific registers in the assembly below describe an ABI example, such as System V AMD64, not every 64-bit system. An ABI tells the caller and callee what they can rely on; hardware instructions alone do not define the entire convention.

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

## What to carry forward

A frame pointer may be omitted when code can address the frame another way. [[Non-Local Variable Access in Static and Dynamic Scopes]] introduces the separate problem of locating an enclosing activation's variables.

## Related notes

- [[Run-Time Storage Organization and Activation Records]]
- [[Non-Local Variable Access in Static and Dynamic Scopes]]

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Chapter 7 (Slides 222–230).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 7.2.2–7.2.4 (Calling Sequences).
