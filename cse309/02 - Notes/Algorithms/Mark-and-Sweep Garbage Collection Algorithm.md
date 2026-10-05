---
type: algorithm
course: cse309
status: active
order: 27
---

# Mark-and-Sweep Garbage Collection Algorithm

> 📖 **Reading Order:** Step 27 of 55 | **Module 3:** Run-Time Environments  
> ◄ **Previous:** [[Trace-Based Garbage Collection Algorithms]] | ► **Next:** [[Copying Garbage Collection Algorithm]]

---

## Building the idea

Mark-and-sweep separates **finding survivors** from **reclaiming the rest**. Begin with the roots, mark each newly reached object, and put its outgoing references on a worklist. Mark before recursively revisiting it so a cycle cannot cause endless traversal.

After the worklist empties, sweep allocated blocks: marked blocks survive, unmarked blocks become free, and marks are reset for the next collection. The mark phase establishes reachability; the sweep phase acts on that classification.

Why does marking find every reachable object? Roots are discovered initially. Whenever an object is discovered, all its outgoing references are eventually examined, so the next object along any finite root path is discovered. Conversely, every discovery begins at a root or follows a discovered object's pointer. This proves the marked set is exactly the reachable set under the model.

[[Trace-Based Garbage Collection Algorithms]] supplies the traversal invariant. Survivors keep their locations, which avoids relocation but can leave separated holes.

## How It Works

### Performance Bottlenecks & Modern Industrial Optimizations

While Mark-and-Sweep is elegant, naive implementations suffer from two severe hardware bottlenecks:

### Sweep cost and mark storage

Marking follows reachable references. Sweeping additionally inspects the allocator's managed blocks or metadata to identify unmarked allocations. Its cost depends on that representation; it need not read every byte of reserved virtual address space.

Mark bits can live in object headers or in side metadata such as a bitmap. Header updates affect object cache lines; a side bitmap concentrates mark state, but still needs enough information to identify object boundaries and reclaim regions. A bitmap changes locality and storage costs, rather than guaranteeing a fixed speedup or eliminating all heap-management work.

---

### Theorem: Correctness of Mark-and-Sweep
*Upon termination of the Mark-and-Sweep algorithm:*
1. *Every heap object reachable from the root set remains allocated and uncorrupted.*
2. *Every heap object unreachable from the root set is reclaimed into the free list.*
3. *All mark bits of surviving objects are restored to zero.*

### Proof:
1. **Correctness of Mark Phase:**
   - Let $G = (V, E)$ be the heap graph and $R$ be the root set.
   - We claim: an object $v \in V$ has $v.marked == \mathbf{True}$ at the end of Phase 1 iff $v \in \text{Reachable}(G, R)$.
   - *Base Case:* All $r \in R$ are marked and pushed to `worklist`.
   - *Inductive Step:* A node $u$ is popped from `worklist`. For every outgoing edge $(u, w) \in E$, if $w$ is unmarked, it is marked and pushed. By standard properties of graph search (Breadth-First or Depth-First Search), the algorithm visits the transitive closure of $R$.
   - Since $V$ is finite, the search terminates. Upon termination, $v.marked == \mathbf{True} \iff \exists r \in R, r \rightsquigarrow v$.
2. **Correctness of Sweep Phase:**
   - The loop sequentially visits every contiguous allocated chunk $c$ in $[heap\_start, heap\_end)$.
   - **Case 1 ($c$ was reachable):** By Step 1, $c.marked == \mathbf{True}$. The sweep branch executes:
     $$c.is\_allocated = \mathbf{True} \quad (\text{remains live}), \quad c.marked = \mathbf{False}$$
     The object is preserved, and its mark bit is cleanly reset.
   - **Case 2 ($c$ was unreachable):** By Step 1, $c.marked == \mathbf{False}$. The sweep branch executes:
     $$c.is\_allocated = \mathbf{False}, \quad \text{add\_to\_free\_list}(c)$$
     The object is reclaimed into the free memory pool.
3. Therefore, exactly the unreachable objects are freed, all live objects are preserved, and all mark bits are restored. $\blacksquare$

---

## Complexity

### Time Complexity
Root scanning plus $O(V_L+E_L)$ marking and $O(B)$ sweeping in an object/block model, where $V_L,E_L$ describe the reachable graph and $B$ the managed blocks scanned.

### Space Complexity
Mark metadata plus a traversal worklist of up to $O(V_L)$ object references; allocator/free-list storage is accounted for separately.

---

## Exam Relevance

---

### Exam Relevance

Frequently tested on final examinations via hand-simulation of Mark-and-Sweep Garbage Collection Algorithm on given code fragments or graphs.

---

## What to carry forward

A conventional cost is root scanning plus reachable pointer traversal plus allocated-block sweeping. Sweeping need not examine every byte of reserved address space. Worklist storage can grow with discovered objects; iterative traversal avoids relying on an unbounded language call stack.

## Related notes

- [[Trace-Based Garbage Collection Algorithms]]

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Chapter 7 (Slides 270–278).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 7.6.1 (Mark-and-Sweep Garbage Collection).
- **McCarthy, J.:** *Recursive Functions of Symbolic Expressions and Their Computation by Machine, Part I*, Communications of the ACM, 1960.
