# CSE 309 — Quick Revision Sheet (Part II)

A high-density reference sheet designed for rapid pre-exam review across all 6 core back-end modules.

---

## 1. Syntax-Directed Translation (Chapter 5)

- **SDD vs SDT:** SDD is declarative (grammar + attributes + mathematical rules); SDT is imperative (grammar + embedded actions $\{ \dots \}$).
- **Synthesized Attribute:** Value at node $N$ computed strictly from children of $N$ (or $N$ itself). Terminal attributes are synthesized by lexer.
- **Inherited Attribute:** Value at node $N$ computed from parent or siblings. Terminals **cannot** have inherited attributes.
- **S-Attributed SDD:** Contains **only** synthesized attributes. Evaluated bottom-up in post-order. Implemented natively on LR parser stack upon reduction.
- **L-Attributed SDD:** Contains synthesized and restricted inherited attributes: in $A \to X_1 \dots X_n$, inherited attribute $X_j.i$ depends only on inherited attributes of $A$ and attributes of left siblings $X_1 \dots X_{j-1}$.
  - Evaluated in depth-first, left-to-right traversal.
  - Implemented in LL recursive descent: inherited $\to$ function arguments; synthesized $\to$ return values.
  - Implemented in LR parsers via marker non-terminals ($M \to \epsilon$) and stack negative offsets (`val[top-k]`).
- **Eliminating Left Recursion in SDTs:**
  - With side effects: $A \to A \{a\} \alpha \mid \beta \implies A \to \beta R, \; R \to \{a\} \alpha R \mid \epsilon$.
  - With attribute calculations: convert synthesized attribute into an inherited accumulator attribute on $R$.

---

## 2. Intermediate Code Generation (Chapter 6)

- **Three-Address Code (TAC):** At most one operator on RHS; at most 3 addresses per instruction: $x = y \text{ op } z$.
- **Representations:**
  - **Quadruple:** `(op, arg1, arg2, result)`. Explicit temporaries. Easiest to move/optimize.
  - **Triple:** `(op, arg1, arg2)`. No explicit temporaries; result is statement index. Hard to reorder.
  - **Indirect Triple:** Array of pointers to triples. Easy to reorder by swapping pointers.
- **SSA Form:** Every variable assigned exactly once; $\phi$-functions merge converging control paths ($x_3 = \phi(x_1, x_2)$).
- **Array Addressing Formulas:**
  - 1D: $\text{base} + i \times w$.
  - 2D Row-Major: $\text{base} + (i_1 \times n_2 + i_2) \times w$.
  - 2D Column-Major: $\text{base} + (i_2 \times n_1 + i_1) \times w$.
  - $k$-Dimensional Row-Major (Horner recurrence): $\text{offset}_j = \text{offset}_{j-1} \times n_j + i_j$.
- **Backpatching:**
  - Operates in one pass without inherited labels using synthesized lists: `truelist`, `falselist`, `nextlist`.
  - Operations: `makelist(i)`, `merge(p1, p2)`, `backpatch(p, i)`.
  - Markers: $M \to \epsilon \{ M.quad = nextquad \}$, $N \to \epsilon \{ N.nextlist = \text{makelist}(nextquad); \; \text{emit}('goto \_') \}$.

---

## 3. Run-Time Environments (Chapter 7)

- **Memory Segments:** Code (read-only), Static Data (globals), Heap (dynamic, grows up), Stack (call frames, grows down).
- **Activation Record (AR):** Parameters, Return value, Dynamic Link (points to caller), Static Link (points to lexical parent), Machine status (saved PC/regs), Local data, Temporaries.
- **Static Links vs Displays:**
  - Static links: require traversing $\Delta = n_{\text{use}} - n_{\text{def}}$ pointer hops ($O(\Delta)$ time).
  - Displays: global array $\text{Display}[d]$ pointing to most recent frame at depth $d$. Resolves non-locals in strictly **$O(1)$ time** ($\text{Display}[d] + \text{offset}$).
- **Heap Allocation Policies:** First-Fit (fast, fragments front), Best-Fit (minimizes waste, creates dust), Next-Fit. Segregated bins provide $O(1)$ small allocations. Boundary tags enable $O(1)$ bidirectional free space coalescing.
- **Garbage Collection:**
  - **Reference Counting:** Incremental, immediate reclamation; **fails on circular references**; high assignment overhead.
  - **Mark-and-Sweep:** McCarthy 2-phase; handles cycles; non-relocating; leaves heap fragmented; cost $O(L + H)$.
  - **Mark-and-Compact:** Relocating; computes `NewLocation`, updates pointers, slides objects; zero fragmentation; cost $O(H)$.
  - **Copying (Cheney Stop-and-Copy):** Divides heap into From-space and To-space; queue-less BFS copying using `scan` and `free` pointers; cost **$O(L)$ (proportional only to LIVE data)**; bump-pointer $O(1)$ allocation; halves memory.

---

## 4. Code Generation (Chapter 8)

- **Basic Block:** Maximal straight-line sequence of TAC entered only at first instruction and exited only at last instruction.
- **The 3 Leader Rules:**
  1. Instruction 1 is a leader.
  2. Any target of a conditional or unconditional jump is a leader.
  3. Any instruction immediately following a jump is a leader.
- **Next-Use Analysis:** Computed via **backward scan** from last instruction to first instruction. Defined variable is killed (dead, none); used variables are generated (live, current instruction).
- **DAG for Basic Blocks:**
  - Leaves = initial variable values ($a_0$).
  - Interior nodes = operators.
  - Identifies local common subexpressions and eliminates dead roots.
  - **Array store rule:** Node `[]=` has 3 children (`array, index, value`) and **kills all active load nodes** on that array unless indices are provably distinct.
- **Peephole Optimizations:** Sliding window (2–4 instructions) eliminating redundant loads/stores, unreachable dead code, jump chains (`goto L1 ... L1: goto L2`), and applying strength reduction ($x * 8 \to x \ll 3$).

---

## 5. Register Allocation (Deep Dive)

- **Linear Scan:**
  - Operates on 1D live intervals $[start, end]$ sorted by start point.
  - Maintains `active` list sorted by end point.
  - Reclaims registers when intervals expire (`end < current.start`).
  - Spills interval with the **latest end point** (Belady heuristic).
  - Complexity: **$O(V \log V + V \log R)$**; ideal for JIT compilers.
- **Chaitin's Graph Coloring:**
  - Builds Register Interference Graph (RIG) where edges represent overlapping liveness.
  - **Kempe's Rule:** If node $v$ has $\text{degree}(v) < K$, it can always be colored. Remove and push to stack (Simplify).
  - **Spill Metric:** If all degrees $\ge K$, pick node minimizing $\frac{\text{SpillCost}(v)}{\text{degree}(v)}$.
  - **Select:** Pop stack and color. If spill candidate cannot be colored, insert spill load before uses and store after definitions, rebuild RIG, and repeat!

---

## 6. Machine-Independent Optimization (Chapter 9)

- **Optimization Criteria:** Must preserve semantics (safety), must speed up execution, must have acceptable compilation cost.
- **Global Common Subexpression Elimination:** Replaces redundant calculation reaching along all paths with earlier temporary.
- **Copy Propagation:** Replaces uses of $x$ with $y$ following $x = y$, exposing $x$ as dead code.
- **Dead Code Elimination:** Removes statements calculating values never used on any path.
- **Loop-Invariant Code Motion (Hoisting):** Moves invariant computations to loop pre-header.
- **Induction Variables & Strength Reduction:**
  - Basic IV: $i = i + c$.
  - Derived IV: $j = c_1 \times i + c_2$.
  - Replaces $j = c_1 \times i$ inside loop with $j = j + (c_1 \times c)$, eliminating expensive multiplications.
  - Induction Variable Elimination: rewrites loop test in terms of derived IV, allowing the loop counter $i = i + c$ to be deleted entirely!
