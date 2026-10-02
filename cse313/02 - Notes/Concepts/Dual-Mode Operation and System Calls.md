---
type: concept
course: cse313
status: active
order: 2
---

# Dual-Mode Operation and System Calls

> 📖 **Reading Order:** Step 02 of 34 | **Module 1:** OS Architecture & Kernel Fundamentals  
> ◄ **Previous:** [[Operating System Structures and Functions]] | ► **Next:** [[Computer Booting and Hardware Abstractions]]

---

> [!IMPORTANT] 🎯 **Exam Frequency & Intelligence (Appeared in 2017 Q1c, 2019 Q4a, 2021 Q3b)**
> **Frequency:** ⭐⭐⭐⭐ **High Recurrence (Appeared across 3 exam years, repeated!)**
>
> ### What Exam Questions Expect & How to Master Them:
> 1. **Step-by-Step Execution of a System Call (2017 Q1c & 2019 Q4a verbatim):**
>    - **The 7-Step Sequence:**
>      1. *User-Space Invocation:* User application calls a C library wrapper (e.g., `read()`). The wrapper places arguments into CPU registers (`%rdi`, `%rsi`, `%rdx`) and loads the unique system call number into `%rax`.
>      2. *Trap Trigger:* The program executes a software interrupt/trap instruction (`syscall` or `int 0x80`).
>      3. *Hardware Mode Switch:* CPU hardware switches the execution mode bit from User Mode (Ring 3) to Kernel Mode (Ring 0) and switches the stack pointer from user stack to kernel stack.
>      4. *State Preservation:* Hardware/microcode pushes the User Program Counter (PC) and processor flags onto the kernel stack.
>      5. *Dispatch Table Lookup:* The kernel interrupt handler indexes into `sys_call_table[]` using the system call number in `%rax`.
>      6. *Service Execution:* The kernel function validates user pointer bounds and executes the privileged operation.
>      7. *Mode Return:* Kernel places return code into `%rax`, pops saved registers, and executes `sysret` or `iret`, resetting the mode bit to Ring 3 and resuming user program execution.
> 2. **Differentiating User/Kernel Mode vs User/Kernel Space (2021 Q3b):**
>    - **User Mode vs Kernel Mode (CPU Privilege):** Hardware state controlled by the CPU mode bit in the Program Status Word (PSW). Dictates whether privileged instructions (like `cli`, `sti`, modifying CR3, accessing I/O ports) are allowed.
>    - **User Space vs Kernel Space (Memory Address Segmentation):** Division of the virtual address space. User space (lower addresses) is mapped per-process; Kernel space (upper addresses) is reserved for the OS core, page tables, and drivers, guarded by supervisor bit flags in page table entries.

---

## Definition

To prevent user programs from interfering with the proper operation of the system, crashing other programs, or taking exclusive control of hardware, modern computer architectures implement **Dual-Mode Operation**:
- **Kernel Mode (also called Supervisor Mode, Protected Mode, System Mode, or Ring 0):**  
  The CPU has unrestricted access to all hardware, can execute all machine instructions (including privileged instructions), and can access all physical memory. The operating system kernel runs exclusively in this mode.
- **User Mode (Ring 3):**  
  The CPU executes with restricted privileges. It can only execute a safe subset of instructions (non-privileged) and can only access the memory mapped to the currently running process. All user applications (web browsers, text editors, games) execute in user mode.

The boundary between these two worlds is crossed safely and strictly via **System Calls**, **Interrupts**, and **Exceptions**.

---

## Hardware Mechanism: The Mode Bit

Dual-mode operation requires direct hardware enforcement. The CPU architecture includes a dedicated **mode bit** inside the Program Status Word (PSW) or flags register:
- $\text{Mode Bit} = 0 \implies \mathbf{Kernel\ Mode}$
- $\text{Mode Bit} = 1 \implies \mathbf{User\ Mode}$

When the system boots, hardware initializes the CPU in **Kernel Mode** ($\text{bit} = 0$). Before handing control over to a user program, the kernel sets the mode bit to $1$.

```mermaid
stateDiagram-v2
    direction LR
    User: User Mode (Mode Bit = 1)\nApplication Execution
    Kernel: Kernel Mode (Mode Bit = 0)\nOS Kernel Execution

    User --> Kernel: System Call / Trap / Hardware Interrupt / Fault\n(Hardware switches Mode Bit to 0)
    Kernel --> User: Return from Trap / Interrupt (iret / sysret)\n(Hardware sets Mode Bit back to 1)
```

---

## Privileged vs. Non-Privileged Instructions

Hardware categorizes CPU instructions into two strict classes:

| Class | Definition | Examples | What happens if executed in User Mode? |
|---|---|---|---|
| **Privileged Instructions** | Instructions that can compromise system integrity, alter hardware state, or bypass protection | - Direct I/O instructions (`in`, `out`)<br>- Halting the CPU (`hlt`)<br>- Modifying the mode bit or PSW<br>- Disabling/Enabling interrupts (`cli`, `sti`)<br>- Modifying MMU page table base registers (`cr3`)<br>- Loading timer count registers | Hardware immediately detects the violation, aborts execution, generates a **CPU Trap / General Protection Fault**, and terminates the offending process. |
| **Non-Privileged Instructions** | Pure computational or local data movement instructions safe for ordinary execution | - Arithmetic operations (`add`, `sub`, `mul`)<br>- Branching / jumping (`jmp`, `call`, `ret`)<br>- Reading/writing process-owned memory (`mov`, `push`, `pop`)<br>- Reading system time | Executes normally without kernel intervention. |

---

## The Three Triggers for Mode Switching

A transition from User Mode ($\text{bit}=1$) to Kernel Mode ($\text{bit}=0$) occurs through exactly three mechanisms:

1. **System Call (Voluntary Software Trap):**
   - The running application explicitly requests an OS service (e.g., read a file, allocate memory, create a process, send a network packet).
   - Triggered by a software interrupt instruction (e.g., `syscall`, `sysenter`, or `int 0x80`).
2. **Hardware Interrupt (Asynchronous External Event):**
   - Generated by physical I/O controllers or the hardware timer clock (e.g., timer quantum expiration, keystroke on keyboard, packet arrived on NIC, disk read completed).
   - Independent of whatever instruction the CPU is currently executing.
3. **Trap / Exception / Fault (Synchronous Internal Error):**
   - Generated automatically by the CPU when an executing instruction encounters an unrecoverable condition (e.g., division by zero, invalid memory address dereference, segmentation fault, page fault).

---

## Step-by-Step Mechanics of a System Call

When a user program calls a library function such as `read(fd, buffer, nbytes)`, the execution proceeds through a precise sequence:

```mermaid
sequenceDiagram
    autonumber
    actor App as User Application
    participant Lib as C Library Wrapper (libc)
    participant HW as CPU Hardware
    participant Kernel as OS System Call Handler
    
    App->>Lib: read(fd, buffer, 100)
    Note over Lib: 1. Put syscall number (__NR_read = 3) in register (eax/rax)<br/>2. Put arguments in registers (ebx, ecx, edx)
    Lib->>HW: Execute trap instruction (syscall / int 0x80)
    Note over HW: 3. Save User PC & SP on Kernel Stack<br/>4. Switch Mode Bit to 0 (Kernel Mode)<br/>5. Jump to Interrupt Vector Table address
    HW->>Kernel: Enter system_call_entry()
    Note over Kernel: 6. Save remaining CPU registers<br/>7. Validate user parameters & memory pointers<br/>8. Look up syscall table: sys_call_table[3] -> sys_read()<br/>9. Execute kernel disk/file driver logic
    Kernel-->>HW: Write return value (bytes read) to eax/rax
    Note over HW: 10. Restore user registers & User PC/SP<br/>11. Set Mode Bit back to 1 (sysret / iret)
    HW-->>Lib: Resume execution in User Mode
    Lib-->>App: Return bytes read to application
```

### Parameter Passing Mechanisms
System calls require arguments (file descriptors, buffer pointers, lengths). Since user space and kernel space have distinct stacks, three standard parameter-passing conventions exist:
1. **CPU Registers (Fastest):** Parameters are loaded directly into general-purpose registers (e.g., `rdi`, `rsi`, `rdx` in x86-64). Limited by the number of registers (typically $\le 6$).
2. **Memory Block / Table:** Parameters are placed in a contiguous memory block in user space, and a single pointer to the block is passed in a register.
3. **Stack:** Parameters are pushed onto the user stack by the calling program and popped by the operating system.

---

## Edge Cases & Common Pitfalls

1. **Confusing Function Calls with System Calls:**
   - A normal C function call (e.g., `strcpy()`, `strlen()`, `sqrt()`) stays entirely in **User Mode** using standard `call`/`ret` instructions without involving the kernel.
   - A system call (e.g., `open()`, `fork()`, `write()`) crosses the hardware privilege boundary into **Kernel Mode**.
2. **Buffer Security & Parameter Validation:**
   - The kernel can never trust pointers passed by user programs. If a malicious user passes a buffer pointer pointing into kernel memory, an unvalidated `read()` could overwrite kernel data structures. The kernel must explicitly verify that all user buffer addresses lie strictly within the user's allocated virtual memory space before dereferencing them.
3. **Disabling Interrupts in User Mode:**
   - Allowing a user program to disable interrupts would allow a rogue `while(1)` loop to seize the CPU forever, destroying time-sharing. Therefore, `cli` (clear interrupt flag) is strictly privileged.

---

## Cross-Topic Connections / Exam Relevance

- **Next Step:** How the hardware and OS bootstrap themselves into dual-mode operation (see [[Computer Booting and Hardware Abstractions]]).
- **Process Context:** Every process initiates state changes via system calls (see [[Process Concepts and Memory Layout]] and [[Process Creation and Termination Operations]]).
- **CPU Scheduling:** Preemption relies upon hardware timer interrupts forcing periodic returns to kernel mode (see [[CPU Scheduling Principles and Criteria]]).
- **Exam Testing:** High-yield exam topic. Common questions include:
  - Explaining the difference between an interrupt and a trap.
  - Identifying which instructions from a list are privileged vs non-privileged.
  - Tracing the exact sequence of events during a system call transition.

---

## Sources & Traceability

- **Lectures:** `cse313/01 - Sources/Lectures/1. Introduction-week1-RRR-2026.pdf` (Slides 13–28)
- **Textbook:** Andrew S. Tanenbaum & Herbert Bos, *Modern Operating Systems* (4th Edition), Chapter 1 (Section 1.4: Hardware Review, Section 1.6: System Calls)
