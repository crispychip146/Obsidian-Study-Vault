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

## Building the idea

Suppose you start the same executable twice. The instructions can be identical, yet one instance may be reading input while the other is calculating. They need separate current values, separate call histories, and separate positions in the instructions. These active instances are **processes**.

A process combines an address space with an execution context and OS-managed resources. Its code tells the CPU what operations are possible. Its program counter and registers tell us where this particular execution has reached. Its stack records active function calls; its heap holds dynamically allocated objects; its data regions hold longer-lived program data.

Here is an illustrative way to place variables: a global initialized integer belongs to initialized data; a zero-initialized global belongs to BSS; a function's automatic local often occupies its stack frame; an object obtained through `malloc` belongs to a heap allocation. Optimizing compilers can keep a local in a register, so source-level lifetime does not guarantee a particular address.

Private virtual address spaces let two processes use the same numeric address for different data. The protection and translation machinery from [[Dual-Mode Operation and System Calls]] supplies the separation. Physical pages can still be shared intentionally or shared until a copy-on-write update occurs.

## What a process owns

A process is a running program instance with an address space, execution state, and managed resources. A program file can be used by several processes; each execution may be at a different instruction with different data.

The following is a conventional layout, not a promise about exact addresses or growth directions:

| Region | Typical contents | Lifetime and purpose |
|---|---|---|
| Text | Machine instructions | Provides the executable program; permissions usually prohibit ordinary writes. |
| Initialized data | Initialized globals and static objects | Retains program-wide state. |
| BSS | Zero-initialized globals and static objects | Receives zeroed storage without storing every zero in the executable file. |
| Heap/dynamic regions | Allocated objects | Lifetime follows the allocator or runtime's rules rather than function return. |
| Stack | Active call frames, saved state, some automatic locals | Follows nested calls; a multithreaded process usually has a stack per thread. |

A compiler may keep a local variable in a register or optimize it away. Memory-mapped files, shared libraries, and other mappings also occupy address space beyond this simplified picture.

## Stack and heap costs

Stack allocation often adjusts a pointer, while dynamic allocation manages storage with a runtime allocator. This can make their **allocation** costs differ. Access to an object is not intrinsically faster just because it is on a stack: locality, caching, indirection, and generated instructions determine the cost.

Common diagrams show a downward-growing stack and an upward-growing heap. Actual platforms may use other layouts, multiple dynamic regions, randomization, guard pages, and explicit resource limits. There is no universal eight-megabyte stack limit.

## Sharing without losing isolation

Virtual memory can map the same numeric address to different physical pages in different processes. Read-only code pages may be shared. After `fork`, writable pages can also remain physically shared through copy-on-write until a modification requires a private copy. Explicit shared mappings are another case.

Thus private writable **behavior** does not require every page to be physically separate at every moment. Modifying an ordinary private variable in one process does not modify the corresponding variable in another.

## Failure cases to distinguish

Deep recursion can exhaust stack capacity. A buffer overrun accesses beyond an object's bounds and may corrupt data or violate memory protection. A memory leak retains allocations longer than intended; a dangling pointer refers to storage whose lifetime has ended. These are different errors even though all involve memory.

## What to carry forward

A process is an execution instance, not an executable file. [[Process Lifecycle and State Transitions]] tracks whether it can run now; [[Process Control Block and Context Switching]] tracks the saved information needed to resume it. Growth directions and segment layouts are conventional diagrams, not universal memory-layout laws.

## Related notes

- [[Dual-Mode Operation and System Calls]]
- [[Process Lifecycle and State Transitions]]
- [[Process Control Block and Context Switching]]

## Sources

- **Lectures:** `cse313/01 - Sources/Lectures/2. ProcessAndThread-week2-RRR.pdf` (Slides 1–7)
- **Textbook:** Andrew S. Tanenbaum & Herbert Bos, *Modern Operating Systems* (4th Edition), Chapter 2: Processes and Threads (Section 2.1)
