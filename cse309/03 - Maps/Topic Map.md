# CSE 309 — Topic Map (Complete Curriculum)

This topic map organizes all knowledge notes across the complete curriculum of CSE 309 (Compilers), taught by Ahmed Mahir Sultan Rumi (AMR, Part I: Analysis / Front End) and Khaled Mahmud Shahriar (KMS, Part II: Synthesis / Back End), Department of CSE, BUET, and supported by the course textbook *Compilers: Principles, Techniques, & Tools* (2nd Edition) by Aho, Lam, Sethi, and Ullman (Chapters 1, 3–9).

---

## 1. Compiler Architecture & Language Processors (Chapter 1)

### 1.1 Language Processing System & Compiler Structure
- **Core Concepts:**
  - [[Language Processing System and Translators]] (Translators, Compilers vs Interpreters, Preprocessors, Assemblers, Linkers, Loaders, Hybrid systems like Java JVM)
  - [[Structure and Phases of a Compiler]] (Analysis / Front End vs Synthesis / Back End, 6 core phases, Symbol Table management, Error detection and recovery)

---

## 2. Lexical Analysis (Chapter 3)

### 2.1 Tokens, Buffering, Patterns & Lex Tool
- **Core Concepts:**
  - [[Lexical Analysis Fundamentals]] (Role of lexer, Tokens, Patterns, Lexemes, Stripping whitespace and comments, Line number tracking, Interface with syntax analyzer)
  - [[Input Buffering and Sentinel Techniques]] (Two-buffer scheme, `lexemeBegin` and `forward` pointers, Double-lookahead overhead, Sentinels `eof` eliminating branch per character)
  - [[Specification and Recognition of Tokens]] (Regular expressions, Strings and languages, Transition diagrams, State representation, Longest match / maximal munch rule)
- **Algorithms:**
  - [[Lexical Analyzer Generation with Lex]] (Structure of Lex program `%{%}`, Definitions, Rules `pattern {action}`, User code, DFA synthesis, `yylex()`, `yytext`, `yyleng`)
- **Worked Examples:**
  - [[Token Recognition and Lex Specification Example]] (Complete token specification for arithmetic and relational language, state transition walkthrough, Lex code generation)
- **Problems & Practice:**
  - [[Problem — Lexical Token Recognition and Buffer Simulation]] (`Q-CSE309-010`: Two-buffer pointer tracing, sentinel trigger, buffer reload simulation, token identification)

---

## 3. Syntax Analysis & Top-Down Parsing (Chapter 4)

### 3.1 Grammars, Transformations, FIRST/FOLLOW & LL(1)
- **Core Concepts:**
  - [[Context-Free Grammars and Syntax Analysis]] (Formal 4-tuple $G = (V, T, P, S)$, Derivations, Parse trees, Ambiguity, Eliminating ambiguous constructs)
  - [[Top-Down and Predictive Parsing]] (Recursive-descent parsing, Backtracking perils, Predictive parsing without backtracking, Transition diagram construction)
- **Algorithms:**
  - [[Elimination of Left Recursion and Left Factoring Algorithms]] (Immediate left recursion elimination, General algorithm for indirect left recursion, Left factoring common prefixes)
  - [[LL(1) Parsing Table Construction and Execution Algorithm]] (Constructing table $M[A, a]$, LL(1) non-ambiguity condition, Stack-driven LL(1) table parser engine)
- **Formulas:**
  - [[FIRST and FOLLOW Sets Formulas]] (Formal definitions, inductive computation rules for FIRST($\alpha$) and FOLLOW($A$), handling $\epsilon$-productions, endmarker $\$ $)
- **Worked Examples:**
  - [[LL(1) Parser Construction and Simulation Example]] (Eliminating left recursion, computing FIRST and FOLLOW sets, building $M[A, a]$ table, step-by-step stack parse trace)
- **Problems & Practice:**
  - [[Problem — LL(1) Grammar Verification and Parsing Table Construction]] (`Q-CSE309-011`: Left recursion elimination, FIRST/FOLLOW calculation, parsing table construction, verifying LL(1) compliance)

---

## 4. Bottom-Up Parsing & LR Parsers (Chapter 4)

### 4.1 Reductions, Shift-Reduce, LR(0) Automaton & SLR(1)
- **Core Concepts:**
  - [[Bottom-Up Parsing, Reductions, and Handles]] (Bottom-up philosophy, Rightmost derivation in reverse, Handle definition, Handle pruning, Viable prefixes)
  - [[Shift-Reduce Parsing and Parsing Conflicts]] (Stack-based implementation, 4 actions: Shift, Reduce, Accept, Error, Shift/Reduce and Reduce/Reduce conflicts)
  - [[LR(0) Items and the LR(0) Automaton]] (LR(0) item definition with dot $\cdot$, CLOSURE operation, GOTO operation, Canonical collection of LR(0) items DFA)
- **Algorithms:**
  - [[SLR(1) Parsing Table Construction and LR Parser Algorithm]] (Augmented grammar $S' \to S$, FOLLOW-directed reduction rules, ACTION and GOTO tables, Table-driven LR driver loop)
- **Worked Examples:**
  - [[SLR(1) Automaton and Parsing Table Construction Example]] (Building LR(0) DFA states, constructing SLR(1) table, stack simulation of input string)
- **Problems & Practice:**
  - [[Problem — SLR(1) Table Construction and Conflict Analysis]] (`Q-CSE309-012`: Augmented grammar, complete LR(0) item set generation, SLR(1) table population, detecting and diagnosing conflicts)

---

## 5. Syntax-Directed Translation (Chapter 5)

### 5.1 Definitions, Attributes & Evaluation Order
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

## 6. Intermediate-Code Generation (Chapter 6)

### 6.1 Representations, Declarations, Expressions & Control Flow
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

## 7. Run-Time Environments (Chapter 7)

### 7.1 Memory Layout, Stack Frames, Scoping & Garbage Collection
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

## 8. Code Generation (Chapter 8)

### 8.1 Target Architecture, Basic Blocks & Local Optimization
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

## 9. Register Allocation (Deep Dive)

### 9.1 Linear Scan vs. Chaitin's Graph Coloring
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

## 10. Machine-Independent Optimization (Chapter 9)

### 10.1 Transformations, Common Subexpressions, Loops & Strength Reduction
- **Core Concepts:**
  - [[Principal Sources of Code Optimization]] (Criteria: safety, performance, compilation cost; redundancy causes; local vs global optimizations)
  - [[Global Common Subexpression Elimination and Copy Propagation]] (Available expressions, copy propagation rules, triggering dead code elimination, constant propagation/folding)
  - [[Loop Optimizations and Strength Reduction]] (Loop-invariant code motion hoisting, basic vs derived induction variables, strength reduction replacing multiplications with additions, induction variable elimination)
- **Worked Examples:**
  - [[Quicksort Partition Loop Complete Optimization Example]] (The complete Dragon Book / BUET slide transformation: 6-block unoptimized CFG to fully optimized 3-instruction inner loops)
- **Problems & Practice:**
  - [[Problem — Quicksort Loop Induction Variable Strength Reduction]] (`Q-CSE309-009`: Double-precision array loop, pre-header initialization, strength reduction, loop counter elimination, quantitative cycle savings analysis)
