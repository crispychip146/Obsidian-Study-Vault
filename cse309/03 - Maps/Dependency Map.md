# CSE 309 — Dependency Map (Complete Curriculum)

This dependency map visualizes the conceptual prerequisites, algorithmic foundations, and learning pathways across all ten modules of CSE 309 (Compilers), spanning Part I (Analysis / Front End) and Part II (Synthesis / Back End).

---

## 1. High-Level Modular Learning Flow

```mermaid
flowchart TD
    M1["Module 1: Compiler Architecture & Language Processors<br/>(Steps 01 - 02)"]
    M2["Module 2: Lexical Analysis<br/>(Steps 03 - 08)"]
    M3["Module 3: Syntax Analysis & Top-Down Parsing<br/>(Steps 09 - 15)"]
    M4["Module 4: Bottom-Up Parsing & LR Parsers<br/>(Steps 16 - 21)"]
    M5["Module 5: Syntax-Directed Translation (SDT)<br/>(Steps 22 - 30)"]
    M6["Module 6: Intermediate Code Generation (ICG)<br/>(Steps 31 - 41)"]
    M7["Module 7: Run-Time Environments (RTE)<br/>(Steps 42 - 52)"]
    M8["Module 8: Code Generation (CG)<br/>(Steps 53 - 63)"]
    M9["Module 9: Register Allocation (RA)<br/>(Steps 64 - 71)"]
    M10["Module 10: Machine-Independent Optimization (OPT)<br/>(Steps 72 - 76)"]

    M1 --> M2
    M1 --> M3
    M2 --> M3
    M3 --> M4
    M3 --> M5
    M4 --> M5
    M5 --> M6
    M6 --> M7
    M6 --> M8
    M7 --> M8
    M8 --> M9
    M8 --> M10
    M10 --> M9
```

---

## 2. Detailed Topic Dependency Graph

```mermaid
flowchart TD
    %% Module 1: Architecture
    LANG_PROC["Language Processing System & Translators"] --> PHASES["Structure & Phases of a Compiler"]

    %% Module 2: Lexical Analysis
    PHASES --> LEX_FUND["Lexical Analysis Fundamentals"]
    LEX_FUND --> BUFF["Input Buffering & Sentinel Techniques"]
    LEX_FUND --> RECOG["Specification & Recognition of Tokens"]
    RECOG --> LEX_TOOL["Lexical Analyzer Generation with Lex"]
    RECOG --> EX_LEX["Token Recognition & Lex Example"]
    BUFF --> PROB_LEX["Problem — Lexical Buffer Simulation (Q-010)"]
    RECOG --> PROB_LEX

    %% Module 3: Syntax Analysis & Top-Down
    PHASES --> CFG["Context-Free Grammars & Syntax Analysis"]
    CFG --> LREC_ALG["Elimination of Left Recursion & Factoring"]
    CFG --> FIRST_FOLLOW["FIRST and FOLLOW Sets Formulas"]
    LREC_ALG --> TOP_DOWN["Top-Down and Predictive Parsing"]
    FIRST_FOLLOW --> TOP_DOWN
    TOP_DOWN --> LL1_ALG["LL(1) Parsing Table Construction Algorithm"]
    FIRST_FOLLOW --> LL1_ALG
    LL1_ALG --> EX_LL1["LL(1) Parser Construction Example"]
    LL1_ALG --> PROB_LL1["Problem — LL(1) Grammar Verification (Q-011)"]

    %% Module 4: Bottom-Up Parsing & LR
    CFG --> BU_PARS["Bottom-Up Parsing, Reductions, & Handles"]
    BU_PARS --> SR_PARS["Shift-Reduce Parsing & Conflicts"]
    BU_PARS --> LR0_ITEM["LR(0) Items and the LR(0) Automaton"]
    LR0_ITEM --> SLR1_ALG["SLR(1) Parsing Table Construction Algorithm"]
    FIRST_FOLLOW --> SLR1_ALG
    SLR1_ALG --> EX_SLR1["SLR(1) Automaton Example"]
    SLR1_ALG --> PROB_SLR1["Problem — SLR(1) Table Construction (Q-012)"]

    %% Module 5: SDT
    CFG --> SDD["Syntax-Directed Definitions & Schemes"]
    LL1_ALG --> SDD
    SLR1_ALG --> SDD
    SDD --> ATTR["Synthesized & Inherited Attributes"]
    ATTR --> CLASS["S-Attributed & L-Attributed SDDs"]
    CLASS --> AST["AST Construction with SDDs"]
    CLASS --> LREC["Eliminating Left Recursion from SDTs"]
    CLASS --> BU_EVAL["Bottom-Up L-Attributed Evaluation"]
    ATTR --> EX_CALC["Desk Calculator SDD Example"]
    LREC --> EX_INFIX["Infix to Postfix/Prefix Example"]
    EX_CALC --> PROB_CALC["Problem — Desk Calculator SDD (Q-001)"]

    %% Module 6: ICG
    SDD --> IR_TAC["Intermediate Representations & Three-Address Code"]
    IR_TAC --> VAL_NUM["Value-Number Method for DAGs"]
    IR_TAC --> TYPES["Type Expressions & Storage Layout"]
    TYPES --> FORM_ARR["Multi-Dimensional Array Addressing Formulas"]
    FORM_ARR --> TRANS_EXPR["Translation of Expressions & Arrays"]
    IR_TAC --> TRANS_CTRL["Control Flow Translation & Booleans"]
    TRANS_CTRL --> BACKPATCH["Backpatching in ICG"]
    BACKPATCH --> ALG_BACK["Backpatching Algorithm"]
    TRANS_EXPR --> EX_TAC["Array & Control-Flow TAC Example"]
    TRANS_CTRL --> EX_TAC
    TRANS_EXPR --> PROB_ARR["Problem — Array TAC Generation (Q-002)"]
    ALG_BACK --> PROB_BP["Problem — Backpatching Translation (Q-003)"]

    %% Module 7: RTE
    TYPES --> STORAGE["Run-Time Storage Organization & AR"]
    STORAGE --> CALL_SEQ["Calling Sequences & Frame Management"]
    STORAGE --> NON_LOCAL["Non-Local Access in Static/Dynamic Scopes"]
    STORAGE --> HEAP["Heap Memory Management & Allocation"]
    HEAP --> GC_FUND["Garbage Collection Fundamentals & Refcounting"]
    GC_FUND --> TRACE_GC["Trace-Based GC Algorithms"]
    TRACE_GC --> MS_GC["Mark-and-Sweep GC Algorithm"]
    TRACE_GC --> COPY_GC["Copying GC Algorithm (Cheney)"]
    NON_LOCAL --> EX_DISP["Display Simulation Example"]
    TRACE_GC --> EX_GC["GC Trace & Compaction Example"]
    NON_LOCAL --> PROB_AR["Problem — AR & Display Tracing (Q-004)"]

    %% Module 8: Code Generation
    IR_TAC --> CG_ISSUES["Code Generation Issues & Architecture"]
    CG_ISSUES --> BB_CFG["Basic Blocks & Control Flow Graphs"]
    BB_CFG --> ALG_LEAD["Basic Block Partitioning Algorithm"]
    BB_CFG --> LIVE_NEXT["Liveness & Next-Use Analysis"]
    VAL_NUM --> DAG_OPT["DAG Construction & Local Optimization"]
    BB_CFG --> DAG_OPT
    LIVE_NEXT --> SIMPLE_CG["A Simple Code Generator Algorithm"]
    CG_ISSUES --> PEEPHOLE["Peephole Optimization Techniques"]
    ALG_LEAD --> EX_BB["Block Partitioning & Next-Use Example"]
    LIVE_NEXT --> EX_BB
    DAG_OPT --> EX_DAG["DAG Basic Block Optimization Example"]
    ALG_LEAD --> PROB_BB["Problem — Basic Block Partitioning (Q-005)"]
    DAG_OPT --> PROB_DAG["Problem — DAG Array Store (Q-006)"]

    %% Module 9: Register Allocation
    LIVE_NEXT --> LIVE_INT["Live Ranges & Live Intervals"]
    LIVE_INT --> RIG["Register Interference Graphs & Coloring"]
    LIVE_INT --> LIN_SCAN["Linear Scan Register Allocation Algorithm"]
    RIG --> CHAITIN["Chaitin's Graph Coloring Algorithm"]
    LIN_SCAN --> EX_LS["Linear Scan Step-by-Step Example"]
    CHAITIN --> EX_CHAITIN["Chaitin Graph Coloring Example"]
    LIN_SCAN --> PROB_LS["Problem — Linear Scan Simulation (Q-007)"]
    CHAITIN --> PROB_CHAITIN["Problem — Chaitin Graph Coloring (Q-008)"]

    %% Module 10: Machine-Independent Optimization
    BB_CFG --> OPT_SRC["Principal Sources of Optimization"]
    OPT_SRC --> CSE_CP["Global CSE & Copy Propagation"]
    OPT_SRC --> LOOP_OPT["Loop Optimizations & Strength Reduction"]
    CSE_CP --> EX_QS["Quicksort Partition Loop Complete Example"]
    LOOP_OPT --> EX_QS
    LOOP_OPT --> PROB_SR["Problem — Quicksort Strength Reduction (Q-009)"]
```

---

## 3. Prerequisite Matrix

| Knowledge Note | Direct Prerequisites | Unlocks / Enables |
| :--- | :--- | :--- |
| [[Structure and Phases of a Compiler]] | [[Language Processing System and Translators]] | [[Lexical Analysis Fundamentals]], [[Context-Free Grammars and Syntax Analysis]] |
| [[Lexical Analysis Fundamentals]] | [[Structure and Phases of a Compiler]] | [[Input Buffering and Sentinel Techniques]], [[Specification and Recognition of Tokens]] |
| [[Specification and Recognition of Tokens]] | [[Lexical Analysis Fundamentals]] | [[Lexical Analyzer Generation with Lex]], [[Token Recognition and Lex Specification Example]], [[Problem — Lexical Token Recognition and Buffer Simulation]] |
| [[Context-Free Grammars and Syntax Analysis]] | [[Structure and Phases of a Compiler]] | [[Elimination of Left Recursion and Left Factoring Algorithms]], [[FIRST and FOLLOW Sets Formulas]], [[Bottom-Up Parsing, Reductions, and Handles]] |
| [[FIRST and FOLLOW Sets Formulas]] | [[Context-Free Grammars and Syntax Analysis]] | [[LL(1) Parsing Table Construction and Execution Algorithm]], [[SLR(1) Parsing Table Construction and LR Parser Algorithm]] |
| [[LL(1) Parsing Table Construction and Execution Algorithm]] | [[FIRST and FOLLOW Sets Formulas]], [[Top-Down and Predictive Parsing]] | [[LL(1) Parser Construction and Simulation Example]], [[Problem — LL(1) Grammar Verification and Parsing Table Construction]], [[Syntax-Directed Definitions and Translation Schemes]] |
| [[LR(0) Items and the LR(0) Automaton]] | [[Bottom-Up Parsing, Reductions, and Handles]], [[Shift-Reduce Parsing and Parsing Conflicts]] | [[SLR(1) Parsing Table Construction and LR Parser Algorithm]] |
| [[SLR(1) Parsing Table Construction and LR Parser Algorithm]] | [[LR(0) Items and the LR(0) Automaton]], [[FIRST and FOLLOW Sets Formulas]] | [[SLR(1) Automaton and Parsing Table Construction Example]], [[Problem — SLR(1) Table Construction and Conflict Analysis]], [[Syntax-Directed Definitions and Translation Schemes]] |
| [[Synthesized and Inherited Attributes]] | [[Syntax-Directed Definitions and Translation Schemes]] | [[S-Attributed and L-Attributed SDDs]], [[Arithmetic Expression Desk Calculator SDD Example]] |
| [[S-Attributed and L-Attributed SDDs]] | [[Synthesized and Inherited Attributes]] | [[Abstract Syntax Tree Construction with SDDs]], [[Eliminating Left Recursion from SDTs]], [[Bottom-Up Evaluation of L-Attributed SDDs]] |
| [[Intermediate Representations and Three-Address Code]] | [[Syntax-Directed Definitions and Translation Schemes]] | [[Value-Number Method for DAG Construction]], [[Type Expressions and Storage Layout]], [[Control Flow Translation and Boolean Expressions]] |
| [[Multi-Dimensional Array Addressing Formulas]] | [[Type Expressions and Storage Layout]] | [[Translation of Expressions and Array References]], [[Problem — Array Reference Three-Address Code Generation]] |
| [[Backpatching in Intermediate Code Generation]] | [[Control Flow Translation and Boolean Expressions]] | [[Backpatching Control-Flow Code Generation Algorithm]], [[Problem — Backpatching Boolean Expression Translation]] |
| [[Run-Time Storage Organization and Activation Records]] | [[Type Expressions and Storage Layout]] | [[Calling Sequences and Stack Frame Management]], [[Non-Local Variable Access in Static and Dynamic Scopes]], [[Heap Memory Management and Allocation Strategies]] |
| [[Non-Local Variable Access in Static and Dynamic Scopes]] | [[Run-Time Storage Organization and Activation Records]] | [[Display Maintenance and Non-Local Access Simulation Example]], [[Problem — Activation Record and Display Table Tracing]] |
| [[Trace-Based Garbage Collection Algorithms]] | [[Garbage Collection Fundamentals and Reference Counting]] | [[Mark-and-Sweep Garbage Collection Algorithm]], [[Copying Garbage Collection Algorithm]], [[Garbage Collection Trace and Compaction Example]] |
| [[Basic Blocks and Control Flow Graphs]] | [[Intermediate Representations and Three-Address Code]] | [[Basic Block Partitioning Algorithm]], [[Liveness and Next-Use Analysis within Basic Blocks]], [[Principal Sources of Code Optimization]] |
| [[Liveness and Next-Use Analysis within Basic Blocks]] | [[Basic Blocks and Control Flow Graphs]] | [[A Simple Code Generator Algorithm]], [[Live Ranges and Live Intervals in Register Allocation]] |
| [[Linear Scan Register Allocation Algorithm]] | [[Live Ranges and Live Intervals in Register Allocation]] | [[Linear Scan Register Allocation Step-by-Step Example]], [[Problem — Linear Scan Register Allocation Simulation]] |
| [[Chaitin's Graph Coloring Register Allocation Algorithm]] | [[Register Interference Graphs and Graph Coloring Principles]] | [[Chaitin's Graph Coloring Register Allocation Example]], [[Problem — Chaitin Graph Coloring Register Allocation]] |
| [[Loop Optimizations and Strength Reduction]] | [[Principal Sources of Code Optimization]] | [[Quicksort Partition Loop Complete Optimization Example]], [[Problem — Quicksort Loop Induction Variable Strength Reduction]] |
