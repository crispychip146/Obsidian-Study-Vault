# CSE 309 — Question Bank

This catalog tracks all extracted, fully solved examination and conceptual practice problems for CSE 309 (Compilers), keyed by problem ID with direct links to worked solutions, topic domains, and complexity ratings.

---

## Question Catalog

| ID | Title | Topic Domain | Difficulty | Solved Note Link |
| :---: | :--- | :--- | :---: | :--- |
| **Q-CSE309-001** | Desk Calculator SDD and Annotated Parse Tree | Syntax-Directed Translation | Medium | [[Problem — Desk Calculator SDD and Annotated Parse Tree]] |
| **Q-CSE309-002** | Array Reference Three-Address Code Generation | Intermediate Code Generation | Medium | [[Problem — Array Reference Three-Address Code Generation]] |
| **Q-CSE309-003** | Backpatching Boolean Expression Translation | Intermediate Code Generation | Hard | [[Problem — Backpatching Boolean Expression Translation]] |
| **Q-CSE309-004** | Activation Record and Display Table Tracing | Run-Time Environments | Hard | [[Problem — Activation Record and Display Table Tracing]] |
| **Q-CSE309-005** | Basic Block Partitioning and Next-Use Table | Code Generation | Medium | [[Problem — Basic Block Partitioning and Next-Use Table]] |
| **Q-CSE309-006** | DAG Optimization of Basic Block with Array Store | Code Generation | Hard | [[Problem — DAG Optimization of Basic Block with Array Store]] |
| **Q-CSE309-007** | Linear Scan Register Allocation Simulation | Register Allocation | Medium | [[Problem — Linear Scan Register Allocation Simulation]] |
| **Q-CSE309-008** | Chaitin Graph Coloring Register Allocation | Register Allocation | Hard | [[Problem — Chaitin Graph Coloring Register Allocation]] |
| **Q-CSE309-009** | Quicksort Loop Induction Variable Strength Reduction | Machine-Independent Optimization | Medium | [[Problem — Quicksort Loop Induction Variable Strength Reduction]] |

---

## Detailed Problem Profiles

### Q-CSE309-001: Desk Calculator SDD and Annotated Parse Tree
- **Topic:** Module 1 (Syntax-Directed Translation)
- **Tested Skills:** Context-free grammars with right-associative exponentiation operators, concrete parse trees, bottom-up attribute evaluation, S-attributed vs L-attributed formal classification, dependency graphs.
- **Reference:** [[Problem — Desk Calculator SDD and Annotated Parse Tree]]

---

### Q-CSE309-002: Array Reference Three-Address Code Generation
- **Topic:** Module 2 (Intermediate Code Generation)
- **Tested Skills:** 3-dimensional array address calculation formulas, Horner's nested polynomial recurrence, generating explicit Three-Address Code for array access expressions, constructing Quadruples tables.
- **Reference:** [[Problem — Array Reference Three-Address Code Generation]]

---

### Q-CSE309-003: Backpatching Boolean Expression Translation
- **Topic:** Module 2 (Intermediate Code Generation)
- **Tested Skills:** One-pass intermediate code generation, short-circuit evaluation of compound boolean expressions (`||`, `&&`), marker non-terminals $M$ and $N$, `truelist`, `falselist`, and `nextlist` tracking, list merging, and instruction backpatching.
- **Reference:** [[Problem — Backpatching Boolean Expression Translation]]

---

### Q-CSE309-004: Activation Record and Display Table Tracing
- **Topic:** Module 3 (Run-Time Environments)
- **Tested Skills:** Recursive procedure calls with lexical nesting, activation trees, control stack layout, distinguishing dynamic links (caller) from static links (lexical parent), maintaining the global Display array, $O(1)$ non-local variable address calculation.
- **Reference:** [[Problem — Activation Record and Display Table Tracing]]

---

### Q-CSE309-005: Basic Block Partitioning and Next-Use Table
- **Topic:** Module 4 (Code Generation)
- **Tested Skills:** Applying the 3 formal leader rules to intermediate code, partitioning instructions into basic blocks, constructing Control Flow Graphs (CFGs), performing backward scan next-use and liveness analysis.
- **Reference:** [[Problem — Basic Block Partitioning and Next-Use Table]]

---

### Q-CSE309-006: DAG Optimization of Basic Block with Array Store
- **Topic:** Module 4 (Code Generation)
- **Tested Skills:** Local optimization of basic blocks using Directed Acyclic Graphs, array load (`=[]`) and store (`[]=`) node construction, kill semantics across array writes, compile-time index disambiguation.
- **Reference:** [[Problem — DAG Optimization of Basic Block with Array Store]]

---

### Q-CSE309-007: Linear Scan Register Allocation Simulation
- **Topic:** Module 5 (Register Allocation)
- **Tested Skills:** 1-dimensional live interval sorting, active list maintenance, register expiration checking, handling capacity overflow using Belady's latest-end spill heuristic.
- **Reference:** [[Problem — Linear Scan Register Allocation Simulation]]

---

### Q-CSE309-008: Chaitin Graph Coloring Register Allocation
- **Topic:** Module 5 (Register Allocation)
- **Tested Skills:** Register Interference Graph (RIG) degree computation, Alfred Kempe's degree $< K$ heuristic, Chaitin spill metric selection ($\frac{\text{Cost}}{\text{degree}}$), stack simplification, optimistic coloring select phase.
- **Reference:** [[Problem — Chaitin Graph Coloring Register Allocation]]

---

### Q-CSE309-009: Quicksort Loop Induction Variable Strength Reduction
- **Topic:** Module 6 (Machine-Independent Optimization)
- **Tested Skills:** Basic vs derived induction variable identification, pre-header initialization, strength reduction replacing loop multiplications with additions, induction variable elimination rewriting loop conditions.
- **Reference:** [[Problem — Quicksort Loop Induction Variable Strength Reduction]]
