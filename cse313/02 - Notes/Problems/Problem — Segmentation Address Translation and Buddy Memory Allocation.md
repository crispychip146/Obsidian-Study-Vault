---
type: problem
course: cse313
status: active
order: 41
---

# Problem — Segmentation Address Translation and Buddy Memory Allocation

> 📖 **Reading Order:** Step 41 of 68 | **Module 6: Virtual Memory Foundations**  
> ◄ **Previous:** [[Segmentation Translation and Buddy Allocation Example]] | ► **Next:** [[Paging Architecture and Linear Page Tables]]

---

## Problem Statement

### Catalog Identifier: `Q-CSE313-006`

#### Part A: Segmented Memory Translation (8 Marks)
A system uses a segmented memory model with a 16-bit virtual address space. The 2 most significant bits designate the segment number (`00` = Code, `01` = Data/Heap, `10` = Stack, `11` = Shared Library). The maximum segment size is 16 KB ($2^{14} = 16384$ bytes). Physical memory is 256 KB.

The current process has the following segment table:

| Segment | Base (Decimal) | Bounds (Limit) | Growth Direction |
|---|---|---|---|
| `00` (Code) | `65536` | `4096` (4 KB) | Positive ($0$) |
| `01` (Heap) | `98304` | `8192` (8 KB) | Positive ($0$) |
| `10` (Stack) | `131072` | `4096` (4 KB) | Negative ($1$) |
| `11` (Shared) | `196608` | `2048` (2 KB) | Positive ($0$) |

For each of the following virtual addresses (in hexadecimal), compute the resulting **Physical Address** or state whether a **Segmentation Fault** occurs, showing all bounds checks and arithmetic:
1. `0x0800`
2. `0x4200`
3. `0x9000`
4. `0xBF00`

#### Part B: Binary Buddy Allocation Simulation (7 Marks)
A system starts with a single contiguous free memory pool of **128 KB** at physical address `0x00000`.
The following sequence of requests arrives:
1. Process $A$ requests 18 KB
2. Process $B$ requests 28 KB
3. Process $C$ requests 9 KB
4. Process $A$ terminates and frees its memory
5. Process $D$ requests 12 KB
6. Process $B$ terminates and frees its memory

Draw the memory layout tree, state the physical address ranges assigned to each process, and show the exact state of all free lists after Step 6.

---

## Understanding the Problem and Choosing the Method

- **Part A:** Extract the 2 MSBs for the segment selector. For positive growth segments (Code, Heap, Shared), verify $\text{Offset} < \text{Bounds}$ and compute $\text{Base} + \text{Offset}$. For negative growth (Stack), compute $\text{Negative Offset} = \text{Offset} - 16384$, verify $|\text{Negative Offset}| \le \text{Bounds}$, and compute $\text{Base} + \text{Negative Offset}$.
- **Part B:** In Binary Buddy allocation, every request of size $S$ is rounded up to $2^{\lceil \log_2 S \rceil}$. Memory blocks are recursively split into halves until the matching size is reached. On deallocation, check the XOR buddy address $A \oplus 2^k$ to determine if recursive coalescing can proceed.

---

## Complete Step-by-Step Solution

### Solution to Part A

#### 1. Address `0x0800`
- 16-bit binary: `0000 1000 0000 0000`
- Segment ID: `00` (Code).
- Offset: `00 1000 0000 0000` $= 2048$ bytes.
- Bounds Check: $2048 < 4096 \implies$ **Valid**.
- Physical Address: $65536 + 2048 = \mathbf{67584} \quad (\text{0x10800})$.

#### 2. Address `0x4200`
- 16-bit binary: `0100 0010 0000 0000`
- Segment ID: `01` (Heap).
- Offset: `00 0010 0000 0000` $= 512$ bytes.
- Bounds Check: $512 < 8192 \implies$ **Valid**.
- Physical Address: $98304 + 512 = \mathbf{98816} \quad (\text{0x18200})$.

#### 3. Address `0x9000`
- 16-bit binary: `1001 0000 0000 0000`
- Segment ID: `10` (Stack).
- Offset: `01 0000 0000 0000` $= 4096$ bytes.
- Negative Growth Calculation:
  $$\text{Negative Offset} = 4096 - 16384 = -12288$$
- Bounds Check:
  - Limit is 4 KB ($4096$).
  - $|-12288| = 12288 \le 4096$ is **FALSE**.
- **Result: Segmentation Fault (Stack bounds exceeded).**

#### 4. Address `0xBF00`
- 16-bit binary: `1011 1111 0000 0000`
- Segment ID: `10` (Stack).
- Offset: `11 1111 0000 0000` $= 16128$ bytes.
- Negative Growth Calculation:
  $$\text{Negative Offset} = 16128 - 16384 = -256$$
- Bounds Check:
  - $|-256| = 256 \le 4096 \implies$ **Valid**.
- Physical Address:
  $$\text{PA} = 131072 + (-256) = \mathbf{130816} \quad (\text{0x1FE00}).$$

---

### Solution to Part B

Initial Pool: 128 KB `[0x00000 - 0x1FFFF]`.

#### Step 1: $A$ requests 18 KB
- Round up: nearest power of two $\ge 18\,\text{KB}$ is **32 KB**.
- Split 128 KB $\to$ two 64 KB blocks: `[0x00000 - 0x0FFFF]` and `[0x10000 - 0x1FFFF]`.
- Split 64 KB `[0x00000]` $\to$ two 32 KB blocks: `[0x00000 - 0x07FFF]` and `[0x08000 - 0x0FFFF]`.
- **Allocate `[0x00000 - 0x07FFF]` (32 KB) to $A$.**

#### Step 2: $B$ requests 28 KB
- Round up: nearest power of two $\ge 28\,\text{KB}$ is **32 KB**.
- Free 32 KB block available at `[0x08000 - 0x0FFFF]`.
- **Allocate `[0x08000 - 0x0FFFF]` (32 KB) to $B$.**

#### Step 3: $C$ requests 9 KB
- Round up: nearest power of two $\ge 9\,\text{KB}$ is **16 KB**.
- Free list has one 64 KB block `[0x10000 - 0x1FFFF]`.
- Split 64 KB `[0x10000]` $\to$ two 32 KB blocks: `[0x10000 - 0x17FFF]` and `[0x18000 - 0x1FFFF]`.
- Split 32 KB `[0x10000]` $\to$ two 16 KB blocks: `[0x10000 - 0x13FFF]` and `[0x14000 - 0x17FFF]`.
- **Allocate `[0x10000 - 0x13FFF]` (16 KB) to $C$.**

#### Step 4: $A$ frees its memory (`[0x00000 - 0x07FFF]`, 32 KB)
- Buddy address $= \text{0x00000} \oplus \text{0x08000} = \text{0x08000}$.
- Block `0x08000` is currently allocated to $B$. **No coalescing possible.**
- Add `[0x00000 - 0x07FFF]` to 32 KB free list.

#### Step 5: $D$ requests 12 KB
- Round up: nearest power of two $\ge 12\,\text{KB}$ is **16 KB**.
- Free 16 KB block available at `[0x14000 - 0x17FFF]`.
- **Allocate `[0x14000 - 0x17FFF]` (16 KB) to $D$.**

#### Step 6: $B$ frees its memory (`[0x08000 - 0x0FFFF]`, 32 KB)
- $B$ address $= \text{0x08000}$. Buddy address $= \text{0x08000} \oplus \text{0x08000} = \mathbf{\text{0x00000}}$.
- Is buddy `0x00000` free? **Yes!** ($A$ freed it in Step 4).
- **Coalesce:** `[0x00000 - 0x07FFF]` and `[0x08000 - 0x0FFFF]` merge into a single **64 KB block**: `[0x00000 - 0x0FFFF]`.
- Buddy check for the new 64 KB block:
  - Buddy of `0x00000` (64 KB) $= \text{0x00000} \oplus \text{0x10000} = \mathbf{\text{0x10000}}$.
  - Is `0x10000` completely free? **No**, $C$ and $D$ are currently using chunks of it!
  - Further coalescing stops.

### Final Memory State After Step 6:
- Allocated:
  - Process $C$: `[0x10000 - 0x13FFF]` (16 KB)
  - Process $D$: `[0x14000 - 0x17FFF]` (16 KB)
- Free Lists:
  - 32 KB Free List: `[0x18000 - 0x1FFFF]` (32 KB)
  - 64 KB Free List: `[0x00000 - 0x0FFFF]` (64 KB)
  - Total Free Memory: $32 + 64 = 96\,\text{KB}$.

---

## Reusable Insight

1. **Buddy Coalescing Cascade:** Coalescing is recursive. Always check if the newly created composite block's buddy is also free before stopping.
2. **Internal Fragmentation Cost:** Notice that $A$ requested 18 KB but consumed 32 KB (wasting 14 KB, or 43.75%), and $C$ requested 9 KB but consumed 16 KB (wasting 7 KB, or 43.75%). While buddy allocation makes coalescing instant, internal fragmentation is high.

---

## Related Concepts

- [[Segmentation and External Fragmentation]]
- [[Segregated Free Lists and Binary Buddy Allocation]]
- [[Segmentation Translation and Buddy Allocation Example]]

---

## Sources

- **Lecture Slides:** [[cse313/01 - Sources/Lectures/CSE313_KRV_Merged.pdf|CSE313_KRV_Merged.pdf]], Chapter 16 & 17.
- **Textbook:** Arpaci-Dusseau & Arpaci-Dusseau, *Operating Systems: Three Easy Pieces (OSTEP)*, Chapters 16 & 17.
