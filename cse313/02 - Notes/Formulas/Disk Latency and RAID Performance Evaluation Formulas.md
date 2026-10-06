---
type: formula
course: cse313
status: active
order: 54
---

# Disk Latency and RAID Performance Evaluation Formulas

> 📖 **Reading Order:** Step 54 of 68 | **Module 8: I/O Hardware & Storage Systems**  
> ◄ **Previous:** [[RAID Architectures and Redundancy Models]] | ► **Next:** [[Disk Scheduling Simulation and RAID Small-Write Example]]

---

## Starting Point and Need

Rigorous analysis of storage systems requires mathematical models to evaluate:
1. Physical mechanical latency for disk reads and writes.
2. Usable capacity, fault tolerance, and effective throughput of RAID arrays under sequential and random workloads.

---

## 1. Mechanical Disk Access Latency Formulas

$$\mathbf{T_{\text{I/O}} = T_{\text{Seek}} + T_{\text{Rotation}} + T_{\text{Transfer}}}$$

Where:
- $T_{\text{Seek}}$: Mechanical actuator arm movement time (typically $4 - 10\,\text{ms}$).
- $T_{\text{Rotation}}$: Rotational wait time for target sector:
  $$\mathbf{T_{\text{rot\_avg}} = \frac{1}{2} \cdot \frac{60}{\text{RPM}} = \frac{30}{\text{RPM}}}$$
- $T_{\text{Transfer}}$: Streaming data transfer time:
  $$\mathbf{T_{\text{Transfer}} = \frac{\text{Data Size (Bytes)}}{\text{Transfer Rate (Bytes/sec)}}}$$

---

## 2. RAID Capacity and Storage Efficiency Formulas

For an array of $N$ disks each of capacity $C$:

$$\text{Capacity}_{\text{RAID 0}} = N \cdot C \quad (\text{Efficiency } = 100\%)$$
$$\text{Capacity}_{\text{RAID 1}} = \frac{N}{2} \cdot C \quad (\text{Efficiency } = 50\%)$$
$$\text{Capacity}_{\text{RAID 4/5}} = (N - 1) \cdot C \quad \left(\text{Efficiency } = \frac{N-1}{N} \times 100\%\right)$$
$$\text{Capacity}_{\text{RAID 6}} = (N - 2) \cdot C \quad \left(\text{Efficiency } = \frac{N-2}{N} \times 100\%\right)$$

---

## 3. RAID Throughput Evaluation Formulas

Let:
- $S$: Sequential transfer throughput of a single disk ($\text{MB/s}$).
- $R$: Random I/O operations per second (IOPS) of a single disk ($R = 1 / T_{\text{I/O}}$).

### Throughput Formulas:

| RAID Level | Sequential Read | Sequential Write | Random Read | Random Write |
|---|---|---|---|---|
| **RAID 0** | $N \cdot S$ | $N \cdot S$ | $N \cdot R$ | $N \cdot R$ |
| **RAID 1** | $\frac{N}{2} \cdot S$ (or $N \cdot S$) | $\frac{N}{2} \cdot S$ | $N \cdot R$ | $\frac{N}{2} \cdot R$ |
| **RAID 4** | $(N - 1) \cdot S$ | $(N - 1) \cdot S$ | $(N - 1) \cdot R$ | $\frac{R}{2}$ (Parity Bottleneck) |
| **RAID 5** | $(N - 1) \cdot S$ | $(N - 1) \cdot S$ | $N \cdot R$ | $\mathbf{\frac{N}{4} \cdot R}$ |

---

## 4. The Parity Recalculation Equation

To modify block $D_i$ to $D_i'$ in RAID 4/5 without reading all other $N-2$ data disks:

$$\mathbf{P_{\text{new}} = (D_{\text{old}} \oplus D_{\text{new}}) \oplus P_{\text{old}}}$$

Cost: **2 Reads** ($D_{\text{old}}, P_{\text{old}}$) $+$ **2 Writes** ($D_{\text{new}}, P_{\text{new}}$) $= \mathbf{4\text{ I/O operations per single write}}$.

---

## Worked Numerical Application

### Problem:
An enterprise storage system uses an array of **8 disks** configured in **RAID 5**.
Each disk spins at **10,000 RPM**, has an average seek time of **$6\,\text{ms}$**, and a sequential transfer rate of **$150\,\text{MB/s}$**. Each disk has a capacity of **2 TB**.
1. What is the usable capacity and storage efficiency of the array?
2. Compute the average service time $T_{\text{I/O}}$ and IOPS rating ($R$) of a single disk for random 4 KB reads.
3. Compute the maximum aggregate throughput of the RAID 5 array for:
   - Sequential reads
   - Random reads (in IOPS)
   - Random writes (in IOPS)

### Solution:

#### 1. Usable Capacity & Efficiency:
$$\text{Capacity} = (N - 1) \cdot C = (8 - 1) \times 2\,\text{TB} = \mathbf{14\,\text{TB}}.$$
$$\text{Efficiency} = \frac{7}{8} \times 100\% = \mathbf{87.5\%}.$$

#### 2. Single Disk Random Performance:
- $T_{\text{Seek}} = 6\,\text{ms}$.
- $T_{\text{Rotation}} = \frac{30}{10000} = 3\,\text{ms}$.
- $T_{\text{Transfer}} = \frac{4096\,\text{bytes}}{150 \times 10^6\,\text{bytes/s}} \approx 0.027\,\text{ms}$.
- Total $T_{\text{I/O}} = 6 + 3 + 0.027 = \mathbf{9.027\,\text{ms}} = 0.009027\,\text{s}$.
- Single Disk IOPS:
  $$R = \frac{1}{T_{\text{I/O}}} = \frac{1}{0.009027\,\text{s}} \approx \mathbf{110.8\text{ IOPS}}.$$

#### 3. RAID 5 Aggregate Performance:
- **Sequential Read:**
  $$\text{Throughput} = (N - 1) \cdot S = 7 \times 150\,\text{MB/s} = \mathbf{1050\,\text{MB/s}}.$$
- **Random Read:**
  $$\text{Throughput} = N \cdot R = 8 \times 110.8 = \mathbf{886.4\text{ IOPS}}.$$
- **Random Write (accounting for 4-I/O penalty):**
  $$\text{Throughput} = \frac{N}{4} \cdot R = \frac{8}{4} \times 110.8 = 2 \times 110.8 = \mathbf{221.6\text{ IOPS}}.$$

---

## Conditions and Limitations

- **Homogeneous Disk Assumption:** Formulas assume all disks have identical capacity and performance. In heterogeneous arrays, capacity and throughput are constrained by the slowest, smallest drive.
- **Controller Overhead:** Does not account for PCIe bus saturation or RAID controller XOR computation latency.

---

## Exam Relevance

Frequently tested in Operating Systems exams via:
- Evaluating RAID 1 vs RAID 5 cost and IOPS for database workloads.
- Step-by-step evaluation of single-disk access latency ($T_{\text{Seek}} + T_{\text{Rot}} + T_{\text{Trans}}$).
- Proving the $\frac{N}{4} \cdot R$ random write throughput formula.

---

## Related Concepts

- [[Hard Disk Drive Architecture and Mechanical Latency]]
- [[Disk Arm Scheduling Algorithms]]
- [[RAID Architectures and Redundancy Models]]

---

## Prerequisites

- [[Hard Disk Drive Architecture and Mechanical Latency]]
- [[RAID Architectures and Redundancy Models]]

---

## Problems

- [[Problem — Disk Arm Scheduling and RAID Performance Analysis]]

---

## Sources

- **Lecture Slides:** [[cse313/01 - Sources/Lectures/CSE313_KRV_Merged.pdf|CSE313_KRV_Merged.pdf]], Chapter 37 & 38.
- **Textbook:** Arpaci-Dusseau & Arpaci-Dusseau, *Operating Systems: Three Easy Pieces (OSTEP)*, Chapters 37 & 38.
