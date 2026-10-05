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

## Building the idea

[[Operating System Structures and Functions]] gives the kernel responsibility for protecting resources. But a promise in software is not enough: an application must be unable to bypass it with a machine instruction. The CPU therefore records its current privilege level and checks protected operations in hardware.

Consider `read(fd, buffer, n)`. The application is allowed to ask for a file read. It is not allowed to jump to an arbitrary kernel address and run with kernel privileges. Its library wrapper passes a request through a controlled entry instruction. Kernel entry code preserves the state needed to return, validates the request, and performs the service. On return, execution continues in the requesting program with the result.

Notice two distinct changes. A **mode switch** changes privilege; a **context switch** changes which task executes. A system call can enter and leave the kernel while the same task remains current throughout. If the read must wait, the kernel can block that task and schedule another; that adds a context switch.

Treat register names, numeric syscall IDs, and stack-switch details as architecture-specific implementations of this general sequence. The teaching model's mode bit is a useful abstraction, not a universal encoding in every CPU.

## Privilege and protected memory

**Kernel mode** permits privileged operations; **user mode** restricts them. A CPU may have more than two privilege levels. “Ring 0” and “Ring 3” are x86 terminology, while x86 protected mode is an execution mode that can contain both privilege levels.

The textbook mode bit (zero for kernel, one for user) is a conceptual convention. Actual privilege encoding, entry instructions, and saved state depend on the architecture. Kernel privilege also does not bypass every hardware, hypervisor, or memory restriction automatically.

User and kernel **space** refer to memory protection and mappings. Their address ranges depend on the OS and architecture. An illegal access causes a hardware exception; the OS decides how to handle it, possibly delivering a process signal.

## Following one system call

| Stage | What happens | Why it is needed |
|---|---|---|
| Request | A wrapper prepares a syscall identifier and arguments according to the syscall ABI. | The kernel needs to know which service is requested. |
| Controlled entry | A designated instruction transfers control to a kernel entry point with the required privilege transition. | User code must not choose arbitrary privileged entry points. |
| Preserve state | Hardware and entry software together save the state required for return and establish a suitable kernel execution environment. | The caller must be able to resume correctly. |
| Validate and dispatch | Kernel code checks permissions, argument values, and user-memory access before performing the service. | Crossing the boundary does not make untrusted inputs safe. |
| Complete or wait | The service returns a result, or blocks the task until an event occurs. | A blocked task can release the CPU to another ready task. |
| Return | The return path restores the caller's state and user privilege. | Execution continues after the request with its result. |

For the Linux x86-64 syscall ABI, the syscall number uses `rax` and arguments use `rdi`, `rsi`, `rdx`, `r10`, `r8`, and `r9`. This differs from both the ordinary function-call ABI and the i386 syscall convention. The x86-64 `syscall` instruction saves return information in registers; kernel entry software handles the kernel-stack setup. Do not combine this with an `int 0x80` stack-save diagram.

## Interrupts, exceptions, and calls

A hardware interrupt reports an external event, such as a timer tick. An exception arises from instruction execution, such as a page fault. A system call intentionally requests a service through controlled entry; its instruction need not be an interrupt instruction on every architecture.

A normal library function may run entirely in user mode, while some wrappers invoke system calls. Entering the kernel alone is a mode switch. Scheduling another task additionally changes execution context.

## Checking your understanding

If `read` can return immediately, the caller may enter and leave the kernel without another process running. If its data are unavailable, it may block and a context switch can follow. This separates privilege changes from scheduling decisions.

## What to carry forward

User/kernel **mode** describes the CPU's authority. User/kernel **space** describes protected memory regions. [[Process Control Block and Context Switching]] builds on this distinction: saving a task's execution state is different from merely entering the kernel on its behalf.

## Related notes

- [[Operating System Structures and Functions]]
- [[Process Control Block and Context Switching]]

## Sources

- **Lectures:** `cse313/01 - Sources/Lectures/1. Introduction-week1-RRR-2026.pdf` (Slides 13–28)
- **Textbook:** Andrew S. Tanenbaum & Herbert Bos, *Modern Operating Systems* (4th Edition), Chapter 1 (Section 1.4: Hardware Review, Section 1.6: System Calls)

- **Implementation clarification:** [Linux syscall ABI documentation](https://www.man7.org/linux/man-pages/man2/syscall.2.html).
