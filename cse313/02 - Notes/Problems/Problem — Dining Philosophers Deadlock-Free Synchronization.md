---
type: problem
course: cse313
status: active
order: 25
---

# Problem — Dining Philosophers Deadlock-Free Synchronization

> 📖 **Reading Order:** Step 25 of 34 | **Module 4:** Inter-Process Communication & Synchronization  
> ◄ **Previous:** [[Producer-Consumer Semaphore Implementation Example]] | ► **Next:** [[Deadlock Fundamentals and Coffman Conditions]]

---

## Problem Statement

Consider the classic Dining Philosophers problem where five philosophers ($P_0, P_1, P_2, P_3, P_4$) sit around a circular table. Between each pair of philosophers is a single chopstick ($C_0, C_1, C_2, C_3, C_4$). A philosopher needs two chopsticks to eat:
- Philosopher $i$ needs Chopstick $i$ (left) and Chopstick $(i + 1) \pmod 5$ (right).

A naive implementation where every philosopher picks up their left chopstick first causes **deadlock** if all five become hungry simultaneously.

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

### Tasks:
1. **Strategy A (Asymmetric / Odd-Even Ordering):**
   - Design an asymmetric solution where odd-numbered philosophers pick up their left chopstick first, while even-numbered philosophers pick up their right chopstick first.
   - Mathematically prove that this strategy eliminates the circular wait condition and guarantees deadlock freedom.
2. **Strategy B (Room Capacity Limiter):**
   - Use a counting semaphore `room` to admit at most 4 philosophers simultaneously into the dining room.
   - Prove using the Pigeonhole Principle why at least one philosopher is always guaranteed to acquire both chopsticks and eat.
3. **Strategy C (Tanenbaum's State-Based Solution Tracing):**
   - Given initial states all `THINKING`:
     - At $t_1$: Philosopher 0 becomes `HUNGRY`.
     - At $t_2$: Philosopher 2 becomes `HUNGRY`.
     - At $t_3$: Philosopher 1 becomes `HUNGRY`.
     - At $t_4$: Philosopher 0 finishes eating and calls `put_forks(0)`.
   - Trace the exact states (`state[i]`), values of semaphores `s[i]`, and explain when Philosopher 1 is unblocked.
4. **Comparative Analysis:** Compare the three strategies in terms of maximum concurrent eaters, starvation susceptibility, and runtime overhead.

---

## Detailed Step-by-Step Solutions

### Part 1: Strategy A (Asymmetric Ordering Solution & Proof)

#### Algorithm Formulation:
```c
semaphore chopstick[5] = {1, 1, 1, 1, 1};

void philosopher(int i) {
    while (TRUE) {
        think();
        if (i % 2 == 0) {
            // Even philosophers: Grab right first, then left
            wait(&chopstick[(i + 1) % 5]);
            wait(&chopstick[i]);
        } else {
            // Odd philosophers: Grab left first, then right
            wait(&chopstick[i]);
            wait(&chopstick[(i + 1) % 5]);
        }
        eat();
        chopstick_release();
    }
}
```

#### Formal Proof of Deadlock Freedom:
- **Claim:** A circular wait cannot exist.
- **Proof:**
  - Let $C_0$ be contested between $P_0$ (even) and $P_1$ (odd).
    - $P_0$'s first choice is $C_1$ (right), second choice is $C_0$ (left).
    - $P_1$'s first choice is $C_1$ (left), second choice is $C_2$ (right).
  - Observe Chopstick $C_1$: Both $P_0$ (its right) and $P_1$ (its left) attempt to acquire $C_1$ as their **very first chopstick**.
  - Since $C_1$ is a binary semaphore initialized to 1, exactly one of $\{P_0, P_1\}$ can acquire $C_1$; the other is immediately blocked before acquiring any chopstick at all.
  - Therefore, at most 4 philosophers can hold a first chopstick at the same time.
  - By the Pigeonhole Principle, with 5 chopsticks and at most 4 philosophers holding at most 1 chopstick, at least one philosopher will find their second chopstick available, eat, and release both.
  - Hence, circular wait is impossible. $\blacksquare$

---

### Part 2: Strategy B (Room Capacity Limiter & Pigeonhole Proof)

#### Algorithm Formulation:
```c
semaphore room = 4; // At most 4 philosophers allowed in dining area
semaphore chopstick[5] = {1, 1, 1, 1, 1};

void philosopher(int i) {
    while (TRUE) {
        think();
        wait(&room);                         // Enter dining room
        wait(&chopstick[i]);                 // Grab left
        wait(&chopstick[(i + 1) % 5]);       // Grab right
        eat();
        signal(&chopstick[i]);
        signal(&chopstick[(i + 1) % 5]);
        signal(&room);                        // Leave dining room
    }
}
```

#### Pigeonhole Principle Proof:
- Let the total number of admitted philosophers be $k \le 4$.
- The total number of chopsticks available is $N = 5$.
- Suppose every admitted philosopher acquires their first chopstick. That consumes at most 4 chopsticks.
- Remaining chopsticks $= 5 - 4 = 1$ chopstick free on the table.
- The free chopstick must belong to either the left or right hand of at least one of the 4 seated philosophers.
- That philosopher acquires the 2nd chopstick, completes eating, and releases both chopsticks.
- Deadlock is strictly impossible. $\blacksquare$

---

### Part 3: Strategy C (State-Based Solution Trace)

Recall Tanenbaum's protocol:
- `state[i]` $\in \{\text{THINKING}, \text{HUNGRY}, \text{EATING}\}$
- `s[i]` initialized to 0.
- `test(i)`: If `state[i] == HUNGRY` and left and right neighbors are not `EATING`, set `state[i] = EATING` and `signal(&s[i])`.

#### Chronological Trace:

| Time | Event | `state[0..4]` | `s[0..4]` values | Action / Outcome |
|---|---|---|---|---|
| $t_0$ | Initial State | `[T, T, T, T, T]` | `[0, 0, 0, 0, 0]` | All thinking. |
| $t_1$ | $P_0$ becomes HUNGRY | `[E, T, T, T, T]` | `[1, 0, 0, 0, 0]` | `test(0)` succeeds (neighbors $P_4, P_1$ are T). $P_0$ sets `state[0]=E`, `signal(s[0])` makes $s[0]=1$. In `take_forks`, `wait(s[0])` decrements $1 \to 0$. **$P_0$ eats.** |
| $t_2$ | $P_2$ becomes HUNGRY | `[E, T, E, T, T]` | `[0, 0, 0, 0, 0]` | `test(2)` checks $P_1$ (T) and $P_3$ (T). Both not eating! $P_2$ sets `state[2]=E`, `s[2]` signaled ($0 \to 1$), then decremented ($1 \to 0$). **$P_2$ eats concurrently with $P_0$!** |
| $t_3$ | $P_1$ becomes HUNGRY | `[E, H, E, T, T]` | `[0, 0, 0, 0, 0]` | `test(1)` checks $P_0$ (E) and $P_2$ (E). Both neighbors are EATING! `test(1)` fails. `s[1]` remains 0. $P_1$ calls `wait(&s[1])` $\to$ **$P_1$ BLOCKS!** |
| $t_4$ | $P_0$ finishes eating | `[T, H, E, T, T]` | `[0, 0, 0, 0, 0]` | $P_0$ calls `put_forks(0)`: sets `state[0] = THINKING`. Calls `test(LEFT)` $\to$ `test(4)` (fails, $P_4$ is T). Calls `test(RIGHT)` $\to$ `test(1)`: checks $P_0$ (T) and $P_2$ (E). But wait! $P_2$ is still EATING! So `test(1)` still sees right neighbor eating $\implies P_1$ cannot eat yet! |
| $t_5$ | $P_2$ finishes eating | `[T, E, T, T, T]` | `[0, 0, 0, 0, 0]` | $P_2$ calls `put_forks(2)`: sets `state[2] = THINKING`. Calls `test(LEFT)` $\to$ `test(1)`: Now left neighbor $P_0$ is T, and right neighbor $P_2$ is T! Condition met! `state[1] = EATING`, `signal(&s[1])` awakens $P_1$. **$P_1$ finally eats!** |

---

### Part 4: Comparative Strategy Evaluation

| Metric | Strategy A (Asymmetric) | Strategy B (Room Semaphore) | Strategy C (State-Based) |
|---|---|---|---|
| **Max Concurrent Eaters** | 2 | 2 | 2 (Optimal $\lfloor 5/2 \rfloor$) |
| **Deadlock Freedom** | Guaranteed (Breaks circular wait) | Guaranteed (Pigeonhole principle) | Guaranteed (Two-fork atomic check) |
| **Starvation Freedom** | No (susceptible without fair queues) | No | No (neighbors could alternate eating) |
| **Semaphores Required** | 5 binary semaphores | 5 binary + 1 counting semaphore | 5 binary + 1 mutex |
| **Implementation Complexity** | Minimal (simple branching) | Very low | Moderate (helper `test()` routine) |

---

## Source Traceability & Metadata
- **Source Material:** `4. IPC-week-4-5-RRR.pptx` (Slides 44–48: The Dining Philosophers Problem).
- **Question ID:** `Q-CSE313-003`
- **Related Notes:**
  - Concept: [[Race Conditions and Critical-Section Problem]], [[Semaphores and Synchronization Primitives]]
  - Algorithm: [[Classic Synchronization Solutions]]
  - Example: [[Producer-Consumer Semaphore Implementation Example]]
