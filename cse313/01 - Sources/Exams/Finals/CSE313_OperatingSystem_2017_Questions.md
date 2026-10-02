# CSE 313 (Operating Systems) - 2017

**1(a).** Write down the steps of booting a computer.

**1(b).** Consider the code written in C shown in the following figure. Here `fork()` is an UNIX system call that creates a child process identical to the parent. Executing this code will generate a process tree. Each of the created processes will have its own copy of variable i. Your task is to draw this process tree. At each node of the tree, you have to mention the starting value of i for the corresponding process. Consider that the root process is called P0.

```c
int i = 0;
int main() {
    for (; i < 3; i++) {
        fork();
    }
    return 0;
}
```

**1(c).** Write down the steps in making a system call.

**1(d).** What are the advantages of hybrid implementation of threads?

**2(a).** State the four conditions of resource deadlock.

**2(b).** State the problem definition of the classical Dining Philosophers problem. Show that the problem meets all the four conditions for resource deadlock stated in 2(a).

**2(c).** A solution to the Dining Philosophers problem is given in Figure for Q2(c).

- (i) Suppose that in the `put-forks(i)` function, variable `state[i]` was set to THINKING after the two calls to test, rather than before. How would this change affect the solution? Explain with an example.
- (ii) Suppose that in the `test(i)` function, `up(&s[i])` is placed outside the "if" condition. How would this change affect the solution?

Figure for Q2(c):

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

**2(d).** Explain the race-condition that exists in the following solution to the producer-consumer problem.

Producer:

```c
#define N = 100
int count = 0;
void producer(void) {
    int item;
    while (TRUE) {
        item = produce();
        if (count == N) sleep();
        inset(item);
        count = count + 1;
        if (count == 1)
            wakeup(consumer);
    }
}
```

Consumer:

```c
void consumer(void) {
    int item;
    while (TRUE) {
        if (count == 0) sleep();
        item = remove-item();
        count = count - 1;
        if (count == N-1)
            wakeup(producer);
        consume(item);
    }
}
```

**3(a).** A system has four processes and five allocatable resources. The current allocation and maximum needs are as follows:

| Process | Allocated | Maximum |
| --- | --- | --- |
| Process A | 1 0 2 1 1 | 1 1 2 1 3 |
| Process B | 2 0 1 1 0 | 2 2 2 1 0 |
| Process C | 1 1 0 1 0 | 2 1 3 1 0 |
| Process D | 1 1 1 1 0 | 1 1 2 2 1 |

Available: 0 0 x 1 1

What is the smallest value of x for which this is a safe state? Show the intermediate steps.

**3(b).** What is the difference between livelock and starvation?

**3(c).** Draw the resource graph for the following scenario where A, B, C, D, and E denote processes and 1, 2, 3, 4, and 5 denote resource types. There exists only one resource of each type. Show the steps of the execution of the deadlock detection algorithm on the constructed graph starting from node B.

- (i) Process A holds 1 and 3, wants 2
- (ii) Process B holds 4, wants 3 and 5
- (iii) Process C holds nothing, wants 2
- (iv) Process D holds nothing, wants 2
- (v) Process E holds 5 and wants 1

**4(a).** Discuss the difference between compute-bound and I/O bound process with figures.

**4(b).** Consider the following workload:

| Process | Priority | Duration (sec) | Arrival Time (sec) |
| --- | --- | --- | --- |
| P1 | 3 | 70 | 40 |
| P2 | 1 | 40 | 0 |
| P3 | 1 | 100 | 10 |
| P4 | 2 | 50 | 70 |

Draw the Gantt chart and calculate the average turnaround time for each of the following scheduling algorithms:

- (i) Non-preemptive Shortest Job First
- (ii) Round Robin with quantum 30 sec. Consider the processes enter FIFO queue according to their approval times.
- (iii) Priority Scheduling. Within the same priority class, schedule according to the scheduling algorithm mentioned in (ii).

**4(c).** Write down the goals of scheduling algorithms for Batch Systems.

Unless otherwise specified, 1 k = 2¹⁰, 1 M = 2¹⁰, 1 G = 2³⁰.

**5(a).** In 64-bit systems, 48 bit addressing is usually used. Calculate the amount of space needed (in GB) for a single-level page table for 48 bit addressing if the page size is 4 kB and each page table entry takes 4 bytes. Discuss the feasibility of such a page table and mention two (2) better alternatives.

**5(b).** Describe the advantages and disadvantages of memory-mapped I/O.

**5(c).** Draw an omega switching network for 8 CPUs and 8 memory modules. Determine whether the following requests can be processed in parallel in this omega switching network and explain the reason.

Request from CPU 000 to Memory 111 and

Request from CPU 110 to Memory 100

**6(a).** The i-node of a Unix-like file system has 12 direct, one single-indirect and one double-indirect pointers. The disk block size is 4 kB and the disk block address is 32-bits long. Calculate the maximum possible file size for this file system. (Note that a single-indirect pointer points to a block of pointers that then point to blocks of the file's data. A double-indirect pointer points to a block of pointers that point to other blocks of the pointers that then point to blocks of the file's data.)

**6(b).** In the snapshot of memory given below, the dotted areas indicate holes and A, B, C, D, E, F are processes currently in memory. Create a linked list of processes and holes for swapping, assuming that there is no virtual memory.

Memory snapshot, from left to right (each tick interval is one allocation unit):

| Region | Allocation units |
| --- | --- |
| A | 7 |
| B | 11 |
| Hole (dotted) | 3 |
| C | 10 |
| Hole (dotted) | 6 |
| D | 8 |
| Hole (dotted) | 7 |
| E | 5 |
| Hole (dotted) | 13 |
| F | 15 |
| Hole (dotted) | 4 |
| Continuation beyond the diagram break | |

A new process named G has to be allocated in memory now. The size of G is 4 allocation units. Mention the hole where G will be allocated if we follow:

- (i) First-fit
- (ii) Best-fit

**6(c).** Define stable storage and mention the assumptions associated with it. Describe the three (3) basic operations a stable storage ensure.

**7(a).** Define precise interrupt and mention its four (4) properties. Describe the disadvantages of precise interrupts.

**7(b).** Mention the three (3) most essential properties of files. Explain how contiguous allocation of files may create both internal and external fragmentation in disk.

**7(c).** Mention the three key characteristics of a NUMA machine. In a directory-based NUMA multiprocessor having 1024 nodes, each memory address contains 48 bits. Each node consists of one CPU and 4 GB of RAM connected to the CPU via a local bus. Each cache line is 128 bytes long. The memory is statically allocated among the nodes. Calculate the directory overhead in percentage with respect to memory and discuss the feasibility of this system.

**8(a).** In a hypothetical machine, there are a total of 8 physical page frames.

| Page Table Index | Time of Last Use (t) | Referenced Bit | Modified Bit |
| --- | --- | --- | --- |
| 0 | 214 | 1 | 1 |
| 1 | 381 | 0 | 1 |
| 2 | 402 | 1 | 0 |
| 3 | 289 | 1 | 1 |
| 4 | 409 | 0 | 0 |
| 5 | 160 | 1 | 1 |
| 6 | 315 | 1 | 0 |
| 7 | 387 | 0 | 1 |

At t = 430 and t = 440, two page faults occur. Using working set page replacement algorithm (with execution time approximation), identify the page frames that will be evicted. Assume the age threshold (τ) to be 50. Also assume that the clock interrupt to clear referenced bit last occurred at t = 425, and this interrupt occurs at 20 units time interval.

**8(b).** Describe one (1) major advantage and one (1) major disadvantage of DMA. Briefly explain the major functions of device independent I/O software.

**8(c).** Write the full forms of the following abbreviations:

NFTS, SMP, ECC, MBR, UDF
