---
type: algorithm
course: cse313
status: active
order: 19
---

# Peterson's Algorithm and Hardware Mutual Exclusion

> 📖 **Reading Order:** Step 19 of 34 | **Module 4:** Inter-Process Communication & Synchronization  
> ◄ **Previous:** [[Race Conditions and Critical-Section Problem]] | ► **Next:** [[Semaphores and Synchronization Primitives]]

---

> [!IMPORTANT] 🎯 **Exam Frequency & Intelligence (Appeared in 2018 Q1b, 2018 Q1c, 2019 Q2a, 2019 Q2c, 2021 Q4a)**
> **Frequency:** ⭐⭐⭐⭐ **High Recurrence (Appeared 4 out of 5 recent exam years)**
>
> ### What Exam Questions Expect & How to Master Them:
> 1. **Priority Inversion: Strict Priority Scheduler vs Round Robin (2019 Q2a & 2021 Q4a verbatim):**
>    - **The Scenario:** Process $L$ (low priority) enters its critical section (`flag[L] = true`). Process $H$ (high priority) wakes up, preempts $L$, sets `flag[H] = true, turn = L`, and executes the busy-wait spin: `while (flag[L] && turn == L);`.
>    - **Strict Priority Failure:** Because $H$ has higher priority, the scheduler grants all CPU cycles to $H$. Process $L$ is starved and never scheduled; thus $L$ can never exit its critical section or clear `flag[L] = false`. $H$ spins forever $\implies$ **System Livelock / Priority Inversion Deadlock!**
>    - **Round Robin Fix:** Under Round Robin, $H$'s time quantum expires via hardware timer interrupt. The scheduler forcibly switches execution to $L$. $L$ completes its critical section, sets `flag[L] = false`, and on the next turn $H$'s spin condition becomes false, allowing $H$ to enter cleanly.
> 2. **Four Criteria for Critical Section Solution (2019 Q2c):**
>    - **Mutual Exclusion:** Only one process inside at any time.
>    - **Progress:** If CS is empty, only processes wishing to enter decide who goes next; decision cannot be postponed indefinitely.
>    - **Bounded Waiting:** A bound exists on how many times others can enter before an applicant is admitted.
>    - **No Speed Assumptions:** Must work regardless of relative CPU clock speeds or core counts.
> 3. **Spinlock vs Sleep Lock Delay Formulations (2018 Q1b):**
>    - Let critical section duration be $T$, context switch cost be $C$, and lock primitive cost be $A$.
>    - **Spinlock:** $T_{best} = A$; $T_{worst} = T + A$ (spins for duration $T$).
>    - **Queue-Based Lock:** $T_{best} = A$; $T_{worst} = 2C + 3A + T$ (contention $A$ + context switch to sleep $C$ + hold time $T$ + wake signal $A$ + context switch back $C$ + acquire $A$).
> 4. **Atomic `FetchAndSubtract` Lock Construction (2018 Q1c):**
>    - Initialize `lock = 1`. Spin `while (FetchAndSubtract(&lock, 1) <= 0) { FetchAndSubtract(&lock, -1); }`. Release with `FetchAndSubtract(&lock, -1);`.

---

---

## The Problem and Earlier Tools

In concurrent programming, multiple threads updating shared variables suffer from race conditions. Early software attempts failed: strict alternation (`turn` variable) fails Progress because a thread in its remainder section blocks the other; boolean intent flags (`flag[i] = true`) fail Mutual Exclusion when both threads set their flags before testing.

We want a pure software solution that guarantees mutual exclusion, progress, and bounded waiting for two concurrent processes without requiring special hardware instructions. The central obstacle is ensuring that when both processes attempt to enter the critical section simultaneously, the tie is broken deterministically and symmetrically.

---

## Developing the Core Idea

In 1981, **G.L. Peterson** discovered that combining intent flags with a polite yielding mechanism creates a provably correct mutual exclusion algorithm:
- Each process sets its own intent flag: `flag[i] = true;`
- Crucially, it sets the turn variable to the *other* process: `turn = j;`
- A process waits only while: `while (flag[j] && turn == j);`
- If both processes arrive simultaneously, the memory write that occurs last sets `turn`, allowing the other process to immediately enter the critical section.

---

## Inputs

- Shared state: `boolean flag[2] = {false, false};`
- Shared state: `int turn;`
- Calling process ID $i \in \{0, 1\}$ and peer ID $j = 1 - i$.

---

## Outputs

- Mutually exclusive execution of the Critical Section.

---

## How It Works

### 4. Formal Proof of Correctness

### 1. Mutual Exclusion
Assume for contradiction that both $P_0$ and $P_1$ are in their critical sections at time $t$. Both must have set their `interested` flags to `TRUE`.
For $P_0$ to exit the `while` loop, either `interested[1] == FALSE` or `turn == 1`.
For $P_1$ to exit the `while` loop, either `interested[0] == FALSE` or `turn == 0`.
Since both are in the critical section, both `interested[0]` and `interested[1]` are `TRUE`.
Thus, it must be that `turn == 1` (letting $P_0$ in) AND `turn == 0` (letting $P_1$ in).
However, `turn` is a single scalar variable that cannot be both 0 and 1 simultaneously. **Contradiction!**

### 2. Progress
If only one process requests entry, it immediately proceeds because `interested[other] == FALSE`. If both request entry, the value of `turn` will be either 0 or 1, guaranteeing that exactly one process will exit the while loop immediately. No process outside the critical section can prevent the other from entering.

### 3. Bounded Waiting
A process $P_i$ waits at most one critical section execution of $P_j$. When $P_j$ exits, it sets `interested[j] = FALSE`. If $P_j$ attempts to re-enter, it sets `turn = j`, forcing itself to wait and allowing $P_i$ to proceed.

> [!WARNING] Modern CPU Out-of-Order Execution Hazard
> On modern out-of-order superscalar processors (x86, ARM), compiler optimizations and processor memory controllers can reorder writes (`interested[process] = TRUE` and `turn = process`). If `turn = process` is committed before `interested[process] = TRUE`, mutual exclusion can be broken! Therefore, in modern C/C++, explicit **memory fences/barriers** (`std::atomic_thread_fence`) are mandatory.

---

---

### 5. Hardware-Enforced Mutual Exclusion

To relieve programmers from software race checks, computer architectures implement hardware-level **atomic read-modify-write** instructions.

### 1. The TSL (Test and Set Lock) Instruction
`TSL RX, LOCK` reads the value of memory location `LOCK` into register `RX`, and stores a non-zero value at `LOCK`. The CPU guarantees that reading and writing are performed as an uninterruptible atomic memory cycle by asserting a lock signal on the system bus.

#### Assembly Implementation of Spinlock:
```assembly
enter_region:
    TSL REGISTER, LOCK      ; Copy LOCK to register and set LOCK to 1
    CMP REGISTER, #0        ; Was LOCK zero before?
    JNE enter_region        ; If it was non-zero, lock was busy; loop again
    RET                     ; Return to caller; critical section acquired!

leave_region:
    MOVE LOCK, #0           ; Store 0 in LOCK
    RET                     ; Return to caller
```

### 2. The XCHG (Atomic Exchange) Instruction
Similar to TSL, x86 provides `XCHG REGISTER, MEMORY`, which atomically swaps the contents of a register with a memory word:
```assembly
enter_region:
    MOVE REGISTER, #1       ; Put 1 into register
    XCHG REGISTER, LOCK     ; Atomically swap register with memory LOCK
    CMP REGISTER, #0        ; If old value was 0, lock was acquired
    JNE enter_region        ; Otherwise, spin
    RET
```

---

---

### 6. The Priority Inversion Problem

A major defect of busy-waiting synchronization (spinlocks) is the **Priority Inversion Problem**:
1. Suppose Process $H$ has high priority and Process $L$ has low priority.
2. $L$ enters its critical region.
3. Mid-way through, $H$ becomes ready to run (e.g., an I/O event arrives).
4. Because $H$ has higher priority, the scheduler preempts $L$ and runs $H$.
5. $H$ attempts to enter the critical region and spins in a tight `while (lock == 1)` loop.
6. Since $H$ has higher priority, $L$ is **never scheduled** to exit its critical region and release the lock.
7. $H$ runs forever in an infinite busy-wait loop, and $L$ starves completely!

**Solution:** Priority Inheritance Protocol or blocking synchronization primitives ([[Semaphores and Synchronization Primitives]]).

---

---

## Pseudocode

### 1. Algorithmic Overview & Motivation

In 1981, Gary L. Peterson discovered an elegant, purely software-based solution to the two-process critical-section problem. Prior solutions (such as Dekker's algorithm in 1965) were convoluted. Peterson combined:
1. **The idea of intent:** An array `interested[2]` (or `flag[2]`) declaring that a process wants to enter.
2. **The idea of courtesy/deference:** A shared `turn` variable that gives priority away to the *other* process.

By politely yielding `turn` to the opponent before spinning, deadlocks and mutual blocking are avoided.

In addition, hardware designers introduced atomic read-modify-write CPU instructions such as **TSL (Test and Set Lock)** and **XCHG (Exchange)** to provide hardware-enforced mutual exclusion without complex software protocols.

---

---

### 2. Peterson's Algorithm Implementation

### Global Data Structures:
```c
#define FALSE 0
#define TRUE  1
#define N     2       // Number of competing processes

int turn;             // Whose turn is it to enter?
int interested[N] = {FALSE, FALSE}; // Does process want to enter?
```

### Protocol Implementation:
```c
void enter_region(int process) {
    int other = 1 - process;          // Index of the other process
    interested[process] = TRUE;       // Announce intent to enter
    turn = process;                   // Set turn to self (deference to other)

    // Wait while the other process wants to enter AND it's your turn to wait
    while (turn == process && interested[other] == TRUE) {
        // Busy wait (spin)
    }
}

void leave_region(int process) {
    interested[process] = FALSE;      // Announce departure from critical region
}
```

---

---

## Example

### 3. Step-by-Step Execution Scenarios

### Scenario A: Uncontested Access (Only Process 0 wants to enter)
1. Process 0 calls `enter_region(0)`:
   - `other = 1`
   - `interested[0] = TRUE`
   - `turn = 0`
   - Condition check: `turn == 0 && interested[1] == TRUE` $\implies$ `TRUE && FALSE` $\implies$ **FALSE**.
2. Process 0 enters the critical section immediately with zero waiting.

### Scenario B: Simultaneous Entry (Both arrive at the same time)
1. Both processes announce intent:
   - $P_0$ sets `interested[0] = TRUE`, then writes `turn = 0`.
   - $P_1$ sets `interested[1] = TRUE`, then writes `turn = 1`.
2. Because memory bus writes are serialized, either $P_0$ writes `turn` last, or $P_1$ writes `turn` last.
   - Suppose $P_1$ writes `turn = 1` *after* $P_0$ writes `turn = 0`.
   - The final value in `turn` is `1`.
3. Now both evaluate their `while` conditions:
   - For $P_0$: `turn == 0 && interested[1] == TRUE` $\implies 1 == 0 \text{ (FALSE)}$. $P_0$ drops out of the loop and **enters the critical region**!
   - For $P_1$: `turn == 1 && interested[0] == TRUE` $\implies 1 == 1 \land \text{TRUE} \implies$ **TRUE**. $P_1$ loops (busy-waits).
4. When $P_0$ leaves, it calls `leave_region(0)` setting `interested[0] = FALSE`.
5. On $P_1$'s next check, `interested[0]` is `FALSE`, so $P_1$'s loop terminates and $P_1$ enters.

---

---

## Complexity

### Time Complexity
$O(1)$ operations in entry and exit; busy-waiting cycles while waiting.

### Space Complexity
$O(1)$ memory (2 booleans and 1 integer).

---

## Properties

- **Mutual Exclusion:** Provably impossible for both processes to occupy CS simultaneously.
- **Progress:** A process outside CS cannot prevent an interested process from entering.
- **Bounded Waiting:** A process waits at most one critical section turn before entering.

---

## Limitations

- Limited to 2 processes (generalizable to $N$ via Filter algorithm, but complex).
- Relies on sequential memory consistency; on modern out-of-order processors, hardware atomic instructions (`TestAndSet`, `CompareAndSwap`) or memory barriers are required.

---

## Common Mistakes

- Misunderstanding preemption boundaries during execution.
- Failing to verify state invariants before granting resource claims.

---

## Exam Relevance

Regularly examined through Gantt chart simulations, state trace matrices, and deadlock sequence proofs.

---

## Related Concepts

- [[Semaphores and Synchronization Primitives]]
- [[Classic Synchronization Solutions]]

---

## Prerequisites

- [[Race Conditions and Critical-Section Problem]]

---

## Problems

- [[Problem — Dining Philosophers Deadlock-Free Synchronization]]

---

## Sources

- **Source Material:** `4. IPC-week-4-5-RRR.pptx` (Slides 16–28: Peterson's Solution, The TSL Instruction, The Priority Inversion Problem).
- **Previous Topic:** [[Race Conditions and Critical-Section Problem]] (Step 18).
- **Next Topic:** [[Semaphores and Synchronization Primitives]] (Step 20).
