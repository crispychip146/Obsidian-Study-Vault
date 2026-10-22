# CSE 309 — Compiler

> 3 hours/week · 3 credits · Department of CSE, BUET  
> **Course Instructors:** Part I (AMR: Analysis / Front End) · Part II (KMS: Synthesis / Back End)  
> **Primary Textbook:** *Compilers: Principles, Techniques, & Tools* (2nd Edition) by Alfred V. Aho, Monica S. Lam, Ravi Sethi, Jeffrey D. Ullman (Pearson, 2007) — "The Dragon Book"

---

## Course Description

Compiler structure, lexical analysis, syntax analysis, syntax-directed translation, intermediate-code generation, runtime environments, code generation, machine-independent optimization, register allocation, and interprocedural analysis.

---

# Master Reading Order Roadmap (Complete 76-Step Curriculum)

Follow this 76-step sequential roadmap for complete conceptual mastery and exam readiness:

### Module 1: Compiler Architecture & Language Processors
- [ ] **Step 01:** [[Language Processing System and Translators]] *(Concept)*
- [ ] **Step 02:** [[Structure and Phases of a Compiler]] *(Concept)*

### Module 2: Lexical Analysis
- [ ] **Step 03:** [[Lexical Analysis Fundamentals]] *(Concept)*
- [ ] **Step 04:** [[Input Buffering and Sentinel Techniques]] *(Concept)*
- [ ] **Step 05:** [[Specification and Recognition of Tokens]] *(Concept)*
- [ ] **Step 06:** [[Lexical Analyzer Generation with Lex]] *(Algorithm)*
- [ ] **Step 07:** [[Token Recognition and Lex Specification Example]] *(Example)*
- [ ] **Step 08:** [[Problem — Lexical Token Recognition and Buffer Simulation]] *(Problem `Q-CSE309-010`)*

### Module 3: Syntax Analysis & Top-Down Parsing
- [ ] **Step 09:** [[Context-Free Grammars and Syntax Analysis]] *(Concept)*
- [ ] **Step 10:** [[Elimination of Left Recursion and Left Factoring Algorithms]] *(Algorithm)*
- [ ] **Step 11:** [[FIRST and FOLLOW Sets Formulas]] *(Formula)*
- [ ] **Step 12:** [[Top-Down and Predictive Parsing]] *(Concept)*
- [ ] **Step 13:** [[LL(1) Parsing Table Construction and Execution Algorithm]] *(Algorithm)*
- [ ] **Step 14:** [[LL(1) Parser Construction and Simulation Example]] *(Example)*
- [ ] **Step 15:** [[Problem — LL(1) Grammar Verification and Parsing Table Construction]] *(Problem `Q-CSE309-011`)*

### Module 4: Bottom-Up Parsing & LR Parsers
- [ ] **Step 16:** [[Bottom-Up Parsing, Reductions, and Handles]] *(Concept)*
- [ ] **Step 17:** [[Shift-Reduce Parsing and Parsing Conflicts]] *(Concept)*
- [ ] **Step 18:** [[LR(0) Items and the LR(0) Automaton]] *(Concept)*
- [ ] **Step 19:** [[SLR(1) Parsing Table Construction and LR Parser Algorithm]] *(Algorithm)*
- [ ] **Step 20:** [[SLR(1) Automaton and Parsing Table Construction Example]] *(Example)*
- [ ] **Step 21:** [[Problem — SLR(1) Table Construction and Conflict Analysis]] *(Problem `Q-CSE309-012`)*

### Module 5: Syntax-Directed Translation (Chapter 5)
- [ ] **Step 22:** [[Syntax-Directed Definitions and Translation Schemes]] *(Concept)*
- [ ] **Step 23:** [[Synthesized and Inherited Attributes]] *(Concept)*
- [ ] **Step 24:** [[S-Attributed and L-Attributed SDDs]] *(Concept)*
- [ ] **Step 25:** [[Abstract Syntax Tree Construction with SDDs]] *(Concept)*
- [ ] **Step 26:** [[Eliminating Left Recursion from SDTs]] *(Algorithm)*
- [ ] **Step 27:** [[Bottom-Up Evaluation of L-Attributed SDDs]] *(Algorithm)*
- [ ] **Step 28:** [[Arithmetic Expression Desk Calculator SDD Example]] *(Example)*
- [ ] **Step 29:** [[Infix to Postfix and Prefix SDT Translation Example]] *(Example)*
- [ ] **Step 30:** [[Problem — Desk Calculator SDD and Annotated Parse Tree]] *(Problem `Q-CSE309-001`)*

### Module 6: Intermediate Code Generation (Chapter 6)
- [ ] **Step 31:** [[Intermediate Representations and Three-Address Code]] *(Concept)*
- [ ] **Step 32:** [[Value-Number Method for DAG Construction]] *(Algorithm)*
- [ ] **Step 33:** [[Type Expressions and Storage Layout]] *(Concept)*
- [ ] **Step 34:** [[Multi-Dimensional Array Addressing Formulas]] *(Formula)*
- [ ] **Step 35:** [[Translation of Expressions and Array References]] *(Concept)*
- [ ] **Step 36:** [[Control Flow Translation and Boolean Expressions]] *(Concept)*
- [ ] **Step 37:** [[Backpatching in Intermediate Code Generation]] *(Concept)*
- [ ] **Step 38:** [[Backpatching Control-Flow Code Generation Algorithm]] *(Algorithm)*
- [ ] **Step 39:** [[Array Reference and Boolean Control-Flow TAC Generation Example]] *(Example)*
- [ ] **Step 40:** [[Problem — Array Reference Three-Address Code Generation]] *(Problem `Q-CSE309-002`)*
- [ ] **Step 41:** [[Problem — Backpatching Boolean Expression Translation]] *(Problem `Q-CSE309-003`)*

### Module 7: Run-Time Environments (Chapter 7)
- [ ] **Step 42:** [[Run-Time Storage Organization and Activation Records]] *(Concept)*
- [ ] **Step 43:** [[Calling Sequences and Stack Frame Management]] *(Concept)*
- [ ] **Step 44:** [[Non-Local Variable Access in Static and Dynamic Scopes]] *(Concept)*
- [ ] **Step 45:** [[Heap Memory Management and Allocation Strategies]] *(Concept)*
- [ ] **Step 46:** [[Garbage Collection Fundamentals and Reference Counting]] *(Concept)*
- [ ] **Step 47:** [[Trace-Based Garbage Collection Algorithms]] *(Concept)*
- [ ] **Step 48:** [[Mark-and-Sweep Garbage Collection Algorithm]] *(Algorithm)*
- [ ] **Step 49:** [[Copying Garbage Collection Algorithm]] *(Algorithm)*
- [ ] **Step 50:** [[Display Maintenance and Non-Local Access Simulation Example]] *(Example)*
- [ ] **Step 51:** [[Garbage Collection Trace and Compaction Example]] *(Example)*
- [ ] **Step 52:** [[Problem — Activation Record and Display Table Tracing]] *(Problem `Q-CSE309-004`)*

### Module 8: Code Generation (Chapter 8)
- [ ] **Step 53:** [[Code Generation Issues and Target Machine Architecture]] *(Concept)*
- [ ] **Step 54:** [[Basic Blocks and Control Flow Graphs]] *(Concept)*
- [ ] **Step 55:** [[Basic Block Partitioning Algorithm]] *(Algorithm)*
- [ ] **Step 56:** [[Liveness and Next-Use Analysis within Basic Blocks]] *(Concept)*
- [ ] **Step 57:** [[DAG Construction and Local Optimization of Basic Blocks]] *(Algorithm)*
- [ ] **Step 58:** [[A Simple Code Generator Algorithm]] *(Algorithm)*
- [ ] **Step 59:** [[Peephole Optimization Techniques]] *(Concept)*
- [ ] **Step 60:** [[Basic Block Partitioning and Next-Use Computation Example]] *(Example)*
- [ ] **Step 61:** [[DAG-Based Basic Block Optimization Example]] *(Example)*
- [ ] **Step 62:** [[Problem — Basic Block Partitioning and Next-Use Table]] *(Problem `Q-CSE309-005`)*
- [ ] **Step 63:** [[Problem — DAG Optimization of Basic Block with Array Store]] *(Problem `Q-CSE309-006`)*

### Module 9: Register Allocation (Deep Dive)
- [ ] **Step 64:** [[Live Ranges and Live Intervals in Register Allocation]] *(Concept)*
- [ ] **Step 65:** [[Register Interference Graphs and Graph Coloring Principles]] *(Concept)*
- [ ] **Step 66:** [[Linear Scan Register Allocation Algorithm]] *(Algorithm)*
- [ ] **Step 67:** [[Chaitin's Graph Coloring Register Allocation Algorithm]] *(Algorithm)*
- [ ] **Step 68:** [[Linear Scan Register Allocation Step-by-Step Example]] *(Example)*
- [ ] **Step 69:** [[Chaitin's Graph Coloring Register Allocation Example]] *(Example)*
- [ ] **Step 70:** [[Problem — Linear Scan Register Allocation Simulation]] *(Problem `Q-CSE309-007`)*
- [ ] **Step 71:** [[Problem — Chaitin Graph Coloring Register Allocation]] *(Problem `Q-CSE309-008`)*

### Module 10: Machine-Independent Optimization (Chapter 9)
- [ ] **Step 72:** [[Principal Sources of Code Optimization]] *(Concept)*
- [ ] **Step 73:** [[Global Common Subexpression Elimination and Copy Propagation]] *(Concept)*
- [ ] **Step 74:** [[Loop Optimizations and Strength Reduction]] *(Concept)*
- [ ] **Step 75:** [[Quicksort Partition Loop Complete Optimization Example]] *(Example)*
- [ ] **Step 76:** [[Problem — Quicksort Loop Induction Variable Strength Reduction]] *(Problem `Q-CSE309-009`)*

---

# Syllabus & Knowledge Graph Mapping

## 1. Compiler Architecture & Language Processors
- [[Language Processing System and Translators]]
- [[Structure and Phases of a Compiler]]

## 2. Lexical Analysis
- [[Lexical Analysis Fundamentals]]
- [[Input Buffering and Sentinel Techniques]]
- [[Specification and Recognition of Tokens]]
- [[Lexical Analyzer Generation with Lex]]
- [[Token Recognition and Lex Specification Example]]
- [[Problem — Lexical Token Recognition and Buffer Simulation]]

## 3. Syntax Analysis
- [[Context-Free Grammars and Syntax Analysis]]
- [[Elimination of Left Recursion and Left Factoring Algorithms]]
- [[FIRST and FOLLOW Sets Formulas]]
- [[Top-Down and Predictive Parsing]]
- [[LL(1) Parsing Table Construction and Execution Algorithm]]
- [[LL(1) Parser Construction and Simulation Example]]
- [[Problem — LL(1) Grammar Verification and Parsing Table Construction]]
- [[Bottom-Up Parsing, Reductions, and Handles]]
- [[Shift-Reduce Parsing and Parsing Conflicts]]
- [[LR(0) Items and the LR(0) Automaton]]
- [[SLR(1) Parsing Table Construction and LR Parser Algorithm]]
- [[SLR(1) Automaton and Parsing Table Construction Example]]
- [[Problem — SLR(1) Table Construction and Conflict Analysis]]

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

- [[cse309/01 - Sources/Lectures/Rumi Sir Merged.pdf|Rumi Sir Merged.pdf]] (Lectures by Ahmed Mahir Sultan Rumi, Part I, covering Chapters 1, 3, 4: Compiler Architecture, Lexical Analysis, Top-Down and Bottom-Up Syntax Analysis)
- [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]] (Lectures by Khaled Mahmud Shahriar, Part II, covering Chapters 5–9 and Register Allocation)
- **Course Textbook:** Alfred V. Aho, Monica S. Lam, Ravi Sethi, Jeffrey D. Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Edition), Pearson/Addison Wesley, 2007.

---

# Knowledge Navigation & Revision

- **Knowledge Organization:** [[Topic Map]]
- **Dependency Flow:** [[Dependency Map]]
- **Rapid Revision:** [[Quick Revision]]
- **Problem Bank:** [[Question Bank]]
