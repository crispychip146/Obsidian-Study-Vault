---
type: formula
course: cse313
status: active
order: 47
---

# Virtual Memory Performance and Address Translation Formulas

> 📖 **Reading Order:** Step 47 of 68 | **Module 7: Paging & Virtual Memory Systems**  
> ◄ **Previous:** [[Page Replacement Policies and the Clock Algorithm]] | ► **Next:** [[Two-Level Page Table Translation and Clock Replacement Example]]

---

## Starting Point and Need

Evaluating virtual memory architectures requires rigorous mathematical formulas to quantify:
1. Address space partitioning and page table memory consumption.
2. The effective latency of memory access under caching and swapping.
3. Cache hit ratios required to maintain target CPU throughput.

---

## 1. Average Memory Access Time (AMAT)

When pages can reside either in physical RAM or secondary swap storage:

$$\mathbf{\text{AMAT} = T_{\text{RAM}} + (P_{\text{Miss}} \times T_{\text{Disk}})}$$

Where:
- $T_{\text{RAM}}$: Access latency of physical RAM (typically $50 - 100\,\text{ns}$).
- $P_{\text{Miss}}$: Page fault probability (miss rate, $0 \le P_{\text{Miss}} \le 1$).
- $T_{\text{Disk}}$: Average disk service latency (typically $5 - 10\,\text{ms}$ for HDD, $100\,\mu\text{s}$ for SSD).

### Accounting for Dirty Page Evictions:
If dirty pages are modified with probability $P_{\text{Dirty}}$:
$$\mathbf{T_{\text{Fault}} = (1 - P_{\text{Dirty}}) \cdot T_{\text{Read}} + P_{\text{Dirty}} \cdot (T_{\text{Write}} + T_{\text{Read}})}$$
$$\mathbf{\text{AMAT} = T_{\text{RAM}} + P_{\text{Miss}} \cdot T_{\text{Fault}}}$$

---

## 2. Effective Access Time (EAT) with TLB

When address translation is accelerated by a Translation Lookaside Buffer:

$$\mathbf{\text{EAT} = (\text{HitRate} \times (T_{\text{TLB}} + T_{\text{RAM}})) + ((1 - \text{HitRate}) \times (T_{\text{TLB}} + (L + 1) \cdot T_{\text{RAM}}))}$$

Where:
- $\text{HitRate}$: Proportion of memory references satisfied by the TLB.
- $T_{\text{TLB}}$: TLB cache search latency ($\approx 1\,\text{ns}$).
- $T_{\text{RAM}}$: Main memory access latency.
- $L$: Number of page table levels traversed on a miss ($L=1$ for linear paging, $L=2$ for two-level paging, $L=4$ for 64-bit x86-64).

---

## 3. Paging Architecture Decomposition Formulas

For an $A$-bit virtual address space with page size $S = 2^P$ bytes:

$$\mathbf{\text{Offset Bits} = P = \log_2(S)}$$
$$\mathbf{\text{VPN Bits} = A - P}$$
$$\mathbf{\text{Total Virtual Pages} = 2^{A - P}}$$

### Linear Page Table Size Formula:
For entry size $E = 2^e$ bytes:
$$\mathbf{\text{Linear Table Size (Bytes)} = 2^{A - P} \times E}$$

### Multi-Level Tree Depth Formula:
If each page table page is size $2^P$ bytes and holds $\frac{2^P}{2^E} = 2^{P - e}$ entries, each hierarchical index level consumes:
$$\mathbf{\text{Bits Per Level} = P - e}$$
$$\mathbf{\text{Number of Hierarchical Levels} = \left\lceil \frac{A - P}{P - e} \right\rceil}$$

---

## Worked Numerical Application

### Problem:
A 32-bit system has a 2-level page table, 4 KB pages ($2^{12}$ bytes), and 4-byte PTEs.
Physical RAM access time is $T_{\text{RAM}} = 100\,\text{ns}$, TLB access time is $T_{\text{TLB}} = 5\,\text{ns}$, and swap disk latency is $T_{\text{Disk}} = 10\,\text{ms} = 10,000,000\,\text{ns}$.
1. If the TLB hit rate is $98\%$ and all accessed pages are in RAM ($P_{\text{Miss}} = 0$), compute the EAT.
2. What is the maximum allowable page fault rate $P_{\text{Miss}}$ if effective memory access time cannot degrade by more than $20\%$ of baseline RAM access ($120\,\text{ns}$)?

### Solution:

#### 1. Computing EAT with TLB:
On a TLB hit: access TLB ($5\,\text{ns}$) + access RAM data ($100\,\text{ns}$) $= 105\,\text{ns}$.
On a TLB miss: access TLB ($5\,\text{ns}$) + access Page Directory ($100\,\text{ns}$) + access Page Table ($100\,\text{ns}$) + access RAM data ($100\,\text{ns}$) $= 305\,\text{ns}$.

$$\text{EAT} = (0.98 \times 105) + (0.02 \times 305) = 102.9 + 6.1 = \mathbf{109.0\,\text{ns}}.$$

#### 2. Computing Maximum Allowable Page Fault Rate:
Baseline RAM access $= 100\,\text{ns}$.
Maximum allowed $\text{AMAT} = 120\,\text{ns}$.
$$\text{AMAT} = T_{\text{RAM}} + (P_{\text{Miss}} \times T_{\text{Disk}})$$
$$120 = 100 + (P_{\text{Miss}} \times 10,000,000)$$
$$20 = P_{\text{Miss}} \times 10,000,000$$
$$P_{\text{Miss}} = \frac{20}{10,000,000} = \mathbf{0.000002} \quad (\mathbf{0.0002\% \text{ or 1 fault per 500,000 accesses}})!$$

This demonstrates why virtual memory systems require exceptionally low page fault rates.

---

## Conditions and Limitations

- **TLB Independence:** The EAT equation assumes TLB hit rate is independent of page fault rate; in reality, page faults trigger trap overhead and context switches that partially flush the TLB.
- **Disk Saturation:** The linear AMAT formula assumes constant disk latency; under heavy page fault storms, disk queuing causes $T_{\text{Disk}}$ to explode exponentially.

---

## Exam Relevance

Regularly tested through:
- Calculating maximum allowable page fault rates given target access times.
- Solving for EAT across multi-level page table depths ($L=1, 2, 4$).
- Deriving page table size in megabytes for custom architecture specifications.

---

## Related Concepts

- [[Paging Architecture and Linear Page Tables]]
- [[Translation Lookaside Buffers and Hardware Caching]]
- [[Page Replacement Policies and the Clock Algorithm]]

---

## Prerequisites

- [[Paging Architecture and Linear Page Tables]]
- [[Translation Lookaside Buffers and Hardware Caching]]

---

## Problems

- [[Problem — Multi-Level Paging and Page Replacement Simulation]]

---

## Sources

- **Lecture Slides:** [[cse313/01 - Sources/Lectures/CSE313_KRV_Merged.pdf|CSE313_KRV_Merged.pdf]], Chapter 18, 19, 21, 22.
- **Textbook:** Arpaci-Dusseau & Arpaci-Dusseau, *Operating Systems: Three Easy Pieces (OSTEP)*, Chapters 18–22.
