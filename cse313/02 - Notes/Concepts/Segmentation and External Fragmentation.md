---
type: concept
course: cse313
status: active
order: 37
---

# Segmentation and External Fragmentation

> 📖 **Reading Order:** Step 37 of 68 | **Module 6: Virtual Memory Foundations**  
> ◄ **Previous:** [[Memory API and Allocation Safety]] | ► **Next:** [[Free-Space Management and Allocation Policies]]

---

## Starting Point and the Problem

As demonstrated in [[Address Space Abstraction and Hardware Relocation]], pure **Base-and-Bounds** relocation forces the entire virtual address space of a process to be placed contiguously into physical RAM.

Consider a 32-bit architecture where each process has a 16 KB virtual address space, but uses only:
- 2 KB for Code
- 2 KB for Heap
- 2 KB for Stack

Under base-and-bounds, the OS must allocate 16 KB of contiguous physical memory. The 10 KB void between the heap and the stack must also be allocated in physical RAM, even though it contains no valid data!
- If memory is scarce, a process cannot be loaded unless a large, unbroken contiguous block is found.
- Wasting 60–90% of allocated physical memory on unused address space voids prevents effective multiprogramming.

---

## Developing the Idea: Generalized Base-and-Bounds

To eliminate this waste, hardware designers introduced **Segmentation**. Instead of one pair of Base and Bounds registers for the entire address space, the MMU provides a Base and Bounds pair for **each logical segment** of the address space:
1. **Code Segment:** Base and Bounds for machine instructions.
2. **Heap Segment:** Base and Bounds for dynamic heap allocations.
3. **Stack Segment:** Base and Bounds for the call stack.

```
Virtual Address Space:            Physical Memory (Scattered):
+--------------------+ 0KB        +--------------------+ 0KB
| Code (2KB)         | ---------> | Operating System   |
+--------------------+ 2KB        +--------------------+ 32KB
| Heap (2KB)         | ---------> | Process Code (2KB) |
+--------------------+ 4KB        +--------------------+ 34KB
|                    |            | (Free / Other)     |
| (Unallocated Void) |            +--------------------+ 40KB
|                    | ---------> | Process Heap (2KB) |
+--------------------+ 14KB       +--------------------+ 42KB
| Stack (2KB)        |            | (Free / Other)     |
+--------------------+ 16KB       +--------------------+ 48KB
                                  | Process Stack (2KB)|
                                  +--------------------+ 50KB
```

By segmenting the address space, only the **actually used memory** (6 KB) is allocated in physical RAM! The unused 10 KB void consumes zero physical memory frames.

---

## How It Works: Address Translation Under Segmentation

In a segmented system, the virtual address is divided into two fields:
1. **Segment Identifier (Top Bits):** Selects which segment register pair to use.
2. **Offset (Bottom Bits):** Specifies the byte offset within that segment.

For example, in a 14-bit virtual address space with three segments:
- Top 2 bits = Segment ID (`00` = Code, `01` = Heap, `10` = Stack).
- Bottom 12 bits = Offset within the segment ($0$ to $4095$ bytes, max segment size 4 KB).

```
14-Bit Virtual Address:
+-------------------+---------------------------------------+
|  Segment ID (2b)  |             Offset (12b)              |
+-------------------+---------------------------------------+
```

### Segment Translation Equations (Positive Growth: Code and Heap)
For segments that grow in the positive direction (Code and Heap):
1. Check: $	ext{Offset} < 	ext{Bounds}[	ext{SegID}]$ (if false, trigger Segmentation Fault).
2. Physical Address:
   $$	ext{Physical Address} = 	ext{Base}[	ext{SegID}] + 	ext{Offset}$$

### Negative Growth: The Stack Segment
The Stack grows **downward** (toward lower addresses), but physical memory addresses grow upward. To support this, hardware adds a **Growth Direction Bit** ($0 = 	ext{Positive}, 1 = 	ext{Negative}$):
1. In a 4 KB segment, if the stack contains 2 KB of data, the valid virtual addresses are at the top of the segment ($2048$ to $4095$).
2. The negative offset is computed by subtracting the maximum segment size:
   $$	ext{Negative Offset} = 	ext{Offset} - 	ext{Max Segment Size}$$
3. Check: $|	ext{Negative Offset}| \le 	ext{Bounds}[	ext{Stack}]$ (otherwise trigger Fault).
4. Physical Address:
   $$	ext{Physical Address} = 	ext{Base}[	ext{Stack}] + 	ext{Negative Offset}$$

---

## Protection and Sharing in Segmentation

Segmentation also provides fine-grained protection bits:
- **Read / Write / Execute Bits:** The MMU stores permission bits for each segment.
  - **Code Segment:** Marked `Read-Only` and `Execute`. If a program attempts to write to its code segment, the MMU halts execution with a protection fault.
  - **Code Sharing:** Because code is read-only, multiple instances of the same program (e.g., 50 running shells or text editors) can share the **exact same physical code segment**, saving immense amounts of physical RAM!
  - **Heap & Stack:** Marked `Read-Write` and `No-Execute` to prevent stack-smashing code injection attacks.

---

## The Downfall of Segmentation: External Fragmentation

While segmentation completely eliminates internal fragmentation caused by address space voids, it creates a far worse problem: **External Fragmentation**.

```
Physical Memory Over Time:
+----------+----------+----------+----------+----------+----------+
| Proc 1   | Free     | Proc 2   | Free     | Proc 3   | Free     |
| (Code)   | (1.5 KB) | (Heap)   | (2.0 KB) | (Stack)  | (1.0 KB) |
+----------+----------+----------+----------+----------+----------+
```

Because segments are variable in size (Code is 1.5 KB, Heap is 6 KB, Stack is 2 KB), allocating and freeing segments over time leaves physical memory riddled with small, non-contiguous holes of free space.
- **The Dilemma:** Total free memory across all holes may be 4.5 KB, but an incoming request to allocate a 3 KB segment will **fail** because no single contiguous block of 3 KB exists!
- **Compaction:** The OS can pause all running processes and copy physical memory segments to consolidate free space into one large contiguous block. However, memory compaction is prohibitively expensive, consuming immense CPU cycles and memory bus bandwidth.

This fundamental flaw led modern operating systems to abandon pure segmentation in favor of **Paging**.

---

## Example: Step-by-Step Translation

Assume the following Segment Table for a process with 14-bit virtual addresses ($4\,	ext{KB}$ max segment size):

| Segment | Base | Bounds | Growth Direction | Permissions |
|---|---|---|---|---|
| **00 (Code)** | `32768` (`0x8000`) | `2048` (`2 KB`) | Positive ($0$) | Read-Execute |
| **01 (Heap)** | `34816` (`0x8800`) | `3072` (`3 KB`) | Positive ($0$) | Read-Write |
| **10 (Stack)** | `28672` (`0x7000`) | `2048` (`2 KB`) | Negative ($1$) | Read-Write |

Translate Virtual Address `0x0064` (Code):
- Binary: `00 0000 0110 0100` $\implies 	ext{SegID} = 00$, $	ext{Offset} = 100$.
- Bounds Check: $100 < 2048$ (Valid).
- Physical Address: $32768 + 100 = \mathbf{32868}$.

Translate Virtual Address `0x1080` (Heap):
- Binary: `01 0000 1000 0000` $\implies 	ext{SegID} = 01$, $	ext{Offset} = 128$.
- Bounds Check: $128 < 3072$ (Valid).
- Physical Address: $34816 + 128 = \mathbf{34944}$.

Translate Virtual Address `0x2C00` (Stack):
- Binary: `10 1100 0000 0000` $\implies 	ext{SegID} = 10$, $	ext{Offset} = 3072$.
- Negative Offset: $3072 - 4096 = -1024$.
- Bounds Check: $|-1024| = 1024 \le 2048$ (Valid).
- Physical Address: $28672 + (-1024) = \mathbf{27648}$.

---

## Important Properties and Why They Hold

- **Zero Void Waste Invariant:** Physical memory is allocated only for the active, live regions of segments. The vast address space void between heap and stack is never assigned physical memory frames.
- **Independent Relocation Theorem:** Each segment can be moved, expanded, or placed anywhere in physical RAM independently of the other segments of the same process.

---

## Common Mistakes

- **Miscalculating Negative Stack Offsets:** Students frequently add the raw offset to the base for stack addresses, forgetting that the stack grows downward from the base and that the offset must be adjusted relative to the maximum segment size.
- **Confusing Internal vs. External Fragmentation:** Internal fragmentation occurs *inside* an allocated block due to fixed allocation units; external fragmentation occurs *between* allocated blocks due to variable-sized units. Segmentation eliminates internal fragmentation but introduces severe external fragmentation.

---

## Exam Relevance

Commonly tested through:
- Translating virtual addresses under segmentation with both positive and negative growth segments.
- Calculating external fragmentation metrics given a physical memory map.
- Analyzing segment table protection bit violations (e.g., trying to write to segment `00`).

---

## Related Concepts

- [[Address Space Abstraction and Hardware Relocation]]
- [[Free-Space Management and Allocation Policies]]
- [[Paging Architecture and Linear Page Tables]]

---

## Prerequisites

- [[Address Space Abstraction and Hardware Relocation]]
- [[Process Concepts and Memory Layout]]

---

## Problems

- [[Problem — Segmentation Address Translation and Buddy Memory Allocation]]

---

## Sources

- **Lecture Slides:** [[cse313/01 - Sources/Lectures/CSE313_KRV_Merged.pdf|CSE313_KRV_Merged.pdf]], Chapter 16 (Slides 62–78).
- **Textbook:** Arpaci-Dusseau & Arpaci-Dusseau, *Operating Systems: Three Easy Pieces (OSTEP)*, Chapter 16 (Segmentation).
