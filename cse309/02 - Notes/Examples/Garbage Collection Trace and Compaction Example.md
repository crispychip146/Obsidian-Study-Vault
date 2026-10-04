---
type: example
course: cse309
status: active
order: 30
---

# Garbage Collection Trace and Compaction Example

> 📖 **Reading Order:** Step 30 of 55 | **Module 3:** Run-Time Environments  
> ◄ **Previous:** [[Display Maintenance and Non-Local Access Simulation Example]] | ► **Next:** [[Problem — Activation Record and Display Table Tracing]]

---

---

## Problem

Consider a program executing with a heap of 100 memory units. The heap currently contains 6 allocated objects:

| Object | Size (units) | Heap Address Range | Pointers Held | Reachable from Root Set? |
| :---: | :---: | :---: | :---: | :---: |
| **$O_1$** | 10 | `[0 .. 9]` | Points to $O_3$ | **YES (in Root Set)** |
| **$O_2$** | 15 | `[10 .. 24]` | Points to $O_4$ | **NO (Dead)** |
| **$O_3$** | 20 | `[25 .. 44]` | Points to $O_5$ | **YES (via $O_1$)** |
| **$O_4$** | 10 | `[45 .. 54]` | Points to $O_2$ (Cyclic) | **NO (Dead)** |
| **$O_5$** | 15 | `[55 .. 69]` | None | **YES (via $O_3$)** |
| **$O_6$** | 10 | `[70 .. 79]` | None | **NO (Dead)** |

Heap space `[80 .. 99]` is currently unallocated.

### Tasks:
1. Simulate the **Mark-and-Sweep** algorithm and illustrate the resulting free list.
2. Simulate the **Mark-and-Compact** algorithm and calculate the `NewLocation` for all live objects.
3. Simulate **Cheney's Copying Collector** assuming a 2-semispace configuration.

---

---

## Given

- Grammar productions, semantic rules, basic blocks, or register sets as specified in the problem setup.

---

## Required

- Full step-by-step annotated tree derivation, TAC generation, DAG reduction, or register assignment trace.

---

## Understanding the Problem and Choosing the Method

Analyze the input program structure, identify the governing compiler phase algorithms, and simulate the execution step by step while maintaining all internal invariants.

---

## Solution

### Simulation 1: Mark-and-Sweep

```
Initial Heap:
[ O1: 10 ] [ O2: 15 ] [ O3: 20 ] [ O4: 10 ] [ O5: 15 ] [ O6: 10 ] [ Unallocated: 20 ]
Address: 0         10         25         45         55         70        80             100
```

1. **Mark Phase:**
   - Root set contains $O_1 \implies \text{mark}(O_1)$.
   - $O_1$ points to $O_3 \implies \text{mark}(O_3)$.
   - $O_3$ points to $O_5 \implies \text{mark}(O_5)$.
   - Marked set: $\{ O_1, O_3, O_5 \}$.
   - Unmarked set (Dead): $\{ O_2, O_4, O_6 \}$.
2. **Sweep Phase:**
   - `[10 .. 24]` ($O_2$): Freed $\implies$ Chunk of size 15 added to free list.
   - `[45 .. 54]` ($O_4$): Freed $\implies$ Chunk of size 10 added to free list.
   - `[70 .. 79]` ($O_6$): Freed $\implies$ Merged with trailing unallocated space `[80 .. 99]` to form a free chunk of size 30.

### Post-Sweep Heap State:
```
[ O1: 10 ] [ Free: 15 ] [ O3: 20 ] [ Free: 10 ] [ O5: 15 ] [ Free: 30 ]
```
**Total Free Space:** $15 + 10 + 30 = 55$ units.  
**Fragmentation Hazard:** If the mutator requests an allocation of size 35, the allocation **FAILS** due to external fragmentation, even though 55 units are completely free!

---
### Simulation 2: Mark-and-Compact (Sliding Compaction)

The Mark-and-Compact algorithm relocates live objects to eliminate all gaps:

### Phase 1: Compute New Addresses (`NewLocation`)
Compaction slides live objects to the beginning of the heap starting at address `0`:
- $O_1$: Size 10 $\implies \text{NewLocation}(O_1) = 0$.
- $O_3$: Size 20 $\implies \text{NewLocation}(O_3) = 0 + 10 = 10$.
- $O_5$: Size 15 $\implies \text{NewLocation}(O_5) = 10 + 20 = 30$.

### Phase 2: Update Pointer Fields
- Root pointer $\to \text{NewLocation}(O_1) = 0$.
- $O_1$'s pointer to $O_3 \to \text{NewLocation}(O_3) = 10$.
- $O_3$'s pointer to $O_5 \to \text{NewLocation}(O_5) = 30$.

### Phase 3: Move Objects
Data bytes are copied to the new contiguous addresses:

```
Compacted Heap Layout:
[ O1: 10 ] [ O3: 20 ] [ O5: 15 ] [ Single Contiguous Free Block: 55 units ]
Address: 0         10         30         45                                100
```
**Result:** Zero fragmentation! A request for 35 units is now easily satisfied in $O(1)$ time.

---
### Simulation 3: Cheney's Copying Collector

Let total memory 100 be split into two 50-unit semispaces:
- `From-space`: addresses `[0 .. 49]`
- `To-space`: addresses `[50 .. 99]`

1. Initial state in To-space: `scan = 50`, `free = 50`.
2. **Copy Root $O_1$:**
   - Copy $O_1$ (size 10) to address `50`.
   - `free` advances to $50 + 10 = 60$.
   - Old $O_1$ gets forwarding pointer $50$. Root updated to $50$.
3. **Scan Object at `50` ($O_1$):**
   - Inspect child pointer $O_3$ (size 20).
   - Copy $O_3$ to address `60`; `free = 80`.
   - $O_1$'s child pointer updated to $60$. Old $O_3$ gets forwarding pointer $60$.
   - `scan` advances to `60`.
4. **Scan Object at `60` ($O_3$):**
   - Inspect child pointer $O_5$ (size 15).
   - Copy $O_5$ to address `80`; `free = 95`.
   - $O_3$'s child pointer updated to $80$. Old $O_5$ gets forwarding pointer $80$.
   - `scan` advances to `80`.
5. **Scan Object at `80` ($O_5$):**
   - No pointers $\implies$ `scan` advances to `95`.
6. `scan == free == 95`: Collection terminates!
7. To-space has contiguous live objects in `[50 .. 94]`, with 5 units remaining before next swap.

---

---

## Result

The compilation pass finishes with verified intermediate representations and correct register assignments.

---

## Why This Works

Every transformation maintains semantic program equivalence while optimizing instruction counts, memory foot-print, or register usage.

---

## Common Mistakes

- Incorrectly calculating stack frame offsets or TAC temporaries.
- Forgetting to spill registers when register demand exceeds hardware pool size.

---

## General Method

Extract the general procedure: parse/partition input, construct intermediate data structures, apply optimizations iteratively, and emit final code.

---

## Related Concepts

- [[Syntax-Directed Definitions and Translation Schemes]]
- [[Basic Blocks and Control Flow Graphs]]

---

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Chapter 7 (Slides 270–293).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 7.6.
