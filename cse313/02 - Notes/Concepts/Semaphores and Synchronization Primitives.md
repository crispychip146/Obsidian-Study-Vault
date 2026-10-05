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

> [!IMPORTANT] **Exam practice references (Appeared in 2017 Q2b, 2018 Q2a, 2018 Q3a, 2020 Q2b)**
>
> ### Practice tasks and reasoning:
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

## Building the idea

A semaphore is a count of available permissions together with atomic operations for taking and returning them. With three printer permissions, the first three successful waits each take one. A fourth caller must wait until some printer user returns a permission.

A second use is ordering events. Initialize a semaphore to zero: a wait cannot pass until a signal supplies a permission. If the signal comes first, the permission remains available. This is the memory that a bare wakeup or condition-variable notification lacks.

The essential atomic step joins the availability test with either consuming a permission or registering the caller as a waiter. If another thread could signal between a failed test and the caller actually sleeping, the lost-wakeup problem would return. The semaphore implementation protects that transition.

Two textbook representations are common. One keeps a nonnegative available-permit count and a separate wait queue. Another decrements below zero and uses the negative count to represent waiting callers. They describe the same blocking idea, but their numeric traces differ. State which representation a trace uses before interpreting zero or a negative value.

## How It Works

### 2. Dijkstra's Semaphore Architecture

A **semaphore** $S$ is a protected integer variable that, apart from initialization, can only be accessed through two standard, indivisible (atomic) operations:
- **`wait(S)`** (originally **`down(S)`**, or Dutch **`P(S)`** for *proberen* / "to test")
- **`signal(S)`** (originally **`up(S)`**, or Dutch **`V(S)`** for *verhogen* / "to increment")

### Fundamental Invariant:
All modifications to the semaphore integer and its internal waiting queue must execute **atomically** (uninterruptibly).

---

## Example

This trace uses the **signed-count textbook convention**, where negative values encode queued waiters. It is not a portable assertion about an API's reported semaphore value.

Managing a pool of 3 printer devices using a Counting Semaphore initialized to $S = 3$:
1. Job 1 calls `wait(S)` $	o S = 2$, enters printer.
2. Job 2 calls `wait(S)` $	o S = 1$, enters printer.
3. Job 3 calls `wait(S)` $	o S = 0$, enters printer.
4. Job 4 calls `wait(S)` $	o S = -1 < 0$, Job 4 blocks and enters the semaphore wait queue.
5. Job 1 completes printing and calls `signal(S)` $	o S = 0 \le 0$, Job 4 is unblocked and granted printer access.

---

## Technical Details

### 1. Binary semaphore and mutex use
- At most one available permit. A signed internal representation can still record negative waiter counts. A mutex additionally has ownership rules, unlike a general semaphore.
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
- The available-permit count is nonnegative in one representation; a signed textbook representation uses negative values for waiters. Real APIs have finite limits.
- Initialized to the total quantity of available resources $N$.
- Used to manage finite resource pools (e.g., $N$ open database connections, $N$ buffer slots).

---

## Important Properties and Why They Hold

- **Signed-count convention:** If the textbook semaphore value $S < 0$, then the absolute value $|S|$ represents the exact number of processes currently blocked in the semaphore queue.
- **Atomicity Invariant:** The test-and-decrement in `wait()` and the increment-and-wakeup in `signal()` are indivisible atomic operations protected by kernel spinlocks or disabled interrupts.
- **Protocol distinction:** A one-permit semaphore can protect a critical section when used correctly. A counting semaphore manages permits for capacity or events; a mutex additionally restricts unlocking according to ownership semantics.

---

## Common Mistakes

Because semaphores are low-level procedural primitives, small developer mistakes result in deadlocks:

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

## What to carry forward

A semaphore can coordinate capacity or event order; a mutex additionally has ownership rules. They are not interchangeable in every API. [[Monitors and Condition Variables]] protects shared state through a structured interface, and its waiters check a predicate rather than consume remembered notifications.

## Related notes

- [[Monitors and Condition Variables]]

## Sources

- **Source Material:** `4. IPC-week-4-5-RRR.pptx` (Slides 29–42: Sleep and Wakeup, The Lost Wakeup Problem, Semaphores, Mutexes in Pthreads).
- **Previous Topic:** [[Peterson's Algorithm and Hardware Mutual Exclusion]] (Step 19).
- **Next Topic:** [[Monitors and Condition Variables]] (Step 21).
