---
type: concept
course: cse313
status: active
order: 4
---

# Process Concepts and Memory Layout

> 📖 **Reading Order:** Step 04 of 34 | **Module 2:** Processes & Threads  
> ◄ **Previous:** [[Computer Booting and Hardware Abstractions]] | ► **Next:** [[Process Lifecycle and State Transitions]]

---

## Definition

A **process** is a **program in execution**. It is the fundamental unit of computation and resource allocation in an operating system.

While a **program** is a passive, inert collection of instructions stored on disk (an executable binary such as `/bin/ls` or `firefox.exe`), a **process** is an active, dynamic entity that possesses:
- An allocated address space in physical/virtual memory.
- An execution context, including a Program Counter (PC), CPU registers, and call stack.
- Dedicated operating system resources (open file descriptors, network sockets, child process references).

Multiple distinct processes can run instances of the same underlying program simultaneously (e.g., opening three separate terminal windows or browser tabs running the same binary).

---

## The Cake Baking Analogy (Tanenbaum)

To intuitively understand the separation between a program, a process, a processor, and input data, consider baking a birthday cake:
- **The Recipe:** The **program** (a static, written sequence of instructions).
- **The Ingredients:** The **input data** (flour, eggs, sugar, milk).
- **The Baker:** The **processor (CPU)** (the active physical agent executing instructions).
- **The Process:** The **entire dynamic activity** of the baker reading the recipe, mixing ingredients, heating the oven, and baking the cake over time.

*Analogy Extension (Preemption):* If the baker's child runs into the kitchen crying with a bee sting, the baker saves their place in the recipe (records current state / Program Counter), switches tasks to administer first aid (handles higher-priority interrupt / context switch), and then returns to the kitchen, restoring the saved recipe step to continue baking.

---

## Process Memory Layout (Address Space)

When an executable binary is loaded into memory, the operating system constructs a structured **Virtual Address Space** for the process. In a standard 32-bit or 64-bit architecture, the memory layout is organized into distinct segments:

```
High Memory Address (0xFFFFFFFF in 32-bit)
+---------------------------------------------------+
|               Kernel Space                        | (Protected: accessible only in Kernel Mode)
+---------------------------------------------------+
|               Environment Variables & CLI Args    | (argc, argv, envp)
+---------------------------------------------------+
|                       STACK                       | (Local variables, function frames)
|                         |                         |
|                         v (Grows DOWNWARD)        |
|                                                   |
|                         ^ (Grows UPWARD)          |
|                         |                         |
|                       HEAP                        | (Dynamic memory: malloc, calloc, new)
+---------------------------------------------------+
|               BSS Segment                         | (Uninitialized global & static variables; zeroed)
+---------------------------------------------------+
|               DATA Segment                        | (Initialized global & static variables)
+---------------------------------------------------+
|               TEXT (CODE) Segment                 | (Read-Only machine instructions)
+---------------------------------------------------+
Low Memory Address (0x00000000)
```

### Detailed Segment Breakdown:

1. **Text (Code) Segment:**
   - Contains the compiled executable machine code instructions.
   - **Protection:** Marked **Read-Only** by the MMU. Attempting to write to the text segment generates a segmentation fault.
   - **Sharability:** If multiple processes run the same program (e.g., multiple instances of `bash`), they share a single physical copy of the text segment in RAM, conserving physical memory.
2. **Initialized Data Segment:**
   - Stores global variables and static local variables that have an explicit initial non-zero value specified by the programmer (e.g., `int max_users = 100;`).
3. **Uninitialized Data Segment (BSS — Block Started by Symbol):**
   - Stores global and static variables that are uninitialized or initialized to zero (e.g., `int buffer[1024];`).
   - Does not occupy physical space inside the executable file on disk; the OS simply zeroes out this memory block during process loading.
4. **Heap Segment:**
   - Used for dynamic memory allocation at runtime via standard library calls (`malloc()`, `calloc()`, `realloc()`, or C++ `new`).
   - Managed via kernel system calls `brk()` and `sbrk()`, or memory-mapping `mmap()`.
   - **Grows upward** toward higher memory addresses.
5. **Stack Segment:**
   - Manages automatic storage: function call frames (activation records), function parameters, return addresses, and local variables.
   - **Grows downward** toward lower memory addresses.
   - Every time a function is invoked, a new stack frame is pushed; when the function returns, its frame is popped.

---

## Stack vs. Heap: Critical Comparison

| Dimension | Stack Segment | Heap Segment |
|---|---|---|
| **Allocation Mechanism** | Automatic by compiler instructions (`sub esp, N`) | Explicit by programmer (`malloc()`, `free()`) |
| **Growth Direction** | Grows **downward** (toward lower addresses) | Grows **upward** (toward higher addresses) |
| **Deallocation** | Automatic on function return | Manual (or via garbage collection); risk of memory leaks |
| **Access Speed** | Blazing fast (contiguous, cached in L1/L2) | Slower (pointer dereferencing, fragmentation) |
| **Size Limit** | Fixed default limit (e.g., 8 MB in Linux; exceeds $\to$ **Stack Overflow**) | Bounded only by available virtual memory and swap space |

---

## Edge Cases & Common Pitfalls

1. **Stack Overflow:**
   - Caused by infinite or deeply nested recursion, or allocating large arrays locally on the stack (e.g., `char huge[10000000];` inside a function).
   - The stack pointer collides with the guard page or heap, generating a segmentation fault (`SIGSEGV`).
2. **Buffer Overflow Attacks:**
   - Writing beyond the bounds of a stack-allocated buffer can overwrite the saved function return address, hijacking the CPU's Program Counter to execute malicious code (mitigated by stack canaries, ASLR, and non-executable stack bits).
3. **Memory Leaks and Dangling Pointers:**
   - Failure to `free()` heap memory exhausts available virtual addresses over time.
   - Accessing heap memory after calling `free()` leads to undefined behavior.

---

## Cross-Topic Connections / Exam Relevance

- **Next Step:** As a process executes through its memory segments, how does its status change between waiting for I/O and running on the CPU? (See [[Process Lifecycle and State Transitions]]).
- **Process Context:** The hardware registers and segment pointers are tracked inside the [[Process Control Block and Context Switching]].
- **Process Duplication:** When `fork()` is called, how are these memory segments replicated? (See [[Process Creation and Termination Operations]] and [[Process Forking and Zombie Orphan Example]]).
- **Exam Testing:** Standard exam questions include:
  - Drawing and labeling the 5 segments of a process memory layout from low to high memory.
  - Identifying which segment a given variable resides in (e.g., global, static, local, or dynamically allocated).

---

## Sources & Traceability

- **Lectures:** `cse313/01 - Sources/Lectures/2. ProcessAndThread-week2-RRR.pdf` (Slides 1–7)
- **Textbook:** Andrew S. Tanenbaum & Herbert Bos, *Modern Operating Systems* (4th Edition), Chapter 2: Processes and Threads (Section 2.1)
