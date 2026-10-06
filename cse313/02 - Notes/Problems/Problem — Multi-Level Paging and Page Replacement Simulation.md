---
type: problem
course: cse313
status: active
order: 49
---

# Problem — Multi-Level Paging and Page Replacement Simulation

> 📖 **Reading Order:** Step 49 of 68 | **Module 7: Paging & Virtual Memory Systems**  
> ◄ **Previous:** [[Two-Level Page Table Translation and Clock Replacement Example]] | ► **Next:** [[IO System Architecture and Direct Memory Access]]

---

## Problem Statement

### Catalog Identifier: `Q-CSE313-007`

#### Part A: Hierarchical Paging Architecture and Memory Sizing (8 Marks)
A 32-bit virtual memory architecture uses a two-level hierarchical page table. The system has 8 KB pages ($2^{13}$ bytes). Each Page Table Entry (PTE) and Page Directory Entry (PDE) is 4 bytes.
1. Determine the bit allocation for the Virtual Address: how many bits are allocated to the **Page Directory Index**, **Page Table Index**, and **Offset**?
2. A process has a memory footprint consisting of:
   - 32 KB of Code starting at virtual address `0x00000000`.
   - 16 KB of Heap starting at virtual address `0x10000000`.
   - 24 KB of Stack located at the top of the address space ending at `0xFFFFFFFF`.
   Compute the total physical memory in bytes consumed by the page table structures (Page Directory and Page Table pages) for this process under:
   - A single-level linear page table.
   - The two-level page table.

#### Part B: Comparative Page Replacement Simulation (7 Marks)
Consider a system with **3 physical page frames** initially empty. The sequence of page references is:
$$\mathbf{1, 2, 3, 4, 2, 1, 5, 6, 2, 1, 2, 3, 7, 6, 3, 2, 1, 2, 3, 6}$$
Simulate the page replacement behavior and compute the total number of page faults for:
1. **FIFO (First-In, First-Out)**
2. **LRU (Least Recently Used)**
3. **Optimal (Belady's MIN)**

---

## Understanding the Problem and Choosing the Method

- **Part A:** Offset bits $= \log_2(8192) = 13$ bits. Each page holds $\frac{8192}{4} = 2048 = 2^{11}$ entries, so the inner Page Table Index consumes 11 bits. The remaining $32 - (13 + 11) = 8$ bits form the Page Directory Index. Under linear paging, all $2^{19}$ PTEs must be allocated. Under two-level paging, only the Page Directory plus page table pages corresponding to active PDEs are allocated.
- **Part B:** Maintain a 3-frame state across all 20 references. Track arrival order for FIFO, access recency for LRU, and future access distance for Optimal.

---

## Complete Step-by-Step Solution

### Solution to Part A

#### 1. Bit Allocation:
- Page Size $= 8\,\text{KB} = 8192\,\text{bytes} = 2^{13}\,\text{bytes} \implies \mathbf{\text{Offset Bits} = 13}$.
- Entries per 8 KB page table page $= \frac{8192\,\text{bytes}}{4\,\text{bytes}} = 2048 = 2^{11} \implies \mathbf{\text{Page Table Index} = 11\,\text{bits}}$.
- Total VPN bits $= 32 - 13 = 19\,\text{bits}$.
- Page Directory Index bits $= 19 - 11 = \mathbf{8\,\text{bits}}$.
- **Structure:** `[ PD Index: 8 bits | PT Index: 11 bits | Offset: 13 bits ]`.

#### 2. Memory Consumption Comparison:

##### Single-Level Linear Page Table:
- Total virtual pages $= 2^{19} = 524,288$ pages.
- Memory $= 524,288 \times 4\,\text{bytes} = 2,097,152\,\text{bytes} = \mathbf{2\,\text{MB}}$.

##### Two-Level Page Table:
- One Page Directory page is always allocated:
  - Directory has $2^8 = 256$ PDEs.
  - Sized to one physical frame $= \mathbf{8\,\text{KB}}$.
- Each PDE covers $2^{11}$ pages $\times 8\,\text{KB/page} = 2^{24}\,\text{bytes} = \mathbf{16\,\text{MB}}$ of address space!
- Process memory distribution across 16 MB regions:
  - Code (`0x00000000`, 32 KB $\implies$ fits within first 16 MB) $\implies$ mapped by **PDE 0**.
  - Heap (`0x10000000` $= 268,435,456 \implies 268.4\,\text{MB} \implies$ index $\lfloor 268.4 / 16 \rfloor = 16$) $\implies$ mapped by **PDE 16**.
  - Stack (Top of memory `0xFFFFFFFF`) $\implies$ mapped by **PDE 255**.
- Active PDEs: Exactly **3 distinct PDEs** (PDE 0, PDE 16, PDE 255) are valid.
- Therefore, exactly **3 second-level page table pages** are allocated ($3 \times 8\,\text{KB} = 24\,\text{KB}$).
- Total Memory Consumed:
  $$\text{Total} = 8\,\text{KB (Directory)} + 24\,\text{KB (Page Tables)} = \mathbf{32\,\text{KB}}.$$
- **Comparison:** The two-level table uses only **32 KB** compared to **2,048 KB** for the linear table (a **98.4% reduction** in memory waste).

---

### Solution to Part B: Page Replacement Simulation

Reference String: `1, 2, 3, 4, 2, 1, 5, 6, 2, 1, 2, 3, 7, 6, 3, 2, 1, 2, 3, 6` (20 references)

#### 1. FIFO Simulation (3 Frames):
1. `1` $\to$ [1] (Fault 1)
2. `2` $\to$ [1, 2] (Fault 2)
3. `3` $\to$ [1, 2, 3] (Fault 3)
4. `4` $\to$ evict 1 $\to$ [2, 3, 4] (Fault 4)
5. `2` $\to$ Hit!
6. `1` $\to$ evict 2 $\to$ [3, 4, 1] (Fault 5)
7. `5` $\to$ evict 3 $\to$ [4, 1, 5] (Fault 6)
8. `6` $\to$ evict 4 $\to$ [1, 5, 6] (Fault 7)
9. `2` $\to$ evict 1 $\to$ [5, 6, 2] (Fault 8)
10. `1` $\to$ evict 5 $\to$ [6, 2, 1] (Fault 9)
11. `2` $\to$ Hit!
12. `3` $\to$ evict 6 $\to$ [2, 1, 3] (Fault 10)
13. `7` $\to$ evict 2 $\to$ [1, 3, 7] (Fault 11)
14. `6` $\to$ evict 1 $\to$ [3, 7, 6] (Fault 12)
15. `3` $\to$ Hit!
16. `2` $\to$ evict 3 $\to$ [7, 6, 2] (Fault 13)
17. `1` $\to$ evict 7 $\to$ [6, 2, 1] (Fault 14)
18. `2` $\to$ Hit!
19. `3` $\to$ evict 6 $\to$ [2, 1, 3] (Fault 15)
20. `6` $\to$ evict 2 $\to$ [1, 3, 6] (Fault 16)
- **FIFO Total Faults = 16.**

#### 2. LRU Simulation (3 Frames):
1. `1` $\to$ [1] (Fault 1)
2. `2` $\to$ [1, 2] (Fault 2)
3. `3` $\to$ [1, 2, 3] (Fault 3)
4. `4` $\to$ least recent is 1 $\to$ [2, 3, 4] (Fault 4)
5. `2` $\to$ Hit! Recency: 3, 4, 2
6. `1` $\to$ least recent is 3 $\to$ [4, 2, 1] (Fault 5)
7. `5` $\to$ least recent is 4 $\to$ [2, 1, 5] (Fault 6)
8. `6` $\to$ least recent is 2 $\to$ [1, 5, 6] (Fault 7)
9. `2` $\to$ least recent is 1 $\to$ [5, 6, 2] (Fault 8)
10. `1` $\to$ least recent is 5 $\to$ [6, 2, 1] (Fault 9)
11. `2` $\to$ Hit! Recency: 6, 1, 2
12. `3` $\to$ least recent is 6 $\to$ [1, 2, 3] (Fault 10)
13. `7` $\to$ least recent is 1 $\to$ [2, 3, 7] (Fault 11)
14. `6` $\to$ least recent is 2 $\to$ [3, 7, 6] (Fault 12)
15. `3` $\to$ Hit! Recency: 7, 6, 3
16. `2` $\to$ least recent is 7 $\to$ [6, 3, 2] (Fault 13)
17. `1` $\to$ least recent is 6 $\to$ [3, 2, 1] (Fault 14)
18. `2` $\to$ Hit! Recency: 3, 1, 2
19. `3` $\to$ Hit! Recency: 1, 2, 3
20. `6` $\to$ least recent is 1 $\to$ [2, 3, 6] (Fault 15)
- **LRU Total Faults = 15.**

#### 3. Optimal (Belady's MIN) Simulation (3 Frames):
1. `1` $\to$ [1] (Fault 1)
2. `2` $\to$ [1, 2] (Fault 2)
3. `3` $\to$ [1, 2, 3] (Fault 3)
4. `4` $\to$ next uses: 2 (at 5), 1 (at 6), 3 (at 12) $\implies$ evict 3 $\to$ [1, 2, 4] (Fault 4)
5. `2` $\to$ Hit!
6. `1` $\to$ Hit!
7. `5` $\to$ next uses: 2 (at 9), 1 (at 10), 4 (never) $\implies$ evict 4 $\to$ [1, 2, 5] (Fault 5)
8. `6` $\to$ next uses: 2 (at 9), 1 (at 10), 5 (never) $\implies$ evict 5 $\to$ [1, 2, 6] (Fault 6)
9. `2` $\to$ Hit!
10. `1` $\to$ Hit!
11. `2` $\to$ Hit!
12. `3` $\to$ next uses: 6 (at 14), 2 (at 16), 1 (at 17) $\implies$ evict 1 $\to$ [2, 6, 3] (Fault 7)
13. `7` $\to$ next uses: 6 (at 14), 3 (at 15), 2 (at 16) $\implies$ evict 2 $\to$ [6, 3, 7] (Fault 8)
14. `6` $\to$ Hit!
15. `3` $\to$ Hit!
16. `2` $\to$ next uses: 3 (at 19), 6 (at 20), 7 (never) $\implies$ evict 7 $\to$ [6, 3, 2] (Fault 9)
17. `1` $\to$ next uses: 2 (at 18), 3 (at 19), 6 (at 20) $\implies$ evict 6 $\to$ [3, 2, 1] (Fault 10)
18. `2` $\to$ Hit!
19. `3` $\to$ Hit!
20. `6` $\to$ evict 1 $\to$ [3, 2, 6] (Fault 11)
- **Optimal Total Faults = 11.**

### Comparison Table:
| Algorithm | Page Faults (out of 20) | Hit Rate |
|---|---|---|
| **Optimal** | **11** | **45.0%** |
| **LRU** | **15** | **25.0%** |
| **FIFO** | **16** | **20.0%** |

---

## Reusable Insight

1. **PDE Address Span:** Each PDE covers an address space range of $\text{Entries Per PT} \times \text{Page Size}$. In this problem, $2048 \times 8\,\text{KB} = 16\,\text{MB}$. Identifying how many 16 MB boundaries the active process segments span instantly yields the exact number of allocated page table frames.
2. **Optimal Lookahead Rule:** When breaking ties in Optimal replacement, evict pages that are never referenced again first; among pages referenced in the future, evict the one with the highest future index.

---

## Related Concepts

- [[Multi-Level Page Tables and Advanced Address Translation]]
- [[Page Replacement Policies and the Clock Algorithm]]
- [[Virtual Memory Performance and Address Translation Formulas]]

---

## Sources

- **Lecture Slides:** [[cse313/01 - Sources/Lectures/CSE313_KRV_Merged.pdf|CSE313_KRV_Merged.pdf]], Chapter 20 & 22.
- **Textbook:** Arpaci-Dusseau & Arpaci-Dusseau, *Operating Systems: Three Easy Pieces (OSTEP)*, Chapters 20 & 22.
