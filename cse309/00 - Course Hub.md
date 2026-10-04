# CSE 309 — Compiler

> 3 hours/week · 3 credits · Department of CSE, BUET  
> **Course Instructors:** Part I (AMR: Analysis / Front End) · Part II (KMS: Synthesis / Back End)  
> **Primary Textbook:** *Compilers: Principles, Techniques, & Tools* (2nd Edition) by Alfred V. Aho, Monica S. Lam, Ravi Sethi, Jeffrey D. Ullman (Pearson, 2007) — "The Dragon Book"

---

## Course Description

Compiler structure, lexical analysis, syntax analysis, syntax-directed translation, intermediate-code generation, runtime environments, code generation, machine-independent optimization, register allocation, and interprocedural analysis.

---

# Master Reading Order Roadmap (Part II: Synthesis & Back End)

Follow this 55-step sequential roadmap for complete conceptual mastery and exam readiness:

### Module 1: Syntax-Directed Translation (Chapter 5)
- [ ] **Step 01:** [[Syntax-Directed Definitions and Translation Schemes]] *(Concept)*
- [ ] **Step 02:** [[Synthesized and Inherited Attributes]] *(Concept)*
- [ ] **Step 03:** [[S-Attributed and L-Attributed SDDs]] *(Concept)*
- [ ] **Step 04:** [[Abstract Syntax Tree Construction with SDDs]] *(Concept)*
- [ ] **Step 05:** [[Eliminating Left Recursion from SDTs]] *(Algorithm)*
- [ ] **Step 06:** [[Bottom-Up Evaluation of L-Attributed SDDs]] *(Algorithm)*
- [ ] **Step 07:** [[Arithmetic Expression Desk Calculator SDD Example]] *(Example)*
- [ ] **Step 08:** [[Infix to Postfix and Prefix SDT Translation Example]] *(Example)*
- [ ] **Step 09:** [[Problem — Desk Calculator SDD and Annotated Parse Tree]] *(Problem `Q-CSE309-001`)*

### Module 2: Intermediate Code Generation (Chapter 6)
- [ ] **Step 10:** [[Intermediate Representations and Three-Address Code]] *(Concept)*
- [ ] **Step 11:** [[Value-Number Method for DAG Construction]] *(Algorithm)*
- [ ] **Step 12:** [[Type Expressions and Storage Layout]] *(Concept)*
- [ ] **Step 13:** [[Multi-Dimensional Array Addressing Formulas]] *(Formula)*
- [ ] **Step 14:** [[Translation of Expressions and Array References]] *(Concept)*
- [ ] **Step 15:** [[Control Flow Translation and Boolean Expressions]] *(Concept)*
- [ ] **Step 16:** [[Backpatching in Intermediate Code Generation]] *(Concept)*
- [ ] **Step 17:** [[Backpatching Control-Flow Code Generation Algorithm]] *(Algorithm)*
- [ ] **Step 18:** [[Array Reference and Boolean Control-Flow TAC Generation Example]] *(Example)*
- [ ] **Step 19:** [[Problem — Array Reference Three-Address Code Generation]] *(Problem `Q-CSE309-002`)*
- [ ] **Step 20:** [[Problem — Backpatching Boolean Expression Translation]] *(Problem `Q-CSE309-003`)*

### Module 3: Run-Time Environments (Chapter 7)
- [ ] **Step 21:** [[Run-Time Storage Organization and Activation Records]] *(Concept)*
- [ ] **Step 22:** [[Calling Sequences and Stack Frame Management]] *(Concept)*
- [ ] **Step 23:** [[Non-Local Variable Access in Static and Dynamic Scopes]] *(Concept)*
- [ ] **Step 24:** [[Heap Memory Management and Allocation Strategies]] *(Concept)*
- [ ] **Step 25:** [[Garbage Collection Fundamentals and Reference Counting]] *(Concept)*
- [ ] **Step 26:** [[Trace-Based Garbage Collection Algorithms]] *(Concept)*
- [ ] **Step 27:** [[Mark-and-Sweep Garbage Collection Algorithm]] *(Algorithm)*
- [ ] **Step 28:** [[Copying Garbage Collection Algorithm]] *(Algorithm)*
- [ ] **Step 29:** [[Display Maintenance and Non-Local Access Simulation Example]] *(Example)*
- [ ] **Step 30:** [[Garbage Collection Trace and Compaction Example]] *(Example)*
- [ ] **Step 31:** [[Problem — Activation Record and Display Table Tracing]] *(Problem `Q-CSE309-004`)*

### Module 4: Code Generation (Chapter 8)
- [ ] **Step 32:** [[Code Generation Issues and Target Machine Architecture]] *(Concept)*
- [ ] **Step 33:** [[Basic Blocks and Control Flow Graphs]] *(Concept)*
- [ ] **Step 34:** [[Basic Block Partitioning Algorithm]] *(Algorithm)*
- [ ] **Step 35:** [[Liveness and Next-Use Analysis within Basic Blocks]] *(Concept)*
- [ ] **Step 36:** [[DAG Construction and Local Optimization of Basic Blocks]] *(Algorithm)*
- [ ] **Step 37:** [[A Simple Code Generator Algorithm]] *(Algorithm)*
- [ ] **Step 38:** [[Peephole Optimization Techniques]] *(Concept)*
- [ ] **Step 39:** [[Basic Block Partitioning and Next-Use Computation Example]] *(Example)*
- [ ] **Step 40:** [[DAG-Based Basic Block Optimization Example]] *(Example)*
- [ ] **Step 41:** [[Problem — Basic Block Partitioning and Next-Use Table]] *(Problem `Q-CSE309-005`)*
- [ ] **Step 42:** [[Problem — DAG Optimization of Basic Block with Array Store]] *(Problem `Q-CSE309-006`)*

### Module 5: Register Allocation (Deep Dive)
- [ ] **Step 43:** [[Live Ranges and Live Intervals in Register Allocation]] *(Concept)*
- [ ] **Step 44:** [[Register Interference Graphs and Graph Coloring Principles]] *(Concept)*
- [ ] **Step 45:** [[Linear Scan Register Allocation Algorithm]] *(Algorithm)*
- [ ] **Step 46:** [[Chaitin's Graph Coloring Register Allocation Algorithm]] *(Algorithm)*
- [ ] **Step 47:** [[Linear Scan Register Allocation Step-by-Step Example]] *(Example)*
- [ ] **Step 48:** [[Chaitin's Graph Coloring Register Allocation Example]] *(Example)*
- [ ] **Step 49:** [[Problem — Linear Scan Register Allocation Simulation]] *(Problem `Q-CSE309-007`)*
- [ ] **Step 50:** [[Problem — Chaitin Graph Coloring Register Allocation]] *(Problem `Q-CSE309-008`)*

### Module 6: Machine-Independent Optimization (Chapter 9)
- [ ] **Step 51:** [[Principal Sources of Code Optimization]] *(Concept)*
- [ ] **Step 52:** [[Global Common Subexpression Elimination and Copy Propagation]] *(Concept)*
- [ ] **Step 53:** [[Loop Optimizations and Strength Reduction]] *(Concept)*
- [ ] **Step 54:** [[Quicksort Partition Loop Complete Optimization Example]] *(Example)*
- [ ] **Step 55:** [[Problem — Quicksort Loop Induction Variable Strength Reduction]] *(Problem `Q-CSE309-009`)*

---

# Syllabus & Knowledge Graph Mapping

## 1. Compiler Fundamentals
*(To be populated upon receipt of Part I material)*

## 2. Lexical Analysis
*(To be populated upon receipt of Part I material)*

## 3. Syntax Analysis
*(To be populated upon receipt of Part I material)*

## 4. Syntax-Directed Translation
- [[Syntax-Directed Definitions and Translation Schemes]]
- [[Synthesized and Inherited Attributes]]
- [[S-Attributed and L-Attributed SDDs]]
- [[Abstract Syntax Tree Construction with SDDs]]
- [[Eliminating Left Recursion from SDTs]]
- [[Bottom-Up Evaluation of L-Attributed SDDs]]
- [[Arithmetic Expression Desk Calculator SDD Example]]
- [[Infix to Postfix and Prefix SDT Translation Example]]
- [[Problem — Desk Calculator SDD and Annotated Parse Tree]]

## 5. Intermediate-Code Generation
- [[Intermediate Representations and Three-Address Code]]
- [[Value-Number Method for DAG Construction]]
- [[Type Expressions and Storage Layout]]
- [[Multi-Dimensional Array Addressing Formulas]]
- [[Translation of Expressions and Array References]]
- [[Control Flow Translation and Boolean Expressions]]
- [[Backpatching in Intermediate Code Generation]]
- [[Backpatching Control-Flow Code Generation Algorithm]]
- [[Array Reference and Boolean Control-Flow TAC Generation Example]]
- [[Problem — Array Reference Three-Address Code Generation]]
- [[Problem — Backpatching Boolean Expression Translation]]

## 6. Run-Time Environments
- [[Run-Time Storage Organization and Activation Records]]
- [[Calling Sequences and Stack Frame Management]]
- [[Non-Local Variable Access in Static and Dynamic Scopes]]
- [[Heap Memory Management and Allocation Strategies]]
- [[Garbage Collection Fundamentals and Reference Counting]]
- [[Trace-Based Garbage Collection Algorithms]]
- [[Mark-and-Sweep Garbage Collection Algorithm]]
- [[Copying Garbage Collection Algorithm]]
- [[Display Maintenance and Non-Local Access Simulation Example]]
- [[Garbage Collection Trace and Compaction Example]]
- [[Problem — Activation Record and Display Table Tracing]]

## 7. Code Generation & Register Allocation
- [[Code Generation Issues and Target Machine Architecture]]
- [[Basic Blocks and Control Flow Graphs]]
- [[Basic Block Partitioning Algorithm]]
- [[Liveness and Next-Use Analysis within Basic Blocks]]
- [[DAG Construction and Local Optimization of Basic Blocks]]
- [[A Simple Code Generator Algorithm]]
- [[Peephole Optimization Techniques]]
- [[Live Ranges and Live Intervals in Register Allocation]]
- [[Register Interference Graphs and Graph Coloring Principles]]
- [[Linear Scan Register Allocation Algorithm]]
- [[Chaitin's Graph Coloring Register Allocation Algorithm]]
- [[Basic Block Partitioning and Next-Use Computation Example]]
- [[DAG-Based Basic Block Optimization Example]]
- [[Linear Scan Register Allocation Step-by-Step Example]]
- [[Chaitin's Graph Coloring Register Allocation Example]]
- [[Problem — Basic Block Partitioning and Next-Use Table]]
- [[Problem — DAG Optimization of Basic Block with Array Store]]
- [[Problem — Linear Scan Register Allocation Simulation]]
- [[Problem — Chaitin Graph Coloring Register Allocation]]

## 8. Machine-Independent Optimization
- [[Principal Sources of Code Optimization]]
- [[Global Common Subexpression Elimination and Copy Propagation]]
- [[Loop Optimizations and Strength Reduction]]
- [[Quicksort Partition Loop Complete Optimization Example]]
- [[Problem — Quicksort Loop Induction Variable Strength Reduction]]

---

# Sources & Course Materials

- [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]] (Lectures by Khaled Mahmud Shahriar, Part II, covering Chapters 5–9 and Register Allocation)
- **Course Textbook:** Alfred V. Aho, Monica S. Lam, Ravi Sethi, Jeffrey D. Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Edition), Pearson/Addison Wesley, 2007.

---

# Knowledge Navigation & Revision

- **Knowledge Organization:** [[cse309/03 - Maps/Topic Map|Topic Map]]
- **Dependency Flow:** [[cse309/03 - Maps/Dependency Map|Dependency Map]]
- **Rapid Revision:** [[cse309/04 - Revision/Quick Revision|Quick Revision Reference Sheet]]
- **Problem Bank:** [[cse309/05 - Testing/Question Bank/Question Bank|Question Bank]]
