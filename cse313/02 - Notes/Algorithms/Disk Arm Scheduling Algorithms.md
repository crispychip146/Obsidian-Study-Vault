---
type: algorithm
course: cse313
status: active
order: 52
---

# Disk Arm Scheduling Algorithms

> 📖 **Reading Order:** Step 52 of 68 | **Module 8: I/O Hardware & Storage Systems**  
> ◄ **Previous:** [[Hard Disk Drive Architecture and Mechanical Latency]] | ► **Next:** [[RAID Architectures and Redundancy Models]]

---

## Starting Point and Earlier Tools

As established in [[Hard Disk Drive Architecture and Mechanical Latency]], mechanical seek time ($4 - 10\,\text{ms}$) dominates hard disk latency.

When multiple processes issue I/O requests concurrently, the operating system maintains an **I/O Request Queue**. The OS scheduler must decide:
> In what order should pending disk requests be serviced to minimize total head movement and prevent starvation?

---

## The Disk Scheduling Algorithms

Consider a disk with cylinders numbered $0$ to $199$, and an incoming request queue:
$$\mathbf{98, 183, 37, 122, 14, 124, 65, 67}$$
Current head position is at cylinder **53**.

---

### 1. First-Come, First-Served (FCFS / FIFO)
- **Rule:** Requests are serviced in their exact arrival order.
- **Trace from 53:** $53 \to 98 \to 183 \to 37 \to 122 \to 14 \to 124 \to 65 \to 67$.
- **Total Head Movement:**
  $$|98-53| + |183-98| + |37-183| + |122-37| + |14-122| + |124-14| + |65-124| + |67-65|$$
  $$= 45 + 85 + 146 + 85 + 108 + 110 + 59 + 2 = \mathbf{640\text{ cylinders}}.$$
- **Evaluation:** Completely fair, zero starvation; but causes wild, erratic arm oscillations resulting in atrocious throughput.

---

### 2. Shortest Seek Time First (SSTF)
- **Rule:** Select the request with the minimum seek distance from the current head position.
- **Trace from 53:**
  - Nearest to 53 is **65** (distance 12).
  - Nearest to 65 is **67** (distance 2).
  - Nearest to 67 is **37** (distance 30).
  - Nearest to 37 is **14** (distance 23).
  - Nearest to 14 is **98** (distance 84).
  - Nearest to 98 is **122** (distance 24).
  - Nearest to 122 is **124** (distance 2).
  - Nearest to 124 is **183** (distance 59).
  - Order: $53 \to 65 \to 67 \to 37 \to 14 \to 98 \to 122 \to 124 \to 183$.
- **Total Head Movement:**
  $$12 + 2 + 30 + 23 + 84 + 24 + 2 + 59 = \mathbf{236\text{ cylinders}}.$$
- **Evaluation:** Huge reduction in seek time (from 640 down to 236), but prone to **severe starvation** of requests on distant cylinders if a steady stream of requests clusters near the current track.

---

### 3. SCAN (The Elevator Algorithm)
- **Rule:** The arm moves in one direction across the platters, servicing all pending requests in its path until it reaches the edge of the disk ($0$ or $199$). Upon hitting the boundary, the arm reverses direction and services requests on the return sweep.
- **Trace from 53 (moving toward 199):**
  - Services: $65 \to 67 \to 98 \to 122 \to 124 \to 183 \to \mathbf{199}$ (edge!).
  - Reverses toward 0, services: $37 \to 14$.
- **Total Head Movement:**
  $$(199 - 53) + (199 - 14) = 146 + 185 = \mathbf{331\text{ cylinders}}.$$
- **Evaluation:** Completely prevents starvation; provides good throughput. However, waiting times are non-uniform: requests just behind the arm wait the longest.

---

### 4. C-SCAN (Circular SCAN)
- **Rule:** The arm moves in **one direction only** (e.g., toward higher cylinders), servicing requests along the way. When it reaches the outer boundary ($199$), it immediately **sweeps all the way back to cylinder 0 without servicing any requests**, and starts servicing again!
- **Trace from 53 (moving toward 199):**
  - Services: $65 \to 67 \to 98 \to 122 \to 124 \to 183 \to \mathbf{199}$.
  - Resets to $\mathbf{0}$ (no servicing).
  - Services: $14 \to 37$.
- **Evaluation:** Provides a strictly **uniform waiting time distribution** across all cylinders.

---

### 5. LOOK and C-LOOK
- **Rule:** Real-world optimizations of SCAN and C-SCAN. The arm moves only as far as the **final request in that direction**, rather than traveling all the way to the physical cylinder edge ($0$ or $199$).
- **C-LOOK Trace from 53:**
  - Services up to maximum request: $65 \to 67 \to 98 \to 122 \to 124 \to \mathbf{183}$.
  - Resets directly to smallest request: $\mathbf{14}$.
  - Services: $37$.
- **Total Head Movement (C-LOOK):**
  $$(183 - 53) + (183 - 14) + (37 - 14) = 130 + 169 + 23 = \mathbf{322\text{ cylinders}}.$$

---

## Comparison Table

| Algorithm | Head Movement | Starvation Possible? | Waiting Time Uniformity | Primary Strength |
|---|---|---|---|---|
| **FCFS** | High (640) | No | Moderate | Minimal algorithmic complexity |
| **SSTF** | Low (236) | **Yes** | Poor | Optimal greedy seek reduction |
| **SCAN** | Moderate (331) | No | Fair | Practical, starvation-free |
| **C-SCAN** | Moderate (382) | No | **Excellent** | Uniform wait time across all tracks |
| **C-LOOK** | Moderate (322) | No | **Excellent** | Eliminates wasted arm travel to edges |

---

## Complexity

- **Time Complexity:**
  - *FCFS:* $O(1)$ per request.
  - *SSTF:* $O(N)$ per request to find the closest track ($O(N^2)$ for $N$ requests).
  - *SCAN / C-LOOK:* $O(N \log N)$ to sort requests by cylinder, then $O(N)$ linear traversal.
- **Space Complexity:** $O(N)$ queue storage.

---

## Important Properties and Guarantees

- **Elevator Fairness Invariant:** Under SCAN and C-SCAN, no request waits longer than two full sweeps across the platters, guaranteeing bounded starvation-free service.
- **SPTF / SATF (Shortest Positioning / Access Time First):** Inside modern drive firmware, scheduling algorithms factor in both cylinder seek time and instantaneous rotational position, servicing whichever request minimizes total $T_{\text{Seek}} + T_{\text{Rotation}}$.

---

## Common Mistakes

- **Forgetting the End Turnaround in SCAN:** In classic SCAN, the arm travels all the way to cylinder $0$ or $199$ before turning around, whereas in LOOK it reverses at the last requested cylinder.
- **Counting the Reset Travel in C-SCAN:** When computing total head travel in C-SCAN, students frequently forget to include the distance of the full sweep from the edge back to 0.

---

## Exam Relevance

Appears in virtually every Operating Systems final examination:
- Calculating total head movement (in cylinders) for FCFS, SSTF, SCAN, and C-LOOK given a starting head position and request queue.
- Identifying algorithms that suffer from starvation.
- Explaining the difference between SCAN and C-SCAN.

---

## Related Concepts

- [[Hard Disk Drive Architecture and Mechanical Latency]]
- [[RAID Architectures and Redundancy Models]]
- [[Disk Latency and RAID Performance Evaluation Formulas]]

---

## Prerequisites

- [[Hard Disk Drive Architecture and Mechanical Latency]]

---

## Problems

- [[Problem — Disk Arm Scheduling and RAID Performance Analysis]]

---

## Sources

- **Lecture Slides:** [[cse313/01 - Sources/Lectures/CSE313_KRV_Merged.pdf|CSE313_KRV_Merged.pdf]], Chapter 37 (Slides 266–277).
- **Textbook:** Arpaci-Dusseau & Arpaci-Dusseau, *Operating Systems: Three Easy Pieces (OSTEP)*, Chapter 37 (Hard Disk Drives).
