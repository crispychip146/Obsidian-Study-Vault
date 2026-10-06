---
type: problem
course: cse313
status: active
order: 64
---

# Problem — File System Inode Capacity and Journaling Recovery

> 📖 **Reading Order:** Step 64 of 68 | **Module 9: File Systems & Persistence**  
> ◄ **Previous:** [[LFS Segment Allocation and Imap Checkpoint Example]] | ► **Next:** [[Kernel Memory Allocation Architecture and the Slab Allocator]]

---

## Problem Statement

### Catalog Identifier: `Q-CSE313-009`

#### Part A: Multi-Level Inode Capacity and Addressing (8 Marks)
An operating system uses a UNIX-style file system with the following specifications:
- Block size = 2 KB ($2048 = 2^{11}$ bytes).
- Block address pointers = 4 bytes ($2^2$ bytes).
- Each inode contains:
  - 10 Direct block pointers
  - 2 Single Indirect block pointers
  - 1 Double Indirect block pointer
  - 1 Triple Indirect block pointer

1. Compute the maximum file size supported by this file system in gigabytes.
2. An application reads 2,048 bytes starting at byte offset **`5,246,976`** within a file.
   - Which file block number (FBN) does this correspond to?
   - Trace the exact sequence of pointer dereferences required to access this data block from the inode, specifying the indirect table levels and relative index positions.

#### Part B: Crash Consistency and Journaling Analysis (7 Marks)
A journaling file system operating in **Ordered Metadata Journaling Mode** is appending a data block $D$ to file `/etc/hosts` (Inode $I$). The update requires writing:
- New Data Block $D$
- Updated Inode $I$
- Updated Data Bitmap $B$

1. Write the precise chronological timeline of disk writes (including transaction markers and disk flush barriers) required to guarantee crash consistency.
2. Suppose the disk controller crashes under each of the following conditions:
   - **Crash Scenario 1:** Crash occurs immediately after data block $D$ is written to its permanent location, but before anything is written to the journal.
   - **Crash Scenario 2:** Crash occurs after the transaction metadata ($I, B$) is written to the journal, but before the Transaction End (`TxE`) commit record lands on disk.
   - **Crash Scenario 3:** Crash occurs after the Transaction End (`TxE`) block is written to the journal, but before $I$ and $B$ are checkpointed to their permanent in-place locations.
   For each scenario, explain what state the file system is left in and the exact recovery steps taken upon reboot.

---

## Complete Step-by-Step Solution

### Solution to Part A

#### 1. Maximum File Size Calculation:
- Block Size $= 2048\,\text{bytes} = 2\,\text{KB}$.
- Pointers per indirect block $= \frac{2048\,\text{bytes}}{4\,\text{bytes}} = 512 = 2^9$ pointers.

Capacity by pointer level:
1. **10 Direct Pointers:**
   $$10 \times 2\,\text{KB} = \mathbf{20\,\text{KB}}.$$
2. **2 Single Indirect Pointers:**
   $$2 \times (512 \times 2\,\text{KB}) = 2 \times 1024\,\text{KB} = 2 \times 1\,\text{MB} = \mathbf{2\,\text{MB}}.$$
3. **1 Double Indirect Pointer:**
   $$1 \times (512 \times 512 \times 2\,\text{KB}) = 262,144 \times 2\,\text{KB} = 524,288\,\text{KB} = \mathbf{512\,\text{MB}}.$$
4. **1 Triple Indirect Pointer:**
   $$1 \times (512 \times 512 \times 512 \times 2\,\text{KB}) = 134,217,728 \times 2\,\text{KB} = 268,435,456\,\text{KB} = \mathbf{256\,\text{GB}}.$$

**Total Maximum File Size:**
$$\text{Max Size} = 20\,\text{KB} + 2\,\text{MB} + 512\,\text{MB} + 256\,\text{GB} \approx \mathbf{256.502\,\text{GB}}.$$

#### 2. Address Offset Dereference Trace:
- Byte Offset $= 5,246,976$.
- $\text{FBN} = \frac{5246976}{2048} = \mathbf{2562}$.
- Range Coverage Analysis:
  - Direct blocks: $0$ to $9$ ($10$ blocks).
  - Single indirect blocks: $10$ to $10 + 1024 - 1 = \mathbf{1033}$ ($1024$ blocks).
  - Double indirect blocks: $1034$ to $1034 + 262,144 - 1 = \mathbf{263,177}$ ($262,144$ blocks).
- Since $1034 \le 2562 \le 263177$, the block is located in the **Double Indirect Pointer** tree!
- Relative index within double indirect range:
  $$\text{RelIndex} = 2562 - 1034 = \mathbf{1528}$$
- First-level indirect block index:
  $$\text{Level 1 Index} = \left\lfloor \frac{1528}{512} \right\rfloor = \mathbf{2}$$
- Second-level data block pointer index:
  $$\text{Level 2 Index} = 1528 \pmod{512} = \mathbf{504}$$

**Traversal Sequence:**
1. Read inode to extract `inode.double_indirect` pointer.
2. Read the first-level indirect block; extract pointer at index **`[2]`**.
3. Read the second-level indirect block pointed to by entry `[2]`; extract data block pointer at index **`[504]`**.
4. Read physical data block.

---

### Solution to Part B

#### 1. Ordered Metadata Journaling Timeline:
```
1. Write Data Block D to permanent disk location
2. Issue Hardware Flush Barrier (Wait for D to land on disk)
3. Write [TxB, Inode I, Bitmap B] to Journal
4. Issue Hardware Flush Barrier (Wait for metadata to land in journal)
5. Write TxE (Commit Record) to Journal
   ---> [TRANSACTION COMMITTED!]
6. Checkpoint: Write Inode I and Bitmap B to permanent disk locations
7. Free Transaction in Journal Superblock
```

#### 2. Crash Scenarios and Recovery:

- **Scenario 1 (Crash after $D$ written, before Journal write):**
  - *State:* Data block $D$ is on disk, but neither the journal nor permanent metadata knows about it.
  - *Recovery:* Upon reboot, the journal contains no active transaction. No action is taken. The file size remains unchanged. Block $D$ is considered free space by the bitmap and will be overwritten safely by future writes. **Result: Consistent state, uncommitted write discarded.**
- **Scenario 2 (Crash after metadata written to journal, before `TxE`):**
  - *State:* The journal contains `TxB`, $I$, and $B$, but lacks the `TxE` commit record.
  - *Recovery:* The recovery manager detects an incomplete transaction lacking `TxE`. The transaction is discarded. The permanent inode and bitmap remain untouched. **Result: Consistent state, uncommitted write discarded.**
- **Scenario 3 (Crash after `TxE`, before Checkpointing):**
  - *State:* The journal holds a fully committed transaction (`TxB`, $I$, $B$, `TxE`). Permanent inode and bitmap still hold old values.
  - *Recovery:* The recovery manager reads the journal, validates `TxE`, and executes **Redo Replay**: it copies Inode $I$ and Bitmap $B$ from the journal to their permanent in-place locations. **Result: 100% data and metadata recovered, zero loss!**

---

## Reusable Insight

1. **The Invariant of Ordered Journaling:** User data must always reach magnetic storage before the commit record `TxE` is written. This guarantees that replaying metadata upon recovery will *never* cause an inode to point to uninitialized disk sectors.
2. **Multi-Level Indexing Scalability:** Inode structures preserve $O(1)$ fast lookups for small files while supporting multi-gigabyte files via indirect tree expansion.

---

## Related Concepts

- [[File System Implementation and VSFS On-Disk Structures]]
- [[Crash Consistency, FSCK, and Write-Ahead Journaling]]
- [[Log-Structured File Systems (LFS) and Segment Cleaning]]

---

## Sources

- **Lecture Slides:** [[cse313/01 - Sources/Lectures/CSE313_KRV_Merged.pdf|CSE313_KRV_Merged.pdf]], Chapter 40 & 42.
- **Textbook:** Arpaci-Dusseau & Arpaci-Dusseau, *Operating Systems: Three Easy Pieces (OSTEP)*, Chapters 40 & 42.
