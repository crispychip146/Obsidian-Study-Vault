# CSE 313 (Operating Systems) — 2021 Final Solutions (Questions 1 to 4)

---

## Question 1(a)
**Consider a system with 4 processes: P1 through P4 and 3 resource types: A (9 units), B (3 units), and C (6 units). Current allocation of resources and maximum requirement of a process for each resource are given in Figure for Question 1(a). Evaluate whether the current state of this system is safe or unsafe.**

**Note that the resources are non-preemptive and a process releases all its acquired resources when it runs to completion. Also note that all of the four processes may suddenly request their maximum number of resources immediately:**

Current Allocation Matrix ($CA$):
| Process | A | B | C |
|---|:---:|:---:|:---:|
| **P1** | 4 | 2 | 3 |
| **P2** | 2 | 0 | 1 |
| **P3** | 1 | 0 | 1 |
| **P4** | 2 | 1 | 0 |
| **Total** | 9 | 3 | 5 |

Maximum Requirement Matrix ($MaxReq$):
| Process | A | B | C |
|---|:---:|:---:|:---:|
| **P1** | 6 | 2 | 5 |
| **P2** | 2 | 0 | 2 |
| **P3** | 6 | 3 | 3 |
| **P4** | 7 | 1 | 6 |

### Answer:

#### Step 1: Calculate Available Vector ($A$):
Total resources: $E = (9, 3, 6)$.
Sum of allocated resources:
$$\sum CA = (4+2+1+2, \; 2+0+0+1, \; 3+1+1+0) = (9, 3, 5)$$
$$A = E - \sum CA = (9-9, \; 3-3, \; 6-5) = \mathbf{(0, 0, 1)}$$

#### Step 2: Calculate Request / Need Matrix ($R = MaxReq - CA$):
- $R(P_1) = (6-4, \; 2-2, \; 5-3) = \mathbf{(2, 0, 2)}$
- $R(P_2) = (2-2, \; 0-0, \; 2-1) = \mathbf{(0, 0, 1)}$
- $R(P_3) = (6-1, \; 3-0, \; 3-1) = \mathbf{(5, 3, 2)}$
- $R(P_4) = (7-2, \; 1-1, \; 6-0) = \mathbf{(5, 0, 6)}$

#### Step 3: Execute Safety Algorithm Simulation:
Initialize $Work = A = (0, 0, 1)$ and $Finish = [F, F, F, F]$.

1. **Iteration 1:**
   - Compare Needs with $Work = (0, 0, 1)$:
     - $P_1: (2, 0, 2) \le (0, 0, 1) \implies$ FALSE ($2 > 0$).
     - $P_2: (0, 0, 1) \le (0, 0, 1) \implies$ **TRUE!** ($0 \le 0, 0 \le 0, 1 \le 1$).
     - $P_3: (5, 3, 2) \le (0, 0, 1) \implies$ FALSE.
     - $P_4: (5, 0, 6) \le (0, 0, 1) \implies$ FALSE.
   - Only **$P_2$** can be safely executed!
   - Process $P_2$ runs to completion and releases its allocated resources $(2, 0, 1)$:
     $$Work = Work + CA(P_2) = (0, 0, 1) + (2, 0, 1) = \mathbf{(2, 0, 2)}$$
   - $Finish[P_2] = \text{TRUE}$.
2. **Iteration 2:**
   - Compare remaining $\{P_1, P_3, P_4\}$ with $Work = (2, 0, 2)$:
     - $P_1: (2, 0, 2) \le (2, 0, 2) \implies$ **TRUE!** ($2 \le 2, 0 \le 0, 2 \le 2$).
     - $P_3: (5, 3, 2) \le (2, 0, 2) \implies$ FALSE ($5 > 2$).
     - $P_4: (5, 0, 6) \le (2, 0, 2) \implies$ FALSE ($5 > 2$).
   - Only **$P_1$** can be safely executed!
   - Process $P_1$ runs to completion and releases its allocated resources $(4, 2, 3)$:
     $$Work = Work + CA(P_1) = (2, 0, 2) + (4, 2, 3) = \mathbf{(6, 2, 5)}$$
   - $Finish[P_1] = \text{TRUE}$.
3. **Iteration 3:**
   - Available pool is now $Work = (6, 2, 5)$. Compare remaining $\{P_3, P_4\}$:
     - For $P_3$: Need is $(5, \mathbf{3}, 2)$.
       - Inspect Resource B: $P_3$ needs **3 units**, but available $Work_B$ is only **2 units**!
       - Since $3 > 2$, $Need(P_3) \not\le Work$. **$P_3$ cannot proceed!**
     - For $P_4$: Need is $(5, 0, \mathbf{6})$.
       - Inspect Resource C: $P_4$ needs **6 units**, but available $Work_C$ is only **5 units**!
       - Since $6 > 5$, $Need(P_4) \not\le Work$. **$P_4$ cannot proceed!**
4. **Deadlock / Unsafe Trapping:**
   - Neither $P_3$ nor $P_4$ can be satisfied with the available resources.
   - The safety algorithm halts with $Finish[P_3] == \text{FALSE}$ and $Finish[P_4] == \text{FALSE}$.

#### Conclusion:
No complete safe sequence exists. The current state of this system is **UNSAFE**.

### Sources:
- `5. Deadlocks-week6-7-RRR.pdf` (Slides 28–31: Banker's Algorithm for Multiple Resources).
- `Notes on algorithm simulation.pdf` (Pages 1–4).

### Link to Notes:
- [[Banker's Algorithm]]
- [[Deadlock Prevention and Avoidance Strategies]]
- [[Problem — Banker's Algorithm Safe State and Request Granting]]

---

## Question 1(b)
**Illustrate the position of process/thread scheduler, process table, and thread table in user-level thread implementation and kernel-level thread implementation with diagrams. With the help of your diagrams, analyze that kernel-level thread implementation is better than user-level thread implementation with respect to blocking.**

### Answer:
*(Identical comprehensive architecture as 2019 Q4(c)):*

#### Architectural Diagrams:
```
User-Level Threads (ULT):
+-------------------------------------------------------------+
| USER SPACE                                                  |
|   [ Thread 1 ]    [ Thread 2 ]    [ Thread 3 ]              |
|   [ Thread Table (Private per process) ]                    |
|   [ User-Space Runtime Scheduler (e.g. pthread library) ]   |
+-------------------------------------------------------------+
====================== KERNEL BOUNDARY ========================
| KERNEL SPACE                                                |
|   [ Process Table (1 entry per process) ]                   |
|   [ Kernel CPU Scheduler ]                                  |
+-------------------------------------------------------------+

Kernel-Level Threads (KLT):
+-------------------------------------------------------------+
| USER SPACE                                                  |
|   [ Thread 1 ]    [ Thread 2 ]    [ Thread 3 ]              |
+-------------------------------------------------------------+
====================== KERNEL BOUNDARY ========================
| KERNEL SPACE                                                |
|   [ Process Table ]                                         |
|   [ Thread Table (Maintained directly by OS kernel) ]       |
|   [ Kernel CPU Scheduler (Directly schedules threads) ]     |
+-------------------------------------------------------------+
```

#### Analysis with Respect to Blocking:
1. **User-Level Thread Blocking Failure:**
   - In ULT, the OS kernel is completely unaware of individual threads; it only knows about the enclosing process.
   - If Thread 1 issues a blocking system call (e.g., waiting for network socket read), the kernel puts the **entire process** into the `BLOCKED` state.
   - Even though Thread 2 and Thread 3 are CPU-ready, they cannot execute because the kernel unschedules the entire process.
2. **Kernel-Level Thread Superiority:**
   - In KLT, the kernel maintains individual thread state descriptors in its kernel Thread Table.
   - When Thread 1 blocks on I/O, the kernel only changes **Thread 1's state** to `BLOCKED`.
   - The kernel scheduler immediately switches CPU context to **Thread 2 or Thread 3** belonging to the same process. Concurrency is preserved without stalling the application.

### Sources:
- `2. ProcessAndThread-week2-RRR.pdf` (Slides 36–42).

### Link to Notes:
- [[Threads and Multithreading Models]]

---

## Question 1(c)
**Explain how to attack the "Circular wait condition" to structurally prevent deadlock.**

### Answer:
*(Identical formal strategy as 2019 Q3(c)(ii)):*
To eliminate circular wait, the operating system imposes a **global linear resource ordering**:
1. Assign a unique integer to every resource type using a one-to-one function:
   $$F: R \to \mathbb{N}$$
   Example: $F(\text{Tape Drive}) = 1$, $F(\text{Plotter}) = 2$, $F(\text{Printer}) = 3$, $F(\text{Disk}) = 4$.
2. **The Acquisition Protocol:**
   A process may request resource $R_j$ **if and only if** $F(R_j) > F(R_i)$ for all resources $R_i$ currently held by that process.
3. **Formal Proof:**
   A circular wait requires a closed cycle: $P_0 \to R_1 \to P_1 \to R_2 \dots \to P_k \to R_0 \to P_0$.
   By the ordering rule:
   $$F(R_0) < F(R_1) < F(R_2) < \dots < F(R_k) < F(R_0)$$
   This implies $F(R_0) < F(R_0)$, which is impossible. Hence, **no cycle can ever form**, structurally preventing deadlock.

### Sources:
- `5. Deadlocks-week6-7-RRR.pdf` (Slides 36–37).

### Link to Notes:
- [[Deadlock Prevention and Avoidance Strategies]]

---

## Question 2(a)
**Consider the following workload:**

| Process | Priority (Lowest Number has Highest Priority) | Duration (sec) | Arrival Time (sec) |
|---|:---:|:---:|:---:|
| **P1** | 1 | 30 | 0 |
| **P2** | 2 | 40 | 30 |
| **P3** | 2 | 65 | 50 |
| **P4** | 1 | 70 | 70 |

**Construct the Gantt chart and calculate the average turnaround time for Priority Scheduling algorithm. Within priority class 1, schedule according to Round Robin (RR) with quantum size 30. For priority class 2, schedule according to First-Come First-Serve (FCFS) algorithm. Note that each priority class maintains separate FIFO queue. Also note that, once a process starts executing, it continues until it completes (in FCFS) or it finishes the current quantum (in RR), even if a higher-priority process becomes ready.**

### Answer:

#### Specific Scheduling Rules:
- Priority 1: RR ($q=30$).
- Priority 2: FCFS (non-preemptive).
- Crucial Constraint: *Once a process starts executing, it continues until completion (FCFS) or quantum ends (RR), even if a higher-priority process arrives!*

#### Step-by-Step Chronological Execution:
1. **$t=0$:** Only $P_1$ (Priority 1) arrives with duration 30.
   - $P_1$ runs its 30s quantum (from $t=0$ to $t=30$).
   - **$P_1$ completes at $t=30$!** ($C_1 = 30$).
2. **$t=30$:** Priority 1 queue is empty.
   - At $t=30$, $P_2$ (Priority 2) arrives (duration 40).
   - $P_2$ starts running under Priority 2 (FCFS).
   - Because of the non-preemption rule, once $P_2$ starts, it runs until full completion ($t=30$ to $t=70$)!
   - *At $t=50$: $P_3$ (Priority 2, burst 65) arrives.* Enqueued in Priority 2 FIFO queue behind $P_2$.
   - *At $t=70$: $P_4$ (Priority 1, burst 70) arrives.*
   - **$P_2$ completes at $t=70$!** ($C_2 = 70$).
3. **$t=70$:**
   - Priority 1 has $P_4$ (arrived at 70).
   - Priority 2 has $P_3$ (arrived at 50).
   - Since Priority 1 > Priority 2, **$P_4$ is dispatched!**
   - $P_4$ runs quantum 30 (from $t=70$ to $t=100$). Remaining $P_4 = 40$.
4. **$t=100$:**
   - Only $P_4$ in Priority 1 queue.
   - $P_4$ runs quantum 30 (from $t=100$ to $t=130$). Remaining $P_4 = 10$.
5. **$t=130$:**
   - $P_4$ runs remaining 10s (from $t=130$ to $t=140$).
   - **$P_4$ completes at $t=140$!** ($C_4 = 140$).
6. **$t=140$:**
   - Priority 1 is now empty.
   - Dispatch from Priority 2 (FCFS): $P_3$ (burst 65).
   - $P_3$ runs until completion (from $t=140$ to $t=205$).
   - **$P_3$ completes at $t=205$!** ($C_3 = 205$).

#### Gantt Chart:
```
|    P1    |        P2        |    P4    |    P4    |  P4  |                P3                |
0         30                 70        100        130    140                               205
```

#### Metrics Table:
- $T_{\text{turn}}(P_1) = 30 - 0 = 30\text{ s}$
- $T_{\text{turn}}(P_2) = 70 - 30 = 40\text{ s}$
- $T_{\text{turn}}(P_4) = 140 - 70 = 70\text{ s}$
- $T_{\text{turn}}(P_3) = 205 - 50 = 155\text{ s}$
$$\mathbf{\text{Average Turnaround Time} = \frac{30 + 40 + 70 + 155}{4} = \frac{295}{4} = 73.75\text{ seconds}}$$

### Sources:
- `3. Scheduling-week-3-RRR.pdf` (Slides 12–50).

### Link to Notes:
- [[Interactive Scheduling Algorithms]]
- [[Batch Scheduling Algorithms]]
- [[Problem — CPU Scheduling Algorithm Simulation and Gantt Chart]]

---

## Question 2(b)
**Give an example demonstrating Convoy Effect that may occur due to FCFS scheduling algorithm.**

### Answer:
*(Identical canonical concept to 2020 Q2(a)):*
- Consider one massive CPU-bound job $P_1$ (burst 100 ms) and three small I/O-bound jobs $P_2, P_3, P_4$ (burst 2 ms each).
- When $P_1$ runs first under FCFS, $P_2, P_3, P_4$ sit waiting in the queue for 100 ms while all disk/network I/O devices sit idle.
- When $P_1$ finishes, the short jobs quickly finish their 2 ms bursts and block on I/O, leaving the CPU completely idle.
- This creates poor device utilization and inflates average waiting time from an optimal $2.5\text{ ms}$ (under SJF) to $76.5\text{ ms}$ (under FCFS).

### Sources:
- `3. Scheduling-week-3-RRR.pdf` (Slides 14–17).

### Link to Notes:
- [[Batch Scheduling Algorithms]]
- [[CPU Scheduling Principles and Criteria]]

---

## Question 2(c)
**Differentiate between non-preemptive SJF and preemptive SJF with an example.**

### Answer:

| Feature | Non-Preemptive SJF | Preemptive SJF (SRTF) |
|---|---|---|
| **Preemption Behavior** | Once a process is allocated the CPU, it runs to completion of its burst without interruption. | If a new process arrives with a remaining CPU burst shorter than the currently running process, the current process is **forcibly preempted**. |
| **Response Time** | Slower for newly arrived short tasks. | Excellent for short interactive tasks. |
| **Average Waiting Time** | Good, but suboptimal when long tasks start first. | **Mathematically optimal** average waiting and turnaround times. |

#### Comparative Example:
Consider processes $P_1$ (Arrival 0, Burst 8) and $P_2$ (Arrival 1, Burst 2):
- **Non-Preemptive SJF:** $P_1$ starts at 0 and cannot be preempted. It runs until $t=8$. Then $P_2$ runs from $t=8$ to $t=10$.
  $T_{\text{turn}}(P_2) = 10 - 1 = 9\text{ ms}$. Average turnaround $= \frac{8 + 9}{2} = 8.5\text{ ms}$.
- **Preemptive SJF (SRTF):** $P_1$ starts at 0. At $t=1$, $P_2$ arrives with burst 2. Since $2 < 7$ ($P_1$'s remaining burst), **$P_1$ is preempted!** $P_2$ runs from $t=1$ to $t=3$. Then $P_1$ resumes from $t=3$ to $t=10$.
  $T_{\text{turn}}(P_2) = 3 - 1 = 2\text{ ms}$. Average turnaround $= \frac{10 + 2}{2} = 6.0\text{ ms}$.

### Sources:
- `3. Scheduling-week-3-RRR.pdf` (Slides 26–36: SJF vs SRTF).

### Link to Notes:
- [[Batch Scheduling Algorithms]]
- [[Comprehensive CPU Scheduling Simulation Example]]

---

## Question 2(d)
**Four jobs have arrived at the same time in a batch system. Their expected run times are 9, 3, 5, and X. In what order should they be run to obtain optimal average turnaround time? (Hint: The jobs are non-preemptive.)**

### Answer:
*(Identical problem to 2019 Q1(b)):*
By the Shortest Job First optimality theorem, the optimal order sorts burst times in ascending order:
1. If $X \le 3$: $\mathbf{\langle X, 3, 5, 9 \rangle}$
2. If $3 < X \le 5$: $\mathbf{\langle 3, X, 5, 9 \rangle}$
3. If $5 < X \le 9$: $\mathbf{\langle 3, 5, X, 9 \rangle}$
4. If $X > 9$: $\mathbf{\langle 3, 5, 9, X \rangle}$

### Sources:
- `3. Scheduling-week-3-RRR.pdf` (Slides 26–31).

### Link to Notes:
- [[Batch Scheduling Algorithms]]

---

## Question 3(a)
**Define safe state and an unsafe state. Discuss the resource deadlock avoidance strategy of banker's algorithm for multiple resource types using the concept of safe state and unsafe state.**

### Answer:

#### Definitions:
- **Safe State:** A state where there exists at least one order (Safe Sequence $\langle P_1, \dots, P_n \rangle$) such that the maximum remaining resource needs of each process can be satisfied by the currently free resources plus the resources held by all preceding processes.
- **Unsafe State:** A state where no such sequence exists. If all processes request their maximum claims, the system cannot prevent deadlock.

#### Banker's Algorithm Avoidance Strategy:
Dijkstra's Banker's Algorithm avoids deadlocks by ensuring that the system **never enters an unsafe state**:
1. When a process $P_i$ issues a resource request $Request_i$:
   - The OS checks if $Request_i \le Need_i$ and $Request_i \le Available$.
2. The OS performs a **speculative (tentative) allocation**:
   $$Available = Available - Request_i$$
   $$Allocation_i = Allocation_i + Request_i$$
   $$Need_i = Need_i - Request_i$$
3. The OS runs the **Safety Check Algorithm** on this hypothetical state.
4. **The Decision Rule:**
   - If the resulting state is **SAFE**: The tentative allocation is committed; resources are granted to $P_i$.
   - If the resulting state is **UNSAFE**: The tentative allocation is rolled back; $P_i$ is suspended and must wait until resources are freed by other processes.

### Sources:
- `5. Deadlocks-week6-7-RRR.pdf` (Slides 25–31: Safe states, Banker's Algorithm).

### Link to Notes:
- [[Banker's Algorithm]]
- [[Deadlock Prevention and Avoidance Strategies]]

---

## Question 3(b)
**Mention the differences:**
- **(i) User mode and Kernel mode**
- **(ii) User space and Kernel Space**

### Answer:

#### (i) User Mode vs Kernel Mode (CPU Hardware Execution Modes):
- **User Mode (Ring 3):** The CPU execution state for normal applications. Privileged CPU instructions (disabling interrupts, direct I/O port instructions, writing to control registers like CR3) are prohibited and trigger hardware exceptions. Direct access to hardware is blocked.
- **Kernel Mode (Ring 0 / Supervisor Mode):** The privileged CPU execution state for the OS kernel. The CPU has unrestricted access to all machine instructions, physical memory pages, and hardware peripherals.

#### (ii) User Space vs Kernel Space (Virtual Memory Regions):
- **User Space:** The upper/lower portion of a process's virtual address space where user code, application heap, libraries, and user stacks reside. Process-isolated: each process has its own private user space.
- **Kernel Space:** The protected portion of the virtual address space reserved exclusively for the OS kernel code, kernel data structures (PCBs, page tables), and device drivers. Inaccessible when CPU is in user mode; shared across all processes.

### Sources:
- `1. Introduction-week1-RRR-2026.pdf` (Slides 20–25: Dual-mode operation).

### Link to Notes:
- [[Dual-Mode Operation and System Calls]]
- [[Operating System Structures and Functions]]

---

## Question 3(c)
**Differentiate between process context switching and thread context switching.**

### Answer:
*(Identical canonical comparison to 2019 Q4(b) and 2020 Q1(c)):*
1. **Memory Context:** Process Context Switching (PCS) switches virtual memory address spaces (updates page directory / CR3), which forces a complete TLB flush. Thread Context Switching (TCS) switches between threads within the same address space, requiring **no page table change and no TLB flush**.
2. **State Scope:** PCS saves and restores complete Process Control Blocks (PCBs), file descriptor tables, and IPC states. TCS saves and restores only private Thread Control Blocks (TCBs), registers, and stack pointers.
3. **Performance:** TCS is significantly faster with lower CPU cache-eviction overhead.

### Sources:
- `2. ProcessAndThread-week2-RRR.pdf` (Slides 15–18, 33–36).

### Link to Notes:
- [[Process Control Block and Context Switching]]
- [[Threads and Multithreading Models]]

---

## Question 3(d)
**Write down the steps of Booting a Computer.**

### Answer:
*(Identical canonical procedure to 2017 Q1(a)):*
1. **Power-On Reset:** CPU initializes program counter to ROM reset vector.
2. **POST (Power-On Self-Test):** Hardware checks (RAM, motherboard buses, video adapter).
3. **Boot Device Selection:** BIOS/UEFI queries CMOS settings for boot priority.
4. **MBR / Boot Sector Read:** Sector 0 (512 bytes) loaded into RAM at `0x7C00`.
5. **Stage 1 Bootloader:** MBR code executes and loads Stage 2 Bootloader (GRUB2).
6. **Stage 2 Bootloader & Mode Switch:** Switches CPU from real mode to protected/long mode, loads kernel image (`vmlinuz`) and `initramfs`.
7. **Kernel Initialization:** Initializes device drivers, virtual memory, mounts root filesystem, and launches initial user process (`systemd`, PID 1).

### Sources:
- `1. Introduction-week1-RRR-2026.pdf` (Slides 27–34).

### Link to Notes:
- [[Computer Booting and Hardware Abstractions]]

---

## Question 4(a)
**Peterson's solution for achieving mutual exclusion for using critical regions is presented in Figure for Question 4(a).**
- **(i) Discuss priority inversion problem with a high-priority process, H, and a low-priority process, L.**
- **(ii) Does the same problem occur if round-robin scheduling is used instead of priority scheduling? Justify.**

### Answer:
*(Identical problem to 2019 Q2(a)):*
- **(i) Priority Inversion:** If low-priority process $L$ enters the critical region, and high-priority process $H$ preempts $L$ and attempts to enter, $H$ enters a busy-wait loop (`while (turn == H && interested[L]);`). Under strict priority scheduling, because $H$ is ready and has higher priority, the scheduler **never schedules $L$**. Thus, $L$ cannot exit its critical section, and $H$ spins in an infinite loop forever.
- **(ii) Under Round Robin:** The deadlock does **NOT occur**. Round Robin forcibly preempts $H$ when its quantum expires and allocates the next quantum to $L$. Process $L$ completes its critical section, sets `interested[L] = FALSE`, allowing $H$ to proceed on its next quantum.

### Sources:
- `4. IPC-week-4-5-RRR.pptx` (Slides 16–28).

### Link to Notes:
- [[Peterson's Algorithm and Hardware Mutual Exclusion]]
- [[Race Conditions and Critical-Section Problem]]

---

## Question 4(b)
**A solution to the Dining Philosophers problem is given in Figure for Q 4(b).**
- **(i) Suppose that in the `put_forks(i)` function, variable `state[i]` was set to THINKING after the two calls to test, rather than before. How would this change affect the solution? Explain with an example.**
- **(ii) Suppose that in the `test(i)` function, `up(&s[i])` is placed outside the "if" condition. How would this change affect the solution?**

### Answer:
*(Identical problem to 2017 Q2(c)):*
- **(i) Setting `state[i] = THINKING` after `test()` calls:**
  When philosopher $i$ finishes eating, their state is still `EATING` when `test(LEFT)` and `test(RIGHT)` are called. The `test` routine checks if the neighbor's neighbors are eating. Because philosopher $i$ is still marked `EATING`, both test calls fail, and hungry waiting neighbors are **never awakened**! Phil $i$ only changes to `THINKING` after the tests have already completed, causing waiting neighbors to block indefinitely.
- **(ii) Placing `up(&s[i])` outside the `if` condition:**
  Semaphore `s[i]` is incremented unconditionally. In `take_forks(i)`, the philosopher executes `down(&s[i])`, which will immediately succeed without blocking even if their neighbors are actively eating. **Mutual exclusion is violated**, allowing two adjacent philosophers to eat simultaneously with the same chopstick.

### Sources:
- `4. IPC-week-4-5-RRR.pptx` (Slides 45–48).

### Link to Notes:
- [[Classic Synchronization Solutions]]
- [[Problem — Dining Philosophers Deadlock-Free Synchronization]]
