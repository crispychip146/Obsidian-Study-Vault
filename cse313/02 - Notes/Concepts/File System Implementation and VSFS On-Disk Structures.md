---
type: concept
course: cse313
status: active
order: 58
---

# File System Implementation and VSFS On-Disk Structures

> 📖 **Reading Order:** Step 58 of 68 | **Module 9: File Systems & Persistence**  
> ◄ **Previous:** [[File and Directory Abstractions and POSIX File API]] | ► **Next:** [[Locality and the Berkeley Fast File System (FFS)]]

---

## Starting Point and the Problem

In [[File and Directory Abstractions and POSIX File API]], we established the high-level concepts of files, inodes, and directories.

Now we examine the low-level physical implementation:
> How does an operating system organize raw, flat disk sectors into structured on-disk data structures that store inodes, directories, file payloads, and free-space trackers?

To answer this, OSTEP and modern OS courses analyze the canonical **Very Simple File System (VSFS)** (the conceptual foundation of Linux `ext2`).

---

## Developing the Idea: On-Disk Layout

VSFS partitions a raw storage disk (e.g., 64 blocks of 4 KB each) into five distinct functional regions:

```
VSFS Physical Block Layout:
+----+----+----+----+----+----+----+----+-----------------------------------+
| S  | ib | db | I  | I  | I  | I  | I  | D  | D  | D  | D  | ... | D  | D  | D |
+----+----+----+----+----+----+----+----+-----------------------------------+
  0    1    2    3    4    5    6    7    8                                63
```

1. **Superblock ($S$, Block 0):** Stores global file system metadata:
   - Magic number identifying file system type (e.g., `0xef53` for ext2).
   - Total number of inodes ($80$) and total data blocks ($56$).
   - Block size (4 KB) and pointers to where inode and data tables begin.
2. **Inode Bitmap ($ib$, Block 1):** A bit array tracking which inodes are free ($0$) or allocated ($1$). A single 4 KB block can track $32,768$ inodes!
3. **Data Bitmap ($db$, Block 2):** A bit array tracking which data blocks in the data region are free ($0$) or allocated ($1$).
4. **Inode Table ($I$, Blocks 3 to 7):** An array of contiguous `struct inode` records. At 256 bytes per inode, a single 4 KB block holds 16 inodes; 5 blocks hold 80 inodes.
5. **Data Region ($D$, Blocks 8 to 63):** The remaining blocks reserved to store user file data blocks and directory records.

---

## Multi-Level Inode Indexing

How does an inode point to the data blocks that belong to a file?
A naive array of direct pointers cannot support large files. Storing thousands of pointers in every inode wastes immense space for tiny files.

VSFS uses **Multi-Level Indexing** (the classic UNIX Inode structure):

```
Inode Structure (Multi-Level Pointers):
+-----------------------------------+
| File Metadata (Size, UID, Time)   |
+-----------------------------------+
| Direct Pointer 0                  | ----> Data Block 0
| Direct Pointer 1                  | ----> Data Block 1
| ... (12 Direct Pointers)          | ----> Data Blocks 2 .. 11
+-----------------------------------+
| Single Indirect Pointer           | ----> Indirect Block [1024 Pointers] ----> Data Blocks
+-----------------------------------+
| Double Indirect Pointer           | ----> Block of 1024 Indirect Ptrs ----> ...
+-----------------------------------+
| Triple Indirect Pointer           | ----> Block of Double Indirect Ptrs ----> ...
+-----------------------------------+
```

### The Capacity Math (4 KB Blocks, 4-Byte Pointers):
- **12 Direct Pointers:** Point directly to the first 12 data blocks:
  $$12 \times 4\,\text{KB} = \mathbf{48\,\text{KB}}.$$
  Small files ($< 48\,\text{KB}$) enjoy immediate $O(1)$ block lookups!
- **1 Single Indirect Pointer:** Points to a 4 KB block containing $\frac{4096}{4} = 1024$ direct pointers:
  $$1024 \times 4\,\text{KB} = \mathbf{4\,\text{MB}}.$$
- **1 Double Indirect Pointer:** Points to a block containing 1024 indirect block pointers:
  $$1024 \times 1024 \times 4\,\text{KB} = \mathbf{4\,\text{GB}}.$$
- **1 Triple Indirect Pointer:** Points to a block containing 1024 double-indirect pointers:
  $$1024 \times 1024 \times 1024 \times 4\,\text{KB} = \mathbf{4\,\text{TB}}.$$
- **Maximum File Size:**
  $$\text{Max Size} = 48\,\text{KB} + 4\,\text{MB} + 4\,\text{GB} + 4\,\text{TB} \approx \mathbf{4.004\,\text{TB}}.$$

---

## Directory Organization and Path Traversal

A directory is simply a file whose data blocks hold an array of directory entries:
```c
struct dirent {
    int inode_num;       // Target inode number
    int record_len;      // Byte length of this directory entry
    int name_len;        // Length of filename string
    char name[256];      // Null-terminated filename string
};
```

### Access Path Trace: Reading `/foo/bar`
To read file `/foo/bar` from disk:
1. **Read Root Inode:** The root directory `/` is always at well-known Inode 2. Read Inode 2 from the Inode Table.
2. **Read Root Data:** Read the data block pointed to by Inode 2; scan directory entries to find name `"foo"`. It maps to Inode 40.
3. **Read `foo` Inode:** Read Inode 40 from the Inode Table.
4. **Read `foo` Data:** Read data block of Inode 40; scan directory entries to find name `"bar"`. It maps to Inode 85.
5. **Read `bar` Inode:** Read Inode 85 from the Inode Table to get file size and data block pointers.
6. **Read `bar` Data:** Read data block(s) of Inode 85.
7. **Update Access Time:** Write updated `atime` in Inode 85 back to disk.

Notice that reading a tiny file requires **8 separate disk I/O operations**!

---

## The Buffer Cache / Page Cache

To prevent these repeated disk traversals from destroying performance, the operating system maintains a unified **Buffer Cache / Page Cache** in physical RAM:
- Traversed directory inodes and data blocks are cached in RAM.
- Subsequent reads for `/foo/bar` hit in the page cache with **0 physical disk I/O operations**!

---

## Important Properties and Guarantees

- **Inode Address Locality Formula:** Given an inode number $N$, its exact physical disk byte address is calculated deterministically in $O(1)$ time without searching:
  $$\mathbf{\text{Byte Address} = \text{InodeTableStart} + (N \times \text{sizeof(struct inode)})}$$
  $$\mathbf{\text{Disk Block} = \text{InodeBlockStart} + \left\lfloor \frac{N \times \text{sizeof(inode)}}{\text{BlockSize}} \right\rfloor}$$
- **Multi-Level Scale Invariant:** Tiny files consume minimal metadata (only the inode itself), while massive multi-gigabyte files dynamically expand through indirect pointer trees.

---

## Common Mistakes

- **Confusing Bitmaps with Inodes:** The Inode Bitmap only tracks 1 bit per inode ($0 = \text{free}, 1 = \text{used}$). The actual file metadata is stored in the Inode Table.
- **Forgetting Directory Read Costs:** Beginners assume opening a file is instantaneous. Without a page cache, deep directory nesting (`/a/b/c/d/e.txt`) incurs severe disk seek penalties.

---

## Exam Relevance

Consistently tested through:
- Calculating maximum file sizes supported by custom multi-level inode pointer configurations.
- Tracing disk read/write sequences for file creation, reading, and appending.
- Computing physical block locations from given inode numbers and block sizes.

---

## Related Concepts

- [[File and Directory Abstractions and POSIX File API]]
- [[Locality and the Berkeley Fast File System (FFS)]]
- [[Crash Consistency, FSCK, and Write-Ahead Journaling]]

---

## Prerequisites

- [[File and Directory Abstractions and POSIX File API]]
- [[Hard Disk Drive Architecture and Mechanical Latency]]

---

## Problems

- [[Problem — File System Inode Capacity and Journaling Recovery]]

---

## Sources

- **Lecture Slides:** [[cse313/01 - Sources/Lectures/CSE313_KRV_Merged.pdf|CSE313_KRV_Merged.pdf]], Chapter 40 (Slides 320–337).
- **Textbook:** Arpaci-Dusseau & Arpaci-Dusseau, *Operating Systems: Three Easy Pieces (OSTEP)*, Chapter 40 (File System Implementation).
