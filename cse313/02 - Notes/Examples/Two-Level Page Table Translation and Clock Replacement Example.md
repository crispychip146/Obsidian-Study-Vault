---
type: example
course: cse313
status: active
order: 48
---

# Two-Level Page Table Translation and Clock Replacement Example

> 📖 **Reading Order:** Step 48 of 68 | **Module 7: Paging & Virtual Memory Systems**  
> ◄ **Previous:** [[Virtual Memory Performance and Address Translation Formulas]] | ► **Next:** [[Problem — Multi-Level Paging and Page Replacement Simulation]]

---

## Background and Problem Setup

This example works through two central mechanisms in memory virtualization:
1. **Part 1:** Hardware address translation through a two-level hierarchical page table, extracting Directory Index, Table Index, and Offset from a 32-bit hex virtual address.
2. **Part 2:** Simulation of the Clock (Second-Chance) page replacement algorithm across a 12-reference sequence on a system with 4 physical frames.

---

## Part 1: Two-Level Page Table Translation Walkthrough

### System Specification:
- 32-bit virtual address space.
- Page size = 4 KB ($2^{12}$ bytes $\implies 12$-bit offset).
- Page Table Entry size = 4 bytes.
- Page Directory Index = 10 bits.
- Page Table Index = 10 bits.
- Page Directory Base Register (PDBR) = Physical address `0x1000`.

Suppose a process accesses virtual address **`0x00803ABC`**.

```
Hexadecimal:  0   0   8   0   3   A   B   C
Binary:      0000 0000 1000 0000 0011 1010 1011 1100
Field Split: [ 10-bit PD Index ] [ 10-bit PT Index ] [ 12-bit Offset ]
Bits:        0000 0000 10        00 0000 0011        1010 1011 1100
Hex Values:  0x002               0x003               0xABC
Decimal:     PD Index = 2        PT Index = 3        Offset = 2748
```

### Translation Steps:

#### Step 1: Access Page Directory
- The MMU inspects the Page Directory starting at physical address `0x1000`.
- Target PDE address:
  $$\text{PDE Addr} = \text{PDBR} + (\text{PD Index} \times 4) = \text{0x1000} + (2 \times 4) = \text{0x1000} + \text{0x08} = \mathbf{\text{0x1008}}.$$
- Suppose physical memory at `0x1008` contains:
  `PDE: PFN = 0x50, Present = 1, Writable = 1, User = 1`.
- Since $\text{Present} == 1$, the inner Page Table resides in physical memory at frame `0x50` (Physical address $= \text{0x50} \ll 12 = \text{0x50000}$).

#### Step 2: Access Inner Page Table
- Target PTE address:
  $$\text{PTE Addr} = \text{0x50000} + (\text{PT Index} \times 4) = \text{0x50000} + (3 \times 4) = \text{0x50000} + \text{0x0C} = \mathbf{\text{0x5000C}}.$$
- Suppose physical memory at `0x5000C` contains:
  `PTE: PFN = 0x8A, Present = 1, Writable = 1, User = 1`.
- The desired page resides in physical frame `0x8A`.

#### Step 3: Construct Physical Address
- Concatenate $\text{PFN } (\text{0x8A})$ and $\text{Offset } (\text{0xABC})$:
  $$\text{Physical Address} = (\text{0x8A} \ll 12) \mid \text{0xABC} = \mathbf{\text{0x8AABC}}.$$

---

## Part 2: Clock (Second-Chance) Replacement Simulation

### System Setup:
- Physical memory contains **4 page frames** (Frames 0, 1, 2, 3) initialized empty.
- Clock Hand initially points to **Frame 0**.
- Page reference sequence:
  $$\mathbf{2, 3, 2, 1, 5, 2, 4, 5, 3, 2, 5, 2}$$

### Step-by-Step Execution Trace:

1. **Access 2:** Frame 0 empty $\to$ Load 2 into Frame 0. Set $\text{Use}=1$. Hand moves to Frame 1. [Fault 1]
   - State: `[F0: 2 (U=1)*, F1: -, F2: -, F3: -]`, Hand $\to$ F1.
2. **Access 3:** Frame 1 empty $\to$ Load 3 into Frame 1. Set $\text{Use}=1$. Hand moves to Frame 2. [Fault 2]
   - State: `[F0: 2 (U=1), F1: 3 (U=1)*, F2: -, F3: -]`, Hand $\to$ F2.
3. **Access 2:** Already in Frame 0! Set $\text{Use}=1$. Hand stays at Frame 2. [Hit]
4. **Access 1:** Frame 2 empty $\to$ Load 1 into Frame 2. Set $\text{Use}=1$. Hand moves to Frame 3. [Fault 3]
   - State: `[F0: 2 (U=1), F1: 3 (U=1), F2: 1 (U=1)*, F3: -]`, Hand $\to$ F3.
5. **Access 5:** Frame 3 empty $\to$ Load 5 into Frame 3. Set $\text{Use}=1$. Hand wraps to Frame 0. [Fault 4]
   - State: `[F0: 2 (U=1)*, F1: 3 (U=1), F2: 1 (U=1), F3: 5 (U=1)]`, Hand $\to$ F0.
6. **Access 2:** Already in Frame 0. Set $\text{Use}=1$. Hand stays at Frame 0. [Hit]

#### Replacement Event 1: Access 4 (Memory Full!)
- Target 4 is not in memory $\implies$ **Page Fault 5**.
- Hand is at Frame 0 (`Page 2, U=1`): Clear $\text{Use}=0$, advance hand to Frame 1.
- Hand is at Frame 1 (`Page 3, U=1`): Clear $\text{Use}=0$, advance hand to Frame 2.
- Hand is at Frame 2 (`Page 1, U=1`): Clear $\text{Use}=0$, advance hand to Frame 3.
- Hand is at Frame 3 (`Page 5, U=1`): Clear $\text{Use}=0$, advance hand to Frame 0.
- Hand is at Frame 0 (`Page 2, U=0`): **Victim found!** Evict 2, load 4 into Frame 0. Set $\text{Use}=1$. Advance hand to Frame 1.
- State: `[F0: 4 (U=1), F1: 3 (U=0)*, F2: 1 (U=0), F3: 5 (U=0)]`, Hand $\to$ F1.

7. **Access 5:** Already in Frame 3! Set $\text{Use}=1$. Hand stays at Frame 1. [Hit]
   - State: `[F0: 4 (U=1), F1: 3 (U=0)*, F2: 1 (U=0), F3: 5 (U=1)]`, Hand $\to$ F1.

#### Replacement Event 2: Access 3
- Already in Frame 1! Set $\text{Use}=1$. Hand stays at Frame 1. [Hit]
   - State: `[F0: 4 (U=1), F1: 3 (U=1)*, F2: 1 (U=0), F3: 5 (U=1)]`, Hand $\to$ F1.

#### Replacement Event 3: Access 2 (Memory Full!)
- Target 2 not in memory $\implies$ **Page Fault 6**.
- Hand is at Frame 1 (`Page 3, U=1`): Clear $\text{Use}=0$, advance hand to Frame 2.
- Hand is at Frame 2 (`Page 1, U=0`): **Victim found!** Evict 1, load 2 into Frame 2. Set $\text{Use}=1$. Advance hand to Frame 3.
- State: `[F0: 4 (U=1), F1: 3 (U=0), F2: 2 (U=1), F3: 5 (U=1)*]`, Hand $\to$ F3.

8. **Access 5:** Already in Frame 3! Set $\text{Use}=1$. Hand stays at Frame 3. [Hit]
9. **Access 2:** Already in Frame 2! Set $\text{Use}=1$. Hand stays at Frame 3. [Hit]

### Summary of Results:
- Total References: 12
- Hits: 6
- Faults: 6
- **Hit Rate: $\frac{6}{12} = \mathbf{50.0\%}$**.

---

## Key Takeaways

1. **Clock Hand State:** The clock hand never resets to 0 after an eviction; it resumes from its current position for the next eviction event.
2. **Hit Handling in Clock:** When a page hit occurs, the algorithm sets its Use bit to 1, but **does not move the clock hand**.

---

## Related Concepts

- [[Multi-Level Page Tables and Advanced Address Translation]]
- [[Page Replacement Policies and the Clock Algorithm]]
- [[Problem — Multi-Level Paging and Page Replacement Simulation]]

---

## Sources

- **Lecture Slides:** [[cse313/01 - Sources/Lectures/CSE313_KRV_Merged.pdf|CSE313_KRV_Merged.pdf]], Chapter 20 & 22.
- **Textbook:** Arpaci-Dusseau & Arpaci-Dusseau, *Operating Systems: Three Easy Pieces (OSTEP)*, Chapters 20 & 22.
