---
type: concept
course: cse313
status: active
order: 20
---

# Semaphores and Synchronization Primitives

> 📖 **Reading Order:** Step 20 of 34 | **Module 4:** Inter-Process Communication & Synchronization  
> ◄ **Previous:** [[Peterson's Algorithm and Hardware Mutual Exclusion]] | ► **Next:** [[Monitors and Condition Variables]]

---

> [!IMPORTANT] 🎯 **Exam Frequency & Intelligence (Appeared in 2017 Q2b, 2018 Q2a, 2018 Q3a, 2020 Q2b)**
> **Frequency:** ⭐⭐⭐⭐⭐ **100% Core Recurrence (Appeared across 4 exam years!)**
>
> ### What Exam Questions Expect & How to Master Them:
> 1. **Solving the Shared Counter Race Condition (2018 Q2a):**
>    - Initialize binary semaphore `mutex = 1`. Each worker executes: `sem_wait(&mutex); counter++; sem_post(&mutex);`.
> 2. **Signaling / Precedence Constraints (2018 Q3a):**
>    - **Requirement:** Print "world" only after "hello" has been printed at least twice.
>    - **The Solution:** Initialize `sem_t sem_hello = 0`. Each time a thread prints "hello", it calls `sem_post(&sem_hello);`. The printing thread for "world" executes two back-to-back waits: `sem_wait(&sem_hello); sem_wait(&sem_hello); printf("world\n");`.
> 3. **The Lost Wakeup Flaw and How Semaphores Solve It:**
>    - When using bare `sleep()` and `wakeup()` calls, a wakeup signal sent before a process enters sleep is dropped by the OS and permanently lost. Semaphores solve this because **they have memory**: calling `sem_post()` increments an integer value, ensuring any past signal is saved for future callers.
> 4. **Cyclic Producer Synchronization for $M$ Producers (2020 Q2b):**
>    - Use an array of $M$ turn semaphores `turn[M]`, where `turn[0]=1` and all others are `0`. Producer $i$ waits on `turn[i]` and, upon inserting an item, signals `turn[(i+1)%M]`.

---

## 1. Intuition & The Lost Wakeup Problem

Prior to semaphores, synchronization relied either on CPU-burning busy-waiting or on elementary OS system calls: `sleep()` (suspend self) and `wakeup(pid)` (awaken suspended process).

### The Lost Wakeup Flaw:
Consider the classic Producer-Consumer scenario with a shared buffer of size $N$ and an item counter `count`:
1. The buffer is empty (`count == 0`).
2. The consumer inspects `count`, sees it is 0, and prepares to call `sleep()`.
3. Just before the consumer invokes `sleep()`, the scheduler preempts the consumer and runs the producer.
4. The producer produces an item, inserts it, increments `count` to 1, and notes that `count` was just 0. It calls `wakeup(consumer)` to alert the consumer.
5. However, the consumer is **not yet asleep**! The wakeup signal is discarded by the OS (it has no memory).
6. The consumer resumes and finally executes `sleep()`.
7. Eventually, the producer fills the entire buffer (`count == N`), calls `sleep()`, and blocks.
8. **Result:** Both processes are asleep forever. The system is completely deadlocked.

```
Consumer                         Producer                         Buffer Count
-------------------------------------------------------------------------------
reads count (0)                                                        0
[PREEMPTED before sleep()]
                                 produces item                         0
                                 inserts item, count++                 1
                                 sends wakeup(Consumer) -> LOST!       1
resumes, executes sleep()                                              1
[Consumer ASLEEP]
                                 fills buffer to N, executes sleep()   N
[DEADLOCK: Both asleep forever]
```

To resolve this, Edsger Dijkstra (1965) introduced a synchronization primitive that remembers signals: the **Semaphore**.

---

## 2. Dijkstra's Semaphore Architecture

A **semaphore** $S$ is a protected integer variable that, apart from initialization, can only be accessed through two standard, indivisible (atomic) operations:
- **`wait(S)`** (originally **`down(S)`**, or Dutch **`P(S)`** for *proberen* / "to test")
- **`signal(S)`** (originally **`up(S)`**, or Dutch **`V(S)`** for *verhogen* / "to increment")

### Fundamental Invariant:
All modifications to the semaphore integer and its internal waiting queue must execute **atomically** (uninterruptibly).

---

## 3. Kernel-Level Non-Busy Waiting Implementation

In modern operating systems, semaphores do not spin. A process that cannot proceed is placed into a blocked FIFO queue in the semaphore structure and yields the CPU:

```c
typedef struct {
    int value;
    struct task_struct *queue_head; // FIFO list of blocked PCBs
} semaphore;

void wait(semaphore *S) {
    S->value--;
    if (S->value < 0) {
        // Add this calling process to S->queue_head
        // Set process state to BLOCKED / SLEEPING
        block(); // Yield CPU to scheduler
    }
}

void signal(semaphore *S) {
    S->value++;
    if (S->value <= 0) {
        // Remove a process P from S->queue_head
        // Set process P state to READY
        wakeup(P); // Insert P into CPU ready queue
    }
}
```

### Critical Mathematical Property of Negative Semaphore Values:
When `S->value < 0`, its absolute magnitude $|S\text{->value}|$ represents the **exact number of processes currently blocked and waiting** on that semaphore!
- If $S = 3$: 3 resources are currently available.
- If $S = 0$: 0 resources are available; no process is waiting.
- If $S = -4$: 0 resources are available, and exactly 4 processes are queued asleep.

---

## 4. Types of Semaphores

### 1. Binary Semaphore (Mutual Exclusion / Mutex)
- Integer value restricted strictly between $0$ and $1$.
- Initialized to $1$.
- Used to enforce mutual exclusion around critical sections:
  ```c
  semaphore mutex = 1;

  void process() {
      wait(&mutex);
      /* Critical Section */
      signal(&mutex);
      /* Remainder Section */
  }
  ```

### 2. Counting Semaphore
- Integer value can span an unrestricted positive/negative range.
- Initialized to the total quantity of available resources $N$.
- Used to manage finite resource pools (e.g., $N$ open database connections, $N$ buffer slots).

---

## 5. Mutexes vs Binary Semaphores

While binary semaphores are often used as mutexes, strict POSIX/OS standards distinguish between them:

| Property | Mutex (Mutual Exclusion Lock) | Binary Semaphore |
|---|---|---|
| **Primary Intent** | Mutual Exclusion locking | Signaling and Synchronization |
| **Ownership** | **Has ownership:** Only the thread that locked the mutex is legally permitted to unlock it. | **No ownership:** Any thread/interrupt handler can call `signal()` to awaken a waiting thread. |
| **Use Case** | Protecting critical sections | Task coordination / Event notification |
| **POSIX API** | `pthread_mutex_lock()`, `pthread_mutex_unlock()` | `sem_wait()`, `sem_post()` |

---

## 6. Common Synchronization Errors with Semaphores

Because semaphores are low-level procedural primitives, small developer mistakes result in fatal system deadlocks:

1. **Inverted Wait / Signal Order:**
   ```c
   signal(&mutex); // Premature unlock
   critical_section();
   wait(&mutex);   // Blocks caller afterwards
   ```
   *Consequence:* Mutual exclusion is completely violated.

2. **Accidental Double `wait()`:**
   ```c
   wait(&mutex);
   wait(&mutex); // Deadlocks itself immediately!
   ```

3. **Missing `signal()`:**
   If a thread crashes or exits without signaling `mutex`, all subsequent processes requesting the lock will hang forever.

4. **Nested Semaphore Deadlock (Lock Ordering):**
   ```c
   // Thread 1:
   wait(&semA);
   wait(&semB);

   // Thread 2:
   wait(&semB);
   wait(&semA);
   ```
   *Consequence:* If Thread 1 acquires `semA` and Thread 2 acquires `semB`, each waits on the other's lock $\implies$ **Permanent Deadlock**!

---

## Source Traceability & Metadata
- **Source Material:** `4. IPC-week-4-5-RRR.pptx` (Slides 29–42: Sleep and Wakeup, The Lost Wakeup Problem, Semaphores, Mutexes in Pthreads).
- **Previous Topic:** [[Peterson's Algorithm and Hardware Mutual Exclusion]] (Step 19).
- **Next Topic:** [[Monitors and Condition Variables]] (Step 21).
