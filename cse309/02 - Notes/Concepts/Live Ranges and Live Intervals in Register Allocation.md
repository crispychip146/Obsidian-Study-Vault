---
type: concept
course: cse309
status: active
order: 43
---

# Live Ranges and Live Intervals in Register Allocation

> 📖 **Reading Order:** Step 43 of 55 | **Module 5:** Register Allocation  
> ◄ **Previous:** [[Problem — DAG Optimization of Basic Block with Array Store]] | ► **Next:** [[Register Interference Graphs and Graph Coloring Principles]]

---

## Building the idea

A computed value needs storage only while some future use may still require it. Its **live range** consists of the relevant program points; different definitions of one source variable may form different ranges.

A **live interval** approximates that range along a chosen linear instruction order, from an early required point to a late one. If the true range has holes, the interval includes them, creating conservative overlaps that may increase register pressure.

An illustrative value used at positions 2 and 10 may not be needed on every path or at every intervening point. A single interval hides those details; splitting can create separate register-resident pieces joined by copies or spills.

[[Liveness and Next-Use Analysis within Basic Blocks]] develops the local need information. Register allocation uses that information to decide which values may share a physical register without overwriting one another while both are required.

## How It Works

### Live Range Splitting

When register pressure is high, a single variable with a long live interval may conflict with many other variables:

```
Variable x:   [------------------ Long Live Interval ------------------]
Contenders:        [-- a --]        [-- b --]        [-- c --]
```
If we must spill $x$, spilling $x$ across the *entire* program generates heavy memory traffic.

**Live Range Splitting** divides the long interval of $x$ into smaller independent sub-intervals $[s_1, e_1]$ and $[s_2, e_2]$ separated by explicit spill stores and loads. This allows $x$ to occupy a register during critical inner loops and spill to memory only during intermediate dormant periods.

---

### Register Interference

Two variables $u$ and $v$ **interfere** with each other if their live intervals **overlap**:
$$[ \text{start}_u, \text{end}_u ] \cap [ \text{start}_v, \text{end}_v ] \ne \emptyset$$

### The Golden Rule of Register Allocation:
If two variables interfere, they **CANNOT** be assigned the same physical hardware register. If their live intervals are completely disjoint ($[ \text{start}_u, \text{end}_u ] \cap [ \text{start}_v, \text{end}_v ] = \emptyset$), they can safely share the same physical register!

---
### Trade-Off: Precise Live Ranges vs. Live Intervals

| Representation | Precision | Algorithmic Paradigm | Primary Use Case |
| :--- | :--- | :--- | :--- |
| **Live Intervals** ($[s, e]$) | Conservative (ignores holes between uses) | [[Linear Scan Register Allocation Algorithm]] | Just-In-Time (JIT) Compilers (V8, JVM HotSpot) where compilation speed is paramount. |
| **Exact Live Ranges** | Highly precise | [[Chaitin's Graph Coloring Register Allocation Algorithm]] | Ahead-of-Time (AOT) Compilers (GCC, Clang/LLVM) optimizing for maximum execution performance. |

---

## What to carry forward

[[Linear Scan Register Allocation Algorithm]] exploits linear intervals. [[Register Interference Graphs and Graph Coloring Principles]] represents conflicts more directly. State endpoint conventions: inclusive and half-open intervals require different expiration comparisons.

## Related notes

- [[Liveness and Next-Use Analysis within Basic Blocks]]
- [[Linear Scan Register Allocation Algorithm]]
- [[Register Interference Graphs and Graph Coloring Principles]]

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Register Allocation (Slides 366–390).
- **Textbook / Papers:** Poletto & Sarkar, "Linear Scan Register Allocation", ACM TOPLAS 1999; Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 8.8.
