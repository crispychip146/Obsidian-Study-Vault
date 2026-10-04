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

---

## Starting Point and the Problem

In concurrent programming, low-level mutual exclusion using spinlocks (busy-waiting loops like `while (test_and_set(&lock));`) forces CPU cores to consume 100% power executing useless spin cycles while waiting for a lock to clear. Furthermore, primitive integer flags suffer from "lost wakeup" race conditions.

We want an expressive, general-purpose synchronization primitive that allows threads to coordinate mutual exclusion and manage shared resource pools without wasting CPU cycles. The central obstacle is that the operations of testing, decrementing, and sleeping must occur completely atomically: if a thread is preempted halfway through checking a counter, synchronization collapses.

---

## Developing the Idea

In 1965, **Edsger Dijkstra** introduced the **Semaphore**: an integer variable $S$ that can only be accessed through two standardized, strictly atomic primitives:
1. **`wait(S)`** (originally named `P(S)` from Dutch *proberen*, to test): Decrements $S$. If $S < 0$, the calling thread is blocked and placed onto a wait queue.
2. **`signal(S)`** (originally named `V(S)` from Dutch *verhogen*, to increment): Increments $S$. If $S \le 0$, the kernel awakens one blocked thread from the wait queue.

Unlike spinlocks, a thread calling `wait()` when resources are unavailable voluntarily yields the CPU via `sleep()` / `block()`, enabling the OS to run productive work until `signal()` awakens it.

---

## Definition



---

## How It Works

### 2. Dijkstra's Semaphore Architecture

A **semaphore** $S$ is a protected integer variable that, apart from initialization, can only be accessed through two standard, indivisible (atomic) operations:
- **`wait(S)`** (originally **`down(S)`**, or Dutch **`P(S)`** for *proberen* / "to test")
- **`signal(S)`** (originally **`up(S)`**, or Dutch **`V(S)`** for *verhogen* / "to increment")

### Fundamental Invariant:
All modifications to the semaphore integer and its internal waiting queue must execute **atomically** (uninterruptibly).

---

---

## Example

Managing a pool of 3 printer devices using a Counting Semaphore initialized to $S = 3$:
1. Job 1 calls `wait(S)` $	o S = 2$, enters printer.
2. Job 2 calls `wait(S)` $	o S = 1$, enters printer.
3. Job 3 calls `wait(S)` $	o S = 0$, enters printer.
4. Job 4 calls `wait(S)` $	o S = -1 < 0$, Job 4 blocks and enters the semaphore wait queue.
5. Job 1 completes printing and calls `signal(S)` $	o S = 0 \le 0$, Job 4 is unblocked and granted printer access.

---

## Technical Details

### 4. Types of Semaphores

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

---

## Important Properties and Why They Hold

- **Wait Queue Invariance:** If semaphore value $S < 0$, then the absolute value $|S|$ represents the exact number of processes currently blocked in the semaphore queue.
- **Atomicity Invariant:** The test-and-decrement in `wait()` and the increment-and-wakeup in `signal()` are indivisible atomic operations protected by kernel spinlocks or disabled interrupts.
- **Mutex vs. Counting Distinction:** A binary semaphore ($S \in \{0, 1\}$) provides mutual exclusion; a general counting semaphore ($S \ge 0$) manages counting pools of identical resources.

---

## Common Mistakes

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

---

## Exam Relevance

Frequently examined through conceptual comparison questions, trace diagrams, and architectural trade-off evaluations.

---

## Related Concepts

- [[Monitors and Condition Variables]]
- [[Classic Synchronization Solutions]]
- [[Producer-Consumer Semaphore Implementation Example]]

---

## Prerequisites

- [[Race Conditions and Critical-Section Problem]]
- [[Threads and Multithreading Models]]

---

## Problems

- [[Problem — Dining Philosophers Deadlock-Free Synchronization]]

---

## Sources

- **Source Material:** `4. IPC-week-4-5-RRR.pptx` (Slides 29–42: Sleep and Wakeup, The Lost Wakeup Problem, Semaphores, Mutexes in Pthreads).
- **Previous Topic:** [[Peterson's Algorithm and Hardware Mutual Exclusion]] (Step 19).
- **Next Topic:** [[Monitors and Condition Variables]] (Step 21).
