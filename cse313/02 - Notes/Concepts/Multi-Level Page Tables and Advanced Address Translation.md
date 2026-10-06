---
type: concept
course: cse313
status: active
order: 44
---

# Multi-Level Page Tables and Advanced Address Translation

> 📖 **Reading Order:** Step 44 of 68 | **Module 7: Paging & Virtual Memory Systems**  
> ◄ **Previous:** [[Translation Lookaside Buffers and Hardware Caching]] | ► **Next:** [[Swapping Mechanisms and Page Fault Handling]]

---

## Starting Point and the Problem

In [[Paging Architecture and Linear Page Tables]], we saw that a linear page table for a 32-bit address space with 4 KB pages consumes **4 MB of physical RAM per process**. For 100 processes, that requires 400 MB of memory just to track translations.

Yet typical processes use only a small fraction of their address space (a few KB of code, a few KB of heap, and a few KB of stack). The remaining 3.99 GB is an empty void. Linear page tables force the OS to allocate physical frames for thousands of unused PTEs marked `Valid = 0`.

We need an indexing structure that scales with the amount of **actually used memory**, rather than the theoretical size of the address space.

---

## Developing the Idea: The Page Directory Tree

To eliminate the memory overhead of sparse address spaces, modern systems use **Multi-Level Page Tables**.

Instead of storing all PTEs in one massive contiguous array:
1. Slices of the page table are grouped into **page-sized chunks** (e.g., 4 KB each, holding 1,024 PTEs).
2. A top-level array called the **Page Directory** is introduced.
3. Each entry in the directory—a **Page Directory Entry (PDE)**—points to a physical frame containing a page of PTEs.
4. **The Critical Space Saving:** If an entire region of the address space is unused (e.g., 4 MB of unallocated space), its corresponding PDE marks **`Present = 0`**, and the underlying page table page is **never allocated in physical memory**!

```
32-Bit Virtual Address Decomposition (Two-Level Paging):
+-----------------------+-----------------------+-----------------------+
|  Page Directory Index |   Page Table Index    |      Offset Bits      |
|        (10 bits)      |       (10 bits)       |       (12 bits)       |
+-----------------------+-----------------------+-----------------------+
            |                       |                       |
            v                       |                       |
     Page Directory                 v                       |
     +--------------+          Page Table                   |
     |    PDE 0     | -------> +--------------+             |
     |    PDE 1     |          |    PTE 0     | ------+     |
     |     ...      |          |     ...      |       |     |
     | PDE (Invalid)| ---X     +--------------+       |     |
     +--------------+                                 v     v
                                                 +--------------+
                                                 | Physical RAM |
                                                 +--------------+
```

---

## How It Works: Two-Level Address Translation

Consider a 32-bit virtual address with 4 KB ($2^{12}$) pages and 4-byte PTEs:
- A page-sized table chunk fits: $\frac{4096\,\text{bytes}}{4\,\text{bytes/PTE}} = 1024 = 2^{10}$ PTEs.
- Therefore, the inner **Page Table Index (PT Index)** needs **10 bits**.
- The top **10 bits** become the **Page Directory Index (PD Index)** ($2^{10} = 1024$ PDEs in the directory).

### Translation Procedure:
1. Extract fields:
   - $\text{PD Index} = (\text{Virtual Address} \gg 22) \ \& \ \text{0x3FF}$
   - $\text{PT Index} = (\text{Virtual Address} \gg 12) \ \& \ \text{0x3FF}$
   - $\text{Offset} = \text{Virtual Address} \ \& \ \text{0xFFF}$
2. Read the Page Directory Base Register (PDBR / CR3 on x86) to locate the Page Directory.
3. Fetch $\text{PDE} = \text{Page Directory}[\text{PD Index}]$.
4. **Check PDE Present Bit:** If $\text{PDE.Present} == 0$, raise a **Segmentation Fault** (memory is unmapped).
5. Fetch $\text{PTE} = \text{Page Table}[\text{PT Index}]$ using the physical frame address stored in the PDE.
6. **Check PTE Present Bit:** If $\text{PTE.Present} == 0$, trigger a **Page Fault** (page swapped to disk).
7. Extract $\text{PFN} = \text{PTE.PFN}$.
8. Construct $\text{Physical Address} = (\text{PFN} \ll 12) \mid \text{Offset}$.

---

## Space Comparison: Linear vs. Two-Level Page Table

Suppose a process uses:
- 12 KB for Code (3 pages)
- 4 KB for Heap (1 page)
- 8 KB for Stack (2 pages)
Total memory used: **24 KB (6 pages)**.

### Linear Page Table:
- Must allocate all $1,048,576$ PTEs:
  $$\text{Memory} = 1,048,576 \times 4\,\text{bytes} = \mathbf{4\,\text{MB}}.$$

### Two-Level Page Table:
- Page Directory: 1 page (4 KB, containing 1,024 PDEs).
- Code & Heap (low memory, mapped by PDE 0): 1 page table page (4 KB).
- Stack (high memory, mapped by PDE 1023): 1 page table page (4 KB).
- All other 1,022 PDEs are marked invalid $\implies$ no other page tables allocated!
- Total Memory:
  $$\text{Memory} = 4\,\text{KB (Directory)} + 4\,\text{KB (Low PT)} + 4\,\text{KB (High PT)} = \mathbf{12\,\text{KB}}.$$
- **Result:** Reduces page table memory consumption by **$99.7\%$**!

---

## Advanced Alternative: Inverted Page Tables

In 64-bit architectures, address spaces are so massive that multi-level page tables require 4 or 5 levels (e.g., PML4, PML5 on x86-64), increasing the cost of TLB misses.

An alternative architecture used in PowerPC and IA-64 is the **Inverted Page Table**:
- Instead of mapping $\text{VPN} \to \text{PFN}$, it maintains a single global table indexed by **PFN**!
- Each entry records which process (PID) and which VPN currently owns that physical frame: $\text{Entry}[\text{PFN}] = (\text{PID}, \text{VPN})$.
- **Space Complexity:** The size of the inverted page table is proportional to **physical RAM size**, completely independent of virtual address space size!
- **Lookup Overhead:** Finding a translation requires hashing $(\text{PID}, \text{VPN})$ to find the PFN.

---

## Important Properties and Trade-offs

- **Time-Space Trade-off:** Multi-level page tables dramatically reduce physical memory consumption at the cost of increased TLB miss latency. A TLB miss now requires **two or more memory accesses** to traverse the directory tree.
- **Tree Depth Formula:** For an $A$-bit address space with page size $2^P$ and entry size $2^E$, the bits per level is $P - E$, and the number of tree levels required is:
  $$\text{Levels} = \left\lceil \frac{A - P}{P - E} \right\rceil$$

---

## Common Mistakes

- **Assuming PDEs Point Directly to User Data:** A PDE points to another *page table page*, never directly to user data frames.
- **Forgetting that Page Directories are Paged:** In systems with heavy memory pressure, second-level page table pages can themselves be swapped out to disk.

---

## Exam Relevance

Frequently tested through:
- Calculating bit allocations (PD Index, PT Index, Offset) for custom virtual address architectures.
- Computing total page table memory requirements for given process memory layouts.
- Tracing multi-level translation walks for specific hex addresses.

---

## Related Concepts

- [[Paging Architecture and Linear Page Tables]]
- [[Translation Lookaside Buffers and Hardware Caching]]
- [[Swapping Mechanisms and Page Fault Handling]]

---

## Prerequisites

- [[Paging Architecture and Linear Page Tables]]
- [[Translation Lookaside Buffers and Hardware Caching]]

---

## Problems

- [[Problem — Multi-Level Paging and Page Replacement Simulation]]

---

## Sources

- **Lecture Slides:** [[cse313/01 - Sources/Lectures/CSE313_KRV_Merged.pdf|CSE313_KRV_Merged.pdf]], Chapter 20 (Slides 146–175).
- **Textbook:** Arpaci-Dusseau & Arpaci-Dusseau, *Operating Systems: Three Easy Pieces (OSTEP)*, Chapter 20 (Paging: Smaller Tables).
