---
type: problem
course: cse313
status: active
order: 68
---

# Problem — Multiprocessor Memory Latency and Kernel Memory Allocation

> 📖 **Reading Order:** Step 68 of 68 | **Module 10: Advanced Kernel Systems & Multiprocessors**  
> ◄ **Previous:** [[Linux System Architecture and Remote Procedure Calls (RPC)]] | ► **Next:** *End of Course*

---

## Problem Statement

### Catalog Identifier: `Q-CSE313-010`

#### Part A: NUMA Memory Latency and Access Overhead (8 Marks)
A 4-node Non-Uniform Memory Access (NUMA) server runs a high-performance database. Each node contains 16 CPU cores and 64 GB of local physical RAM.
Memory access latencies are:
- Local memory access time: $T_{\text{local}} = 60\,\text{ns}$.
- Remote memory access time across interconnect: $T_{\text{remote}} = 180\,\text{ns}$.
- L1/L2 Cache hit latency: $T_{\text{cache}} = 2\,\text{ns}$.
- Cache hit rate: $H = 90\%$.

1. A naive operating system scheduler randomly distributes memory pages across all 4 nodes, meaning a thread on Node 0 has a $25\%$ probability of accessing local RAM and a $75\%$ probability of accessing remote RAM on cache misses. Compute the Average Memory Access Time ($\text{AMAT}_{\text{naive}}$).
2. A NUMA-aware operating system scheduler binds the thread and its memory allocations strictly to Node 0, achieving a $95\%$ local memory hit rate on cache misses. Compute the improved Average Memory Access Time ($\text{AMAT}_{\text{numa}}$) and the percentage speedup.

#### Part B: Slab Allocator Memory Packing and Fragmentation Analysis (7 Marks)
The Linux kernel allocates process control blocks (`struct task_struct`) of size **840 bytes**.
The kernel manages memory in 4 KB ($4096 = 2^{12}$ bytes) physical pages.
1. If the kernel used a power-of-two free-list allocator (rounding allocations up to the next power of two), determine the block size allocated to each `task_struct`, the number of blocks per 4 KB page, and the internal fragmentation percentage.
2. In the Slab Allocator, how many 840-byte `task_struct` objects can be packed into a single 4 KB page slab? Compute the leftover unused bytes per slab and the resulting internal fragmentation percentage.
3. Explain why the Slab Allocator achieves zero external fragmentation and why it is safe to invoke in interrupt context using `GFP_ATOMIC`.

---

## Understanding the Problem and Choosing the Method

- **Part A:** Compute Effective Access Time on cache misses: $\text{Penalty} = P_{\text{local}} \cdot T_{\text{local}} + P_{\text{remote}} \cdot T_{\text{remote}}$. Combine with cache hit rate: $\text{AMAT} = T_{\text{cache}} + (1 - H) \cdot \text{Penalty}$.
- **Part B:** Nearest power of two $\ge 840$ is $1024$ bytes ($1\,\text{KB}$). Compute objects per 4 KB page ($\lfloor 4096 / 840 \rfloor = 4$). Calculate leftover space and percentage internal fragmentation.

---

## Complete Step-by-Step Solution

### Solution to Part A

#### 1. Naive Scheduler AMAT:
- On a cache miss (probability $1 - H = 0.10$):
  $$\text{Miss Penalty}_{\text{naive}} = (0.25 \times 60\,\text{ns}) + (0.75 \times 180\,\text{ns}) = 15 + 135 = \mathbf{150\,\text{ns}}.$$
- Overall AMAT:
  $$\text{AMAT}_{\text{naive}} = T_{\text{cache}} + (1 - H) \cdot \text{Miss Penalty}_{\text{naive}}$$
  $$\text{AMAT}_{\text{naive}} = 2\,\text{ns} + (0.10 \times 150\,\text{ns}) = 2 + 15 = \mathbf{17.0\,\text{ns}}.$$

#### 2. NUMA-Aware Scheduler AMAT:
- On a cache miss (probability $0.10$):
  $$\text{Miss Penalty}_{\text{numa}} = (0.95 \times 60\,\text{ns}) + (0.05 \times 180\,\text{ns}) = 57 + 9 = \mathbf{66\,\text{ns}}.$$
- Overall AMAT:
  $$\text{AMAT}_{\text{numa}} = 2\,\text{ns} + (0.10 \times 66\,\text{ns}) = 2 + 6.6 = \mathbf{8.6\,\text{ns}}.$$

#### 3. Percentage Improvement / Speedup:
$$\text{Speedup} = \frac{\text{AMAT}_{\text{naive}}}{\text{AMAT}_{\text{numa}}} = \frac{17.0}{8.6} \approx \mathbf{1.98\times \quad (97.7\% \text{ faster memory latency!})}.$$

---

### Solution to Part B

Object Size $= 840\,\text{bytes}$. Page Size $= 4096\,\text{bytes}$.

#### 1. Power-of-Two Allocator:
- Nearest power of two $\ge 840$ is **1024 bytes (1 KB)**.
- Allocated block size $= \mathbf{1024\,\text{bytes}}$.
- Number of blocks per 4 KB page:
  $$\text{Blocks} = \frac{4096}{1024} = \mathbf{4\text{ blocks}}.$$
- Unused space per object $= 1024 - 840 = 184\,\text{bytes}$.
- Internal fragmentation percentage:
  $$\text{Internal Frag} = \frac{184}{1024} \times 100\% = \mathbf{17.97\%}.$$

#### 2. Slab Allocator:
- Number of 840-byte objects per 4 KB page:
  $$\text{Objects} = \left\lfloor \frac{4096}{840} \right\rfloor = \mathbf{4\text{ objects}}.$$
- Total memory occupied by objects:
  $$4 \times 840\,\text{bytes} = 3360\,\text{bytes}.$$
- Leftover unused bytes per slab:
  $$\text{Leftover} = 4096 - 3360 = \mathbf{736\,\text{bytes}}.$$
- Internal fragmentation percentage:
  $$\text{Internal Frag} = \frac{736}{4096} \times 100\% = \mathbf{17.97\%}.$$
*(Note: If a slab spans two 4 KB pages (8192 bytes), $\lfloor 8192 / 840 \rfloor = 9$ objects $= 7560$ bytes, reducing leftover to 632 bytes, or $7.7\%$ internal fragmentation!)*

#### 3. Zero External Fragmentation and Interrupt Safety:
- **Zero External Fragmentation:** Slabs are composed of whole virtual pages and divided into identical fixed-size object slots. Because objects are never variable in size, free slots can be immediately reused by any subsequent request for that object type; no irregular free-space holes ever develop.
- **Interrupt Safety with `GFP_ATOMIC`:** The Slab Allocator services allocations from its pre-allocated, pre-initialized pool of objects in a partial slab. Using `GFP_ATOMIC` guarantees that if no free object exists in the slab cache, the allocator returns `NULL` immediately rather than putting the caller to sleep or waiting on disk swapping, satisfying the non-blocking execution invariant of interrupt contexts.

---

## Reusable Insight

1. **NUMA Placement Optimization:** In multi-socket architectures, thread scheduling and memory allocation must be strictly co-located. Allowing threads to drift across sockets doubles average memory access times.
2. **Object Caching Advantage:** The Slab Allocator transforms memory allocation from expensive variable-sized chunk management into ultra-fast $O(1)$ object pool popping, avoiding CPU cache pollution and lock contention.

---

## Related Concepts

- [[Kernel Memory Allocation Architecture and the Slab Allocator]]
- [[Multiprocessor Operating System Architectures]]
- [[Linux System Architecture and Remote Procedure Calls (RPC)]]

---

## Sources

- **Lecture Slides:** [[cse313/01 - Sources/Lectures/CSE313_KRV_Merged.pdf|CSE313_KRV_Merged.pdf]], Kernel Allocators & Multiprocessors.
- **Textbook:** Tanenbaum & Bos, *Modern Operating Systems (3rd/4th Ed.)*, Chapters 8 & 10.
