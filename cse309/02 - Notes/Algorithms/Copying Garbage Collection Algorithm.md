---
type: algorithm
course: cse309
status: active
order: 28
---

# Copying Garbage Collection Algorithm

> 📖 **Reading Order:** Step 28 of 55 | **Module 3:** Run-Time Environments  
> ◄ **Previous:** [[Mark-and-Sweep Garbage Collection Algorithm]] | ► **Next:** [[Display Maintenance and Non-Local Access Simulation Example]]

---

## Building the idea

Cheney's collector copies reachable objects from from-space into to-space. Its key observation is that copied-but-unscanned objects already form a queue inside to-space: `scan` points to its front and `free` to its end.

Copy each root target. Then scan the object at `scan`; for every pointer field, copy its target if needed and rewrite the field to the destination. Move `scan` past the processed object. New copies extend `free`, so discovery and queueing happen together.

A forwarding address in the old object ensures shared references and cycles reuse one destination object. Without it, two incoming pointers could produce duplicate copies or a cycle could copy forever. When `scan==free`, every copied object has been scanned and all reachable references have been redirected.

[[Trace-Based Garbage Collection Algorithms]] supplies reachability; copying also compacts survivors. It requires accurate root and pointer identification, sufficient destination capacity, and a relocation-compatible runtime.

## How It Works

### Comparing costs

| Property | Mark-and-sweep | Two-semispace copying |
|---|---|---|
| Work | Scan roots, trace reachable pointers, and sweep managed allocated blocks/metadata. | Scan roots and reachable pointers; copy live bytes. |
| Layout | Survivors remain at their addresses; free regions may be fragmented. | Survivors are packed into destination space, subject to alignment. |
| Extra space | Mark metadata and a traversal worklist. | Destination space and forwarding metadata; the scan/free queue uses constant control state. |
| Allocation | Depends on the free-list or size-class design. | A bump-pointer fast path can be constant time, excluding checks and initialization costs. |

The weak generational hypothesis is the empirical observation that many objects in many workloads die young. It motivates collecting a young region frequently, but supplies no universal percentage, lifetime, or pause-time guarantee. Generational collection also needs to account for pointers from older regions into the young one.

### The Complete Algorithmic Implementation

```python
class CheneyCollector:
    def __init__(self, from_space, to_space):
        self.from_space = from_space
        self.to_space = to_space

    def copy(self, obj_ptr, free):
        """Copies an object to To-space or returns existing forwarding address."""
        if obj_ptr is None:
            return None, free

        # Case 1: Object has ALREADY been copied during this cycle
        if obj_ptr.has_forwarding_address():
            # Return new destination in To-space
            return obj_ptr.forwarding_address, free

        # Case 2: Object encountered for the first time
        new_addr = free
        copy_bytes(src=obj_ptr, dst=new_addr, size=obj_ptr.size)
        free += obj_ptr.size

        # Leave a forwarding address in the old From-space header!
        obj_ptr.set_forwarding_address(new_addr)
        return new_addr, free

    def collect(self, root_set):
        scan = self.to_space.start
        free = self.to_space.start

        # Step 1: Copy all immediate Root Set objects into To-space
        for i in range(len(root_set)):
            root_set[i], free = self.copy(root_set[i], free)

        # Step 2: Traverse the implicit BFS Queue using 'scan' pointer
        while scan < free:
            current_obj = deref(scan)
            for field in current_obj.get_pointer_fields():
                child_ptr = getattr(current_obj, field)
                # Forward child: copies child to To-space and updates field
                new_child_addr, free = self.copy(child_ptr, free)
                setattr(current_obj, field, new_child_addr)

            # Advance 'scan' past current object (dequeues object from BFS queue)
            scan += current_obj.size

        # Step 3: Swap roles of From-space and To-space
        self.from_space, self.to_space = self.to_space, self.from_space
        return free  # New allocation pointer for mutator
```

---

## Complexity

### Theorem: Correctness and Graph Isomorphism
*Cheney's Copying Garbage Collection algorithm produces a compact, isomorphic copy of the reachable graph $\text{Reachable}(G, R)$ in To-space, updates all references consistently, and runs in time strictly proportional to the number and size of live objects:*
$$\text{Time Complexity} = O(L) \quad \text{where } L \text{ includes live bytes, roots, and scanned pointer fields}$$

### Proof:
1. **Queue Property:**
   - In Step 1, all roots are copied to To-space. They occupy addresses $[to\_space.start, free_0)$. Since $scan = to\_space.start$, the queue initially contains the depth-0 reachable objects.
   - At iteration $k$, the object at `scan` is dequeued. For each outgoing pointer $p$, `copy()` is called.
   - If the child was not previously copied, it is appended at `free` (enqueued), and its old header is tagged with a forwarding pointer.
   - If the child was already copied (by a previous parent sharing the reference), `copy()` reads the forwarding pointer without duplicating the object.
   - Because the queue operates in first-in, first-out order, objects are visited in exact Breadth-First Search order.
2. **Termination:**
   - Since $V$ is finite, each reachable object is copied at most once (enforced by the forwarding pointer check).
   - Each copied object advances `free` by a finite size.
   - In each loop iteration, `scan` advances by the size of the current object.
   - Therefore, `scan` strictly increases toward `free`. Since no new objects can be enqueued once all reachable objects are visited, `scan` must eventually equal `free`. The loop terminates.
3. **Pointer Consistency (Isomorphism):**
   - For every edge $(u, v)$ in the original heap: when $u_{new}$ is scanned, its pointer field to $v$ is replaced by $v_{new}$ (either newly copied or retrieved via forwarding pointer).
   - Thus, $(u_{new}, v_{new})$ exists in To-space. All pointers in the root set and inside live objects point exclusively to valid To-space addresses.
4. **Time Complexity:**
   - Notice what happens to unreachable garbage: **Unreachable objects in From-space are NEVER visited, NEVER inspected, and NEVER copied.**
   - Total work includes root scanning, pointer-field traversal, and $\sum_{v \in \text{Reachable}} \text{size}(v)$ bytes copied.
   - Therefore, the time complexity is strictly $O(L)$, completely independent of total heap capacity $H$! $\blacksquare$

---

### Space Complexity
The implicit queue uses $O(1)$ control state. Destination capacity must hold all live objects, and forwarding/object metadata is additional storage; this is not an $O(1)$ total-memory collector.

---

## Exam Relevance

---

### Exam Relevance

Frequently tested on final examinations via hand-simulation of Copying Garbage Collection Algorithm on given code fragments or graphs.

---

## What to carry forward

The queue needs only scan/free control state beyond the destination and object metadata. Total work includes roots, pointer fields, and live bytes copied. [[Garbage Collection Trace and Compaction Example]] compares its packed result with nonmoving sweep holes.

## Related notes

- [[Trace-Based Garbage Collection Algorithms]]
- [[Garbage Collection Trace and Compaction Example]]

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Chapter 7 (Slides 285–293).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 7.6.3 (Copying Collectors).
- **Cheney, C. J.:** *A Nonrecursive List Compacting Algorithm*, Communications of the ACM, Vol. 13, No. 11, 1970.
