---
type: example
course: cse313
status: active
order: 24
---
# Producer-Consumer Semaphore Implementation Example

> 📖 **Reading Order:** Step 24 of 68 | **Module 4:** Inter-Process Communication & Synchronization  
> ◄ **Previous:** [[Classic Synchronization Solutions]] | ► **Next:** [[Problem — Dining Philosophers Deadlock-Free Synchronization]]

---
## Problem

Consider a concurrent system with:
- A shared circular buffer of fixed size $N = 3$.
- Two producer threads: $P_1, P_2$.
- Two consumer threads: $C_1, C_2$.
- Three synchronization semaphores:
  1. `mutex = 1` (Binary semaphore for mutual exclusion over the circular buffer array).
  2. `empty = 3` (Counting semaphore tracking vacant slots).
  3. `full  = 0` (Counting semaphore tracking available filled items).

**Circular Buffer Pointers:**
- `in = 0`: Index where the next produced item is placed.
- `out = 0`: Index from which the next consumed item is extracted.

---
## Given

- System state matrices, resource vectors, and process workload parameters as specified in problem setup.

---
## Required

- Determine step-by-step state transitions, verify system invariants, and calculate resulting performance metrics.

---
## Understanding the Problem and Choosing the Method

Analyze initial conditions, verify prerequisite invariants, track state changes iteratively, and check final consistency against theoretical rules.

---
## Solution

### 1. Chronological Step-by-Step Execution Trace

Below is a detailed time trace demonstrating process synchronization, buffer filling, process suspension upon full buffer, and wake-up upon item consumption.

### Step 1: $P_1$ produces Item `A`
- $P_1$ calls `wait(&empty)`: `empty` decrements $3 \to 2$ ($2 \ge 0$, no block).
- $P_1$ calls `wait(&mutex)`: `mutex` decrements $1 \to 0$ ($0 \ge 0$, lock acquired).
- $P_1$ writes `buffer[in] = 'A'` (`buffer[0] = 'A'`), updates `in = (0 + 1) % 3 = 1`.
- $P_1$ calls `signal(&mutex)`: `mutex` increments $0 \to 1$.
- $P_1$ calls `signal(&full)`: `full` increments $0 \to 1$.

### Step 2: $P_2$ produces Item `B`
- $P_2$ calls `wait(&empty)`: `empty` decrements $2 \to 1$.
- $P_2$ calls `wait(&mutex)`: `mutex` decrements $1 \to 0$.
- $P_2$ writes `buffer[1] = 'B'`, updates `in = (1 + 1) % 3 = 2`.
- $P_2$ calls `signal(&mutex)`: `mutex` increments $0 \to 1$.
- $P_2$ calls `signal(&full)`: `full` increments $1 \to 2$.

### Step 3: $P_1$ produces Item `C`
- $P_1$ calls `wait(&empty)`: `empty` decrements $1 \to 0$.
- $P_1$ calls `wait(&mutex)`: `mutex` decrements $1 \to 0$.
- $P_1$ writes `buffer[2] = 'C'`, updates `in = (2 + 1) % 3 = 0`.
- $P_1$ calls `signal(&mutex)`: `mutex` increments $0 \to 1$.
- $P_1$ calls `signal(&full)`: `full` increments $2 \to 3$.
- *Buffer status:* Full (`['A', 'B', 'C']`). `empty = 0`, `full = 3`.

### Step 4: $P_2$ attempts to produce Item `D` (Buffer Full!)
- $P_2$ calls `wait(&empty)`: `empty` decrements $0 \to -1$.
- Because `empty < 0`, $P_2$ is **suspended by the kernel** and enqueued in `empty->queue_head`.
- $P_2$ enters `BLOCKED` state. It has *not* acquired `mutex`!

### Step 5: $C_1$ arrives to consume an item
- $C_1$ calls `wait(&full)`: `full` decrements $3 \to 2$ ($2 \ge 0$, no block).
- $C_1$ calls `wait(&mutex)`: `mutex` decrements $1 \to 0$.
- $C_1$ reads item from `buffer[out]` (`buffer[0] = 'A'`), updates `out = (0 + 1) % 3 = 1`.
- $C_1$ calls `signal(&mutex)`: `mutex` increments $0 \to 1$.
- $C_1$ calls `signal(&empty)`: `empty` increments $-1 \to 0$.
  - Because `empty <= 0`, the kernel removes $P_2$ from the blocked queue and transitions $P_2$ to `READY`!

### Step 6: $P_2$ resumes and finishes inserting Item `D`
- $P_2$ leaves `wait(&empty)` and now calls `wait(&mutex)`: `mutex` decrements $1 \to 0$.
- $P_2$ writes `buffer[in] = 'D'` (`buffer[0] = 'D'`), updates `in = (0 + 1) % 3 = 1`.
- $P_2$ calls `signal(&mutex)`: `mutex` increments $0 \to 1$.
- $P_2$ calls `signal(&full)`: `full` increments $2 \to 3$.

---
### 2. Semaphore State Matrix

| Time | Active Process | Action Taken | `mutex` | `empty` | `full` | `empty` Queue | `full` Queue | Buffer State `[0, 1, 2]` |
|---|---|---|---|---|---|---|---|---|
| $t_0$ | System Init | Initial State | 1 | 3 | 0 | $\emptyset$ | $\emptyset$ | `[ - , - , - ]` |
| $t_1$ | $P_1$ | Inserts `'A'` | 1 | 2 | 1 | $\emptyset$ | $\emptyset$ | `['A', - , - ]` |
| $t_2$ | $P_2$ | Inserts `'B'` | 1 | 1 | 2 | $\emptyset$ | $\emptyset$ | `['A', 'B', - ]` |
| $t_3$ | $P_1$ | Inserts `'C'` | 1 | 0 | 3 | $\emptyset$ | $\emptyset$ | `['A', 'B', 'C']` |
| $t_4$ | $P_2$ | `wait(empty)` $\to$ BLOCKED | 1 | **-1** | 3 | $\{P_2\}$ | $\emptyset$ | `['A', 'B', 'C']` |
| $t_5$ | $C_1$ | Removes `'A'`; signals `empty` | 1 | **0** | 2 | $\emptyset$ ($P_2$ woken) | $\emptyset$ | `[ - , 'B', 'C']` |
| $t_6$ | $P_2$ | Inserts `'D'` into slot 0 | 1 | 0 | 3 | $\emptyset$ | $\emptyset$ | `['D', 'B', 'C']` |

---
### 3. Concrete POSIX C Implementation

```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <semaphore.h>
#include <unistd.h>

#define BUFFER_SIZE 3

char buffer[BUFFER_SIZE];
int in = 0;
int out = 0;

sem_t empty;
sem_t full;
pthread_mutex_t mutex_lock;

void *producer(void *param) {
    char items[3] = {'A', 'B', 'C'};
    for (int i = 0; i < 3; i++) {
        sem_wait(&empty);
        pthread_mutex_lock(&mutex_lock);

        buffer[in] = items[i];
        printf("[Producer] Inserted '%c' at index %d\n", items[i], in);
        in = (in + 1) % BUFFER_SIZE;

        pthread_mutex_unlock(&mutex_lock);
        sem_post(&full);
        sleep(1);
    }
    return NULL;
}

void *consumer(void *param) {
    for (int i = 0; i < 3; i++) {
        sem_wait(&full);
        pthread_mutex_lock(&mutex_lock);

        char item = buffer[out];
        printf("[Consumer] Extracted '%c' from index %d\n", item, out);
        out = (out + 1) % BUFFER_SIZE;

        pthread_mutex_unlock(&mutex_lock);
        sem_post(&empty);
        sleep(2);
    }
    return NULL;
}

int main() {
    pthread_t tid_p, tid_c;
    sem_init(&empty, 0, BUFFER_SIZE);
    sem_init(&full, 0, 0);
    pthread_mutex_init(&mutex_lock, NULL);

    pthread_create(&tid_p, NULL, producer, NULL);
    pthread_create(&tid_c, NULL, consumer, NULL);

    pthread_join(tid_p, NULL);
    pthread_join(tid_c, NULL);

    sem_destroy(&empty);
    sem_destroy(&full);
    pthread_mutex_destroy(&mutex_lock);
    return 0;
}
```

---
## Result

The simulation completes successfully, confirming that all process requests and state transitions respect system invariants.

---
## Why This Works

Each state transformation follows the operational semantics of kernel execution, ensuring mutual exclusion, safe scheduling, or deadlock freedom.

---
## Common Mistakes

- Overlooking state changes between execution phases.
- Incorrectly calculating intermediate residual capacities or queue offsets.

---
## General Method

Extract the generic algorithmic pattern: initialize tracking vectors, simulate execution step by step, verify invariant conditions, and calculate final summary metrics.

---
## Related Concepts

- [[Operating System Structures and Functions]]
- [[Process Concepts and Memory Layout]]

---
## Sources

- Andrew S. Tanenbaum & Herbert Bos, *Modern Operating Systems* (4th Edition)
- Silberschatz et al., *Operating System Concepts* (10th Edition)
