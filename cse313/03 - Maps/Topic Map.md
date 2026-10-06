# CSE313 — Topic Map

This map organizes all topics covered in CSE313 (Operating Systems), linking syllabus domains to corresponding conceptual notes, algorithms, formulas, examples, and practice problems.

---

## 1. Operating System Architecture & Kernel Fundamentals

### 1.1 Core Architecture & Protection
- **Core Concepts:**
  - [[Operating System Structures and Functions]] (Kernel role, monolithic vs microkernel architecture, virtual machines)
  - [[Dual-Mode Operation and System Calls]] (User vs Kernel mode, trap handlers, privileged instructions, memory protection)
  - [[Computer Booting and Hardware Abstractions]] (BIOS/UEFI, MBR/GPT, bootstrap sequence, hardware abstraction layers)

---

## 2. Processes, Multiprogramming & Multithreading

### 2.1 Process Execution Model
- **Core Concepts:**
  - [[Process Concepts and Memory Layout]] (Address space: Text, Data, BSS, Heap, Stack)
  - [[Process Lifecycle and State Transitions]] (New, Ready, Running, Waiting, Terminated, dispatch queues)
  - [[Process Control Block and Context Switching]] (PCB structure, CPU register save/restore, switch overhead)
  - [[Process Creation and Termination Operations]] (POSIX `fork()`, `exec()`, `wait()`, `exit()`, process trees)
  - [[Threads and Multithreading Models]] (User vs Kernel threads, Many-to-One, One-to-One, Many-to-Many models)
- **Mathematical Formula:**
  - [[CPU Multiprogramming Utilization Formula]] ($U = 1 - p^n$)
- **Worked Examples:**
  - [[Process Forking and Zombie Orphan Example]] (Memory isolation, zombie termination, orphan reparenting to init)
- **Practice Problems:**
  - [[Problem — Fork Execution Tree and Process Tracing]] (`Q-CSE313-001`: loop and recursive tree expansions)

---

## 3. CPU Scheduling

### 3.1 Scheduling Criteria & Algorithms
- **Core Concept:**
  - [[CPU Scheduling Principles and Criteria]] (CPU/IO burst cycles, preemptive vs non-preemptive scheduling, turnaround, waiting, response times)
- **Algorithms:**
  - [[Batch Scheduling Algorithms]] (FCFS, Non-Preemptive SJF, SRTF)
  - [[Interactive Scheduling Algorithms]] (Round Robin, Priority Scheduling, Multilevel Queue, MLFQ)
- **Mathematical Formula:**
  - [[Scheduling Metrics and Burst Estimation Formulas]] (Exponential smoothing burst prediction: $\tau_{n+1} = \alpha t_n + (1-\alpha)\tau_n$, waiting time formulas)
- **Worked Examples:**
  - [[Comprehensive CPU Scheduling Simulation Example]] (Detailed 4-process comparative Gantt chart trace)
- **Practice Problems:**
  - [[Problem — CPU Scheduling Algorithm Simulation and Gantt Chart]] (`Q-CSE313-002`: 5-process simulation across 5 scheduling disciplines)

---

## 4. Inter-Process Communication & Synchronization

### 4.1 Race Conditions, Hardware Primitives & Software Protocols
- **Core Concepts:**
  - [[Race Conditions and Critical-Section Problem]] (Shared memory hazards, spooler example, 4 criteria for valid mutual exclusion)
  - [[Semaphores and Synchronization Primitives]] (Lost wakeup problem, atomic `wait`/`signal`, counting vs binary semaphores)
  - [[Monitors and Condition Variables]] (Compiler-enforced exclusion, Hoare signal-and-wait vs Mesa signal-and-continue)
  - [[Message Passing and IPC Models]] (Direct vs indirect mailboxes, rendezvous, buffering, UNIX pipes and sockets)
- **Algorithms:**
  - [[Peterson's Algorithm and Hardware Mutual Exclusion]] (Peterson's 2-process protocol, TSL/XCHG hardware atomic instructions, priority inversion)
  - [[Classic Synchronization Solutions]] (Producer-Consumer bounded buffer, Readers-Writers with reader preference, Dining Philosophers, Sleeping Barber)
- **Worked Examples:**
  - [[Producer-Consumer Semaphore Implementation Example]] (Step-by-step state matrix trace and POSIX C implementation)
- **Practice Problems:**
  - [[Problem — Dining Philosophers Deadlock-Free Synchronization]] (`Q-CSE313-003`: Asymmetric ordering, room capacity limiter, and state-based solutions)

---

## 5. Deadlocks

### 5.1 Deadlock Conditions, Avoidance & Detection
- **Core Concepts:**
  - [[Deadlock Fundamentals and Coffman Conditions]] (The 4 Coffman conditions, Ostrich algorithm, Deadlock vs Livelock vs Starvation)
  - [[Resource Allocation Graphs and Deadlock Modeling]] (Holt's bipartite graph, cycle vs deadlock theorems for single/multi-instance systems)
  - [[Deadlock Prevention and Avoidance Strategies]] (Attacking Coffman conditions, linear resource ordering, safe vs unsafe states, resource trajectories)
- **Algorithms:**
  - [[Banker's Algorithm]] (Dijkstra's multi-resource Banker's algorithm, safety verification, and speculative request testing)
  - [[Deadlock Detection and Recovery Algorithms]] (DFS cycle detection with backtracking, matrix reduction, recovery via preemption, rollback, and process abort)
- **Worked Examples:**
  - [[Banker's Algorithm Multi-Resource Step-by-Step Example]] (Exact 4-process, 3-resource simulation from course notes)
  - [[Resource Allocation Graph Cycle Detection Example]] (Exact DFS stack $L$ traversal from nodes $D$ and $R$ from course notes)
- **Practice Problems:**
  - [[Problem — Banker's Algorithm Safe State and Request Granting]] (`Q-CSE313-004`: 5-process, 4-resource safety check and dynamic request granting)
  - [[Problem — Resource Allocation Graph Reduction and Cycle Detection]] (`Q-CSE313-005`: Single-instance DFS cycle detection and multi-instance graph reduction)
---

## 6. Virtual Memory Foundations

### 6.1 Address Spaces, Segmentation & Free-Space Management
- **Core Concepts:**
  - [[Address Space Abstraction and Hardware Relocation]] (The address space illusion, transparency, efficiency, protection, MMU Base-and-Bounds dynamic relocation)
  - [[Memory API and Allocation Safety]] (POSIX `malloc`, `free`, `calloc`, `realloc`, underlying `brk`/`sbrk`/`mmap`, and 7 deadly memory bugs)
  - [[Segmentation and External Fragmentation]] (Generalized base-and-bounds, segment registers, negative growth stack, and external fragmentation)
- **Algorithms:**
  - [[Free-Space Management and Allocation Policies]] (Explicit free lists, splitting, coalescing, First-Fit, Next-Fit, Best-Fit, Worst-Fit)
  - [[Segregated Free Lists and Binary Buddy Allocation]] (Segregated pools, McKusick-Karels 4.3BSD allocator, Binary Buddy power-of-two recursive splitting and XOR coalescing)
- **Worked Examples:**
  - [[Segmentation Translation and Buddy Allocation Example]] (14-bit segmented MMU address translation and 64 KB Binary Buddy allocation trace)
- **Practice Problems:**
  - [[Problem — Segmentation Address Translation and Buddy Memory Allocation]] (`Q-CSE313-006`: Multi-segment address translation and 128 KB buddy allocator state trace)

---

## 7. Paging & Virtual Memory Systems

### 7.1 Paging, TLB Caching, Multi-Level Tables & Swapping
- **Core Concepts:**
  - [[Paging Architecture and Linear Page Tables]] (Fixed-size pages and frames, VPN and Offset decomposition, PTE bits, space/time overheads)
  - [[Translation Lookaside Buffers and Hardware Caching]] (MMU TLB hardware cache, TLB hit/miss flow, CISC hardware vs RISC software TLBs, ASID tagging)
  - [[Multi-Level Page Tables and Advanced Address Translation]] (Sparse address space scaling, Page Directory and PDEs, two-level walk, Inverted Page Tables)
  - [[Swapping Mechanisms and Page Fault Handling]] (Secondary swap space, PTE Present bit $= 0$, page fault handling sequence, instruction restart, memory watermarks)
- **Algorithms:**
  - [[Page Replacement Policies and the Clock Algorithm]] (Optimal Belady's MIN, FIFO and Belady's Anomaly proof, LRU, Clock second-chance circular buffer, Thrashing)
- **Mathematical Formula:**
  - [[Virtual Memory Performance and Address Translation Formulas]] (Average Memory Access Time $	ext{AMAT}$, Effective Access Time $	ext{EAT}$ with TLB, page table sizing formulas)
- **Worked Examples:**
  - [[Two-Level Page Table Translation and Clock Replacement Example]] (32-bit hex virtual address walk through Page Directory and Page Table, 4-frame Clock simulation)
- **Practice Problems:**
  - [[Problem — Multi-Level Paging and Page Replacement Simulation]] (`Q-CSE313-007`: Two-level page table sizing, PDE span, and comparative FIFO vs LRU vs Optimal simulation)

---

## 8. I/O Hardware & Storage Systems

### 8.1 Devices, Hard Disk Scheduling & RAID Redundancy
- **Core Concepts:**
  - [[IO System Architecture and Direct Memory Access]] (System bus hierarchy, canonical device interface, Polling vs Interrupts vs DMA engine, device drivers)
  - [[Hard Disk Drive Architecture and Mechanical Latency]] (Platters, tracks, sectors, cylinders, spindle, latency components: Seek, Rotation, Transfer, random vs sequential disparity)
  - [[RAID Architectures and Redundancy Models]] (Multi-disk virtualization, RAID 0, RAID 1, RAID 4 dedicated parity, RAID 5 rotated parity, small-write parity bottleneck)
- **Algorithms:**
  - [[Disk Arm Scheduling Algorithms]] (FCFS, SSTF, SCAN elevator algorithm, C-SCAN circular sweep, LOOK, C-LOOK, SPTF/SATF)
- **Mathematical Formula:**
  - [[Disk Latency and RAID Performance Evaluation Formulas]] (Disk latency equations, average rotational delay, RAID capacity and sequential/random throughput formulas)
- **Worked Examples:**
  - [[Disk Scheduling Simulation and RAID Small-Write Example]] (8-request track traversal simulation for SSTF, SCAN, and C-LOOK + XOR small-write parity trace)
- **Practice Problems:**
  - [[Problem — Disk Arm Scheduling and RAID Performance Analysis]] (`Q-CSE313-008`: Arm traversal on 5,000-cylinder drive, seek latency, and 5-disk RAID 5 mixed workload throughput)

---

## 9. File Systems & Persistence

### 9.1 File APIs, VSFS Inodes, FFS Locality, Journaling & LFS
- **Core Concepts:**
  - [[File and Directory Abstractions and POSIX File API]] (Files as byte arrays, inodes, directories, POSIX API: `open`, `read`, `write`, `lseek`, `fsync`, Hard vs Symbolic links)
  - [[File System Implementation and VSFS On-Disk Structures]] (VSFS on-disk layout, multi-level indexing: 12 direct, single, double, triple indirect, directory traversal, page cache)
  - [[Locality and the Berkeley Fast File System (FFS)]] (Flaws of original UNIX FS, Cylinder Groups, FFS directory and file placement policies, Large-File Exception, fragments)
  - [[Crash Consistency, FSCK, and Write-Ahead Journaling]] (Crash consistency problem, FSCK flaws, Data Journaling protocol, Metadata Ordered Journaling, flush barriers)
  - [[Log-Structured File Systems (LFS) and Segment Cleaning]] (RAM absorbing reads, sequential segment bursts, Wandering Inode problem, Inode Map `imap`, Checkpoint Region `CR`, segment cleaning)
- **Worked Examples:**
  - [[VSFS Inode Block Indexing and Journaling Crash Recovery Example]] (Multi-level inode byte offset lookup trace and Ordered Journaling crash replay)
  - [[LFS Segment Allocation and Imap Checkpoint Example]] (End-to-end trace of file creation, appends, overwrites, imap updates, and segment cleaning live block detection)
- **Practice Problems:**
  - [[Problem — File System Inode Capacity and Journaling Recovery]] (`Q-CSE313-009`: Multi-level inode maximum file size calculation, byte offset pointer traversal, and crash recovery reconstruction)

---

## 10. Advanced Kernel Systems & Multiprocessors

### 10.1 Kernel Memory, Multiprocessor Architecture & RPC
- **Core Concepts:**
  - [[Kernel Memory Allocation Architecture and the Slab Allocator]] (Kernel allocation constraints, interrupt context, McKusick-Karels, Jeff Bonwick's Slab Allocator: caches, slabs, slab coloring)
  - [[Multiprocessor Operating System Architectures]] (UMA vs NUMA latency disparity, MESI cache coherence protocol, Master-Slave vs SMP, fine-grained locking, CPU affinity)
  - [[Linux System Architecture and Remote Procedure Calls (RPC)]] (Linux kernel structure, distributed IPC, RPC architecture: Client/Server stubs, parameter marshalling, `rpcgen`, UDP vs TCP)
- **Practice Problems:**
  - [[Problem — Multiprocessor Memory Latency and Kernel Memory Allocation]] (`Q-CSE313-010`: NUMA vs naive scheduler access latency, and Slab Allocator object packing and internal fragmentation analysis)
