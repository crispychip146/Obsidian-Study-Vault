# CSE 313 (Operating System) - 2021

**1(a).** Consider a system with 4 processes: P1 through P4 and 3 resource types: A (9 units), B (3 units), and C (6 units). Current allocation of resources and maximum requirement of a process for each resource are given in Figure for Question 1(a). Evaluate whether the current state of this system is safe or unsafe.

Note that the resources are non-preemptive and a process releases all its acquired resources when it runs to completion. Also note that all of the four processes may suddenly request their maximum number of resources immediately:

Current Allocation Matrix:

| Process | A | B | C |
| --- | --- | --- | --- |
| P1 | 4 | 2 | 3 |
| P2 | 2 | 0 | 1 |
| P3 | 1 | 0 | 1 |
| P4 | 2 | 1 | 0 |

Maximum Requirement Matrix:

| Process | A | B | C |
| --- | --- | --- | --- |
| P1 | 6 | 2 | 5 |
| P2 | 2 | 0 | 2 |
| P3 | 6 | 3 | 3 |
| P4 | 7 | 1 | 6 |

**1(b).** Illustrate the position of process/thread scheduler, process table, and thread table in user-level thread implementation and kernel-level thread implementation with diagrams. With the help of your diagrams, analyze that kernel-level thread implementation is better than user-level thread implementation with respect to blocking.

**1(c).** Explain how to attack the "Circular wait condition" to structurally prevent deadlock.

**2(a).** Considerations the following workload:

| Process | Priority (Lowest Number has Highest Priority) | Duration (sec) | Arrival Time (sec) |
| --- | --- | --- | --- |
| P1 | 1 | 30 | 0 |
| P2 | 2 | 40 | 30 |
| P3 | 2 | 65 | 50 |
| P4 | 1 | 70 | 70 |

Construct the Gantt chart and calculate the average turnaround time for Priority Scheduling algorithm. Within the priority class 1, schedule according to Round Robin (RR) with quantum size 30. For the priority class 2, schedule according to First-Come First-Serve (FCFS) algorithm. Note that each priority class maintains separate FIFO queue. Also note that, once a process starts executing, it continues until it completes (in FCFS) or it finishes the current quantum (in RR), even if a higher-priority process becomes ready.

**2(b).** Give an example demonstrating Convoy Effect that may occur due to FCFS scheduling algorithm.

**2(c).** Differentiate between non-preemptive SJF and preemptive SJF with an example.

**2(d).** Four jobs have arrived at the same time in a batch system. Their expected run times are 9, 3, 5, and X. In what order should they be run to obtain optimal average turnaround time? (Hint: The jobs are non-preemptive.)

**3(a).** Define safe state and an unsafe state. Discuss the resource deadlock avoidance strategy of banker's algorithm for multiple resource types using the concept of safe state and unsafe state.

**3(b).** Mention the differences.

- (i) User mode and Kernel mode
- (ii) User space and Kernel Space

**3(c).** Differentiate between process context switching and thread context switching.

**3(d).** Write down the steps of Booting a Computer.

**4(a).** Peterson's solution for achieving mutual exclusion for using critical regions is presented in Figure for Question 4(a).

- (i) Discuss priority inversion problem with a high-priority process, H, and a low-priority process, L.
- (ii) Does the same problem occur if round-robin scheduling is used instead of priority scheduling? Justify.

Figure for Question 4(a): Peterson's solution for 2 processes:

```c
#define FALSE 0
#define TRUE  1
#define N     2                  /* number of processes */

int turn;                       /* whose turn is it? */
int interested[N];              /* all values initially 0 (FALSE) */

void enter_region(int process); /* process is 0 or 1 */
{
    int other;                  /* number of the other process */

    other = 1 - process;        /* the opposite of process */
    interested[process] = TRUE; /* to show that you are interested */
    turn = process;             /* set flag */
    while (turn == process && interested[other] == TRUE) /* null statement */ ;
}

void leave_region(int process)  /* process: who is leaving */
{
    interested[process] = FALSE; /* indicate departure from critical region */
}
```

**4(b).** A solution to the Dining Philosophers problem is given in Figure for Q 4(b).

- (i) Suppose that in the `put_forks(i)` function, variable `state[i]` was set to THINKING after the two calls to test, rather than before. How would this change affect the solution? Explain with an example.
- (ii) Suppose that in the `test(i)` function, `up(&s[i])` is placed outside the "if" condition. How would this change affect the solution?

Figure for Question 4(b): A solution to the Dining Philosophers Problem:

```c
#define N        5             /* number of philosophers */
#define LEFT     (i+N-1)%N     /* number of i's left neighbor */
#define RIGHT    (i+1)%N       /* number of i's right neighbor */
#define THINKING 0             /* philosopher is thinking */
#define HUNGRY   1             /* philosopher is trying to get forks */
#define EATING   2             /* philosopher is eating */

typedef int semaphore;        /* semaphores are a special kind of int */
int state[N];                 /* array to keep track of everyone's state */
semaphore mutex = 1;          /* mutual exclusion for critical regions */
semaphore s[N];               /* one semaphore per philosopher */

void philosopher(int i)       /* i: philosopher number, from 0 to N-1 */
{
    while (TRUE) {            /* repeat forever */
        think();              /* philosopher is thinking */
        take_forks(i);        /* acquire two forks or block */
        eat();                /* yum-yum, spaghetti */
        put_forks(i);         /* put both forks back on table */
    }
}

void take_forks(int i)        /* i: philosopher number, from 0 to N-1 */
{
    down(&mutex);             /* enter critical region */
    state[i] = HUNGRY;        /* record fact that philosopher i is hungry */
    test(i);                  /* try to acquire 2 forks */
    up(&mutex);               /* exit critical region */
    down(&s[i]);              /* block if forks were not acquired */
}

void put_forks(i)             /* i: philosopher number, from 0 to N-1 */
{
    down(&mutex);             /* enter critical region */
    state[i] = THINKING;      /* philosopher has finished eating */
    test(LEFT);               /* see if left neighbor can now eat */
    test(RIGHT);              /* see if right neighbor can now eat */
    up(&mutex);               /* exit critical region */
}

void test(i)                 /* i: philosopher number, from 0 to N-1 */
{
    if (state[i] == HUNGRY && state[LEFT] != EATING && state[RIGHT] != EATING) {
        state[i] = EATING;
        up(&s[i]);
    }
}
```

**4(c).** State the four requirements that must be present in a solution we design to ensure mutual exclusion among processes sharing resources. Give an example solution that violates any of the four requirements.

**5(a).** Consider a 32-bit virtual address space with 4KB pages. Each page table entry (PTE) or page directory entry (PDE) is 4-byte. A 3-level paging scheme is used where the virtual address is divided into 8 bits for level-1, 6 bits for level-3, 6 bits for level-3, and 12 bits for the page offset.

A process uses virtual pages 0-3 for code, 1024-2047 for heap, and (2²⁰ − 4) to (2²⁰ − 1) for stack. The rest of the pages are unused.

- (i) If a single-level page table is used, determine the total amount memory required to store only the page table.
- (ii) If a 3-level page table is used, compare the total amount of memory used by the page table structures for this process.
- (iii) For the 3-level page table, identify and explain any memory wastage due to internal or external fragmentation in our current setting. Determine the reason and propose a possible solution to mitigate such wastage.

**5(b).** Formulate the following equation for segment size in Log-structured File System (LFS), considering the notations as conventional ones.

`D = [F / (1 − F)] × R_peak × T_position`

Suppose the disk spends 4ms time for positioning on an average and has 120 MB/s of transfer rate. What will be the segment size if we want effective rate to be 80%?

**5(c).** When using the Clock Page Replacement Algorithm, dirty pages require special care since they must be written back to disk before being evicted.

Design an enhanced Clock algorithm that efficiently handles dirty pages. In your approach, clearly explain.

- (i) What additional bits or flags may be needed in the page table entry (PTE)?
- (ii) What support is required from hardware (e.g., MMU)?
- (iii) What modifications are made to the standard Clock algorithm steps to handle dirty pages?

You may optionally illustrate your algorithm with a diagram or pseudocode for better clarity.

**6(a).** A process references the following sequence of pages:

1, 2, 3, 4, 1, 2, 5, 1, 2, 3, 4, 5

The system has 3 page frames available for this process. Assume that, all frames are initially empty.

Simulate the execution of the Clock page replacement algorithm for the given page references. Show the state of the memory after each page reference, including the reference bit and the clock hand position. Count the total number of page faults that occur during the simulation.

**6(b).** Alice was studying different RAID systems. She particularly liked RAID level 5 for its all-around performance. However, one aspect she didn't like was that RAID 5 cannot tolerate more than one disk failure. So, she took matters into her own hands and designed a modified RAID level, which she called RAID-5X. As you may recall, in RAID 5, data and parity are striped across all disks. In RAID-5X, the only modification is that parity blocks are written twice:

- (I) Once in the usual rotating parity position.
- (II) Again, on a fixed disk dedicated to parity backup.

Now, Alice knows you are an OS expert. She has come to you to validate her system.

- (i) Will the reliability increase in practice?
- (ii) What will be the effective capacity of the system?
- (iii) Analyze the performance under random and sequential read and write workloads.

**6(c).** Suppose you are using LFS. Now, 4 blocks were written to file *A.txt* and 3 blocks were written to *B.txt*. *A.txt* and *B.txt* both reside in a directory *exam*. Draw the current state of the corresponding segment.

**7(a).** A network interface card (NIC) in a server receives data packets at a high frequency. The system designer is trying to decide between using polling or interrupt-driven I/O for packet handling. Recommend which method would be more suitable if the packet arrival rate is:

- (i) Low and bursty, and
- (ii) High and regular

Justify your answer.

**7(b).**

- (i) Crash Scenario A: You are in metadata journaling mode. Crash occurs after data block D is written to disk but before metadata journal commit record is written. What is the on-disk state after reboot? Is it consistent? Could stale metadata point to new or invalid data?
- (ii) Crash Scenario B: You are in metadata journaling mode. Crash occurs after the metadata commit record is written but before data block D is flushed to disk. What inconsistency can arise?
- (iii) Crash Scenario C: Now assume data journaling mode. Crash occurs after the data block is journaled but before it is written to its home location. Will recovery result in a consistent state? Explain why.
- (iv) Compare metadata journaling vs data journaling for performance and durability:

  - Which is faster, and why?
  - Which guarantees that users will never see garbage data after a crash?

**7(c).** Bob was studying different kinds of journaling techniques, including data journaling and metadata journaling. He was fascinated to learn about the corner case where the disk may crash during journal writes. To handle such situations, the concept of transaction commit was introduced - where the TxE block is written only after all previous blocks in the journal transaction are successfully written. However, Bob was concerned that waiting for all previous blocks to be written before committing the transaction would make writes too slow. So, he proposed a new solution:

*A 32-bit checksum will be computed over all the blocks in the transaction, excluding the TxB (Transaction Begin) and TxE (Transaction End) blocks. This checksum will be stored in both the TxB and TxE blocks. After a crash, the validity of the journal transaction can be verified by computing the checksum of the journaled data and comparing it against the checksums in TxB and TxE.*

Now, Bob knows you are an OS expert. He has come to you to validate his journaling system.

- (i) Does the proposed solution have any correctness issues for both data journaling and metadata journaling?
- (ii) Will the write performance really improve? Are there any hidden drawbacks?

**8(a).** The system has a total of 1024 KB of memory managed using the Buddy Memory Allocation algorithm. The minimum allocatable block size is 64 KB. Assume no memory is used initially. A sequence of memory allocation requests comes in:

- (I) Request A: 200 KB
- (II) Request B: 100 KB
- (III) Request C: 300 KB
- (IV) Then Request A is freed.

Now answer the following.

- (i) Show how the memory is divided to fulfill these requests using the Buddy system.
- (ii) After A is freed, explain if Buddy merging is possible. If so, show the new memory layout.
- (iii) What is the fragmentation after all allocations and deallocation? (Internal + External, if any)

**8(b).** Bob has created the following files in an FFS (file sizes are written in brackets).

- /a/f1 (3KB)
- /b/f2 (5KB)
- /c/b/f3 (9KB)
- /c/f4 (20KB)
- /b/f5 (10KB)

Suppose, the disk has 4KB blocks and 5 block groups. Also the chunk size is 4 blocks.

If /c/b is a symbolic link to the directory /b (we are uplifting the limitation) and the files were created in order of they appear, show how the files will be saved with illustrative diagram(s).

**8(c).** Mention the two crash scenarios that may arise during writing to disk in LFS. What is the need for roll forward?
