# CSE 309 — Topic Map (Part II: Synthesis & Back End)

This topic map organizes all knowledge notes derived from Part II of CSE 309 (Compilers), taught by Khaled Mahmud Shahriar (KMS), Department of CSE, BUET, and supported by the course textbook *Compilers: Principles, Techniques, & Tools* (2nd Edition) by Aho, Lam, Sethi, and Ullman (Chapters 5–9).

---

## 1. Syntax-Directed Translation (Chapter 5)

### 1.1 Definitions, Attributes & Evaluation Order
- **Core Concepts:**
  - [[Syntax-Directed Definitions and Translation Schemes]] (Declarative SDD vs imperative SDT, grammar attributes, dependency graphs, cycle detection, topological sort)
  - [[Synthesized and Inherited Attributes]] (Bottom-up synthesis vs top-down/left-to-right inheritance, terminal token constraints, scope propagation)
  - [[S-Attributed and L-Attributed SDDs]] (Classification, post-order LR parsing stack compatibility, LL recursive-descent passing conventions)
  - [[Abstract Syntax Tree Construction with SDDs]] (AST leaf and interior node constructors, eliminating concrete grammar clutter, S-attributed and L-attributed AST SDDs)
- **Algorithms:**
  - [[Eliminating Left Recursion from SDTs]] (Case 1 side-effect output actions vs Case 2 synthesized attribute calculation using inherited accumulators)
  - [[Bottom-Up Evaluation of L-Attributed SDDs]] (Marker non-terminals $\epsilon$, LR stack copy rules, negative stack offsets `val[top-k]`)
- **Worked Examples:**
  - [[Arithmetic Expression Desk Calculator SDD Example]] (Parse tree, synthesized attribute annotation, dependency graph for `(2+3)*4+7`)
  - [[Infix to Postfix and Prefix SDT Translation Example]] (LR stack postfix translation, proof of prefix impossibility without buffering)
- **Problems & Practice:**
  - [[Problem — Desk Calculator SDD and Annotated Parse Tree]] (`Q-CSE309-001`: Right-associative exponentiation SDD, annotated parse tree, dependency DAG)

---

## 2. Intermediate-Code Generation (Chapter 6)

### 2.1 Representations, Declarations, Expressions & Control Flow
- **Core Concepts:**
  - [[Intermediate Representations and Three-Address Code]] (Why IR, modularity, retargetability, TAC instruction types, Quadruples, Triples, Indirect Triples, SSA form)
  - [[Type Expressions and Storage Layout]] (Basic types, array/record/pointer/function constructors, structural vs nominal equivalence, relative offsets, symbol table environment chaining)
  - [[Translation of Expressions and Array References]] (Synthesized temporaries `newtemp()`, array load `L.addr` vs array store `[]=` rules)
  - [[Control Flow Translation and Boolean Expressions]] (Short-circuit evaluation, jumping code vs truth values, `if-then`, `if-then-else`, `while` loops)
  - [[Backpatching in Intermediate Code Generation]] (One-pass translation, eliminating inherited labels, `truelist`, `falselist`, `nextlist`, marker non-terminals $M$ and $N$)
- **Algorithms:**
  - [[Value-Number Method for DAG Construction]] (Hash table signature lookup, value numbering, common subexpression identification)
  - [[Backpatching Control-Flow Code Generation Algorithm]] (Formal SDT rules for booleans and statements, `makelist`, `merge`, `backpatch`)
- **Formulas:**
  - [[Multi-Dimensional Array Addressing Formulas]] (1D, 2D row-major, 2D column-major, $k$-dimensional Horner polynomial recurrence, compile-time invariant factoring)
- **Worked Examples:**
  - [[Array Reference and Boolean Control-Flow TAC Generation Example]] (Full translation of `while` loop with 1D array comparisons and updates)
- **Problems & Practice:**
  - [[Problem — Array Reference Three-Address Code Generation]] (`Q-CSE309-002`: 3D array $A[10][20][30]$ address derivation, Horner recurrence, quadruples table)
  - [[Problem — Backpatching Boolean Expression Translation]] (`Q-CSE309-003`: Short-circuit translation of `if (a < b || c < d && e < f)` with backpatching trace)

---

## 3. Run-Time Environments (Chapter 7)

### 3.1 Memory Layout, Stack Frames, Scoping & Garbage Collection
- **Core Concepts:**
  - [[Run-Time Storage Organization and Activation Records]] (Code, Static, Heap, Stack layout; Activation trees; AR structure: parameters, return value, dynamic link, static link, machine status, locals, temporaries)
  - [[Calling Sequences and Stack Frame Management]] (Division of labor between caller and callee, frame pointer $fp$ vs stack pointer $sp$, handling closures on heap)
  - [[Non-Local Variable Access in Static and Dynamic Scopes]] (Static links, static distance $\Delta$, Displays for $O(1)$ non-local access, Dynamic scope deep vs shallow access)
  - [[Heap Memory Management and Allocation Strategies]] (Internal vs external fragmentation, First-fit, Best-fit, Next-fit, Segregated bin-based heaps, Boundary tags $O(1)$ free coalescing)
  - [[Garbage Collection Fundamentals and Reference Counting]] (Mutator vs Collector, Root set, Reachability, Refcounting algorithm, cyclical garbage failure)
  - [[Trace-Based Garbage Collection Algorithms]] (Tri-color states: Free, Unreached, Unscanned, Scanned; Mark-and-Sweep vs Mark-and-Compact vs Copying collectors)
- **Algorithms:**
  - [[Mark-and-Sweep Garbage Collection Algorithm]] (McCarthy's mark phase BFS/DFS, linear sweep phase, cyclic reclamation)
  - [[Copying Garbage Collection Algorithm]] (Stop-and-Copy Cheney queue-less algorithm with `scan` and `free` pointers, semispaces, forwarding addresses)
- **Worked Examples:**
  - [[Display Maintenance and Non-Local Access Simulation Example]] (3-level nested procedures, dynamic calls $Main \to P \to R \to Q$, $O(1)$ display access)
  - [[Garbage Collection Trace and Compaction Example]] (Comparative simulation of Mark-and-Sweep, Mark-and-Compact sliding, and Cheney copying)
- **Problems & Practice:**
  - [[Problem — Activation Record and Display Table Tracing]] (`Q-CSE309-004`: Recursive nested call stack, dynamic vs static links, display maintenance)

---

## 4. Code Generation (Chapter 8)

### 4.1 Target Architecture, Basic Blocks & Local Optimization
- **Core Concepts:**
  - [[Code Generation Issues and Target Machine Architecture]] (Instruction selection, register allocation/assignment, evaluation order, RISC machine model, instruction cost model)
  - [[Basic Blocks and Control Flow Graphs]] (Single-entry single-exit property, CFG edges, loop definitions, optimization hierarchy)
  - [[Liveness and Next-Use Analysis within Basic Blocks]] (Liveness definition, backward scan algorithm, attaching next-use info)
  - [[Peephole Optimization Techniques]] (Sliding window, redundant load/store, unreachable code, flow-of-control jump chains, algebraic identities, machine idioms)
- **Algorithms:**
  - [[Basic Block Partitioning Algorithm]] (The 3 leader rules, partitioning algorithm)
  - [[DAG Construction and Local Optimization of Basic Blocks]] (DAG for basic blocks, local common subexpression elimination, dead code elimination, array write kill rules, reassembling TAC)
  - [[A Simple Code Generator Algorithm]] (Register descriptors, Address descriptors, `getReg(I)` function, emitting assembly for $x = y + z$)
- **Worked Examples:**
  - [[Basic Block Partitioning and Next-Use Computation Example]] (13-statement TAC program, CFG construction, full backward pass next-use table)
  - [[DAG-Based Basic Block Optimization Example]] (Step-by-step DAG construction, local CSE, dead root elimination, TAC reassembly)
- **Problems & Practice:**
  - [[Problem — Basic Block Partitioning and Next-Use Table]] (`Q-CSE309-005`: Leader detection with explicit rule attribution, CFG construction, backward next-use table)
  - [[Problem — DAG Optimization of Basic Block with Array Store]] (`Q-CSE309-006`: Why array load reuse across stores is unsafe, DAG store kill node, compile-time disambiguation)

---

## 5. Register Allocation (Deep Dive)

### 5.1 Linear Scan vs. Chaitin's Graph Coloring
- **Core Concepts:**
  - [[Live Ranges and Live Intervals in Register Allocation]] (Live range definitions, 1D live intervals $[s, e]$, interference overlap, live range splitting)
  - [[Register Interference Graphs and Graph Coloring Principles]] (RIG definition, vertices, conflict edges, NP-completeness of $K$-coloring, Kempe's heuristic theorem)
- **Algorithms:**
  - [[Linear Scan Register Allocation Algorithm]] (Poletto & Sarkar algorithm, active list, `expireOldIntervals`, `spillAtInterval` Belady heuristic, $O(V)$ JIT complexity)
  - [[Chaitin's Graph Coloring Register Allocation Algorithm]] (5 phases: Build, Coalesce, Simplify, Spill, Select; Chaitin spill metric $\frac{\text{Cost}}{\text{degree}}$, inserting spill loads/stores)
- **Worked Examples:**
  - [[Linear Scan Register Allocation Step-by-Step Example]] (Exact KMS lecture 7-variable program, simulation with 4 registers and 2 registers with spilling)
  - [[Chaitin's Graph Coloring Register Allocation Example]] (Exact KMS lecture RIG, Kempe simplification, spill candidate selection, pop-and-color select phase)
- **Problems & Practice:**
  - [[Problem — Linear Scan Register Allocation Simulation]] (`Q-CSE309-007`: 6-variable interval set, active list trace, expiration, Belady spill selection)
  - [[Problem — Chaitin Graph Coloring Register Allocation]] (`Q-CSE309-008`: 5-node RIG, Chaitin spill candidate selection, optimistic coloring triumph)

---

## 6. Machine-Independent Optimization (Chapter 9)

### 6.1 Transformations, Common Subexpressions, Loops & Strength Reduction
- **Core Concepts:**
  - [[Principal Sources of Code Optimization]] (Criteria: safety, performance, compilation cost; redundancy causes; local vs global optimizations)
  - [[Global Common Subexpression Elimination and Copy Propagation]] (Available expressions, copy propagation rules, triggering dead code elimination, constant propagation/folding)
  - [[Loop Optimizations and Strength Reduction]] (Loop-invariant code motion hoisting, basic vs derived induction variables, strength reduction replacing multiplications with additions, induction variable elimination)
- **Worked Examples:**
  - [[Quicksort Partition Loop Complete Optimization Example]] (The complete Dragon Book / BUET slide transformation: 6-block unoptimized CFG to fully optimized 3-instruction inner loops)
- **Problems & Practice:**
  - [[Problem — Quicksort Loop Induction Variable Strength Reduction]] (`Q-CSE309-009`: Double-precision array loop, pre-header initialization, strength reduction, loop counter elimination, quantitative cycle savings analysis)
