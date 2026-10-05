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

## Solution

The object table is a pointer graph, not merely an address diagram. Start from O1 and follow O1→O3→O5. Those three objects survive. O2 and O4 point to each other, but have no path from a root, so their cycle is garbage by [[Trace-Based Garbage Collection Algorithms]].

Mark-and-sweep returns their regions and O6's region to the free pool without moving O1, O3, or O5. Compaction places the 45 live units consecutively, but must rewrite every pointer affected by relocation.

For the copying comparison, use a separate hypothetical destination region with enough capacity. The original example occupies addresses through 79, so it cannot already be a valid heap with only [0,49] as a 50-unit active semispace. Treating half the stated 100-unit region as its existing from-space would contradict the initial layout.

[[Copying Garbage Collection Algorithm]] then copies O1, O3, and O5, updating pointers through forwarding addresses. Different collection methods preserve the same reachable graph while producing different free-space layouts.

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

The stated object layout cannot fit in a 50-unit active semispace. For this comparison, retain the original `[0 .. 99]` region as from-space and reserve a separate 100-unit to-space `[100 .. 199]`. This explicitly changes the memory reservation for part 3, while keeping the input object graph and sizes unchanged.

| Event | Destination/action | `scan` after event | `free` after event |
|---|---|---|---|
| Initialize | Empty to-space | 100 | 100 |
| Copy root O1 | O1 occupies `[100 .. 109]`; root becomes 100 | 100 | 110 |
| Scan O1 | Copy O3 to `[110 .. 129]`; rewrite O1's pointer to 110 | 110 | 130 |
| Scan O3 | Copy O5 to `[130 .. 144]`; rewrite O3's pointer to 130 | 130 | 145 |
| Scan O5 | No child pointers | 145 | 145 |

Each old live object stores its forwarding address. `scan == free` means every copied object has been scanned. Swap the regions' roles; the new active region has 45 live units and 55 free units. The old region becomes the destination for the next collection.

## Result

All three methods retain O1, O3, and O5. Sweep leaves separate holes; compaction and the amended copying setup produce contiguous survivors. The copying setup requires additional reserved destination memory rather than splitting the inconsistent original layout in half.

## What to carry forward

Verify live size 10+20+15=45 and dead allocated size 15+10+10=35. Initial unallocated space contributes another 20 units. A moving collector must preserve graph edges as well as those totals.

## Related notes

- [[Trace-Based Garbage Collection Algorithms]]
- [[Copying Garbage Collection Algorithm]]

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Chapter 7 (Slides 270–293).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 7.6.
