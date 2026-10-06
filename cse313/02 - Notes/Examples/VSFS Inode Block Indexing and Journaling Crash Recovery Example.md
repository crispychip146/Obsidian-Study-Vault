---
type: example
course: cse313
status: active
order: 62
---

# VSFS Inode Block Indexing and Journaling Crash Recovery Example

> 📖 **Reading Order:** Step 62 of 68 | **Module 9: File Systems & Persistence**  
> ◄ **Previous:** [[Log-Structured File Systems (LFS) and Segment Cleaning]] | ► **Next:** [[LFS Segment Allocation and Imap Checkpoint Example]]

---

## Background and Problem Setup

This example works through two essential mechanics of persistent file systems:
1. **Part 1:** Computing physical disk block lookups for arbitrary byte offsets within a multi-level indexed inode (VSFS / ext2).
2. **Part 2:** Chronological step-by-step trace of a system crash during Ordered (Metadata) Journaling and the recovery procedure executed upon reboot.

---

## Part 1: Inode Multi-Level Block Lookup

### System Parameters:
- Block size = 4 KB ($4096 = 2^{12}$ bytes).
- Disk block pointers = 4 bytes ($2^2$ bytes).
- Pointers per indirect block = $\frac{4096}{4} = 1024 = 2^{10}$.
- Inode structure:
  - 12 Direct Pointers (`direct[0]` to `direct[11]`)
  - 1 Single Indirect Pointer (`indirect`)
  - 1 Double Indirect Pointer (`double_indirect`)

Determine the exact pointer path traversed to access byte offset **`4,300,800`** within a file.

### Step-by-Step Resolution:

#### Step 1: Compute File Block Number
$$\text{File Block Number (FBN)} = \left\lfloor \frac{4300800}{4096} \right\rfloor = \mathbf{1050}$$
$$\text{Offset within Block} = 4300800 \pmod{4096} = \mathbf{0}$$

#### Step 2: Determine Pointer Level
1. **Direct Blocks:** Cover FBN $0$ to $11$ (12 blocks total).
   - $1050 \ge 12 \implies$ Not in direct blocks.
2. **Single Indirect Blocks:** Cover FBN $12$ to $12 + 1024 - 1 = \mathbf{1035}$ (1024 blocks).
   - $1050 > 1035 \implies$ Not in single indirect blocks.
3. **Double Indirect Blocks:** Cover FBN $1036$ and beyond.
   - $1050 \ge 1036 \implies$ Located in **Double Indirect Blocks**!

#### Step 3: Compute Double Indirect Indices
- Relative index within double indirect range:
  $$\text{RelIndex} = 1050 - 1036 = \mathbf{14}$$
- Each first-level indirect block in the double-indirect tree covers $1024$ data blocks:
  $$\text{First-Level Index} = \left\lfloor \frac{14}{1024} \right\rfloor = \mathbf{0}$$
  $$\text{Second-Level Index} = 14 \pmod{1024} = \mathbf{14}$$

#### Traversal Path:
1. Read the inode to obtain `inode.double_indirect` block pointer.
2. Read physical block at `inode.double_indirect` to fetch the first-level table; access entry `[0]`.
3. Read the physical block pointed to by entry `[0]` to fetch the second-level table; access entry `[14]`.
4. Read the target data block pointed to by entry `[14]`.

---

## Part 2: Ordered Journaling Crash Recovery Trace

Consider a process appending 4 KB of data to `/var/log/syslog`.
- Data Block: $D_{50}$ (placed at physical data block 500).
- Updated Inode: $I_{22}$ (increased file size, points to block 500).
- Updated Bitmap: $B_1$ (bit 500 marked 1).

The file system operates in **Ordered (Metadata) Journaling Mode**.

```
Chronological Sequence of Events:
Event 1: Write D_50 to final location (Block 500)
Event 2: Wait on Hardware Flush Barrier (D_50 lands on magnetic platters)
Event 3: Write [TxB (TxID: 104), I_22, B_1] to Journal Blocks 100-102
Event 4: Wait on Hardware Flush Barrier
Event 5: Write TxE (Commit Record) to Journal Block 103
Event 6: [CRASH OCCURS HERE!]
```

### Analysis of the Crash:
At Event 6, power fails before the OS can checkpoint $I_{22}$ and $B_1$ to their permanent on-disk locations.

```
On-Disk State Upon Reboot:
Final Disk Locations:
- Block 500: Contains D_50 (Safely written in Event 1)
- Inode Table: Old Inode 22 (Old size, does not point to Block 500!)
- Data Bitmap: Old Bitmap (Bit 500 is still 0!)

Journal State:
- Block 100: TxB (TxID: 104)
- Block 101: Updated Inode 22
- Block 102: Updated Bitmap 1
- Block 103: TxE (TxID: 104) <-- VALID COMMIT RECORD FOUND!
```

### Recovery Routine Executed by Kernel Upon Mount:
1. The kernel mounts the file system and reads the Journal.
2. It discovers Transaction `104` has a valid, matching `TxE` commit record!
3. **Replay Phase (Redo Logging):**
   - The kernel copies Inode $I_{22}$ from Journal Block 101 to its permanent location in the Inode Table.
   - The kernel copies Bitmap $B_1$ from Journal Block 102 to its permanent location in the Data Bitmap.
4. **Checkpoint Complete:** Transaction 104 is fully applied.
5. The kernel marks Transaction 104 as free in the journal superblock.
6. **Result:** Zero data lost, zero corruption. Recovery completes in **$2.5\,\text{ms}$** without scanning the partition!

---

## Key Takeaways

1. **Multi-Level Index Subtraction:** Always subtract the prior capacities (12 direct, 1024 single-indirect) before computing indices into double-indirect tables.
2. **The Commit Barrier Invariant:** If power had failed before Event 5, Transaction 104 would have had no `TxE` and been cleanly discarded, leaving the old file consistent.

---

## Related Concepts

- [[File System Implementation and VSFS On-Disk Structures]]
- [[Crash Consistency, FSCK, and Write-Ahead Journaling]]
- [[Problem — File System Inode Capacity and Journaling Recovery]]

---

## Sources

- **Lecture Slides:** [[cse313/01 - Sources/Lectures/CSE313_KRV_Merged.pdf|CSE313_KRV_Merged.pdf]], Chapter 40 & 42.
- **Textbook:** Arpaci-Dusseau & Arpaci-Dusseau, *Operating Systems: Three Easy Pieces (OSTEP)*, Chapters 40 & 42.
