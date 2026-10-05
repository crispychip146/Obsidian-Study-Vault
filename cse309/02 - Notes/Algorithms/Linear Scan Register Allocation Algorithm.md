---
type: algorithm
course: cse309
status: active
order: 45
---

# Linear Scan Register Allocation Algorithm

> 📖 **Reading Order:** Step 45 of 55 | **Module 5:** Register Allocation  
> ◄ **Previous:** [[Register Interference Graphs and Graph Coloring Principles]] | ► **Next:** [[Chaitin's Graph Coloring Register Allocation Algorithm]]

---

## Building the idea

Linear scan visits intervals in start order and maintains an **active** set whose registers are currently occupied. Before handling a new interval, expire those whose endpoints are already past, releasing their registers.

If a register is free, assign it. If all are occupied, compare the new interval's end with the latest end among active intervals. The basic heuristic keeps the intervals ending sooner and spills the one ending latest, hoping to free capacity sooner. It is a heuristic, not a proof of globally minimum spill cost.

[[Live Ranges and Live Intervals in Register Allocation]] explains the approximation being allocated. With closed intervals [s,e], an interval ending at e still overlaps one starting at e, so the expiration test is `end < start`. A half-open convention would use a different test.

Maintain active order by end, free-register ownership, and each interval's storage assignment. A victim's register is transferred only after the required spill handling preserves its value.

## How It Works

The algorithm transitions through defined phases.

---
## Pseudocode

### The Algorithmic Implementation

```python
def linear_scan_register_allocation(intervals, R):
    # Sort all intervals by their start point
    intervals.sort(key=lambda x: x.start)
    
    active = []
    free_registers = list(range(R))  # Pool of registers 0 .. R-1
    allocation = {}                  # interval -> register or 'spilled'

    def expire_old_intervals(current_interval):
        # Remove any interval from active whose end point precedes current start
        nonlocal active, free_registers
        remaining_active = []
        for j in active:
            if j.end < current_interval.start:
                # Interval j has expired: reclaim its physical register!
                reg = allocation[j]
                free_registers.append(reg)
            else:
                remaining_active.append(j)
        active = remaining_active

    def spill_at_interval(current_interval):
        # Spills either current_interval or the active interval with latest end
        nonlocal active, allocation
        # active is sorted by end point; candidate is the last item
        candidate = active[-1]
        
        if candidate.end > current_interval.end:
            # Candidate lives longer: spill candidate, give its register to current!
            reg = allocation[candidate]
            allocation[candidate] = "spilled"
            allocation[current_interval] = reg
            
            # Replace candidate with current_interval in active list
            active.remove(candidate)
            active.append(current_interval)
            active.sort(key=lambda x: x.end)
        else:
            # Current interval lives longer: spill current interval directly!
            allocation[current_interval] = "spilled"

    # Main allocation loop
    for i in intervals:
        expire_old_intervals(i)
        
        if len(active) == R:
            # All registers are full: must spill!
            spill_at_interval(i)
        else:
            # Allocate a free register
            reg = free_registers.pop(0)
            allocation[i] = reg
            active.append(i)
            active.sort(key=lambda x: x.end)
            
    return allocation
```

---

## Complexity

- **Sorting Intervals:** $O(V \log V)$, where $V$ is the number of variables.
- **Allocation Loop:** Each interval is inserted and removed from the `active` list at most once. Since $|active| \le R$, each operation takes $O(R)$ or $O(\log R)$ with a priority queue.
- **Overall Time Complexity:** **$\mathbf{O(V \log V + V \log R)}$** (linear in practice).
- **Space Complexity:** $O(V)$ storing live intervals.

---

## Limitations

### Key Mechanics: Why Spill the Latest End Point?

When all $R$ registers are occupied and interval $i$ arrives:
$$\text{Candidate} = \arg\max_{j \in \text{active} \cup \{ i \}} (j.end)$$

Spilling the variable whose lifetime extends **farthest into the future**:
1. Keeps registers available for variables that need them soon.
2. Minimizes future register contention.
3. This directly implements **Belady's MIN replacement algorithm** (proven optimal for fixed cache replacement)!

---

## Exam Relevance

---

### Exam Relevance

Frequently tested on final examinations via hand-simulation of Linear Scan Register Allocation Algorithm on given code fragments or graphs.

---

## What to carry forward

[[Linear Scan Register Allocation Step-by-Step Example]] shows how each new interval changes the state. Sorting and active-set operations determine complexity; “linear scan” does not remove a separate sorting cost or imply every implementation is strictly O(V).

## Related notes

- [[Live Ranges and Live Intervals in Register Allocation]]
- [[Linear Scan Register Allocation Step-by-Step Example]]

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Register Allocation (Slides 391–425).
- **Textbook / Papers:** Poletto & Sarkar, "Linear Scan Register Allocation", ACM TOPLAS 1999.
