# CSE 313 (Operating Systems) - 2020

**1(a).** Consider the following workload:

| Process | Priority (Lowest Number has Highest Priority) | Duration (sec) | Arrival Time (sec) |
| --- | --- | --- | --- |
| P1 | 1 | 30 | 0 |
| P2 | 2 | 40 | 30 |
| P3 | 2 | 65 | 50 |
| P4 | 1 | 70 | 70 |

Construct the Gantt chart and calculate the average turnaround time for Priority Scheduling algorithm. Within the same priority class, schedule according to Round Robin with quantum size q. For the first 5 quantums, quantum size q = 20 secs; and for the rest of the quantums, quantum size q = 30 secs. Note that each priority class maintains separate FIFO queue.

**1(b).** Explain the problem definition of the classical Dining Philosophers problem. Argue that the Dining Philosophers problem meets all the four conditions for resource deadlock.

**1(c).** Differentiate between process context switching (PCS) and thread context switching (TCS).

**2(a).** Differentiate between a CPU bound process (CPUB) and an IO bound process (IOB). Give an example demonstrating Convoy Effect that may occur due to FCFS scheduling algorithm.

**2(b).** Consider a variation of the classical producer consumer problem. In this variation, there is only one consumer and M producers P₁, P₂, …, `P_M` and all producers belong to the same priority class. Producers produce item in a cyclic order. For example, producer P₁ produces first, then producer P₂, and so on. After producer `P_M` produces an item, it enables producer P₁ to produce an item. Note that, the order of producing items and the order of insertion in the buffer may not always be the same.

A solution to the classical Producer-Consumer problem is presented in Figure for 2(b). Modify this solution with minimal changes to obtain a solution for the above mentioned problem. [You do not need to write down the consumer code.]

Figure for 2(b). Producer-Consumer problem using semaphores:

```c
#define N 100                 /* number of slots in the buffer */
typedef int semaphore;        /* semaphores are a special kind of int */
semaphore mutex = 1;          /* controls access to critical region */
semaphore empty = N;          /* counts empty buffer slots */
semaphore full = 0;           /* counts full buffer slots */

void producer(void)
{
    int item;
    while (TRUE) {           /* TRUE is the constant 1 */
        item = produce_item(); /* generate something to put in buffer */
        down(&empty);        /* decrement empty count */
        down(&mutex);        /* enter critical region */
        insert_item(item);   /* put new item in buffer */
        up(&mutex);          /* leave critical region */
        up(&full);           /* increment count of full slots */
    }
}

void consumer(void)
{
    int item;
    while (TRUE) {           /* infinite loop */
        down(&full);         /* decrement full count */
        down(&mutex);        /* enter critical region */
        item = remove_item(); /* take item from buffer */
        up(&mutex);          /* leave critical region */
        up(&empty);          /* increment count of empty slots */
        consume_item(item);  /* do something with the item */
    }
}
```

**2(c).** Explain the advantages and disadvantages of kernel-level thread.

**3(a).** Construct the resource graph for the following scenario where A, B, C, and D denote processes and 1, 2, 3, 4, 5, and 6 denote resource types. There exists only one resource of each type. Show the steps of the execution of the deadlock detection algorithm on the constructed graph starting from node C.

- (i) Process A holds 6, and wants 1 and 3
- (ii) Process B holds 1, and wants 4
- (iii) Process C holds 4, and wants 3 and 5
- (iv) Process D holds 5, and wants 6

**3(b).** Distinguish between a safe state and an unsafe state. Consider a system with 4 processes: P1 through P4 and 1 resource type with 20 instances: Current allocation of resources and maximum requirement of each process are given in Figure for Question 3(b). Determine whether the current state of this system is safe or not. Show the intermediate steps.

| Process | Has | Max |
| --- | --- | --- |
| P1 | 5 | 9 |
| P2 | 6 | 12 |
| P3 | 2 | 8 |
| P4 | 0 | 15 |

Free: 7

**3(c).** Suppose that there is a resource deadlock in a system. Devise an example scenario to demonstrate that the set of processes deadlocked can include some processes that are not in the circular chain in the corresponding resource allocation graph.

**4(a).** Draw the state diagram of a process life cycle. Mention the respective states of a process when (i) it is involved in a starvation, and when (ii) it is involved in a livelock.

**4(b).** Consider the given code written in C. Here `fork()` is an UNIX system call that creates a child process identical to the parent. Executing this code will generate a process tree. Each of the created processes will have its own copy of variable i. Your task is to draw this process tree. At each node of the tree, you have to mention the starting value of i for the corresponding process. Consider the root process is called P₀.

```c
int i = 0;
int main() {
    for (; i < 3; i++)
    {
        fork();
    }
    return 0;
}
```

**4(c).** Write short notes on the following concepts.

- (i) Multi-user OS
- (ii) Multi-processor OS
- (iii) Lottery scheduling

**5(a).** Imagine a small address space of size 16KB, with 64-byte pages. Assume each PTE is 4 bytes. A process uses virtual pages 0 and 1 for code segment, virtual pages 4 to 32 for the heap, and virtual pages 254 and 255 for the stack; the rest of the pages of the address space are unused.

- (i) If we use single-level page table, determine the total amount of physical memory used by the process?
- (ii) If we use two-level page table, determine the total amount of physical memory used by the process?

**5(b).** Formulate the equation for segment size in Log-structured File System (LFS).

`D = [F / (1 − F)] × R_peak × T_position`

Suppose the disk spends 5ms time for positioning on average and has 100MB/s of transfer rate. Calculate the segment size if we want effective rate to be 90%.

**5(c).** Analyze the limitations of the Least Recently Used (LRU) implementation as a page replacement policy. How does the Clock algorithm address these limitations? Provide the steps of the Clock algorithm along with an illustrative figure. Additionally, explain the special treatment given to dirty pages in the Clock algorithm and identify the rationale behind this treatment.

**6(a).** Alice designed a modified version of RAID-4 to increase reliability by using two parity disks: one for odd-numbered disks and another for even-numbered disks. Calculate the storage capacity, reliability, and throughput for this modified RAID-4 configuration.

**6(b).** Can there be multiple entries with the same Virtual Page Number (VPN) in a Translation Lookaside Buffer (TLB)? If so, how can this issue be resolved?

**6(c).** Mention the use of dirty bit, present bit and reference bit in a page table entry (PTE).

**6(d).** What is a page fault, and how is thrashing related to it?

**6(e).** Explain why hardlinks can't work across different file systems. How can symbolic link solve this limitation?

**7(a).** Alice has implemented a file system where each inode contains 6 direct pointer, 2 single indirect pointers, and 5 double indirect pointers. Given a disk size of 16 TB with 4 KB blocks, calculate the maximum size of a file that can be managed using this file system. Assume that the indirect pointer blocks are fully utilized, holding the maximum possible number of block addresses.

**7(b).** In VSFS, a new file ('/foo/bar') was created and some text was appended. Mention the order of reading and writing to the blocks. You have to fill up the Figure for 7(b).

Figure for 7(b). File Creation Timeline (Time Increasing Downward):

| Operation | data bitmap | inode bitmap | root inode | foo inode | bar inode | root data | foo data | bar data [0] | bar data [1] | bar data [2] |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| create (/foo/bar) | | | | | | | | | | |
| write() | | | | | | | | | | |

**7(c).** Are interrupts always preferable to polling? If so, why? If not, why?

**7(d).** Is DMA aware of memory virtualization? If not, how can the operating system effectively utilize DMA in a virtualized memory environment?

**8(a).** Bob has created the following files in an FFS (file sizes are written in brackets).

- /a/f1 (3KB)
- /b/f2 (5KB)
- /c/b/f3 (9KB)
- /c/f4 (7KB)
- /b/f5 (10KB)

Suppose, the disk has 4KB blocks and 5 block group. If /c/b is a symbolic link to the directory /b (we are uplifting the limitation) and the files were created in order of they appear, show how the files will be saved with illustrative diagram.

**8(b).** Why do we need checkpoint region in LFS but not in VSFS?

**8(c).** What is a Remote Procedure Call (RPC)? Explain the concept and illustrate it with a diagram.

**8(d).** Suppose you are using a Log-structured File System (LFS). Given the initial layout of the file shown in Figure for 8(d), if a byte in the file is modified, what changes will occur? Finally, draw the updated layout after the modification.

Figure for 8(d). Layout of a file in LFS:

| Disk order | Block | Contents |
| --- | --- | --- |
| 1 | Data block at A0 | D |
| 2 | Inode block | I; b[0]: A0 (points to the data block at A0) |
| 3 onward | Unused space | |
