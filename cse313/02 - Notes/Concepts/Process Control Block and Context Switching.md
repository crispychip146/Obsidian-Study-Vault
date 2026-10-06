---
type: concept
course: cse313
status: active
order: 6
---
# Process Control Block and Context Switching

> 📖 **Reading Order:** Step 06 of 68 | **Module 2:** Processes & Threads  
> ◄ **Previous:** [[Process Lifecycle and State Transitions]] | ► **Next:** [[Process Creation and Termination Operations]]

---
## Starting Point and the Problem

To create the illusion of simultaneous execution (multitasking) on a uniprocessor or multi-core machine, the CPU scheduler must frequently suspend a running process and assign the CPU core to another process.

We want the suspended process to resume execution later at the exact instruction where it was stopped, with all register values, arithmetic flags, and memory state completely intact. The central obstacle is that the CPU hardware has only one set of architectural registers (Program Counter, Stack Pointer, General Purpose Registers, PSW): loading Process $B$'s values overwrites Process $A$'s values entirely.

---
## Developing the Idea

To prevent state destruction, the operating system maintains a dedicated kernel data structure for every active process: the **Process Control Block (PCB)**.

The PCB acts as the operating system's comprehensive bookmark and dossier for the process. When the scheduler decides to switch execution from Process $A$ to Process $B$:
1. The kernel saves the hardware register state of Process $A$ into $A$'s PCB.
2. The kernel updates $A$'s lifecycle state to Ready or Blocked.
3. The kernel selects Process $B$, restores $B$'s register values from its PCB into the CPU hardware registers, switches memory page table registers (CR3 on x86), and jumps to $B$'s saved Program Counter.

This fundamental operation is called a **Context Switch**.

---
## Definition

To manage multiple concurrent processes and enable time-sharing on a single CPU, the operating system requires a dedicated data structure to represent each process.
- **Process Control Block (PCB):** A repository of information stored in kernel memory that contains all metadata and hardware state necessary to track, schedule, and pause/resume a process. (In the Linux kernel, this is implemented as `struct task_struct`).
- **Context Switch:** The hardware and software procedure of stopping the currently executing process, saving its execution state into its PCB, selecting another process, and loading the saved state from that process's PCB into the CPU registers to resume execution seamlessly.

---
## How It Works

### The Context Switching Mechanism

When the CPU scheduler decides to switch execution from Process $P_0$ to Process $P_1$, the kernel executes the following timeline:

```mermaid
sequenceDiagram
    autonumber
    participant P0 as Process P0
    participant OS as Operating System Kernel
    participant P1 as Process P1
    
    Note over P0: Executing in User Mode
    P0->>OS: Timer Interrupt / System Call (Trap)
    Note over OS: 1. Switch to Kernel Mode<br/>2. Save CPU registers & PC into PCB_0<br/>3. Update P0 state to Ready or Blocked
    Note over OS: 4. Scheduler selects Process P1<br/>5. Reload MMU with P1 Page Tables (CR3 register)<br/>6. Load CPU registers & PC from PCB_1
    OS->>P1: Return from Interrupt (sysret / iret)
    Note over P1: Resumes Execution in User Mode
```

### What Happens During a Context Switch?
1. **Save Context of $P_0$:**
   The CPU registers, stack pointer, and program counter of $P_0$ are saved into $PCB_0$ in kernel memory.
2. **Update Process State:**
   The state of $P_0$ in $PCB_0$ is updated from `Running` to `Ready` (if preempted by timer) or `Blocked` (if waiting for I/O).
3. **Move to Appropriate Queue:**
   $PCB_0$ is enqueued into the Ready Queue or the appropriate I/O device wait queue.
4. **Select Next Process:**
   The CPU scheduler selects $PCB_1$ from the Ready Queue according to its scheduling algorithm.
5. **Switch Memory Mapping:**
   The MMU base register (e.g., `cr3` register in x86) is updated to point to $P_1$'s page directory, replacing $P_0$'s virtual address space with $P_1$'s virtual address space.
6. **Restore Context of $P_1$:**
   The hardware registers, stack pointer, and program counter are restored from $PCB_1$.
7. **Switch to User Mode:**
   The CPU mode bit is set to $1$, jumping to the restored Program Counter to resume $P_1$.

---
## Example

Context switch sequence between $P_1$ and $P_2$:
1. Timer interrupt fires while $P_1$ is executing instruction `ADD R1, R2`.
2. CPU mode switches to Kernel Mode ($0$). Hardware pushes $P_1$'s PC and flags to the kernel stack.
3. OS scheduler decides to run $P_2$.
4. OS copies remaining registers (`R1`-`R15`, SP) into $P_1$'s PCB.
5. OS points CPU memory management register (CR3) to $P_2$'s page directory (invalidating TLB entries).
6. OS loads saved registers from $P_2$'s PCB into physical CPU registers.
7. OS executes return-from-interrupt (`iret`), switching to User Mode and resuming $P_2$.

---
## Technical Details

### Context Switch Overhead: The Cost of Time-Sharing

A context switch is **pure system overhead (dead time)**: during a context switch, the CPU performs administrative book-keeping and executes **zero useful user application instructions**.

Context switch costs fall into two categories:

### 1. Direct Costs (Microseconds)
- Saving and restoring tens of CPU registers to and from memory.
- Executing the scheduler's selection logic.
- Flashing and rewriting memory management registers.

### 2. Indirect Costs (The "Cache-Cold" Effect)
- **TLB Invalidation:** Switching page tables invalidates the **Translation Lookaside Buffer (TLB)**. The newly scheduled process experiences a burst of slow, multi-level page table walks in RAM.
- **CPU Cache Pollution:** The L1, L2, and L3 caches contain cache lines belonging to the old process $P_0$. When $P_1$ begins executing, almost every memory access results in a cache miss until $P_1$ warms up the cache.

---
## Important Properties and Why They Hold

- **State Transparency:** Context switching is completely transparent to the user application; no process can detect that it was suspended other than by querying physical wall-clock time.
- **Direct Overhead Invariant:** Context switching performs zero useful application computation; it is pure operating system administrative overhead.
- **Indirect Cache Penalties:** Switching address spaces forces Translation Lookaside Buffer (TLB) flushes and causes CPU L1/L2 cache misses as the new process warms up the cache lines.

---
## Common Mistakes

1. **Context Switch vs. Mode Switch:**
   - **Mode Switch:** A transition between User Mode and Kernel Mode for the *same* process (e.g., handling a quick `getpid()` system call). The process does not change, the address space does not change, and TLB is not flushed. Overhead is very small.
   - **Context Switch:** A transition between *different* processes ($P_0 \to P_1$). Requires changing the active PCB, updating memory page tables, and invalidating caches. Overhead is significantly higher!
2. **Quantum Sizing Dilemma:**
   - If the time quantum $q$ is too small (e.g., $1\text{ ms}$ with a context switch cost of $0.2\text{ ms}$), $16.7\%$ of CPU time is wasted on context switching alone.
   - If $q$ is too large (e.g., $500\text{ ms}$), the system loses interactive responsiveness and degrades to batch FCFS.

---
## Exam Relevance

- **Next Step:** How are new processes generated, and how does the OS clone PCBs during execution? (See [[Process Creation and Termination Operations]]).
- **Threads:** Why were threads invented? Because switching between threads sharing the same address space avoids TLB flushes and heavy context switch overhead (see [[Threads and Multithreading Models]]).
- **Exam Testing:** Frequently asked to:
  - Contrast a mode switch with a context switch.
  - List at least 5 distinct fields stored within a PCB.
  - Explain why frequent context switching degrades memory and CPU performance.

---
## Related Concepts

- [[Dual-Mode Operation and System Calls]]
- [[CPU Scheduling Principles and Criteria]]
- [[Threads and Multithreading Models]]

---
## Prerequisites

- [[Process Concepts and Memory Layout]]
- [[Process Lifecycle and State Transitions]]

---
## Problems

- [[Problem — Fork Execution Tree and Process Tracing]]

---
## Sources

- **Lectures:** `cse313/01 - Sources/Lectures/2. ProcessAndThread-week2-RRR.pdf` (Slides 8–10, 16–21)
- **Textbook:** Andrew S. Tanenbaum & Herbert Bos, *Modern Operating Systems* (4th Edition), Chapter 2 (Section 2.1.3: Process Implementation)
