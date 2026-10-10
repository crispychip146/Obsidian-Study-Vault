---
type: example
course: cse313
status: active
order: 40
---

# Segmentation Translation and Buddy Allocation Example

> 📖 **Reading Order:** Step 40 of 68 | **Module 6: Virtual Memory Foundations**  
> ◄ **Previous:** [[Segregated Free Lists and Binary Buddy Allocation]] | ► **Next:** [[Problem — Segmentation Address Translation and Buddy Memory Allocation]]

---

## Background and Problem Setup

This example works through two classic numerical exam problems in memory virtualization:
1. **Part 1:** Hardware address translation under a segmented memory architecture with positive and negative growth directions.
2. **Part 2:** Execution trace of a Binary Buddy memory allocator handling a chronological sequence of allocations, deallocations, splits, and coalescing events.

---

## Part 1: Segmented Address Translation

Consider a system with a **14-bit virtual address space** and **64 KB physical memory**.
- The top 2 bits specify the Segment ID (`00` = Code, `01` = Heap, `10` = Stack).
- The remaining 12 bits represent the Offset (Maximum segment size = $2^{12} = 4096$ bytes = 4 KB).

The process has the following Segment Table loaded in the MMU:

| Segment ID | Segment Name | Base (Decimal) | Base (Hex) | Bounds (Size) | Growth Direction | Permissions |
|---|---|---|---|---|---|---|
| `00` | Code | `32768` | `0x8000` | `2048` (2 KB) | Positive ($0$) | Read-Execute |
| `01` | Heap | `36864` | `0x9000` | `3072` (3 KB) | Positive ($0$) | Read-Write |
| `10` | Stack | `28672` | `0x7000` | `2048` (2 KB) | Negative ($1$) | Read-Write |

Translate the following Virtual Addresses into Physical Addresses, or determine if a hardware exception occurs:

### Translation 1: Virtual Address `0x0210`
1. Convert to 14-bit binary:
   $$\text{0x0210} = \mathbf{00}\;0010\;0001\;0000_2$$
2. Identify fields:
   - $\text{Segment ID} = \mathbf{00}$ (Code Segment)
   - $\text{Offset} = 0010\;0001\;0000_2 = 528$ (`0x210`)
3. Bounds Check:
   - Growth is positive. Is $\text{Offset} < \text{Bounds}$?
   - $528 < 2048 \implies$ **Valid access**.
4. Compute Physical Address:
   $$\text{PA} = \text{Base} + \text{Offset} = 32768 + 528 = \mathbf{33296} \quad (\text{0x8210})$$

### Translation 2: Virtual Address `0x1900`
1. Convert to 14-bit binary:
   $$\text{0x1900} = \mathbf{01}\;1001\;0000\;0000_2$$
2. Identify fields:
   - $\text{Segment ID} = \mathbf{01}$ (Heap Segment)
   - $\text{Offset} = 1001\;0000\;0000_2 = 2304$ (`0x900`)
3. Bounds Check:
   - Is $\text{Offset} < \text{Bounds}$?
   - $2304 < 3072 \implies$ **Valid access**.
4. Compute Physical Address:
   $$\text{PA} = \text{Base} + \text{Offset} = 36864 + 2304 = \mathbf{39168} \quad (\text{0x9900})$$

### Translation 3: Virtual Address `0x1D00`
1. Convert to 14-bit binary:
   $$\text{0x1D00} = \mathbf{01}\;1101\;0000\;0000_2$$
2. Identify fields:
   - $\text{Segment ID} = \mathbf{01}$ (Heap Segment)
   - $\text{Offset} = 1101\;0000\;0000_2 = 3328$
3. Bounds Check:
   - Is $\text{Offset} < \text{Bounds}$?
   - $3328 < 3072$ is **FALSE**.
4. **Result:** Hardware MMU raises a **Segmentation Fault** (bounds violation trap).

### Translation 4: Virtual Address `0x2A00` (Stack)
1. Convert to 14-bit binary:
   $$\text{0x2A00} = \mathbf{10}\;1010\;0000\;0000_2$$
2. Identify fields:
   - $\text{Segment ID} = \mathbf{10}$ (Stack Segment)
   - $\text{Offset} = 1010\;0000\;0000_2 = 2560$
3. Negative Growth Calculation:
   - Growth is negative ($1$). Max segment size $= 4096$.
   $$\text{Negative Offset} = \text{Offset} - \text{Max Segment Size} = 2560 - 4096 = -1536$$
4. Bounds Check:
   - Is $|\text{Negative Offset}| \le \text{Bounds}$?
   - $|-1536| = 1536 \le 2048 \implies$ **Valid stack access**.
5. Compute Physical Address:
   $$\text{PA} = \text{Base} + \text{Negative Offset} = 28672 + (-1536) = \mathbf{27136} \quad (\text{0x6A00})$$

---

## Part 2: Binary Buddy Allocation Simulation

Consider a memory pool of **64 KB** managed by a Binary Buddy Allocator, starting at address `0x0000`.
We trace the following sequence of operations:
1. $P_1$ requests 7 KB
2. $P_2$ requests 15 KB
3. $P_3$ requests 8 KB
4. $P_1$ frees its memory
5. $P_2$ frees its memory

### Step 1: $P_1$ requests 7 KB
- Smallest power of two $\ge 7\,\text{KB}$ is **8 KB**.
- Initial state: One free block of 64 KB `[0x0000 - 0xFFFF]`.
- Split 64 KB $\to$ two 32 KB blocks: `[0x0000 - 0x7FFF]` and `[0x8000 - 0xFFFF]`.
- Split 32 KB `[0x0000]` $\to$ two 16 KB blocks: `[0x0000 - 0x3FFF]` and `[0x4000 - 0x7FFF]`.
- Split 16 KB `[0x0000]` $\to$ two 8 KB blocks: `[0x0000 - 0x1FFF]` and `[0x2000 - 0x3FFF]`.
- **Allocate `[0x0000 - 0x1FFF]` to $P_1$.**
- Free lists:
  - 8 KB: `[0x2000]`
  - 16 KB: `[0x4000]`
  - 32 KB: `[0x8000]`

### Step 2: $P_2$ requests 15 KB
- Smallest power of two $\ge 15\,\text{KB}$ is **16 KB**.
- The 16 KB free list contains `[0x4000 - 0x7FFF]`.
- **Allocate `[0x4000 - 0x7FFF]` to $P_2$ directly!**
- Free lists:
  - 8 KB: `[0x2000]`
  - 32 KB: `[0x8000]`

### Step 3: $P_3$ requests 8 KB
- Smallest power of two $\ge 8\,\text{KB}$ is **8 KB**.
- The 8 KB free list contains `[0x2000 - 0x3FFF]`.
- **Allocate `[0x2000 - 0x3FFF]` to $P_3$ directly!**
- Free lists:
  - 32 KB: `[0x8000]`

### Step 4: $P_1$ frees memory (`[0x0000 - 0x1FFF]`, size 8 KB)
- $P_1$ address $= \text{0x0000}$.
- Buddy address $= \text{0x0000} \oplus \text{0x2000} = \mathbf{\text{0x2000}}$.
- Is buddy `0x2000` free? **No**, it is currently allocated to $P_3$!
- **Cannot coalesce.** Block `[0x0000 - 0x1FFF]` is added to the 8 KB free list.
- Free lists:
  - 8 KB: `[0x0000]`
  - 32 KB: `[0x8000]`

### Step 5: $P_2$ frees memory (`[0x4000 - 0x7FFF]`, size 16 KB)
- $P_2$ address $= \text{0x4000}$.
- Buddy address $= \text{0x4000} \oplus \text{0x4000} = \mathbf{\text{0x0000}}$.
- Is buddy `0x0000` (16 KB) free? **No**, because half of it (`0x2000`) is still held by $P_3$!
- **Cannot coalesce.** Block `[0x4000 - 0x7FFF]` is added to the 16 KB free list.
- Free lists:
  - 8 KB: `[0x0000]`
  - 16 KB: `[0x4000]`
  - 32 KB: `[0x8000]`

---

## Key Takeaways

1. **Stack Translation Rule:** Always subtract the maximum segment size to get the negative offset before bounds checking and adding to the base.
2. **Buddy Invariant:** Two free blocks of equal size can **only** coalesce if their XOR relationship equals their size. Adjacent blocks belonging to different parents cannot be merged.

---

## Related Concepts

- [[Segmentation and External Fragmentation]]
- [[Segregated Free Lists and Binary Buddy Allocation]]
- [[Problem — Segmentation Address Translation and Buddy Memory Allocation]]

---

## Sources

- **Lecture Slides:** [[cse313/01 - Sources/Lectures/CSE313_KRV_Merged.pdf|CSE313_KRV_Merged.pdf]], Chapter 16 & 17.
- **Textbook:** Arpaci-Dusseau & Arpaci-Dusseau, *Operating Systems: Three Easy Pieces (OSTEP)*, Chapters 16 & 17.
