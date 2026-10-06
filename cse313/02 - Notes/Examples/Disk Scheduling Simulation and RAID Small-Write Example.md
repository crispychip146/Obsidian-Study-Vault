---
type: example
course: cse313
status: active
order: 55
---

# Disk Scheduling Simulation and RAID Small-Write Example

> 📖 **Reading Order:** Step 55 of 68 | **Module 8: I/O Hardware & Storage Systems**  
> ◄ **Previous:** [[Disk Latency and RAID Performance Evaluation Formulas]] | ► **Next:** [[Problem — Disk Arm Scheduling and RAID Performance Analysis]]

---

## Background and Problem Setup

This example works through two practical storage engineering problems:
1. **Part 1:** Full comparative simulation of SSTF, SCAN, and C-LOOK disk arm scheduling algorithms on a given cylinder queue.
2. **Part 2:** Numerical step-by-step trace of a small-write parity update in RAID 4 vs RAID 5, demonstrating bitwise XOR arithmetic and disk saturation.

---

## Part 1: Disk Scheduling Comparative Simulation

A disk has **200 cylinders** numbered $0$ to $199$.
Current head position: **cylinder 50**.
Direction of movement: **toward higher cylinders**.
Pending I/O request queue:
$$\mathbf{82, 170, 43, 140, 24, 16, 190}$$

### 1. SSTF (Shortest Seek Time First) Trace:
Current: **50**.
- Candidates: 43 (dist 7), 82 (dist 32), 24 (dist 26), etc. $\implies$ pick **43**. (Traveled: $7$)
- From 43: pick **24** (dist 19). (Traveled: $19$)
- From 24: pick **16** (dist 8). (Traveled: $8$)
- From 16: nearest is **82** (dist 66). (Traveled: $66$)
- From 82: nearest is **140** (dist 58). (Traveled: $58$)
- From 140: nearest is **170** (dist 30). (Traveled: $30$)
- From 170: nearest is **190** (dist 20). (Traveled: $20$)
- **Order:** $50 \to 43 \to 24 \to 16 \to 82 \to 140 \to 170 \to 190$.
- **Total Head Movement:**
  $$7 + 19 + 8 + 66 + 58 + 30 + 20 = \mathbf{208\text{ cylinders}}.$$

### 2. SCAN (Elevator Algorithm) Trace:
Current: **50**, moving **UP** toward $199$.
- Services all requests $\ge 50$ in ascending order: **82, 140, 170, 190**.
- Reaches disk boundary: **199**!
- Reverses direction, services remaining requests in descending order: **43, 24, 16**.
- **Order:** $50 \to 82 \to 140 \to 170 \to 190 \to \mathbf{199} \to 43 \to 24 \to 16$.
- **Total Head Movement:**
  $$(199 - 50) + (199 - 16) = 149 + 183 = \mathbf{332\text{ cylinders}}.$$

### 3. C-LOOK Trace:
Current: **50**, moving **UP**.
- Services all requests $\ge 50$ in ascending order up to maximum request: **82, 140, 170, 190**.
- Reverses directly to smallest pending request: **16** (no servicing during reset).
- Services remaining requests moving up: **24, 43**.
- **Order:** $50 \to 82 \to 140 \to 170 \to 190 \to \mathbf{16} \to 24 \to 43$.
- **Total Head Movement:**
  $$(190 - 50) + (190 - 16) + (43 - 16) = 140 + 174 + 27 = \mathbf{341\text{ cylinders}}.$$

---

## Part 2: RAID Small-Write Parity Update

Consider a 4-disk RAID 4 system:
- Disks 0, 1, 2 are Data Disks.
- Disk 3 is the dedicated Parity Disk.

Suppose Stripe 10 holds the following 8-bit bytes:
- Disk 0: $D_0 = \mathbf{1011\;0010_2}$ (`0xB2`)
- Disk 1: $D_1 = \mathbf{0100\;1101_2}$ (`0x4D`)
- Disk 2: $D_2 = \mathbf{1100\;0001_2}$ (`0xC1`)

### Step 1: Initial Parity Verification
$$P = D_0 \oplus D_1 \oplus D_2$$
$$\begin{array}{r@{\quad}l}
  1011\;0010 & (D_0) \\
\oplus\; 0100\;1101 & (D_1) \\
\hline
  1111\;1111 & (D_0 \oplus D_1) \\
\oplus\; 1100\;0001 & (D_2) \\
\hline
  \mathbf{0011\;1110} & (P = \text{0x3E})
\end{array}$$
Disk 3 currently stores Parity $P = \text{0x3E}$.

### Step 2: The Small-Write Operation
An application updates block $D_1$ on Disk 1 with new data:
$$\mathbf{D_1' = 1111\;0000_2} \quad (\text{0xF0})$$

Instead of reading Disks 0 and 2, the RAID controller uses the **Subtract-and-Add Parity Formula**:
$$P_{\text{new}} = (D_{\text{old}} \oplus D_{\text{new}}) \oplus P_{\text{old}}$$

1. **Read Operations:**
   - Read $D_{1,\text{old}} = 0100\;1101_2$ from Disk 1.
   - Read $P_{\text{old}} = 0011\;1110_2$ from Disk 3.
   - (Total: 2 disk reads).
2. **Compute Difference Vector:**
   $$\Delta = D_{1,\text{old}} \oplus D_1' = 0100\;1101_2 \oplus 1111\;0000_2 = \mathbf{1011\;1101_2}$$
3. **Compute New Parity:**
   $$P_{\text{new}} = \Delta \oplus P_{\text{old}} = 1011\;1101_2 \oplus 0011\;1110_2 = \mathbf{1000\;0011_2} \quad (\text{0x83})$$
4. **Write Operations:**
   - Write $D_1' = \text{0xF0}$ to Disk 1.
   - Write $P_{\text{new}} = \text{0x83}$ to Disk 3.
   - (Total: 2 disk writes).

### Step 3: Verification of Full Stripe Parity
Verify that $D_0 \oplus D_1' \oplus D_2 == P_{\text{new}}$:
$$\begin{array}{r@{\quad}l}
  1011\;0010 & (D_0) \\
\oplus\; 1111\;0000 & (D_1') \\
\hline
  0100\;0010 & \\
\oplus\; 1100\;0001 & (D_2) \\
\hline
  \mathbf{1000\;0011} & (P_{\text{new}} = \text{0x83}) \quad \checkmark \text{ Matches perfectly!}
\end{array}$$

---

## Key Takeaways

1. **The 4-I/O Bottleneck Visualized:** Every single updated byte requires Disk 1 to perform 1 read + 1 write, and Disk 3 to perform 1 read + 1 write.
2. **RAID 4 vs RAID 5:** In RAID 4, if another process updates $D_0$ at the exact same moment, it must wait because Disk 3 is busy! In RAID 5, different stripes have parities on different disks, enabling concurrent parallel writes.

---

## Related Concepts

- [[Hard Disk Drive Architecture and Mechanical Latency]]
- [[Disk Arm Scheduling Algorithms]]
- [[RAID Architectures and Redundancy Models]]

---

## Sources

- **Lecture Slides:** [[cse313/01 - Sources/Lectures/CSE313_KRV_Merged.pdf|CSE313_KRV_Merged.pdf]], Chapter 37 & 38.
- **Textbook:** Arpaci-Dusseau & Arpaci-Dusseau, *Operating Systems: Three Easy Pieces (OSTEP)*, Chapters 37 & 38.
