# CSE 313 (Operating System) - 2018

**1(a).** Draw the Processes State Transition Diagram. Include the following states: Embryo, Running, Runnable, Zombie, Sleeping.

**1(b).** Given a basic spinlock, Assume that locking the spinlock takes A time units (if no one is holding the lock); unlock also takes A time units. Assume further that a context switch takes C time units, and that a time slice is T time units long.

Assume this code sequence, executed by two threads on one processor at roughly the same time:

```c
mutex_lock();
do_something(); // takes no time to execute
mutex_unlock();
```

- (i) What is the best-case time for the two threads on one CPU to finish this code sequence?
- (ii) What is the worst-case time for the two threads to finish this code sequence? Assume that only three context switches can occur at a maximum.
- (iii) If the spin lock is instead changed to a queue-based lock, how does that change the worst-case time?

**1(c).** You are given a new atomic function, called `FetchAndSubtract()`. It executes as a single atomic instruction, and is defined as follows:

```c
int FetchAndSubtract(int *location) {
    int value = *location;    // read the value pointed to by location
    *location = value - 1;    // decrement it and store result back
    return value;            // return old value
}
```

You are given the task: write the `lock_init()`, `lock()`, and `unlock()` functions (and also Define a `lock_t` structure) that use `FetchAndSubtract()` to implement a working lock.

**2(a).** The variable counter, is shared within process A and process B. The initial value is counter = 0 before execution of either process. Here, R0 is a register.

| Process A | Process B |
| --- | --- |
| `LOAD (counter, R0)` | `LOAD (counter, R0)` |
| `ADD (R0, 1, R0)` | `ADD (R0, 2, R0)` |
| `STORE (R0, counter)` | `STORE (R0, counter)` |

- (i) Add semaphores (with initial values) so that the final value of counter is 2.
- (ii) Add semaphores (with initial values) so that the final value of counter is not 3.

**2(b).** You wrote a piece of code with four threads (1-4) and four locks (A-D).

Thread 1 grabs Locks A and B (in some order); Thread 2 grabs Locks B and C (in some order);

Thread 3 grabs Locks C and D (in some order); Thread 4 grabs Locks D and A (in some order);

Is it possible that this code might result in deadlock? Briefly explain.

**2(c).** Consider the following program:

```text
1   int main{
2       int count = 1;
3       int pid = 0, pid2 = 0;
4       if ((pid = fork())) {
5           count = count + 2;
6           printf("%d ", count);
7       }
8
9       if (count == 1) {
10          count++;
12          pid2=fork();
13          printf("%d ", count);
14      }
15
16      if (pid2) {
17          wait(pid2, NULL, 0);
18          count = count * 2;
19          printf("%d ", count);
20      }
21  }
```

- (i) How many processes are created during the execution of this program? Explain briefly.
- (ii) List all the possible outputs of the program.

**3(a).** Let's examine a program having two threads:

Thread 1:

```c
pending = 1;
while (pending) {
    printf("hello\n");
}
```

Thread 2:

```c
pending = 0;
```

How could we re-write the code such that Thread 2 would only run after "hello" has been printed at least twice? You can use any synchronization primitive of your choice.

**3(b).** Scheduling policies can be easily depicted with some graphs. For example, let's say we run scheduler S for 1 time unit, job A for 5 time units, run scheduler S again for 1 time unit, and then run job B for time units. Our graph of this policy will look like this:

| Time interval | CPU |
| --- | --- |
| 0-1 | S |
| 1-6 | A |
| 6-7 | S |
| 7-12 | B |

- (i) Draw a similar graph of ROUND-ROBIN scheduling for jobs A (arriving at T = 0), B (arriving at T = 5), and C (arriving at T = 10), each running for 6 time-units. Assume a 2 time unit time slice; also assume that the scheduler (S) takes 1 time unit to make a scheduling decision. Make sure to label the x-axis appropriately.
- (ii) What is the average RESPONSE TIME for jobs A, B and C?
- (iii) What is the average TURNAROUND TIME for jobs A, B and C?

**3(c).** Consider the producer/consumer problem and the (broken) solution mentioned below. Briefly describe why solution is broken, and demonstrate it with a specific example of thread interleaving (Hint: you can assume two consumers and one producer).

Producer:

```c
void *producer(void *arg) {
    int i;
    while (1) {
        mutex_lock(&mutex);        // p1
        if (count == MAX)          // p2
            cond_wait(&empty, &mutex); // p3
        put(i);                    // p4
        cond_signal(&full);        // p5
        mutex_unlock(&mutex);      // p6
    }
}
```

Consumer:

```c
void *consumer(void *arg) {
    int i;
    while (1) {
        mutex_lock(&mutex);        // c1
        if (count == 0)            // c2
            cond_wait(&full, &mutex); // c3
        int tmp = get();           // c4
        cond_signal(&empty);       // c5
        mutex_unlock(&mutex);      // c6
        printf("%d\n", tmp);
    }
}
```

**4(a).** A typical OS provides some APIs to create processes. `fork()`, `exec()`, and `wait()` can be used combinedly for that purpose. Write some code that uses these system calls to launch a new child process, have the child executed a program named "hello" (with no arguments), and have the parent wait for the child to complete.

**4(b).** Assume an OS with MLFQ (multi-level feedback queue) scheduler.

Here is a timeline of what happens when two CPU-bound (no I/O) jobs, A and B run:

| Time interval (milliseconds) | Running job |
| --- | --- |
| 0-10 | A |
| 10-20 | B |
| 20-40 | A |
| 40-60 | B |
| 60-115 | A |
| 115-170 | B |
| 170-225 | A |
| 225-280 | B |
| 280-335 | A |
| 335-390 | B |
| 390-445 | A |
| 445-500 | B |
| 500-510 | A |
| 510-520 | B |
| 520-540 | A |
| 540-560 | B |
| 560 onward | A |

This figure shows when A and B run over time. Note that after 500 milliseconds, the behavior repeats, indefinitely (until the jobs are done). To help you further, a closeup of the first part of the graph is shown below:

| Time interval (milliseconds) | Running job |
| --- | --- |
| 0-10 | A |
| 10-20 | B |
| 20-40 | A |
| 40-60 | B |
| 60-115 | A |
| 115-170 | B |
| 170-220 (end of closeup) | A |

Now, answer the following questions:

- (i) How many queues do you think there are in this MLFQ scheduler?
- (ii) How long is the time slice at the top-most (high priority) queue?
- (iii) How long is the time slice at the bottom-most (low priority) queue?
- (iv) How often do processes get moved back to the topmost queue?
- (v) Why does the scheduling policy MLFQ move processes to higher priority levels (i.e., the topmost queue) sometimes? Briefly explain.

**4(c).** With the round robin (RR) scheduling policy, a question arises when a new job arrives in the system: should we put the job at the front of the RR queue, or the back? Does this subtle difference make a difference, or does RR behave pretty much the same way either way? Briefly explain.

**5(a).** Alice has implemented a fast file system (FFS) with inodes having 12 direct pointers, 1 indirect pointer and 1 double indirect pointer.

- (i) In a 1TB disk with 2KB blocks, how big of a file can be handled using Alice's file system?
- (ii) FFS divides the disk into multiple block groups. Now, can a 100 MB file be saved in Alice's file system? If so, then how all the data will be distributed on disk?
- (iii) If the disk spends 5ms time for positioning on the average and has 100MB/s transfer rate, how long will it take to read the file in (ii)?

**5(b).** What is TLB in the context of memory virtualization? Show its structure. Mention its control flow using pseudocode. How does it manage context switches?

**5(c).** What is RPC? Suppose the user has written the following codes:

```c
int main() {
    ...
    m = f1(x1, x2);
    ...
}
int f1(char* a, char* b) {
    ...
    p = f2(*a);
}
float f2(int t) {
    ...
}
```

If the user wishes to run the function f2 in a distributed manner, how will a RPC run-time library handle it? Write down which steps it needs to take and corresponding details that are needed to be taken care of in each step.

**6(a).** Suppose the OS made the following 3 batch of block requests to a disk (each number represents block id):

- 1, 40, 2, 15
- 10, 1, 13, 32, 2, 7
- 70, 49, 0, 6, 28

The requests were sent in such a way that one batch is requested after the previous one is handled. The disk has 1GB capacity, 4KB blocks, 8 blocks in each track and max seek time of 100ms. Now, calculate how much time the disk will spend on seeking, if the disk head is initially on the first track and the scheduling algorithm is:

- (i) SSTF
- (ii) SCAN
- (iii) C-SCAN

**6(b).** A program named 'test' execute some code and writes some test to the console. The program is executed as:

```sh
./test > /etc/out.txt
```

Now, write down the timeline for read/write operation in the file system. Assume that it is vsfs (very simple file system) and there is no cached value to aid.

**6(c).** Bob has created the following files in an FFS (file sizes are written inside brackets).

- /a/f1 (2KB)
- /b/f2 (1KB)
- /c/b/f3 (10 KB)
- /c/f4 (7KB)
- /b/f5 (60KB)

Suppose, the disk has 4KB blocks and 5 block group. If /c/b is symbolic link to the directory /b, show how the files will be saved with illustrative diagrams.

**7(a).** You created an elegant device that has to deal with both big bursts of small I/O requests and very large I/O requests. While writing a device driver for it, what design decisions would you take and why? Elaborate the pros and cons of your design.

**7(b).** We need to write data in block 8, 11, 12 inode is in block 3, bitmap is in block 2. Journal is in blocks 24 to 31. Write down the journaling timeline if the system uses:

- (i) Data Journaling
- (ii) Metadata Journaling
- (iii) Metadata Journaling with Checksum Optimization

**7(c).** What is track skew? Why was it required in older disks? Why is it not good for newer disk models?

**7(d).** You know that RAID-4 has terrible Random Write performance. If the parity disk is mirrored in RAID-4, what would happen to its Random Write performance? What about Random Read or Sequential Read-Write? Explain your derivation.

**8(a).** You ran the following rename operation:

```sh
mv ~/a/foo.txt ~/b/bar.txt
```

Explain with illustrative figure what happens during this operation in:

- (i) Very Simple File System (VSFS)
- (ii) Log-structured File System (LFS)

**8(b).** Using illustrative figures and pseudocodes, show how the OS talks to a canonical IO device while using DMA.

**8(c).** When using the swapping mechanism, the OS reserves some swap space on disk and works with directly. However, we know the OS already has File API to communicate with disk. Why do you think this discrepancy is there?
