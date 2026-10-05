---
type: problem
course: cse309
status: active
order: 49
---

# Problem — Linear Scan Register Allocation Simulation

> 📖 **Reading Order:** Step 49 of 55 | **Module 5:** Register Allocation  
> ◄ **Previous:** [[Chaitin's Graph Coloring Register Allocation Example]] | ► **Next:** [[Problem — Chaitin Graph Coloring Register Allocation]]

---

## Problem

A Just-In-Time compiler computes the following set of live intervals for 6 variables:

| Variable | Live Interval $[start, end]$ |
| :---: | :---: |
| **$v_1$** | $[1, 4]$ |
| **$v_2$** | $[2, 6]$ |
| **$v_3$** | $[3, 8]$ |
| **$v_4$** | $[5, 7]$ |
| **$v_5$** | $[7, 10]$ |
| **$v_6$** | $[8, 11]$ |

Assume the target processor provides **$R = 2$ physical registers**: $\{ R_0, R_1 \}$.

### Tasks:
1. Sort the intervals by start point and simulate the **Linear Scan Register Allocation** algorithm step-by-step.
2. Show the contents of the `active` list, the `free_registers` pool, and any register expirations or spill actions at each step.
3. State the final allocation (register assigned or spilled) for every variable.

---

## Solution

There are only two registers, so the third simultaneously active interval forces a choice. Process intervals by start, expire completed ones, and apply the specified latest-end heuristic to the remaining active set and the newcomer.

For v3 starting at 3, v1 ends at 4 and v2 at 6, while v3 ends at 8. Keeping the two earlier-ending intervals means v3 is spilled. At later starts, repeat the comparison using the updated state rather than permanently reserving registers for their original owners.

[[Linear Scan Register Allocation Algorithm]] supplies the rule; [[Live Ranges and Live Intervals in Register Allocation]] supplies endpoint meaning. Under closed intervals, an active interval ending at 7 does not expire for another starting at 7. It expires only before a strictly later start.

For every row, show whether the algorithm spills the new interval or evicts an active one. These two actions produce different ownership changes even when both reduce pressure.

### Intervals Sorted by Start Point:
1. $v_1: [1, 4]$
2. $v_2: [2, 6]$
3. $v_3: [3, 8]$
4. $v_4: [5, 7]$
5. $v_5: [7, 10]$
6. $v_6: [8, 11]$

---

### Step-by-Step Simulation:

#### Step 1: Process $v_1: [1, 4]$
- `expireOldIntervals(v1)`: No intervals in active.
- `len(active) = 0 < 2`.
- Allocate $R_0$ to $v_1$.
- `active = [ v1:4 ]`
- `free_registers = { R1 }`

---

#### Step 2: Process $v_2: [2, 6]$
- `expireOldIntervals(v2)`: $v_1.end = 4 > 2$ (not expired).
- `len(active) = 1 < 2`.
- Allocate $R_1$ to $v_2$.
- `active = [ v1:4, v2:6 ]`
- `free_registers = { }`

---

#### Step 3: Process $v_3: [3, 8]$
- `expireOldIntervals(v3)`: $v_1.end = 4 > 3$, $v_2.end = 6 > 3$ (none expired).
- `len(active) = 2 == R` $\implies$ **Registers are FULL! Spill required.**
- `candidate = active[-1]` $\implies v_2:6$.
- Compare end points:
  - $candidate.end = 6$
  - $current.end = 8$
  - Since $v_3.end = 8 > v_2.end = 6$, current interval $v_3$ lives longer!
- **Action:** **Spill $v_3$!**
- `active = [ v1:4, v2:6 ]`
- Allocation: $v_3 \to \text{SPILLED}$.

---

#### Step 4: Process $v_4: [5, 7]$
- `expireOldIntervals(v4)`:
  - $v_1.end = 4 < 5 \implies$ **$v_1$ has expired!**
  - Reclaim register $R_0$ from $v_1$.
  - $v_2.end = 6 > 5$ (not expired).
  - `active = [ v2:6 ]`, `free_registers = { R0 }`.
- `len(active) = 1 < 2`.
- Allocate $R_0$ to $v_4$.
- `active = [ v2:6, v4:7 ]`
- `free_registers = { }`

---

#### Step 5: Process $v_5: [7, 10]$
- `expireOldIntervals(v5)`:
  - $v_2.end = 6 < 7 \implies$ **$v_2$ has expired!** (Reclaim $R_1$).
  - $v_4.end = 7 \le 7$ (Wait, if intervals are inclusive $[s, e]$, $v_4$ is used at $7$. If strict inequality $end < start$, $v_4$ expires at point $8$. In either standard, $v_2$ has expired).
  - Reclaim $R_1$.
- `len(active) = 1 < 2`.
- Allocate $R_1$ to $v_5$.
- `active = [ v4:7, v5:10 ]`
- `free_registers = { }`

---

#### Step 6: Process $v_6: [8, 11]$
- `expireOldIntervals(v6)`:
  - $v_4.end = 7 < 8 \implies$ **$v_4$ has expired!**
  - Reclaim $R_0$.
  - `active = [ v5:10 ]`, `free_registers = { R0 }`.
- Allocate $R_0$ to $v_6$.
- `active = [ v5:10, v6:11 ]`.

---
### Final Allocation Table

| Variable | Live Interval | Status | Assigned Hardware Register |
| :---: | :---: | :---: | :---: |
| **$v_1$** | $[1, 4]$ | Allocated | **$R_0$** |
| **$v_2$** | $[2, 6]$ | Allocated | **$R_1$** |
| **$v_3$** | $[3, 8]$ | **Spilled** | Memory Stack Slot |
| **$v_4$** | $[5, 7]$ | Allocated | **$R_0$** (reused from $v_1$) |
| **$v_5$** | $[7, 10]$ | Allocated | **$R_1$** (reused from $v_2$) |
| **$v_6$** | $[8, 11]$ | Allocated | **$R_0$** (reused from $v_4$) |

**Summary:** 5 out of 6 variables successfully fit into only 2 hardware registers with just a single spill ($v_3$)!

---

## What to carry forward

Check overlap against register assignments and verify each free register has no active owner. Latest endpoint is a simple heuristic; practical spill cost also depends on uses, loops, reloads, and target constraints.

## Related notes

- [[Linear Scan Register Allocation Algorithm]]
- [[Live Ranges and Live Intervals in Register Allocation]]

## Source

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Register Allocation (Slides 391–425).
- **Textbook / Papers:** Poletto & Sarkar, "Linear Scan Register Allocation", ACM TOPLAS 1999.
