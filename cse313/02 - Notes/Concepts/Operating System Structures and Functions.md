---
type: concept
course: cse313
status: active
order: 1
---
# Operating System Structures and Functions

> 📖 **Reading Order:** Step 01 of 68 | **Module 1:** OS Architecture & Kernel Fundamentals  
> ◄ **Previous:** *Start of Course* | ► **Next:** [[Dual-Mode Operation and System Calls]]

---
> [!IMPORTANT] 🎯 **Exam Frequency & Intelligence (Appeared in 2019 Q2d, 2020 Q4c)**
> **Frequency:** ⭐⭐⭐ **Recurring Architectural Comparison**
>
> ### What Exam Questions Expect & How to Master Them:
> 1. **Differentiating Monolithic vs Microkernel Architectures (2019 Q2d):**
>    - **Kernel Boundary:** In Monolithic OS (Linux, Windows NT core), file systems, network stacks, and device drivers run inside Kernel Space (Ring 0). In Microkernel OS (Minix, seL4, QNX), only minimal primitives (IPC, low-level scheduling, basic paging) remain in Ring 0; drivers and file systems run as isolated servers in User Space (Ring 3).
>    - **Performance vs Reliability Tradeoff:** Monolithic has higher performance (services communicate via direct function calls without context switching), but poor fault isolation (one buggy GPU or Wi-Fi driver crashes the entire machine). Microkernel has superior fault isolation (crashed driver server restarts transparently), but higher overhead due to frequent IPC context switches between Ring 3 and Ring 0.
> 2. **System Classifications (2020 Q4c):**
>    - *Multi-user OS:* Enables concurrent multi-user execution with robust privilege isolation, user IDs, and file permission ACLs.
>    - *Multiprocessor OS:* Employs Symmetric Multiprocessing (SMP) to balance workloads across multiple cores sharing common RAM and bus architectures.

---
## Starting Point and the Problem

Before the operating system existed, programmers wrote machine instructions directly against bare hardware, manually toggling console switches and reading punch cards. If a programmer made a memory indexing mistake, the hardware halted. If a program needed to read from a disk or tape, it had to implement low-level drive timing and controller commands from scratch.

We want a system where multiple programs can execute reliably, share expensive CPU and memory resources, and access storage without programmers reinventing physical hardware controllers. The central obstacle is hardware vulnerability and resource contention: without a central arbiter, one rogue or buggy program can overwrite memory belonging to another program or monopolize hardware indefinitely.

---
## Developing the Idea

To overcome hardware vulnerability, computer architects and systems designers introduced a software intermediary running in privileged execution mode: the **Operating System (OS)**.

The OS resolves the obstacle by presenting two complementary faces:
1. **Top-Down (The Extended Machine):** It replaces messy, timing-sensitive physical hardware (I/O ports, interrupt lines, disk cylinder addresses) with clean, high-level abstractions: files, directories, processes, and virtual memory.
2. **Bottom-Up (The Resource Manager):** It acts as an impartial controller that allocates CPU cores, memory frames, and I/O bandwidth across competing tasks according to policies of fairness, efficiency, and security.

---
## Definition

An **Operating System (OS)** is a foundational system software layer that runs directly on bare computer hardware in privileged mode, acting as an intermediary between computer hardware and user applications.

Fundamentally, an operating system performs two primary, dual roles:
1. **Extended Machine (Top-Down View / Abstraction Provider):**  
   Presents programmers with clean, standardized, hardware-independent abstractions (such as files, processes, virtual address spaces, and sockets) in place of the complex, error-prone, low-level physical reality of raw hardware (interrupts, disk controllers, timers, and bus interfaces).
2. **Resource Manager (Bottom-Up View / Resource Arbiter):**  
   Manages, coordinates, and multiplexes access to physical hardware resources (CPU cores, physical memory frames, network interfaces, and secondary storage) across competing programs and users, ensuring fairness, protection, efficiency, and system stability.

```mermaid
flowchart TD
    APP["User Applications / Compilers / Browsers / Databases"]
    SYS["System Call Interface (POSIX, Win32 API)"]
    OS["Operating System Kernel (Resource Manager & Abstraction Layer)"]
    HW["Hardware (CPU, MMU, RAM, Disks, Network Interfaces, Clocks)"]
    
    APP --> SYS
    SYS --> OS
    OS --> HW
```

---
## How It Works

### Operating System Architectures

The architectural organization of the kernel governs how OS components interact, execute, and isolate faults:

### 1. Monolithic Architecture
- **Structure:** The entire operating system runs as a single, large, highly privileged binary in kernel mode. All kernel subsystems (process scheduler, virtual memory, file systems, IPC, network stack, and hardware drivers) share a single flat address space.
- **Communication:** Internal subsystems communicate via blazing-fast, direct C function calls.
- **Advantages:** Maximum performance with minimal execution overhead (no context switching between kernel modules).
- **Disadvantages:** Monolithic fragility. Because all modules share the same address space, a bug or null-pointer dereference in a third-party printer driver immediately causes a kernel panic / blue screen of death (BSOD).
- **Examples:** Traditional UNIX, Linux, FreeBSD, MS-DOS.

### 2. Microkernel Architecture
- **Structure:** Strips the kernel down to the absolute bare minimum required to maintain system integrity—typically only:
  - Low-level address space management (virtual memory paging mechanisms).
  - Low-level inter-process communication (IPC / message passing).
  - Basic CPU scheduling.
- All non-essential services (file systems, device drivers, network protocols, user authentication) are evicted from kernel space and run as isolated, unprivileged **user-space servers**.
- **Communication:** User applications and OS servers communicate exclusively via microkernel IPC message passing.
- **Advantages:** High modularity, fault tolerance, and security. If the file system server crashes, it can be restarted automatically without crashing the entire machine.
- **Disadvantages:** Significant performance degradation caused by repeated context switches and boundary crossings during IPC message forwarding.
- **Examples:** Minix 3, QNX (used in automotive/medical safety-critical systems), Mach, seL4.

### 3. Layered Architecture
- Organizes the OS into a strict hierarchy of $N$ layers ($0$ to $N$).
- Layer $0$ interfaces with hardware; Layer $N$ interfaces with user applications.
- **Rule:** Layer $M$ can only invoke functions implemented by Layer $M-1$.
- **Advantages:** Clean modularity and incremental verification/debugging.
- **Disadvantages:** Difficult to strictly define layers without circular dependencies (e.g., backing store driver needs virtual memory, but virtual memory needs backing store driver).

### 4. Hybrid Architectures
- Combines the high performance of monolithic kernels with the modular abstractions of microkernels.
- Drivers and core subsystems run in kernel space for speed, but follow structured, object-oriented, message-like interfaces.
- **Examples:** Windows NT kernel, macOS (XNU / Darwin).

---
## Example

Consider two applications running concurrently: a web browser downloading an image over Wi-Fi and a compiler building a C project:
1. The compiler executes compute instructions at full CPU speed.
2. The browser requests network packets by invoking a system call.
3. The OS steps in, places the browser in a waiting queue, assigns the CPU to the compiler, and configures the network controller to use Direct Memory Access (DMA).
4. When the packet arrives, an interrupt alerts the OS, which wakes the browser without either application ever needing to know the other exists.

---
## Technical Details

### Core Operating System Responsibilities

1. **Process Management:** Creating, terminating, scheduling, and synchronizing processes and threads (see [[Process Concepts and Memory Layout]] and [[CPU Scheduling Principles and Criteria]]).
2. **Memory Management:** Allocating memory dynamically, tracking free frames, translating virtual addresses to physical addresses via the MMU, and swapping/paging (see [[Process Control Block and Context Switching]]).
3. **Storage & File System Management:** Organizing raw physical disk sectors into human-readable hierarchical directories, files, permissions, and metadata.
4. **I/O Device Management:** Providing uniform device driver interfaces, interrupt handlers, DMA (Direct Memory Access) channels, and buffering/caching.
5. **Protection and Security:** Enforcing access control lists, maintaining authentication, and defending hardware resources via [[Dual-Mode Operation and System Calls]].

---
## Important Properties and Why They Hold

- **Fault Isolation:** In microkernel systems, servers run in isolated user-space address spaces; a crash in a device driver server does not corrupt the kernel or halt other processes.
- **Protection Boundary Invariance:** User applications cannot execute privileged CPU instructions (such as disabling interrupts or modifying page table registers); any violation causes a hardware exception caught by the OS.
- **Performance Trade-Off:** Monolithic kernels maximize execution throughput by executing all OS services in Ring 0 with zero context-switching penalty, but sacrifice isolation resilience.

---
## Common Mistakes

1. **"The OS is the same as the GUI or Shell":**
   - The graphical desktop environment (e.g., Windows Desktop, GNOME) or command-line shell (e.g., `bash`, `zsh`) is **NOT** part of the kernel.
   - They are standard user-space programs that make system calls to the kernel just like any user application.
2. **"An OS makes programs run faster":**
   - An OS introduces unavoidable overhead (context switching, privilege boundary crossings, system call dispatching).
   - A single program running alone on bare metal executes faster than on an OS. The OS exists to enable safe *sharing*, *multiplexing*, and *portability*, not raw single-task speed.

---
## Exam Relevance

- **Next Step:** To enforce protection, hardware provides CPU execution rings (see [[Dual-Mode Operation and System Calls]]).
- **Process Abstraction:** The primary resource unit managed by the OS is the process (see [[Process Concepts and Memory Layout]]).
- **Exam Testing:** Frequently tested on midterms via comparison tables (Monolithic vs. Microkernel trade-offs), defining the two primary views of an OS, and explaining why bare-metal execution is unsuitable for modern multitasking.

---
## Related Concepts

- [[Dual-Mode Operation and System Calls]]
- [[Process Concepts and Memory Layout]]
- [[Computer Booting and Hardware Abstractions]]

---
## Prerequisites

- [[Computer Booting and Hardware Abstractions]]

---
## Problems

- [[Problem — Fork Execution Tree and Process Tracing]]

---
## Sources

- **Lectures:** `cse313/01 - Sources/Lectures/1. Introduction-week1-RRR-2026.pdf` (Slides 1–6, 11–12)
- **Textbook:** Andrew S. Tanenbaum & Herbert Bos, *Modern Operating Systems* (4th Edition), Chapter 1: Introduction (Sections 1.1–1.7)
