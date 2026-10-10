# CSE313 — Question Bank

This file catalogs all practice, exam, tutorial, and lecture problems for **CSE313: Operating Systems**, tracking metadata, concepts tested, difficulty, and solution availability.

---

## Question Master Table

| ID | Title | Source | Topic | Question Type | Difficulty | Solution Status | Note Link |
|---|---|---|---|---|---|---|---|
| **Q-CSE313-001** | Fork Execution Tree and Process Tracing | `2. ProcessAndThread-week2-RRR.pdf` (Slides 20–32) | Process Management / POSIX `fork()` | Code Tracing / Tree Modeling | Medium | Solved | [[Problem — Fork Execution Tree and Process Tracing]] |
| **Q-CSE313-002** | CPU Scheduling Simulation and Gantt Chart | `3. Scheduling-week-3-RRR.pdf` (Slides 12–50) | CPU Scheduling Algorithms | Numerical Simulation / Gantt Chart | Medium-Hard | Solved | [[Problem — CPU Scheduling Algorithm Simulation and Gantt Chart]] |
| **Q-CSE313-003** | Dining Philosophers Deadlock-Free Synchronization | `4. IPC-week-4-5-RRR.pptx` (Slides 44–48) | Process Synchronization / Concurrency | Mathematical Proof / State Tracing | Hard | Solved | [[Problem — Dining Philosophers Deadlock-Free Synchronization]] |
| **Q-CSE313-004** | Banker's Algorithm Safe State and Request Granting | `5. Deadlocks-week6-7-RRR.pdf` & `Notes on algorithm simulation.pdf` | Deadlock Avoidance / Banker's Algorithm | Matrix & Vector Simulation | Medium-Hard | Solved | [[Problem — Banker's Algorithm Safe State and Request Granting]] |
| **Q-CSE313-005** | Resource Allocation Graph Reduction and Cycle Detection | `5. Deadlocks-week6-7-RRR.pdf` & `Notes on algorithm simulation.pdf` | Deadlock Detection / RAG Reduction | Graph Traversal / DFS Cycle Tracing | Medium | Solved | [[Problem — Resource Allocation Graph Reduction and Cycle Detection]] |
| **Q-CSE313-006** | Segmentation Address Translation and Buddy Memory Allocation | `CSE313_KRV_Merged.pdf` (Ch 16 & 17) | Virtual Memory / Segmentation & Buddy | Address Translation / Binary Coalescing | Medium-Hard | Solved | [[Problem — Segmentation Address Translation and Buddy Memory Allocation]] |
| **Q-CSE313-007** | Multi-Level Paging and Page Replacement Simulation | `CSE313_KRV_Merged.pdf` (Ch 20 & 22) | Paging & Swapping Policies | Memory Sizing / Algorithm Simulation | Hard | Solved | [[Problem — Multi-Level Paging and Page Replacement Simulation]] |
| **Q-CSE313-008** | Disk Arm Scheduling and RAID Performance Analysis | `CSE313_KRV_Merged.pdf` (Ch 37 & 38) | Storage Systems / Disk & RAID | Track Traversal / Throughput Sizing | Medium-Hard | Solved | [[Problem — Disk Arm Scheduling and RAID Performance Analysis]] |
| **Q-CSE313-009** | File System Inode Capacity and Journaling Recovery | `CSE313_KRV_Merged.pdf` (Ch 40 & 42) | File Systems / Inodes & Journaling | Multi-Level Indexing / Crash Replay | Hard | Solved | [[Problem — File System Inode Capacity and Journaling Recovery]] |
| **Q-CSE313-010** | Multiprocessor Memory Latency and Kernel Memory Allocation | `CSE313_KRV_Merged.pdf` (Ch 8 & Kernel Allocators) | Advanced Kernel / NUMA & Slab | Latency Calculation / Object Packing | Medium | Solved | [[Problem — Multiprocessor Memory Latency and Kernel Memory Allocation]] |

---

## Detailed Question Summaries

### Q-CSE313-001: Fork Execution Tree and Process Tracing
- **Problem Summary:** Traces execution across 4 distinct POSIX C code snippets: sequential forks, loop-based forking with PID condition checks, child-only branching, and shared memory isolation verification.
- **Concepts Tested:** [[Process Creation and Termination Operations]], [[Process Concepts and Memory Layout]], [[Process Lifecycle and State Transitions]]
- **Key Insight:** `fork()` creates an exact duplicate virtual address space; modifications to variables in child processes never alter parent variables. $k$ sequential forks produce $2^k$ total processes.
- **Detailed Note:** [[Problem — Fork Execution Tree and Process Tracing]]

---

### Q-CSE313-002: CPU Scheduling Simulation and Gantt Chart
- **Problem Summary:** Given a workload of 5 processes with arrival times, burst times, and priorities, construct execution Gantt charts and compute Turnaround, Waiting, and Response times under FCFS, SJF, SRTF, Preemptive Priority, and Round Robin ($q=3\text{ ms}$). Analyzes quantum sizing trade-offs.
- **Concepts Tested:** [[CPU Scheduling Principles and Criteria]], [[Batch Scheduling Algorithms]], [[Interactive Scheduling Algorithms]], [[Scheduling Metrics and Burst Estimation Formulas]]
- **Key Insight:** SRTF achieves optimal average turnaround time ($8.80\text{ ms}$) and waiting time ($4.40\text{ ms}$); Round Robin provides fair responsiveness ($4.40\text{ ms}$ response time) without starvation.
- **Detailed Note:** [[Problem — CPU Scheduling Algorithm Simulation and Gantt Chart]]

---

### Q-CSE313-003: Dining Philosophers Deadlock-Free Synchronization
- **Problem Summary:** Evaluates 3 distinct deadlock-free strategies for the 5-philosopher dining problem: asymmetric odd/even chopstick ordering, room capacity limiting semaphore ($room=4$), and Tanenbaum's state-based two-fork atomic acquisition. Traces chronological state progression and proves deadlock freedom.
- **Concepts Tested:** [[Race Conditions and Critical-Section Problem]], [[Semaphores and Synchronization Primitives]], [[Classic Synchronization Solutions]]
- **Key Insight:** Asymmetry breaks the circular wait condition. The room semaphore leverages the Pigeonhole Principle ($4 < 5$). Tanenbaum's state model ensures a philosopher only eats if neither neighbor is eating.
- **Detailed Note:** [[Problem — Dining Philosophers Deadlock-Free Synchronization]]

---

### Q-CSE313-004: Banker's Algorithm Safe State and Request Granting
- **Problem Summary:** In a 5-process, 4-resource system ($A, B, C, D$), computes the Available vector and Need matrix. Verifies that the initial state is safe with sequence $\langle P_0, P_2, P_1, P_3, P_4 \rangle$. Evaluates dynamic requests: immediately grants $Request_1 = (1, 1, 0, 0)$ after speculative safety verification, but blocks $Request_4 = (1, 2, 0, 1)$ due to insufficient available units of Resource $B$.
- **Concepts Tested:** [[Deadlock Fundamentals and Coffman Conditions]], [[Deadlock Prevention and Avoidance Strategies]], [[Banker's Algorithm]]
- **Key Insight:** Speculative allocation must pass the safety check before resources are permanently granted to guarantee the OS never enters an unsafe state.
- **Detailed Note:** [[Problem — Banker's Algorithm Safe State and Request Granting]]

---

### Q-CSE313-005: Resource Allocation Graph Reduction and Cycle Detection
- **Problem Summary:** Solves two distinct deadlock detection problems: traces the course DFS cycle detection algorithm with backtracking on a single-instance graph from nodes $P_1$ and $R_4$, detecting cycle $P_1 \to R_1 \to P_2 \to R_2 \to P_3 \to R_3 \to P_1$. Simulates step-by-step graph reduction on a multi-instance graph, proving that despite containing a directed cycle, the system is completely reducible and deadlock-free.
- **Concepts Tested:** [[Resource Allocation Graphs and Deadlock Modeling]], [[Deadlock Prevention and Avoidance Strategies]], [[Deadlock Detection and Recovery Algorithms]]
- **Key Insight:** In single-instance systems, cycle $\iff$ deadlock. In multi-instance systems, a cycle is necessary but not sufficient for deadlock.
- **Detailed Note:** [[Problem — Resource Allocation Graph Reduction and Cycle Detection]]

---

### Q-CSE313-006: Segmentation Address Translation and Buddy Memory Allocation
- **Problem Summary:** Evaluates 16-bit virtual address translation under segmentation across positive and negative growth segments, identifying bounds violations and stack offset arithmetic. Simulates 128 KB Binary Buddy allocation, tracking recursive power-of-two splits and bitwise XOR coalescing across 6 operations.
- **Concepts Tested:** [[Segmentation and External Fragmentation]], [[Segregated Free Lists and Binary Buddy Allocation]], [[Segmentation Translation and Buddy Allocation Example]]
- **Key Insight:** Stack segments require subtracting maximum segment size to compute negative offset. Two free blocks of size $2^k$ can only coalesce if their XOR relationship equals their size ($A \oplus 2^k$).
- **Detailed Note:** [[Problem — Segmentation Address Translation and Buddy Memory Allocation]]

---

### Q-CSE313-007: Multi-Level Paging and Page Replacement Simulation
- **Problem Summary:** Decomposes a 32-bit virtual address into Page Directory (8 bits), Page Table (11 bits), and Offset (13 bits) for an 8 KB page system. Proves a 98.4% memory reduction over linear tables for a sparse process. Simulates FIFO (16 faults), LRU (15 faults), and Optimal (11 faults) over a 20-reference string.
- **Concepts Tested:** [[Multi-Level Page Tables and Advanced Address Translation]], [[Page Replacement Policies and the Clock Algorithm]], [[Virtual Memory Performance and Address Translation Formulas]]
- **Key Insight:** Unallocated regions consume 0 bytes of physical memory in multi-level paging because their PDE marks Present = 0. Optimal lookahead evicts pages that are never referenced again first.
- **Detailed Note:** [[Problem — Multi-Level Paging and Page Replacement Simulation]]

---

### Q-CSE313-008: Disk Arm Scheduling and RAID Performance Analysis
- **Problem Summary:** Simulates SSTF (7,586 cyl), SCAN (7,492 cyl), and C-LOOK (9,137 cyl, 18.27 ms seek) across a 10-request queue on a 5,000-cylinder drive. Evaluates a 5-disk RAID 5 array under an 80/20 mixed read/write workload, proving the array sustains 781 workload IOPS due to the 4-I/O small-write penalty.
- **Concepts Tested:** [[Disk Arm Scheduling Algorithms]], [[RAID Architectures and Redundancy Models]], [[Disk Latency and RAID Performance Evaluation Formulas]]
- **Key Insight:** In RAID 5, random writes cost 2 reads + 2 writes. Average workload request cost is $\text{Read\%} + 4 \times \text{Write\%} = 1.60\text{ disk ops}$. Failed disk block reconstruction requires XORing all surviving disks.
- **Detailed Note:** [[Problem — Disk Arm Scheduling and RAID Performance Analysis]]

---

### Q-CSE313-009: File System Inode Capacity and Journaling Recovery
- **Problem Summary:** Derives maximum file capacity (256.5 GB) for an inode with 10 direct, 2 single-indirect, 1 double-indirect, and 1 triple-indirect pointers (2 KB blocks). Traces pointer path for byte offset `5,246,976`. Reconstructs file system states and recovery procedures across 3 distinct crash points in Ordered Metadata Journaling.
- **Concepts Tested:** [[File System Implementation and VSFS On-Disk Structures]], [[Crash Consistency, FSCK, and Write-Ahead Journaling]], [[VSFS Inode Block Indexing and Journaling Crash Recovery Example]]
- **Key Insight:** In Ordered Journaling, user data blocks must land on disk before the `TxE` commit record is written. If a crash occurs after `TxE`, redo recovery applies metadata in milliseconds without partition scanning.
- **Detailed Note:** [[Problem — File System Inode Capacity and Journaling Recovery]]

---

### Q-CSE313-010: Multiprocessor Memory Latency and Kernel Memory Allocation
- **Problem Summary:** Compares effective memory access time on a 4-node NUMA server: naive scheduler achieves $17.0\,\text{ns}$ AMAT, whereas NUMA-aware scheduler achieves $8.6\,\text{ns}$ AMAT ($1.98\times$ speedup). Analyzes Slab Allocator packing for 840-byte `task_struct` objects against power-of-two allocators, explaining zero external fragmentation and `GFP_ATOMIC` interrupt safety.
- **Concepts Tested:** [[Kernel Memory Allocation Architecture and the Slab Allocator]], [[Multiprocessor Operating System Architectures]], [[Linux System Architecture and Remote Procedure Calls (RPC)]]
- **Key Insight:** Binding processes to local NUMA nodes cuts memory latency in half. Pre-allocating object pools eliminates external fragmentation and guarantees deterministic non-blocking allocation in interrupt handlers.
- **Detailed Note:** [[Problem — Multiprocessor Memory Latency and Kernel Memory Allocation]]
