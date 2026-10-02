---
type: algorithm
course: cse313
status: active
order: 23
---

# Classic Synchronization Solutions

> 📖 **Reading Order:** Step 23 of 34 | **Module 4:** Inter-Process Communication & Synchronization  
> ◄ **Previous:** [[Message Passing and IPC Models]] | ► **Next:** [[Producer-Consumer Semaphore Implementation Example]]

---

## 1. Algorithmic Overview & Motivation

To evaluate and design synchronization primitives, computer scientists formalized canonical concurrency problems. These benchmarks model real-world operating system challenges:
1. **Producer-Consumer (Bounded Buffer):** Buffer overflow and underflow prevention with mutual exclusion.
2. **Readers-Writers:** Distinguishing shared read-only access from exclusive write access.
3. **Dining Philosophers:** Resource contention, deadlock prevention, and starvation freedom.
4. **Sleeping Barber:** Asymmetric customer-server coordination with finite waiting capacity.

---

## 2. The Producer-Consumer (Bounded Buffer) Problem

### Invariants:
- A shared circular buffer holds at most $N$ items.
- Producers must block when buffer is full (`count == N`).
- Consumers must block when buffer is empty (`count == 0`).
- Buffer insertion and extraction must be mutually exclusive.

### Semaphore Formulation:
- `empty`: Counting semaphore initialized to $N$ (free slots).
- `full`: Counting semaphore initialized to $0$ (occupied slots).
- `mutex`: Binary semaphore initialized to $1$ (protects buffer array).

```c
#define N 100
semaphore mutex = 1;
semaphore empty = N;
semaphore full  = 0;

void producer(void) {
    item data;
    while (TRUE) {
        data = produce_item();
        wait(&empty);         // Decrement empty slots (blocks if buffer is full)
        wait(&mutex);         // Lock buffer access
        insert_item(data);    // CRITICAL SECTION
        signal(&mutex);       // Unlock buffer
        signal(&full);        // Increment full slots (wakes consumer)
    }
}

void consumer(void) {
    item data;
    while (TRUE) {
        wait(&full);          // Decrement full slots (blocks if buffer is empty)
        wait(&mutex);         // Lock buffer access
        data = remove_item(); // CRITICAL SECTION
        signal(&mutex);       // Unlock buffer
        signal(&empty);       // Increment empty slots (wakes producer)
        consume_item(data);
    }
}
```

> [!CAUTION] The Fatal Semaphore Inversion Bug
> Notice the order of the `wait` statements:
> ```c
> wait(&empty);
> wait(&mutex);
> ```
> If a programmer accidentally swaps them to:
> ```c
> wait(&mutex);
> wait(&empty);
> ```
> Suppose the buffer is completely full (`empty == 0`). The producer acquires `mutex`, then executes `wait(&empty)` and blocks! But because the producer holds `mutex`, the consumer cannot enter its critical section to remove an item. **Both processes are deadlocked forever!**
> **Rule:** *Always acquire resource semaphores before acquiring mutual exclusion semaphores.*

---

## 3. The Readers-Writers Problem

### Invariants:
- Multiple readers may read shared data concurrently without interference.
- Only one writer may access the shared data at a time.
- While a writer is writing, **no readers** may read.

### Readers-Preference Solution:
```c
int read_count = 0;
semaphore mutex    = 1; // Protects read_count
semaphore rw_mutex = 1; // Protects shared database/file

void writer(void) {
    while (TRUE) {
        wait(&rw_mutex);       // Request exclusive database access
        write_database();      // Critical Section: Exclusive write
        signal(&rw_mutex);     // Release exclusive database access
    }
}

void reader(void) {
    while (TRUE) {
        wait(&mutex);
        read_count++;
        if (read_count == 1) {
            wait(&rw_mutex);   // First reader locks database against writers
        }
        signal(&mutex);

        read_database();       // Reading allowed concurrently!

        wait(&mutex);
        read_count--;
        if (read_count == 0) {
            signal(&rw_mutex); // Last reader releases database for writers
        }
        signal(&mutex);
    }
}
```

### Analysis & Starvation Trade-Off:
- **Advantage:** Maximum concurrency for readers.
- **Flaw (Writer Starvation):** If readers arrive continuously such that `read_count` never drops to 0, waiting writers will **starve indefinitely**.
- **Alternative:** Writer-preference solutions ensure that once a writer requests access, new incoming readers are queued until the writer completes.

---

## 4. The Dining Philosophers Problem

Five philosophers sit around a circular table. Between each pair of philosophers is a single chopstick (total 5 chopsticks). A philosopher alternates between thinking and eating. To eat, a philosopher must acquire **both** their left and right chopsticks.

```
       Philosopher 0
         /       \
      Chop 4    Chop 0
       /           \
Philosopher 4     Philosopher 1
      |              |
    Chop 3         Chop 1
       \           /
Philosopher 3 --- Chop 2 --- Philosopher 2
```

### The Naive Deadlock Trap:
```c
// Each chopstick is a semaphore initialized to 1
void philosopher(int i) {
    while (TRUE) {
        think();
        wait(&chopstick[i]);                 // Grab left chopstick
        wait(&chopstick[(i + 1) % 5]);       // Grab right chopstick
        eat();
        signal(&chopstick[i]);
        signal(&chopstick[(i + 1) % 5]);
    }
}
```
If all 5 philosophers get hungry at the same moment and grab their left chopsticks simultaneously, all 5 left chopsticks are acquired. Each philosopher attempts to grab their right chopstick, which is held by their neighbor. **Deadlock occurs: circular wait!**

### Tanenbaum's State-Based Deadlock-Free Solution:
We model each philosopher's state explicitly (`THINKING`, `HUNGRY`, `EATING`). A philosopher only eats if *neither neighbor is eating*:

```c
#define N           5
#define LEFT        (i + N - 1) % N
#define RIGHT       (i + 1) % N
#define THINKING    0
#define HUNGRY      1
#define EATING      2

int state[N];
semaphore mutex = 1;       // Protects critical regions modifying state
semaphore s[N];            // One semaphore per philosopher (initialized to 0)

void test(int i) {
    if (state[i] == HUNGRY && state[LEFT] != EATING && state[RIGHT] != EATING) {
        state[i] = EATING;
        signal(&s[i]);     // Awaken philosopher i
    }
}

void take_forks(int i) {
    wait(&mutex);
    state[i] = HUNGRY;
    test(i);               // Try to acquire two forks
    signal(&mutex);
    wait(&s[i]);           // Block if forks were not acquired
}

void put_forks(int i) {
    wait(&mutex);
    state[i] = THINKING;
    test(LEFT);            // Check if left neighbor can now eat
    test(RIGHT);           // Check if right neighbor can now eat
    signal(&mutex);
}

void philosopher(int i) {
    while (TRUE) {
        think();
        take_forks(i);
        eat();
        put_forks(i);
    }
}
```

---

## 5. The Sleeping Barber Problem

A barbershop has 1 barber, 1 barber chair, and $N$ waiting chairs.
- If there are no customers, the barber sleeps in the barber chair.
- When a customer arrives:
  - If all chairs are occupied, the customer leaves.
  - If chairs are available, the customer sits in a waiting chair. If the barber is asleep, the customer awakens him.

```c
#define CHAIRS 5
semaphore customers = 0; // Number of waiting customers
semaphore barbers   = 0; // 0 = barber busy/sleeping, 1 = barber ready
semaphore mutex     = 1; // Protects waiting variable
int waiting = 0;         // Customers waiting for haircut

void barber(void) {
    while (TRUE) {
        wait(&customers);   // Sleep if no customers are waiting
        wait(&mutex);       // Lock waiting count
        waiting--;          // Take one customer from waiting room
        signal(&barbers);   // Barber is ready to cut hair
        signal(&mutex);     // Unlock waiting count
        cut_hair();
    }
}

void customer(void) {
    wait(&mutex);
    if (waiting < CHAIRS) {
        waiting++;
        signal(&customers); // Wake up barber if sleeping
        signal(&mutex);
        wait(&barbers);     // Wait until barber is ready
        get_haircut();
    } else {
        signal(&mutex);     // Shop full; leave without haircut
    }
}
```

---

## Source Traceability & Metadata
- **Source Material:** `4. IPC-week-4-5-RRR.pptx` (Slides 35–52: Producer-Consumer with Semaphores, Dining Philosophers, Readers and Writers, Sleeping Barber Problem).
- **Previous Topic:** [[Message Passing and IPC Models]] (Step 22).
- **Next Topic:** [[Producer-Consumer Semaphore Implementation Example]] (Step 24).
