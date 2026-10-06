---
type: example
course: cse313
status: active
order: 63
---

# LFS Segment Allocation and Imap Checkpoint Example

> 📖 **Reading Order:** Step 63 of 68 | **Module 9: File Systems & Persistence**  
> ◄ **Previous:** [[VSFS Inode Block Indexing and Journaling Crash Recovery Example]] | ► **Next:** [[Problem — File System Inode Capacity and Journaling Recovery]]

---

## Background and Problem Setup

In [[Log-Structured File Systems (LFS) and Segment Cleaning]], we learned that LFS never overwrites disk blocks in place. Instead, it writes all file updates, moving inodes, and inode map (`imap`) pieces sequentially into large segments.

This example provides an end-to-end trace of:
1. File creation and subsequent data appends in LFS.
2. How the Inode Map tracks moving inodes.
3. How the Checkpoint Region (CR) maintains global consistency.
4. Segment cleaning live block detection.

---

## Detailed Execution Trace

### Initial State:
- Empty file system.
- Checkpoint Region (CR) points to initial `imap` piece at disk block `50`.
- Inode 0 (Root Directory `/`) resides at block `40`.

---

### Step 1: Create File `/foo` and Write Block $D_0$
An application creates file `/foo` (assigned Inode 1) and writes its first 4 KB data block $D_0$.

LFS buffers these writes in memory and flushes a segment to physical disk blocks `100 - 103`:
- **Block 100:** Data Block $D_0$ of file `/foo`.
- **Block 101:** Inode 1 (File size $= 4\,\text{KB}$, points to Block 100).
- **Block 102:** Updated `imap` piece for Inodes 0–15:
  - $\text{imap}[0] \to \text{Block } 40$ (Root)
  - $\text{imap}[1] \to \mathbf{\text{Block } 101}$ (Latest location of Inode 1!)
- **Block 103:** Segment Summary Block (records: Block 100 belongs to Inode 1 at offset 0).

The OS updates the in-memory Checkpoint Region: `CR` now records that `imap` piece 0 resides at **Block 102**.

---

### Step 2: Append Data Block $D_1$ to `/foo`
The application appends another 4 KB block $D_1$ to `/foo`.

LFS appends sequentially to the next segment slots `104 - 107`:
- **Block 104:** Data Block $D_1$.
- **Block 105:** **New Inode 1** (File size $= 8\,\text{KB}$, points to Block 100 and Block 104).
  *(Notice: Inode 1 has moved from block 101 to block 105!)*
- **Block 106:** **New `imap` piece**:
  - $\text{imap}[1] \to \mathbf{\text{Block } 105}$!
- **Block 107:** Segment Summary Block.

The in-memory CR updates: `imap` piece 0 now resides at **Block 106**.

---

### Step 3: Overwrite Block $D_0$ with $D_0'$
The application overwrites the first 4 KB of `/foo` with updated content $D_0'$.

LFS appends to blocks `108 - 111`:
- **Block 108:** Data Block $D_0'$.
- **Block 109:** **New Inode 1** (Points to Block 108 and Block 104).
- **Block 110:** **New `imap` piece**:
  - $\text{imap}[1] \to \mathbf{\text{Block } 109}$!
- **Block 111:** Segment Summary Block.

---

### Step 4: Tracing Dead vs. Live Blocks for Segment Cleaning
Look at what has happened to the blocks in the segment:

| Block Address | Content | Is it Live? | Justification |
|---|---|---|---|
| **Block 100** | Old Data $D_0$ | **DEAD (Garbage)** | Current Inode 1 (at block 109) points to Block 108, not 100! |
| **Block 101** | Old Inode 1 | **DEAD (Garbage)** | Current `imap` points to Block 109! |
| **Block 102** | Old `imap` slice | **DEAD (Garbage)** | Current CR points to `imap` at Block 110! |
| **Block 104** | Data $D_1$ | **LIVE** | Current Inode 1 still points to Block 104! |
| **Block 105** | Intermediate Inode 1 | **DEAD** | Superseded by Inode 1 at Block 109! |
| **Block 108** | New Data $D_0'$ | **LIVE** | Current Inode 1 points here! |
| **Block 109** | Latest Inode 1 | **LIVE** | Current `imap` points here! |
| **Block 110** | Latest `imap` slice| **LIVE** | Current CR points here! |

### The Cleaning Operation:
The LFS segment cleaner:
1. Copies the **Live Blocks** (104, 108, 109, 110) into a new segment.
2. Updates `imap` and `CR`.
3. Frees all blocks 100–107 in one sweep, reclaiming the space for new sequential writes!

---

## Key Takeaways

1. **The Inode Map Indirection:** The `imap` shields directories and user applications from having to know where inodes move on disk. A directory only needs to remember `(filename, inode_number)`.
2. **Garbage Accumulation:** Overwriting a block in LFS creates multiple dead blocks (the old data block, the old inode, and the old imap slice).

---

## Related Concepts

- [[Log-Structured File Systems (LFS) and Segment Cleaning]]
- [[Crash Consistency, FSCK, and Write-Ahead Journaling]]
- [[Problem — File System Inode Capacity and Journaling Recovery]]

---

## Sources

- **Lecture Slides:** [[cse313/01 - Sources/Lectures/CSE313_KRV_Merged.pdf|CSE313_KRV_Merged.pdf]], Chapter 43 (Slides 378–396).
- **Textbook:** Arpaci-Dusseau & Arpaci-Dusseau, *Operating Systems: Three Easy Pieces (OSTEP)*, Chapter 43 (Log-structured File Systems).
