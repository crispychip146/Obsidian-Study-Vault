# CSE 313 — Operating Systems

> **Credits:** 3.0 | **Contact Hours:** 3 hours/week  
> **Course Scope:** Operating system architecture, dual-mode protection, process management, CPU scheduling algorithms, inter-process communication, concurrency synchronization, deadlock prevention and avoidance, virtual memory management, paging and segmentation, I/O devices, hard disk scheduling, RAID redundancy, file system implementations (VSFS, FFS, LFS), crash consistency and journaling, kernel memory allocators, and multiprocessor architectures.

---

## 🗺️ Master Sequential Reading Roadmap (Steps 01 – 68)

Read these notes in strict chronological order to build cumulative mastery from bare-metal architecture to complex journaling file systems and multiprocessor kernels:

### Module 1: OS Architecture & Kernel Fundamentals
- [ ] **Step 01 (Concept):** [[Operating System Structures and Functions]] — Role of the kernel, system call interface, monolithic vs microkernel architecture.
- [ ] **Step 02 (Concept):** [[Dual-Mode Operation and System Calls]] — User vs Kernel mode, trap handlers, privileged instructions, and architectural protection.
- [ ] **Step 03 (Concept):** [[Computer Booting and Hardware Abstractions]] — BIOS/UEFI, MBR/GPT, bootstrap loader sequence, and hardware virtualization layers.

### Module 2: Processes, Multiprogramming & Threads
- [ ] **Step 04 (Concept):** [[Process Concepts and Memory Layout]] — Address space segmentation: Text, Data, BSS, Heap, and Stack.
- [ ] **Step 05 (Concept):** [[Process Lifecycle and State Transitions]] — 5-state lifecycle model (New, Ready, Running, Waiting, Terminated) and dispatching queues.
- [ ] **Step 06 (Concept):** [[Process Control Block and Context Switching]] — PCB anatomy, register save/restore, and hardware context switch costs.
- [ ] **Step 07 (Concept):** [[Process Creation and Termination Operations]] — POSIX `fork()`, `exec()`, `wait()`, `exit()`, process trees, and init/systemd.
- [ ] **Step 08 (Concept):** [[Threads and Multithreading Models]] — User-level vs Kernel-level threads, Many-to-One, One-to-One, and Many-to-Many models.
- [ ] **Step 09 (Formula):** [[CPU Multiprogramming Utilization Formula]] — Mathematical CPU utilization model: $U = 1 - p^n$.
- [ ] **Step 10 (Example):** [[Process Forking and Zombie Orphan Example]] — Tracing PID return values, memory isolation, zombie state creation, and orphan reparenting.
- [ ] **Step 11 (Problem):** [[Problem — Fork Execution Tree and Process Tracing]] — Tracing sequential forks, loops, and tree expansions (`Q-CSE313-001`).

### Module 3: CPU Scheduling
- [ ] **Step 12 (Concept):** [[CPU Scheduling Principles and Criteria]] — CPU/IO burst cycles, preemptive vs non-preemptive scheduling, throughput, turnaround, waiting, and response times.
- [ ] **Step 13 (Algorithm):** [[Batch Scheduling Algorithms]] — FCFS (convoy effect), Non-Preemptive SJF, and SRTF (preemptive SJF).
- [ ] **Step 14 (Algorithm):** [[Interactive Scheduling Algorithms]] — Round Robin, Priority Scheduling, Multilevel Queue, and MLFQ.
- [ ] **Step 15 (Formula):** [[Scheduling Metrics and Burst Estimation Formulas]] — Exponential smoothing $\tau_{n+1} = \alpha t_n + (1-\alpha)\tau_n$ and metrics formulas.
- [ ] **Step 16 (Example):** [[Comprehensive CPU Scheduling Simulation Example]] — Comparative 4-process simulation with ASCII Gantt charts.
- [ ] **Step 17 (Problem):** [[Problem — CPU Scheduling Algorithm Simulation and Gantt Chart]] — Comprehensive 5-process simulation across 5 scheduling algorithms (`Q-CSE313-002`).

### Module 4: Inter-Process Communication & Synchronization
- [ ] **Step 18 (Concept):** [[Race Conditions and Critical-Section Problem]] — Shared memory hazards, spooler example, and the 4 criteria for valid mutual exclusion.
- [ ] **Step 19 (Algorithm):** [[Peterson's Algorithm and Hardware Mutual Exclusion]] — Peterson's 2-process protocol, proofs, TSL/XCHG instructions, and priority inversion.
- [ ] **Step 20 (Concept):** [[Semaphores and Synchronization Primitives]] — Lost wakeup problem, Dijkstra's semaphore definition, atomic wait/signal, and POSIX mutexes.
- [ ] **Step 21 (Concept):** [[Monitors and Condition Variables]] — Encapsulated language monitors, condition queues, Hoare (signal-and-wait) vs Mesa (signal-and-continue) semantics.
- [ ] **Step 22 (Concept):** [[Message Passing and IPC Models]] — Direct vs indirect mailboxes, synchronous rendezvous vs asynchronous buffering, UNIX pipes and sockets.
- [ ] **Step 23 (Algorithm):** [[Classic Synchronization Solutions]] — Bounded buffer, Readers-Writers (starvation analysis), Dining Philosophers, and Sleeping Barber.
- [ ] **Step 24 (Example):** [[Producer-Consumer Semaphore Implementation Example]] — Chronological 6-step state trace, full POSIX C implementation, and semaphore inversion bug.
- [ ] **Step 25 (Problem):** [[Problem — Dining Philosophers Deadlock-Free Synchronization]] — Asymmetric ordering, room capacity semaphores, and Tanenbaum's state tracing (`Q-CSE313-003`).

### Module 5: Deadlocks
- [ ] **Step 26 (Concept):** [[Deadlock Fundamentals and Coffman Conditions]] — The 4 Coffman conditions, Ostrich algorithm, Deadlock vs Livelock vs Starvation.
- [ ] **Step 27 (Concept):** [[Resource Allocation Graphs and Deadlock Modeling]] — Holt's bipartite graph, cycle vs deadlock theorems (single vs multiple instances), and graph reduction.
- [ ] **Step 28 (Concept):** [[Deadlock Prevention and Avoidance Strategies]] — Attacking Coffman conditions, linear resource ordering, safe vs unsafe states, and resource trajectories.
- [ ] **Step 29 (Algorithm):** [[Banker's Algorithm]] — Dijkstra's multi-resource Banker's algorithm, safety verification, and speculative request evaluation.
- [ ] **Step 30 (Algorithm):** [[Deadlock Detection and Recovery Algorithms]] — DFS cycle detection with backtracking, matrix reduction, and recovery via preemption, rollback, and termination.
- [ ] **Step 31 (Example):** [[Banker's Algorithm Multi-Resource Step-by-Step Example]] — Step-by-step 4-process, 3-resource numerical simulation from course lecture notes.
- [ ] **Step 32 (Example):** [[Resource Allocation Graph Cycle Detection Example]] — Tracing DFS stack $L$ from nodes $D$ and $R$ from course simulation lecture notes.
- [ ] **Step 33 (Problem):** [[Problem — Banker's Algorithm Safe State and Request Granting]] — 5-process, 4-resource safety and immediate request evaluation (`Q-CSE313-004`).
- [ ] **Step 34 (Problem):** [[Problem — Resource Allocation Graph Reduction and Cycle Detection]] — Single-instance DFS cycle detection and multi-instance graph reduction (`Q-CSE313-005`).

### Module 6: Virtual Memory Foundations
- [ ] **Step 35 (Concept):** [[Address Space Abstraction and Hardware Relocation]] — The address space illusion (Code, Heap, Stack), transparency, efficiency, protection, base-and-bounds hardware MMU dynamic relocation, bounds check, and OS context switching.
- [ ] **Step 36 (Concept):** [[Memory API and Allocation Safety]] — POSIX heap management (`malloc`, `free`, `calloc`, `realloc`), underlying `brk`/`sbrk`/`mmap` system calls, in-band headers, and the 7 classic dynamic memory bugs.
- [ ] **Step 37 (Concept):** [[Segmentation and External Fragmentation]] — Generalized base-and-bounds, segment registers (CS, DS, SS), negative growth for stack, permission sharing, and external fragmentation.
- [ ] **Step 38 (Algorithm):** [[Free-Space Management and Allocation Policies]] — Explicit free lists, splitting, coalescing, and allocation search policies (First-Fit, Next-Fit, Best-Fit, Worst-Fit).
- [ ] **Step 39 (Algorithm):** [[Segregated Free Lists and Binary Buddy Allocation]] — Fixed-size segregated lists, McKusick-Karels 4.3BSD allocator (`kmemsizes`), Binary Buddy Allocation algorithm ($A \oplus 2^k$ buddy check), and recursive coalescing.
- [ ] **Step 40 (Example):** [[Segmentation Translation and Buddy Allocation Example]] — Numerical step-by-step 14-bit segmented address translation with positive/negative growth, and 64 KB Binary Buddy allocation/deallocation trace.
- [ ] **Step 41 (Problem):** [[Problem — Segmentation Address Translation and Buddy Memory Allocation]] — Exam-level multi-part problem covering segmented MMU translation and 128 KB buddy allocator state transitions (`Q-CSE313-006`).

### Module 7: Paging & Virtual Memory Systems
- [ ] **Step 42 (Concept):** [[Paging Architecture and Linear Page Tables]] — Fixed-size pages and frames, elimination of external fragmentation, virtual address decomposition (VPN, Offset), PTE flag bits (Present, Dirty, Accessed, U/S, R/W), and linear table space/time overheads.
- [ ] **Step 43 (Concept):** [[Translation Lookaside Buffers and Hardware Caching]] — Hardware MMU TLB cache, TLB hit vs miss execution flow, hardware-managed (x86 CR3) vs software-managed (RISC/MIPS trap) TLBs, and context switch ASID tagging vs flushes.
- [ ] **Step 44 (Concept):** [[Multi-Level Page Tables and Advanced Address Translation]] — Sparse address space scalability, Page Directory and PDEs, multi-level tree translation (PD Index $\to$ PT Index $\to$ Offset), 99% RAM savings, and Inverted Page Tables.
- [ ] **Step 45 (Concept):** [[Swapping Mechanisms and Page Fault Handling]] — Beyond physical RAM, swap space partition, PTE Present bit $= 0$, complete page fault lifecycle, blocked state transition, instruction restart, and page daemon watermarks (`kswapd`).
- [ ] **Step 46 (Algorithm):** [[Page Replacement Policies and the Clock Algorithm]] — Optimal (Belady's MIN), FIFO and Belady's Anomaly proof, LRU, Clock (Second-Chance) circular buffer algorithm with Use bit and Dirty bit prioritization, and thrashing.
- [ ] **Step 47 (Formula):** [[Virtual Memory Performance and Address Translation Formulas]] — Average Memory Access Time ($\text{AMAT} = T_{\text{RAM}} + P_{\text{Miss}} \cdot T_{\text{Disk}}$), Effective Access Time with TLB ($\text{EAT}$), page table size, and multi-level depth equations.
- [ ] **Step 48 (Example):** [[Two-Level Page Table Translation and Clock Replacement Example]] — 32-bit hex virtual address walk through Page Directory and Page Table, and 4-frame Clock replacement simulation on a 12-reference string.
- [ ] **Step 49 (Problem):** [[Problem — Multi-Level Paging and Page Replacement Simulation]] — Exam-level problem on two-level page table memory sizing, PDE span, and comparative FIFO vs LRU vs Optimal simulation (`Q-CSE313-007`).

### Module 8: I/O Hardware & Storage Systems
- [ ] **Step 50 (Concept):** [[IO System Architecture and Direct Memory Access]] — System bus hierarchy (Memory bus, PCIe, DMI/SATA/USB), canonical device interface (Status, Command, Data registers), Polling vs Interrupts vs DMA controller engine, and device drivers.
- [ ] **Step 51 (Concept):** [[Hard Disk Drive Architecture and Mechanical Latency]] — Magnetic platters, tracks, sectors, cylinders, spindle, actuator arm, total access latency ($T_{\text{Seek}} + T_{\text{Rotation}} + T_{\text{Transfer}}$), average rotational delay ($30/\text{RPM}$), and random vs sequential throughput disparity.
- [ ] **Step 52 (Algorithm):** [[Disk Arm Scheduling Algorithms]] — FCFS, SSTF (and starvation), SCAN (Elevator algorithm), C-SCAN (Circular SCAN), LOOK, C-LOOK, and SPTF/SATF.
- [ ] **Step 53 (Concept):** [[RAID Architectures and Redundancy Models]] — Multi-disk virtualization, RAID 0 (Striping), RAID 1 (Mirroring), RAID 4 (Dedicated Parity), RAID 5 (Rotated Distributed Parity), capacity, reliability, throughput matrix, and small-write parity bottleneck.
- [ ] **Step 54 (Formula):** [[Disk Latency and RAID Performance Evaluation Formulas]] — Disk service time formulas, average rotational latency, transfer time, RAID effective capacity and throughput formulas ($N \cdot S, \frac{N}{4} \cdot R$), and parity XOR equation ($P_{\text{new}} = (D_{\text{old}} \oplus D_{\text{new}}) \oplus P_{\text{old}}$).
- [ ] **Step 55 (Example):** [[Disk Scheduling Simulation and RAID Small-Write Example]] — Comparative 8-request track traversal simulation for SSTF, SCAN, and C-LOOK + step-by-step XOR bitwise arithmetic trace of a RAID 4/5 small-write parity update.
- [ ] **Step 56 (Problem):** [[Problem — Disk Arm Scheduling and RAID Performance Analysis]] — Exam-level problem on disk arm traversal (SSTF, SCAN, C-LOOK), seek latency calculation, and 5-disk RAID 5 mixed workload throughput and failure recovery (`Q-CSE313-008`).

### Module 9: File Systems & Persistence
- [ ] **Step 57 (Concept):** [[File and Directory Abstractions and POSIX File API]] — Files as linear byte arrays, inodes, directories as filename-to-inode tables, POSIX API (`open`, `read`, `write`, `lseek`, `fsync`), and Hard Links vs Symbolic (Soft) Links.
- [ ] **Step 58 (Concept):** [[File System Implementation and VSFS On-Disk Structures]] — Very Simple File System (VSFS) on-disk layout (Superblock, Inode Bitmap, Data Bitmap, Inode Table, Data Region), multi-level indexing (12 direct, single, double, triple indirect), directory traversal paths, and Page Cache.
- [ ] **Step 59 (Concept):** [[Locality and the Berkeley Fast File System (FFS)]] — Flaws of original UNIX FS, Cylinder Groups (Block Groups), FFS placement policies (directories in low-utilization groups, files with parent directory), Large-File Exception, and fragments/sub-blocks.
- [ ] **Step 60 (Concept):** [[Crash Consistency, FSCK, and Write-Ahead Journaling]] — The crash consistency dilemma (3 writes: Inode $I$, Bitmap $B$, Data $D$), crash failure states, why `fsck` fails at scale, Data Journaling protocol (TxB, commit TxE barrier, checkpoint, free), and Metadata (Ordered) Journaling.
- [ ] **Step 61 (Concept):** [[Log-Structured File Systems (LFS) and Segment Cleaning]] — RAM absorbing reads, converting random writes into massive sequential segment bursts ($1-2\,\text{MB}$), the Wandering Inode problem, Inode Map (`imap`), Checkpoint Region (`CR`), and segment cleaning live block reclamation.
- [ ] **Step 62 (Example):** [[VSFS Inode Block Indexing and Journaling Crash Recovery Example]] — Step-by-step byte offset translation for direct, indirect, and double-indirect blocks in VSFS + chronological trace of system crash during Ordered Journaling with recovery replay.
- [ ] **Step 63 (Example):** [[LFS Segment Allocation and Imap Checkpoint Example]] — End-to-end trace of file creation, appends, overwrites, imap updates, checkpoint region modification, and live vs dead block detection during segment cleaning.
- [ ] **Step 64 (Problem):** [[Problem — File System Inode Capacity and Journaling Recovery]] — Exam-level problem on multi-level inode maximum file size, byte offset pointer path traversal, and ordered journaling crash recovery state reconstruction (`Q-CSE313-009`).

### Module 10: Advanced Kernel Systems & Multiprocessors
- [ ] **Step 65 (Concept):** [[Kernel Memory Allocation Architecture and the Slab Allocator]] — Kernel allocation constraints (interrupt context, non-blocking `GFP_ATOMIC`, high frequency), Resource Map, McKusick-Karels 4.3BSD allocator, Jeff Bonwick's Slab Allocator (object caches, slabs: full/partial/empty, zero initialization cost, slab coloring).
- [ ] **Step 66 (Concept):** [[Multiprocessor Operating System Architectures]] — Multicore hardware: UMA (SMP) vs NUMA (node interconnect latency penalty), MESI cache coherence snooping, Master-Slave OS vs Symmetric Multiprocessing (SMP), fine-grained locking, and CPU affinity scheduling.
- [ ] **Step 67 (Concept):** [[Linux System Architecture and Remote Procedure Calls (RPC)]] — Linux kernel subsystem architecture, distributed IPC, Remote Procedure Call (RPC) principles, Client stub, Server stub, parameter marshalling/unmarshalling, Interface Definition Language (`rpcgen`), and RPC over UDP vs TCP trade-offs.
- [ ] **Step 68 (Problem):** [[Problem — Multiprocessor Memory Latency and Kernel Memory Allocation]] — Exam-level problem on NUMA vs naive scheduler access latency, and Slab Allocator object packing and internal fragmentation analysis (`Q-CSE313-010`).

---

## 📚 Course Sources

The notes are derived with strict traceability from the following primary course lectures and reference textbooks:
- `SRC-CSE313-001`: `1. Introduction-week1-RRR-2026.pdf` (Introduction, OS Structures, Dual-Mode, Booting)
- `SRC-CSE313-002`: `2. ProcessAndThread-week2-RRR.pdf` (Processes, PCB, Context Switch, Threads, Multiprogramming)
- `SRC-CSE313-003`: `3. Scheduling-week-3-RRR.pdf` (CPU Scheduling Criteria, FCFS, SJF, SRTF, RR, Priority, MLFQ)
- `SRC-CSE313-004`: `4. IPC-week-4-5-RRR.pptx` (Race Conditions, Peterson's, Semaphores, Monitors, Classic IPC Problems)
- `SRC-CSE313-005`: `5. Deadlocks-week6-7-RRR.pdf` (Coffman Conditions, RAG, Banker's Algorithm, Detection & Recovery)
- `SRC-CSE313-006`: `Notes on algorithm simulation.pdf` (Banker's Algorithm & RAG Cycle Detection Simulation Steps)
- `SRC-CSE313-012`: `CSE313_KRV_Merged.pdf` (445-slide comprehensive deck covering Address Spaces, Memory API, Translation, Segmentation, Free-Space, Paging, TLB, Multi-Level Page Tables, Swapping, I/O Devices, Disks, RAID, Filesystems (VSFS, FFS, FSCK/Journaling, LFS), Kernel Allocators, Multiprocessors, and Linux/RPC)
- `SRC-CSE313-013`: `Operating Systems - Three Easy Pieces.pdf` (Authoritative textbook by Remzi & Andrea Arpaci-Dusseau used to explain the mechanisms, algorithms, trade-offs, and mathematical formulas of the lecture slides)

---

## 🎯 Testing & Practice

- **Question Bank:** [[cse313/05 - Testing/Question Bank/Question Bank|Question Bank]]
  - `Q-CSE313-001`: [[Problem — Fork Execution Tree and Process Tracing]]
  - `Q-CSE313-002`: [[Problem — CPU Scheduling Algorithm Simulation and Gantt Chart]]
  - `Q-CSE313-003`: [[Problem — Dining Philosophers Deadlock-Free Synchronization]]
  - `Q-CSE313-004`: [[Problem — Banker's Algorithm Safe State and Request Granting]]
  - `Q-CSE313-005`: [[Problem — Resource Allocation Graph Reduction and Cycle Detection]]
  - `Q-CSE313-006`: [[Problem — Segmentation Address Translation and Buddy Memory Allocation]]
  - `Q-CSE313-007`: [[Problem — Multi-Level Paging and Page Replacement Simulation]]
  - `Q-CSE313-008`: [[Problem — Disk Arm Scheduling and RAID Performance Analysis]]
  - `Q-CSE313-009`: [[Problem — File System Inode Capacity and Journaling Recovery]]
  - `Q-CSE313-010`: [[Problem — Multiprocessor Memory Latency and Kernel Memory Allocation]]

---

## 🗺️ Visual Maps & Navigation

- **Topic Map:** [[cse313/03 - Maps/Topic Map|Topic Map]]
- **Dependency Map:** [[cse313/03 - Maps/Dependency Map|Dependency Map]]
