---
type: concept
course: cse313
status: active
order: 4
---
# Process Concepts and Memory Layout

> 📖 **Reading Order:** Step 04 of 68 | **Module 2:** Processes & Threads  
> ◄ **Previous:** [[Computer Booting and Hardware Abstractions]] | ► **Next:** [[Process Lifecycle and State Transitions]]

---
## Starting Point and the Problem

On storage media, a computer program is merely an inert, passive sequence of bytes: compiled machine code instructions and static data constants stored inside an ELF or PE binary file.

We want the CPU to execute this program, track its dynamic variables as they change during execution, maintain its function call history, and allow multiple instances of the same program to run simultaneously without interfering with one another. The central obstacle is that a static binary has no runtime state: it has no program counter, no dynamic stack frames, and no allocated heap.

---
## Developing the Idea

To bridge this gap, the operating system creates the **Process** abstraction: an active instance of a program in execution.

A process encapsulates both the static program and its complete dynamic operational environment:
- As Tanenbaum's cake baking analogy illustrates: the recipe is the program, the baker is the CPU, the ingredients are the input data, and the activity of mixing and baking is the process.
- To prevent conflicts, the OS assigns each process a private, continuous **Virtual Address Space** partitioned into distinct logical segments: Text (read-only code), Initialized Data, Uninitialized Data (BSS), Heap (growing dynamically upward), and Stack (growing dynamically downward).

---
## Definition

A **process** is a **program in execution**. It is the fundamental unit of computation and resource allocation in an operating system.

While a **program** is a passive, inert collection of instructions stored on disk (an executable binary such as `/bin/ls` or `firefox.exe`), a **process** is an active, dynamic entity that possesses:
- An allocated address space in physical/virtual memory.
- An execution context, including a Program Counter (PC), CPU registers, and call stack.
- Dedicated operating system resources (open file descriptors, network sockets, child process references).

Multiple distinct processes can run instances of the same underlying program simultaneously (e.g., opening three separate terminal windows or browser tabs running the same binary).

---
## How It Works

The mechanism operates through coordinated hardware execution and operating system kernel protocols.

---
## Example

Consider running two separate terminal windows each executing `./my_program`:
- Both processes share the exact same physical memory frames for their read-only Text segment (code instructions).
- Each process has completely independent physical memory frames for their Data, Heap, and Stack segments.
- If Process 1 modifies variable `x = 100`, Process 2 still reads `x = 0`. Each operates within its own private address space.

---
## Technical Details

### Stack vs. Heap: Critical Comparison

| Dimension | Stack Segment | Heap Segment |
|---|---|---|
| **Allocation Mechanism** | Automatic by compiler instructions (`sub esp, N`) | Explicit by programmer (`malloc()`, `free()`) |
| **Growth Direction** | Grows **downward** (toward lower addresses) | Grows **upward** (toward higher addresses) |
| **Deallocation** | Automatic on function return | Manual (or via garbage collection); risk of memory leaks |
| **Access Speed** | Blazing fast (contiguous, cached in L1/L2) | Slower (pointer dereferencing, fragmentation) |
| **Size Limit** | Fixed default limit (e.g., 8 MB in Linux; exceeds $\to$ **Stack Overflow**) | Bounded only by available virtual memory and swap space |

---
## Important Properties and Why They Hold

- **Address Space Isolation:** Memory protection hardware (MMU page tables) ensures that Process $A$ cannot read or alter memory in Process $B$ without explicit shared-memory IPC primitives.
- **Stack-Heap Separation:** The stack grows downward toward lower memory addresses with every function call; the heap grows upward via `brk()` / `sbrk()` or `mmap()`. Collision between them results in out-of-memory errors or stack overflow exceptions.
- **Reentrancy of Code:** The text segment is marked execute-only and read-only, allowing multiple concurrent processes to safely share the same physical code frames.

---
## Common Mistakes

1. **Stack Overflow:**
   - Caused by infinite or deeply nested recursion, or allocating large arrays locally on the stack (e.g., `char huge[10000000];` inside a function).
   - The stack pointer collides with the guard page or heap, generating a segmentation fault (`SIGSEGV`).
2. **Buffer Overflow Attacks:**
   - Writing beyond the bounds of a stack-allocated buffer can overwrite the saved function return address, hijacking the CPU's Program Counter to execute malicious code (mitigated by stack canaries, ASLR, and non-executable stack bits).
3. **Memory Leaks and Dangling Pointers:**
   - Failure to `free()` heap memory exhausts available virtual addresses over time.
   - Accessing heap memory after calling `free()` leads to undefined behavior.

---
## Exam Relevance

- **Next Step:** As a process executes through its memory segments, how does its status change between waiting for I/O and running on the CPU? (See [[Process Lifecycle and State Transitions]]).
- **Process Context:** The hardware registers and segment pointers are tracked inside the [[Process Control Block and Context Switching]].
- **Process Duplication:** When `fork()` is called, how are these memory segments replicated? (See [[Process Creation and Termination Operations]] and [[Process Forking and Zombie Orphan Example]]).
- **Exam Testing:** Standard exam questions include:
  - Drawing and labeling the 5 segments of a process memory layout from low to high memory.
  - Identifying which segment a given variable resides in (e.g., global, static, local, or dynamically allocated).

---
## Related Concepts

- [[Process Lifecycle and State Transitions]]
- [[Process Control Block and Context Switching]]
- [[Process Creation and Termination Operations]]
- [[Threads and Multithreading Models]]

---
## Prerequisites

- [[Computer Booting and Hardware Abstractions]]
- [[Operating System Structures and Functions]]

---
## Problems

- [[Problem — Fork Execution Tree and Process Tracing]]

---
## Sources

- **Lectures:** `cse313/01 - Sources/Lectures/2. ProcessAndThread-week2-RRR.pdf` (Slides 1–7)
- **Textbook:** Andrew S. Tanenbaum & Herbert Bos, *Modern Operating Systems* (4th Edition), Chapter 2: Processes and Threads (Section 2.1)
