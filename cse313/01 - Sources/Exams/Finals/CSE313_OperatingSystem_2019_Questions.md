# CSE 313 (Operating System) - 2019

**1(a).** Consider the following workload:

| Process | Priority (Lowest Number has Highest Priority) | Duration (sec) | Arrival Time (sec) |
| --- | --- | --- | --- |
| P1 | 1 | 60 | 0 |
| P2 | 1 | 25 | 30 |
| P3 | 2 | 80 | 50 |
| P4 | 3 | 20 | 70 |

Draw the Gantt chart and calculate the average turnaround time for each of the following scheduling algorithms:

- Shortest Remaining Time Next
- Priority Scheduling. Within the same priority class, schedule according to Round Robin with quantum 20 sec. Each priority class maintains separate FIFO queue.

**1(b).** Four jobs have arrived at the same time in a batch system. Their expected run times are 9, 3, 5, and X. In what order should they be run to obtain optimal average turnaround time? (Hint: The jobs are non-preemptive)

**1(c).** Differentiate between an unsafe state and a deadlock state.

**2(a).** Peterson's solution for achieving mutual exclusion for using critical regions is presented in Figure for Question 2(a):

- (i) Discuss priority inversion problem with a high-priority process, H, and a low-priority process, L.
- (ii) Does the same problem occur if round-robin scheduling is used instead of priority scheduling? Justify.

Figure for Question 2(a): Peterson's solution for 2 processes:

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
    interested[process] = TRUE; /* show that you are interested */
    turn = process;             /* set flag */
    while (turn == process && interested[other] == TRUE) /* null statement */ ;
}

void leave_region(int process)  /* process: who is leaving */
{
    interested[process] = FALSE; /* indicate departure from critical region */
}
```

**2(b).** In the dining philosophers problem, let the following protocol be used: An even-numbered philosopher always picks up his left fork before picking up his right fork; an odd-numbered philosopher always picks up his right fork before picking up his left fork. Investigate whether this modified protocol prevents deadlock.

**2(c).** State the four requirements that must be present in a solution to support mutual exclusion among processes sharing resources.

**2(d).** Differentiate between microkernel and monolithic kernel architectures.

**3(a).** Consider a system with 4 processes: P1 through P4 and 3 resources types: A (9 units), B (3 units), C (6 Units). Current allocation of resources and maximum requirement of a process for each resource are given in Figure for Question 3(a). Determine whether the current state of this system is safe or not.

Current Allocation Matrix:

| Process | A | B | C |
| --- | --- | --- | --- |
| P1 | 1 | 0 | 0 |
| P2 | 6 | 1 | 2 |
| P3 | 2 | 1 | 1 |
| P4 | 0 | 0 | 2 |

Maximum Requirement Matrix:

| Process | A | B | C |
| --- | --- | --- | --- |
| P1 | 3 | 2 | 2 |
| P2 | 6 | 1 | 3 |
| P3 | 3 | 1 | 4 |
| P4 | 4 | 2 | 2 |

**3(b).** Suppose that there is a resource deadlock in a system. Give an example scenario to show that the set of processes deadlocked can include some processes that are not in the circular chain in the corresponding resource allocation graph.

**3(c).** Explain how to attack the following conditions to structurally prevent deadlock:

- (i) Hold and wait condition
- (ii) Circular wait condition

**4(a).** Write down the steps in making a system call.

**4(b).** Differentiate between process context switching and thread context switching. Identify which of the following are "per process item" and which are "per thread item"?

Program Counter, Stack, Address Space, Global Variables, Registers, Directories, Child Process, Local Variables

**4(c).** Illustrate the position of scheduler, process table and thread table in user-level thread implementation and kernel-level thread implementation with diagrams. With the help of your diagrams, justify that kernel-level thread implementation is better than user-level thread implementation with respect to blocking.

**5(a).** The following sequence of Virtual Page Numbers (VPN) has been referenced in your system.

1, 2, 3, 4, 5, 2, 3, 1, 2, 3, 4, 5, 1

Now, answer the following questions.

- (i) Explain Belady's anomaly. Show that this anomaly occurs for cache size 3 and 4 in the above case.
- (ii) Calculate hit and miss rate for the optimal page replacement algorithm.
- (iii) If the TLB can keep at most 4 entries and employs LRU for TLB replacement and Page replacement uses CLOCK algorithm with cache size 4. Calculate the memory access time for the above memory access sequence. TLB access takes 5ns, memory access takes 60ns, and disk access takes 4ms.

Hint: The first 5 memory references in CLOCK algorithm gives the following cache states:

| Access | Cache State (After): Slot 1 | Slot 2 | Slot 3 | Slot 4 |
| --- | --- | --- | --- | --- |
| 1 | 1(1)* | ?(0) | ?(0) | ?(0) |
| 2 | 1(1) | 2(1)* | ?(0) | ?(0) |
| 3 | 1(1) | 2(1) | 3(1)* | ?(0) |
| 4 | 1(1) | 2(1) | 3(1) | 4(1)* |
| 5 | 5(1)* | 2(0) | 3(0) | 4(0) |

Here, "?" refers to unused cache, the number in bracket refers to use bit and "*" is the location of the clock hand.

**5(b).** Both ffs and lfs try to optimize on vsfs in some ways. State and elaborate the difference in their optimization criteria.

**6(a).** The following function writes a sequence of n numbers to a given file descriptor fd.

```c
void write_fd(int fd, int n) {
    char buf[10];
    for (int i = 0; i < n; i++) {
        itoa(i, buf, 10);
        write(fd, buf, strlen(buf));
    }
}
```

(i) For each of the following possible values of n, design a device driver where `write_fd` is called frequently with n. Explain your design decisions using illustrative pseudocodes.

- (a) always 10
- (b) always 1000000
- (c) mixture of 10 and 1000000

(ii) What is the difference of output from the following two code blocks? Analyze with necessary figures and arguments showing what happens in the kernel.

Code block 1:

```c
int fd1 = open("file.txt");
int fd2 = open("file.txt");
write_fd(fd1);
write_fd(fd2);
```

Code block 2:

```c
int fd1 = open("file.txt");
int fd2 = dup(fd1);
write_fd(fd1);
write_fd(fd2);
```

**6(b).** Memory allocation API consists mainly of the following two functions:

```c
void *malloc(size_t size);
void free(void *ptr);
```

With illustrative examples, show how the allocation API works. Mention the necessary bookkeeping structure(s) it requires.

**7(a).** You have executed the following commands in the root of an ffs file system with 4 byte block number and 4 KB block size. The disk has an average disk-arm positioning time of 10 ms and max transfer rate of 100 MB/s.

```sh
mkdir p
mv /q/foo.txt /p/bar.txt
```

Here, `foo.txt` is of size 1 GB. Now answer the following questions.

(i) Given, bitmaps are in block 4, inodes are in block 5 and data for the root directory is in block 6. Also, the journal is kept in blocks 26 to 31. Determine the journaling timeline if the system uses:

- (a) Data Journaling
- (b) Metadata Journaling
- (c) Metadata Journaling with Checksum Optimization

(ii) How will this file be laid out in ffs? How does ffs get the relevant information needed to decide that? Calculate how long it will take to sequentially read the whole file. You can assume, there is enough space to save the file in ffs layout and only moving from one block group to another requires a disk-arm positioning.

**7(b).** What is dynamic relocation and segmentation in the context of memory management? Why would you choose one over another? Using pseudocodes, elaborate the address translation process used by these two systems.

**8(a).** You have set up a RAID system with 10 disks and 4KB block size. You are using lfs as the default file system where segment size is 64 MB. The disk config is as follows:

| Property | Value |
| --- | --- |
| Capacity | 1TB |
| Rotation speed | 10,000 RPM |
| Max seek time | 12 ms |
| Max transfer rate | 100 MB/s |
| Platters | 2 |
| Sector size | 1 KB |
| Cache | 16 MB |

The following command has been executed in your file system.

```sh
rm lfsfile.txt
```

Now, answer the following questions.

- (i) How is the inode number problem solved in lfs?
- (ii) Write down the steps that will be executed while running the command. How will lfs invalidate any subsequent access to `lfsfile.txt`?
- (iii) Elaborate the recovery action that lfs will take, if a crash occurs during the operation.
- (iv) Calculate the throughput for read and writes in your file system (lfs) for the following RAID setups: (a) RAID-0; (b) RAID-1.

Hint:

- lfs reads 1 block at a time and writes 1 segment at a time.
- You can read/write one sector at a time on a disk.

**8(b).** What is thrashing in the context of memory management? Why is it a problem? List some solutions for it.
