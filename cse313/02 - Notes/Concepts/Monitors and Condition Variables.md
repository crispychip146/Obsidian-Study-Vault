---
type: concept
course: cse313
status: active
order: 21
---

# Monitors and Condition Variables

> 📖 **Reading Order:** Step 21 of 34 | **Module 4:** Inter-Process Communication & Synchronization  
> ◄ **Previous:** [[Semaphores and Synchronization Primitives]] | ► **Next:** [[Message Passing and IPC Models]]

---

> [!IMPORTANT] **Exam practice references (Appeared in 2018 Q3c)**
>
> ### Practice tasks and reasoning:
> 1. **Why `while` is Mandatory Instead of `if` for Condition Variables (2018 Q3c):**
>    - **The Setup:** A student or engineer implements monitor methods using `if (count == 0) cond_wait(&nonempty);` with 2 consumers and 1 producer. What fatal bug occurs?
>    - **The tracing method (Mesa Semantics / Signal-and-Continue):**
>      1. Buffer is empty ($count = 0$). Consumer $C_1$ executes `if (count == 0)` and sleeps via `cond_wait()`.
>      2. Consumer $C_2$ enters, also finds $count = 0$, and sleeps.
>      3. Producer enters, inserts 1 item ($count = 1$), and invokes `cond_signal()`.
>      4. Language runtime moves $C_1$ from the condition queue to the ready queue. **Crucial point:** $C_1$ does NOT immediately seize the CPU or the monitor lock!
>      5. Before $C_1$ gets scheduled, a new consumer $C_3$ (or $C_2$) enters the monitor, sees $count = 1$, consumes the single item, and leaves ($count$ drops back to 0).
>      6. $C_1$ finally acquires the monitor lock and resumes immediately after `cond_wait()`.
>      7. Because $C_1$ used `if` instead of `while`, it **never re-evaluates `count`**! It assumes an item is available and executes `buffer[out]`, causing a **buffer underflow exception, memory corruption, or system crash**.
>      8. **The Rule:** Always wrap condition waits in a loop: `while (condition) cond_wait(&var);`.

---

## Building the idea

A monitor puts shared data and the procedures that manipulate it behind one mutual-exclusion boundary. That reduces the burden of placing lock operations at every access, but it leaves another problem: what should a consumer do when it enters safely and finds the buffer empty?

Waiting while retaining the monitor lock would stop the producer from entering to fill the buffer. A condition wait therefore **releases the lock and registers the waiter atomically**. When it returns, the waiter holds the lock again and can inspect the protected data.

Under Mesa semantics, a signal means that a waiter may compete to resume; it does not reserve an item for that waiter. The signaler or a different eligible consumer can change the buffer first. Thus use `while (count==0) wait(nonempty)`: the predicate is checked again after every return. With `if`, the consumer can continue on an assumption that is no longer true.

[[Semaphores and Synchronization Primitives]] remembers permissions in a count. A condition variable remembers waiting callers, while the shared predicate records the actual condition. That is why a notification sent with no waiter need not be stored: a later caller checks the predicate itself.

## How It Works

### A bounded FIFO buffer under Mesa semantics

This is monitor pseudocode: procedure entry holds the monitor lock, and `wait` releases it atomically before sleeping and reacquires it before returning.

```text
monitor BoundedBuffer {
    condition not_full, not_empty;
    item buffer[N];
    int count = 0, in = 0, out = 0;

    procedure insert(item value) {
        while (count == N) wait(not_full);
        buffer[in] = value;
        in = (in + 1) % N;
        count++;
        signal(not_empty);
    }

    procedure remove() returns item {
        while (count == 0) wait(not_empty);
        item value = buffer[out];
        out = (out + 1) % N;
        count--;
        signal(not_full);
        return value;
    }
}
```

The count predicate protects capacity, while `in` and `out` preserve FIFO order. Notify after each successful insertion/removal so eligible waiters can compete; every awakened waiter still checks the predicate. A signal is not a stored item reservation.

---

### 3. Condition Variables

Monitors alone cannot handle situations where a process enters a monitor procedure, finds that a condition is not met (e.g., buffer is full), and must wait. If it simply halted, it would hold the monitor lock, blocking all other processes from entering to change the condition!

To resolve this, monitors provide **Condition Variables**:
A condition variable is a synchronization object (not an integer counter) with two primitive operations:

1. **`wait(condition_var)`**:
   - The calling process is suspended and placed into `condition_var`'s waiting queue.
   - The monitor lock is **atomically released**, allowing another process to enter.
2. **`signal(condition_var)`**:
   - Awakens exactly one process currently sleeping on `condition_var`.
   - **Crucial Distinction from Semaphores:** If no process is currently waiting on `condition_var`, the signal is **silently lost and discarded**. Condition variables have **no memory** and do not accumulate counts.

---

## Example

Java Monitor syntax for a thread-safe bank account:
```java
public class BankAccount {
    private int balance = 0;

    public synchronized void deposit(int amount) {
        balance += amount;
        notifyAll(); // Signal waiting withdrawers
    }

    public synchronized void withdraw(int amount) throws InterruptedException {
        while (balance < amount) {
            wait(); // Sleep and release lock until balance increases
        }
        balance -= amount;
    }
}
```

---

## Technical Details

### 4. Signaling Disciplines: Hoare vs Mesa Semantics

When process $P$ executes `signal(c)` inside a monitor, waking up sleeping process $Q$, both $P$ and $Q$ could potentially execute inside the monitor simultaneously, violating the fundamental monitor invariant. Operating systems and languages resolve this using two paradigms:

### 1. Hoare Semantics (Signal-and-Wait)
- **Rule:** The signaler $P$ is immediately suspended and yields the monitor lock directly to $Q$. $Q$ runs immediately. When $Q$ exits or waits, $P$ resumes.
- **Advantage:** The condition that $Q$ waited for is guaranteed to remain strictly TRUE when $Q$ wakes up.
- **Syntax:** A simple `if` condition suffices:
  ```c
  if (count == N)
      wait(full);
  ```

### 2. Mesa Semantics (Signal-and-Continue — Java, POSIX Pthreads, C#)
- **Rule:** The signaler $P$ keeps the monitor lock and continues execution until it naturally leaves the monitor. The awakened process $Q$ is merely moved from the condition queue to the monitor ready queue.
- **Hazard:** By the time $Q$ actually re-acquires the monitor lock, a third process may have sneaked in and altered the shared state!
- **Mandatory Rule:** In Mesa semantics, a process **MUST ALWAYS re-check the condition in a `while` loop**:
  ```c
  while (count == N) {
      wait(full); // Re-evaluates condition upon re-awakening!
  }
  ```

---

### 6. Comprehensive Comparison: Semaphores vs Monitors

| Feature | Semaphore | Monitor |
|---|---|---|
| **Level of Abstraction** | Low-level OS kernel primitive | High-level language construct |
| **Mutual Exclusion** | Explicit developer responsibility (`wait`/`signal` calls) | Handled automatically by compiler / runtime |
| **Data Encapsulation** | None; variables are scattered | Complete; private shared data inside monitor |
| **Memory / History** | `signal()` increments integer; remembered forever | `signal()` without waiters is **discarded immediately** |
| **Error Proneness** | High; easy to forget `signal()` or introduce deadlock | Low; compiler prevents illegal concurrent entry |
| **Supported Systems** | C, OS kernels, POSIX systems | Java (`synchronized`), C#, Concurrent Pascal |

---

## Important Properties and Why They Hold

- **Mesa vs. Hoare Signaling Semantics:**
  - *Mesa Semantics (Java, POSIX Pthreads):* `signal()` awakens a waiter, but the signaling thread keeps running. The waiter is moved to the ready queue and must re-check its condition via `while (!condition)` due to potential race conditions.
  - *Hoare Semantics:* `signal()` immediately transfers the monitor lock directly to the waiting thread; the signaling thread is suspended. Condition check can use `if (!condition)`.
- **Encapsulation assumption:** Mutual exclusion protects shared state only when all relevant accesses use the monitor protocol. Exposing references or using unsynchronized access can still introduce errors.

---

## What to carry forward

Specify Hoare or Mesa semantics before tracing signals. Immediate lock handoff in Hoare's model can support different reasoning from Mesa's eventual reacquisition. Encapsulation helps only when all relevant shared accesses respect the monitor boundary; it cannot repair unsafely exposed state.

## Related notes

- [[Semaphores and Synchronization Primitives]]

## Sources

- **Source Material:** `4. IPC-week-4-5-RRR.pptx` (Slides 43–52: Monitors, Condition Variables, Hoare vs Mesa Semantics).
- **Previous Topic:** [[Semaphores and Synchronization Primitives]] (Step 20).
- **Next Topic:** [[Message Passing and IPC Models]] (Step 22).
