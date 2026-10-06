---
type: concept
course: cse309
status: active
order: 2
---

# Synthesized and Inherited Attributes

> 📖 **Reading Order:** Step 02 of 55 | **Module 1: Syntax-Directed Translation**  
> ◄ **Previous:** [[Syntax-Directed Definitions and Translation Schemes]] | ► **Next:** [[S-Attributed and L-Attributed SDDs]]

---

## Starting Point and the Problem

To truly feel why we have two distinct types of attributes, consider how information naturally flows in any human hierarchy—such as a large organization:

```mermaid
flowchart TD
    subgraph UpwardFlow ["1. Bottom-Up Information Flow (Synthesized)"]
        direction BT
        Worker1["Worker 1 (Value: 2)"] --> Manager["Manager (Computes 2 + 3)"]
        Worker2["Worker 2 (Value: 3)"] --> Manager
        Manager --> VP["VP / Root (Calculates Total: 5)"]
    end

    subgraph DownwardFlow ["2. Top-Down & Lateral Information Flow (Inherited)"]
        direction TB
        CEO["CEO: 'The type for this declaration is FLOAT'"] --> DeptLead["Dept Lead (Holds type 'float')"]
        DeptLead -->|"Passes context to right"| Emp1["Variable 'x' (Receives 'float')"]
        Emp1 -->|"Passes context to right"| Emp2["Variable 'y' (Receives 'float')"]
        Emp2 -->|"Passes context to right"| Emp3["Variable 'z' (Receives 'float')"]
    end
```

### The Two Computational Needs of a Programming Language:
1. **Evaluating Expressions (Bottom-Up):**
   When you write `x = 2 + 3 * 4`, the value of `3 * 4` must be computed *before* you can add `2`. The smaller components report their results up to the larger components. This is **Synthesis** (bottom-up aggregation).
2. **Propagating Context & Types (Top-Down & Sideways):**
   When you write `float a, b, c;`, look at the token `float`. It appears once, at the far left. The identifiers `a`, `b`, and `c` have no idea what data type they are supposed to be! They cannot "synthesize" their type from below because they have no children. They must **inherit** their data type from the context established to their left. This is **Inheritance** (top-down and lateral context sharing).

---

## Developing the Idea

Let $A \longrightarrow X_1 X_2 \dots X_n$ be a production in a context-free grammar.

```mermaid
flowchart TD
    subgraph Synthesized ["Synthesized Attribute: A.s"]
        direction BT
        X1["Child X1"] --> A1["Parent Head: A.s"]
        X2["Child X2"] --> A1
        Xn["Child Xn"] --> A1
        A_other["Parent A's other attributes"] -.-> A1
    end

    subgraph Inherited ["Inherited Attribute: Xj.i"]
        direction TB
        ParentA["Parent Head: A.inh"] --> TargetXj["Child Body: Xj.i"]
        SiblingLeft["Left Siblings: X1, ..., Xj-1"] --> TargetXj
        Xj_other["Xj's other attributes"] -.-> TargetXj
    end
```

### 1. Synthesized Attribute
An attribute $A.s$ associated with the head of a production $A$ is **synthesized** if its value is defined by a semantic rule:
$$A.s = f(X_1.a_1, X_2.a_2, \dots, X_n.a_n, A.a_{\text{other}})$$
- **Defined at:** The **head (LHS)** of the production ($A$).
- **Depends on:** The attribute values of the grammar symbols $X_1, \dots, X_n$ on the **body (RHS)**, and potentially other attributes of $A$ itself.
- **Terminal Symbols:** Terminals can have synthesized attributes, but they are computed **exclusively by the Lexical Analyzer** (e.g., `digit.lexval = 5`, `id.name = 'count'`).

---

### 2. Inherited Attribute
An attribute $X_j.i$ associated with a body symbol $X_j$ ($1 \le j \le n$) is **inherited** if its value is defined by a semantic rule:
$$X_j.i = f(A.a, X_1.a_1, X_2.a_2, \dots, X_{j-1}.a_{j-1}, X_j.a_{\text{other}})$$
- **Defined at:** A **body symbol (RHS)** of the production ($X_j$).
- **Depends on:** The inherited attributes of the **head non-terminal $A$**, and the attributes of the **sibling symbols** appearing to the left or right of $X_j$.
- **Terminals CANNOT have inherited attributes!** Why? (See the Lexer Factory Proof below).

---

## Definition

**Synthesized and Inherited Attributes** is a formal compiler mechanism that structures syntax-directed translation, intermediate representations, runtime environments, or code generation.

---

## How It Works

### Architectural Comparison: Synthesized vs. Inherited

| Feature | Synthesized Attributes | Inherited Attributes |
| :--- | :--- | :--- |
| **Direction of Flow** | Strictly **Bottom-Up** (Children $\to$ Parent) | **Top-Down & Lateral** (Parent / Siblings $\to$ Child) |
| **Associated Symbols** | Non-terminals AND Terminals (from lexer) | Non-terminals **ONLY** |
| **Parsing Harmony** | Natural fit for **Bottom-Up (LR) Parsing** | Natural fit for **Top-Down (LL) Parsing** |
| **LR Stack Mechanism** | Automatic: attribute lives on stack, reduced upon match | Requires **Marker Non-Terminals** or stack offset indexing |
| **Primary Use Cases** | Evaluating math, building AST nodes, computing sizes | Propagating types, scope resolution, passing loop break labels |
| **Cycle Vulnerability** | Impossible on its own (tree has finite depth) | High risk if dependencies flow right-to-left or circularly |

---

## Example

Detailed walkthroughs and traces are provided in the corresponding example and problem notes.

---

## Technical Details

Target architecture and ABI specifications govern low-level alignment and register assignments.

---

## Important Properties and Why They Hold

- **Semantic Soundness:** Preserves program execution equivalence.
- **Algorithmic Efficiency:** Operates in low polynomial or linear time over the program structure.

---

## Common Mistakes

### The Danger Zone: Circular Dependency Traps

Why must compilers be extremely strict about inherited attributes? 

Because the moment you allow an attribute to inherit from *any* sibling, you can write:
```
Production: S -> A B
Rules:      A.inh = B.syn + 1
            B.inh = A.syn + 2
            A.syn = A.inh * 3
            B.syn = B.inh * 4
```

Look at the dependency graph:
$$A.inh \longrightarrow A.syn \longrightarrow B.inh \longrightarrow B.syn \longrightarrow A.inh$$

It's a continuous, unbreakable loop! 
- $A.inh$ cannot be computed without $B.syn$.
- $B.syn$ cannot be computed without $B.inh$.
- $B.inh$ cannot be computed without $A.syn$.
- $A.syn$ cannot be computed without $A.inh$.

This is why the compiler community invented the **L-Attributed restriction**: information is permitted to flow **ONLY from left to right**, completely making cycles impossible!

---

## Exam Relevance

Tested regularly in compiler examinations via syntax-directed translation proofs, activation record diagrams, and control flow optimization problems.

---

## Related Concepts

- [[S-Attributed and L-Attributed SDDs]]
- [[Abstract Syntax Tree Construction with SDDs]]

---

## Prerequisites

- [[Syntax-Directed Definitions and Translation Schemes]]

---

## Problems

- [[Problem — Desk Calculator SDD and Annotated Parse Tree]]

---

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Chapter 5 (Slides 11–24).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 5.1 (Synthesized and Inherited Attributes).
