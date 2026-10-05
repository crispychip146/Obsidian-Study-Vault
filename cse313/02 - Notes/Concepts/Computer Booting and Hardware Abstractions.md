---
type: concept
course: cse313
status: active
order: 3
---

# Computer Booting and Hardware Abstractions

> 📖 **Reading Order:** Step 03 of 34 | **Module 1:** OS Architecture & Kernel Fundamentals  
> ◄ **Previous:** [[Dual-Mode Operation and System Calls]] | ► **Next:** [[Process Concepts and Memory Layout]]

---

> [!IMPORTANT] **Exam practice references (Appeared in 2017 Q1a, 2021 Q3d)**
>
> ### Practice tasks and reasoning:
> 1. **Writing Down the Exact Steps of Booting a Computer (2017 Q1a & 2021 Q3d verbatim):**
>    - **The Setup:** "Write down the steps of booting a computer."
>    - **The 7-Step Full-Credit Answer Key:**
>      1. **Power-On & Hardware Reset Vector:** Power supply asserts `POWER_GOOD`. The CPU starts execution at a hardwired ROM reset vector (`0xFFFFFFF0` on x86).
>      2. **POST (Power-On Self-Test):** Firmware conducts diagnostic hardware self-tests (checks RAM integrity, CPU registers, system buses, keyboard, disks).
>      3. **Boot Device Selection:** BIOS/UEFI scans configured NVRAM boot order (NVMe, SSD/HDD, USB, PXE network) for a bootable medium.
>      4. **MBR / Boot Sector Loading:** The firmware loads the primary boot sector (Sector 0, 512 bytes) into RAM at address `0x7C00` and verifies the `0x55AA` boot signature.
>      5. **Stage 1 Bootloader Execution:** Small bootloader code within the MBR executes, locates the active boot partition, and loads the Stage 2 Bootloader.
>      6. **Stage 2 Bootloader (GRUB2 / NTLDR):** Displays the OS selection menu, loads the OS kernel image (`vmlinuz`) and initial RAM disk (`initramfs`) into memory, and switches CPU from 16-bit real mode to 32/64-bit protected/long mode.
>      7. **Kernel Initialization & PID 1 Launch:** Kernel initializes memory paging, device drivers, and CPU scheduler, mounts real root (`/`), and spawns the first user-space process (`systemd` or `init`, PID 1).

---

## Building the idea

There is a puzzle before [[Dual-Mode Operation and System Calls]] can work: the kernel must be loaded before it can provide loading services. At reset, the CPU begins at an architecture-defined address where firmware is available. We therefore need a small first program that can run without the OS.

Booting solves this by transferring responsibility in stages. Firmware initializes enough hardware to choose a boot target. A loader obtains the kernel and any initial filesystem image, places them in memory, and transfers control. The kernel then initializes its own memory management, interrupt handling, drivers, and process machinery. User-space startup services can now begin.

Keep legacy BIOS and UEFI paths distinct. A traditional BIOS path can start from an MBR boot sector and load a larger second-stage loader. A UEFI boot manager can load an EFI application through firmware services; it does not require that MBR instruction sequence. The shared idea is that each stage establishes the conditions needed by the next.

For each stage, ask: **what code is running, what can it already access, and what does it hand over?** That makes the sequence easier to reconstruct than memorizing an unexplained list of addresses.

## Firmware, loader, and kernel

Booting is a staged transfer of control. At reset, the processor begins at an architecture-defined entry point containing or leading to firmware. Firmware initializes enough hardware to locate the next program. A boot manager or loader selects and loads an operating system; the kernel then establishes its own memory management, drivers, and process environment.

The details differ by platform:

| Stage | Legacy BIOS-style PC boot | UEFI-style boot |
|---|---|---|
| Initial software | Platform firmware initializes hardware. | UEFI firmware initializes hardware and exposes firmware services. |
| Choosing a target | Firmware commonly reads a boot sector from a selected device. | The boot manager follows configured boot options, commonly loading an EFI executable. |
| Loading the OS | A staged loader locates the kernel and related data. | An EFI loader uses firmware services to prepare the kernel and handoff. |
| Kernel control | The kernel establishes its runtime environment. | The loader/kernel leaves boot services at the appropriate handoff; supported runtime services are a separate interface. |

A BIOS master boot record and a UEFI executable are different mechanisms. Likewise, reset addresses and CPU mode transitions are architecture-specific. A simplified PC sequence should not be treated as the universal boot path for every computer.

## What each component contributes

- **Firmware storage:** nonvolatile storage retains the startup software while power is off; firmware can often be updated.
- **RAM:** holds loaded instructions and data during execution. The loader must arrange the memory layout expected by the kernel.
- **CPU:** fetches instructions from the current program counter. Reset defines an initial execution state; it does not randomly choose an instruction from RAM.
- **Device controllers:** provide interfaces for accessing storage and other devices. Firmware supplies early access, then OS drivers take over the relevant management.
- **Hardware description:** platform tables or device descriptions help the kernel discover resources and configure them.

## Hardware state that survives into OS reasoning

The **program counter** identifies where instruction execution continues. The **stack pointer** identifies the current stack position. General registers hold working values, and status/control registers record conditions and configuration. A context switch must preserve the state required to resume a task; the exact saved set depends on the architecture and software convention.

Memory forms a hierarchy: registers and caches keep small amounts close to the CPU, RAM holds active data, and storage retains larger persistent data. Their capacities and delays vary by hardware. Virtual memory gives a process an address-space abstraction through translation and protection; it does not make a storage access as fast as a register access.

The **MMU** translates and protects memory references. An **interrupt controller** routes external events to the CPU. Architecture-defined interrupt/exception entry data selects handlers; on x86, an IDT contains gate descriptors rather than just ordinary function pointers. Entry saves return state according to the architecture, and kernel software completes the handler setup.

With **DMA**, a device can transfer data to or from memory after software configures the transfer. The CPU need not copy each byte itself, but the OS must still manage buffers, access permissions, completion, and any required cache coordination. An interrupt may report completion, linking the hardware event to a blocked process becoming ready.

## From kernel initialization to user work

The kernel initializes essential subsystems, establishes the first user processes, and eventually starts system services and user applications. The exact order and process names depend on the OS. A shell or desktop is a later environment, rather than the component responsible for starting the CPU.

Secure Boot adds an authentication policy for permitted boot images when enabled and configured. A sequence of loaders does not by itself establish a verified chain of trust. Warm and cold boots may perform different initialization, but both must reach a valid kernel execution environment.

## What to carry forward

Boot order describes execution dependencies; it does not automatically imply cryptographic verification. Secure Boot adds an explicit verification policy. Once user programs start, [[Process Concepts and Memory Layout]] explains how their code, data, and execution state become a process.

## Related notes

- [[Dual-Mode Operation and System Calls]]
- [[Process Concepts and Memory Layout]]

## Sources

- **Lectures:** `cse313/01 - Sources/Lectures/1. Introduction-week1-RRR-2026.pdf` (Slides 7–10, 29–36)
- **Textbook:** Andrew S. Tanenbaum & Herbert Bos, *Modern Operating Systems* (4th Edition), Chapter 1 (Section 1.3: Hardware Overview, Section 1.5: Booting)

- **Implementation clarification:** [UEFI boot manager specification](https://uefi.org/specs/UEFI/2.10/03_Boot_Manager.html).
