---
type: example
course: cse313
status: active
order: 10
---

# Process Forking and Zombie Orphan Example

> 📖 **Reading Order:** Step 10 of 34 | **Module 2:** Processes & Threads  
> ◄ **Previous:** [[CPU Multiprogramming Utilization Formula]] | ► **Next:** [[Problem — Fork Execution Tree and Process Tracing]]

---

## Problem

Understanding process creation, variable isolation, and process termination in UNIX requires analyzing real POSIX C implementations.

We examine three fundamental scenarios:
1. **Scenario 1:** Standard process branching with `fork()` and demonstrating private address space variable isolation.
2. **Scenario 2:** Constructing and observing a **Zombie Process** using `sleep()`.
3. **Scenario 3:** Constructing and observing an **Orphan Process** adopted by `init`/`systemd`.

---

## Solution

Read the fork examples as histories of **two identities**. In [[Process Creation and Termination Operations]], a successful fork produces two return paths. Each process then updates its own ordinary variables. Thus the child's `global_counter=65` and the parent's `global_counter=50` can coexist without contradiction: they belong to distinct address spaces.

For termination, ask which side ends first. If the child exits while the parent has not collected its status, the child becomes a **zombie**: it is no longer executing, but an exit record remains. If the parent exits while the child is still running, the child becomes an **orphan**: its execution continues under a new parent or reaper.

The distinction is about relationships and lifetime, not two kinds of running background jobs. In a controlled trace, mark the fork, each branch's updates, each exit, and each wait. Scheduling can change the order of messages, but it cannot make a child's ordinary private variable update change the parent's copy.

The printed PIDs below are illustrative. They identify roles in the trace; they are not outputs that every run must reproduce.

### C Implementation:
```c
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>

int global_counter = 50;

int main() {
    int local_val = 10;
    pid_t pid = fork();

    if (pid < 0) {
        perror("Fork failed");
        return 1;
    } else if (pid == 0) {
        // Child Process
        global_counter += 15;
        local_val += 5;
        printf("[CHILD] PID=%d, Parent PID=%d | global=%d, local=%d\n",
               getpid(), getppid(), global_counter, local_val);
    } else {
        // Parent Process
        wait(NULL); // Wait for child to complete
        printf("[PARENT] PID=%d | global=%d, local=%d\n",
               getpid(), global_counter, local_val);
    }
    return 0;
}
```

### Execution Output:
```text
[CHILD] PID=4521, Parent PID=4520 | global=65, local=15
[PARENT] PID=4520 | global=50, local=10
```

### Detailed Analysis:
- Notice that even though the child modified `global_counter` (to 65) and `local_val` (to 15), the parent's values remained **completely unchanged** (50 and 10).
- This proves that `fork()` creates an **independent copy** of the address space. Child and parent do NOT share memory variables!

---

### Scenario 2: Creating a Zombie Process in C

A zombie occurs when a child terminates, but its parent is sleeping or busy and fails to call `wait()`.

### C Implementation:
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main() {
    pid_t pid = fork();

    if (pid == 0) {
        // Child immediately terminates
        printf("[CHILD] PID=%d exiting immediately...\n", getpid());
        exit(0);
    } else {
        // Parent sleeps without calling wait()
        printf("[PARENT] PID=%d created child PID=%d. Sleeping for 30s...\n", getpid(), pid);
        sleep(30);
        printf("[PARENT] Done sleeping. Exiting.\n");
    }
    return 0;
}
```

### Terminal Observation during the 30-Second Window:
Running `ps -l` or `ps aux | grep Z` in another terminal window produces:
```text
UID   PID  PPID  C STIME TTY          TIME CMD
1000 4520  3210  0 14:02 pts/1    00:00:00 ./zombie_prog
1000 4521  4520  0 14:02 pts/1    00:00:00 [zombie_prog] <defunct>
```
- The child (PID 4521) is marked **`<defunct>`** (state `Z`).
- It has released its memory and file descriptors, but its PCB remains in the Process Table waiting for PID 4520 to invoke `wait()`.
- When the parent finishes its 30-second sleep and exits, the zombie child is adopted by `systemd` (PID 1), which reaps it instantly.

---

### Scenario 3: Creating an Orphan Process in C

An orphan occurs when the parent terminates while the child continues executing.

### C Implementation:
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main() {
    pid_t pid = fork();

    if (pid == 0) {
        // Child displays parent PID, sleeps, then checks parent PID again
        printf("[CHILD] PID=%d, Initial Parent PID=%d\n", getpid(), getppid());
        sleep(5);
        printf("[CHILD] Woke up! PID=%d, New Parent PID=%d (Adopted by systemd)\n",
               getpid(), getppid());
    } else {
        // Parent exits immediately without waiting
        printf("[PARENT] PID=%d exiting immediately...\n", getpid());
        exit(0);
    }
    return 0;
}
```

### Execution Output:
```text
[PARENT] PID=5100 exiting immediately...
[CHILD] PID=5101, Initial Parent PID=5100
$ 
[CHILD] Woke up! PID=5101, New Parent PID=1 (Adopted by systemd)
```
- While the child slept, parent PID 5100 died.
- The operating system kernel automatically updated the child's `PPID` field in its PCB to point to **PID 1 (`systemd` / `init`)**.
- The shell prompt `$` returned early because the parent exited, but the child continued running safely in the background as an orphan.

---

### How to Properly Reap Child Exit Status

To prevent zombies, a parent should always use `wait(&status)` or `waitpid(pid, &status, options)`:

```c
int status;
pid_t child_pid = wait(&status);

if (WIFEXITED(status)) {
    // Child exited normally via exit(code) or return code
    printf("Child %d exited normally with exit code: %d\n", 
           child_pid, WEXITSTATUS(status));
} else if (WIFSIGNALED(status)) {
    // Child was killed by an unhandled signal (e.g., SIGSEGV, SIGKILL)
    printf("Child %d killed by signal: %d\n", 
           child_pid, WTERMSIG(status));
}
```

---

## Common Mistakes

- **Output Order Non-Determinism:** Never assume the child will print before the parent or vice versa. Process scheduling order depends entirely on the CPU scheduler!
- **Memory Copy Rule:** Any modification to variables in the child process is strictly local to the child. The parent will **never** see variable mutations made by the child.

---

## What to carry forward

Check both execution and cleanup. A live orphan can still need CPU time; a zombie cannot execute. Reparenting may use an eligible subreaper rather than PID 1. [[Problem — Fork Execution Tree and Process Tracing]] develops the same branch reasoning when several fork calls are reachable.

## Related notes

- [[Process Creation and Termination Operations]]
- [[Problem — Fork Execution Tree and Process Tracing]]

## Sources

- Andrew S. Tanenbaum & Herbert Bos, *Modern Operating Systems* (4th Edition)
- Silberschatz et al., *Operating System Concepts* (10th Edition)
