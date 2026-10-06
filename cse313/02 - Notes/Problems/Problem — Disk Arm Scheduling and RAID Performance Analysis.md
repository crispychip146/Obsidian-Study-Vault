---
type: problem
course: cse313
status: active
order: 56
---

# Problem — Disk Arm Scheduling and RAID Performance Analysis

> 📖 **Reading Order:** Step 56 of 68 | **Module 8: I/O Hardware & Storage Systems**  
> ◄ **Previous:** [[Disk Scheduling Simulation and RAID Small-Write Example]] | ► **Next:** [[File and Directory Abstractions and POSIX File API]]

---

## Problem Statement

### Catalog Identifier: `Q-CSE313-008`

#### Part A: Disk Arm Traversal and Latency Sizing (8 Marks)
A hard disk drive has 5,000 cylinders numbered $0$ to $4999$. The drive spins at 7,200 RPM, has an average seek time of $5\,\text{ms}$, and moves across cylinders at a constant speed of $0.002\,\text{ms}$ per cylinder.
The drive head is currently stationed at **cylinder 2,150**, having just serviced a request at cylinder 2,000 (moving toward higher cylinders).
The pending I/O request queue contains requests for cylinders:
$$\mathbf{2069, 1212, 2296, 2800, 544, 1618, 356, 1523, 4965, 3681}$$

1. Compute the sequence of cylinders serviced and the total head movement (in cylinders) for:
   - **SSTF (Shortest Seek Time First)**
   - **SCAN**
   - **C-LOOK**
2. For the **C-LOOK** algorithm, compute the total seek time in milliseconds required to service all requests in the queue.

#### Part B: RAID 5 Throughput and Failure Recovery (7 Marks)
An enterprise database server runs on a **5-disk RAID 5 array**. Each disk has a capacity of 4 TB and can sustain:
- Sequential transfer rate $S = 200\,\text{MB/s}$.
- Random I/O operations $R = 250\text{ IOPS}$.

1. Compute the usable storage capacity and storage efficiency of the array.
2. If a database workload issues a stream of 80% random reads and 20% random writes, compute the maximum total IOPS the RAID 5 array can sustain.
3. Suppose Disk 2 suffers a complete physical head crash. Explain how the RAID controller reconstructs a 4 KB block located on Disk 2, and identify the I/O load imposed on the surviving disks during reconstruction.

---

## Understanding the Problem and Choosing the Method

- **Part A:** SSTF repeatedly greedily selects the closest cylinder. SCAN moves upward to 4999, then reverses to 0. C-LOOK moves upward to the highest requested cylinder (4965), jumps to the lowest requested cylinder (356), and continues upward.
- **Part B:** RAID 5 usable capacity is $(N-1) \cdot C$. Each random read costs 1 disk I/O; each random write costs 4 disk I/Os (2 reads + 2 writes). Total disk I/O capacity is $N \cdot R$. Set up the workload constraint equation: $\text{Total Workload IOPS} \times (0.80 \times 1 + 0.20 \times 4) \le N \cdot R$.

---

## Complete Step-by-Step Solution

### Solution to Part A

Queue: `2069, 1212, 2296, 2800, 544, 1618, 356, 1523, 4965, 3681` (10 requests)
Initial Position: **2150**, moving **UP**.

#### 1. SSTF Traversal:
Current: **2150**.
- Nearest: $|2069 - 2150| = 81$ vs $|2296 - 2150| = 146 \implies$ pick **2069** (dist 81).
- From 2069: nearest is **2296** (dist 227) vs 1618 (dist 451) $\implies$ pick **2296** (dist 227).
- From 2296: nearest is **2800** (dist 504) $\implies$ pick **2800** (dist 504).
- From 2800: nearest is **3681** (dist 881) $\implies$ pick **3681** (dist 881).
- From 3681: nearest is **4965** (dist 1284) $\implies$ pick **4965** (dist 1284).
- From 4965: arm must reverse; nearest remaining is **1618** (dist 3347) $\implies$ pick **1618** (dist 3347).
- From 1618: nearest is **1523** (dist 95) $\implies$ pick **1523** (dist 95).
- From 1523: nearest is **1212** (dist 311) $\implies$ pick **1212** (dist 311).
- From 1212: nearest is **544** (dist 668) $\implies$ pick **544** (dist 668).
- From 544: nearest is **356** (dist 188) $\implies$ pick **356** (dist 188).
- **Total SSTF Movement:**
  $$81 + 227 + 504 + 881 + 1284 + 3347 + 95 + 311 + 668 + 188 = \mathbf{7586\text{ cylinders}}.$$

#### 2. SCAN Traversal:
Moving **UP** toward boundary (4999):
- Services requests $\ge 2150$ ascending: **2296, 2800, 3681, 4965**.
- Reaches boundary: **4999**!
- Reverses, services requests descending: **2069, 1618, 1523, 1212, 544, 356**.
- **Total SCAN Movement:**
  $$(4999 - 2150) + (4999 - 356) = 2849 + 4643 = \mathbf{7492\text{ cylinders}}.$$

#### 3. C-LOOK Traversal:
Moving **UP**:
- Services $\ge 2150$ up to highest request: **2296, 2800, 3681, 4965**.
- Jumps directly to lowest request: **356** (no travel to boundaries!).
- Services remaining requests ascending: **544, 1212, 1523, 1618, 2069**.
- **Total C-LOOK Movement:**
  $$(4965 - 2150) + (4965 - 356) + (2069 - 356) = 2815 + 4609 + 1713 = \mathbf{9137\text{ cylinders}}.$$

#### 4. C-LOOK Total Seek Time:
$$\text{Seek Time} = 9137\text{ cylinders} \times 0.002\,\text{ms/cylinder} = \mathbf{18.274\,\text{ms}}.$$

---

### Solution to Part B

#### 1. Usable Capacity & Efficiency:
$$\text{Usable Capacity} = (N - 1) \cdot C = (5 - 1) \times 4\,\text{TB} = \mathbf{16\,\text{TB}}.$$
$$\text{Storage Efficiency} = \frac{4}{5} \times 100\% = \mathbf{80.0\%}.$$

#### 2. Maximum Workload IOPS:
- Total raw disk operations available across array:
  $$\text{Raw IOPS} = N \cdot R = 5 \times 250 = \mathbf{1250\text{ Disk Ops/sec}}.$$
- Let $W$ be the total workload IOPS:
  - Each read generates $1$ disk operation.
  - Each write generates $4$ disk operations (2 reads + 2 writes).
  - Average disk ops per workload request:
    $$\text{Cost} = (0.80 \times 1) + (0.20 \times 4) = 0.80 + 0.80 = \mathbf{1.60\text{ Disk Ops}}.$$
- Setting up the capacity inequality:
  $$W \times 1.60 \le 1250$$
  $$W = \frac{1250}{1.60} = \mathbf{781.25\text{ IOPS}}.$$
- The array can sustain **781 workload IOPS**.

#### 3. Failed Disk Reconstruction:
- To reconstruct missing block $D_2$:
  $$D_2 = D_0 \oplus D_1 \oplus D_3 \oplus P$$
- The controller must issue parallel read operations to **all four surviving disks** (Disk 0, Disk 1, Disk 3, and Disk 4).
- **I/O Load Impact:** Rebuilding an entire 4 TB failed drive requires reading every single sector of all 4 surviving disks, pushing all surviving drives to **100% saturation** for hours, dramatically increasing the risk of a secondary disk crash.

---

## Reusable Insight

1. **The I/O Inflation Factor:** In RAID 5, write requests are inflated by a factor of 4. When sizing storage systems for mixed workloads, always calculate the effective per-request cost: $\text{Read\%} + 4 \times \text{Write\%}$.
2. **C-LOOK Turnaround Economy:** C-LOOK prevents mechanical arm overtravel by terminating sweeps strictly at the outermost pending request rather than traveling to the unrequested disk boundary.

---

## Related Concepts

- [[Disk Arm Scheduling Algorithms]]
- [[RAID Architectures and Redundancy Models]]
- [[Disk Latency and RAID Performance Evaluation Formulas]]

---

## Sources

- **Lecture Slides:** [[cse313/01 - Sources/Lectures/CSE313_KRV_Merged.pdf|CSE313_KRV_Merged.pdf]], Chapter 37 & 38.
- **Textbook:** Arpaci-Dusseau & Arpaci-Dusseau, *Operating Systems: Three Easy Pieces (OSTEP)*, Chapters 37 & 38.
