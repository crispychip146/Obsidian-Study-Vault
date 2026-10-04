---
type: concept
course: cse309
status: active
order: 3
---

# S-Attributed and L-Attributed SDDs

> 📖 **Reading Order:** Step 3 of 55 | **Module 1:** Syntax-Directed Translation  
> ◄ **Previous:** [[Synthesized and Inherited Attributes]] | ► **Next:** [[Abstract Syntax Tree Construction with SDDs]]

---

---

---

---

---

## Starting Point and the Problem

In theoretical computer science, you can define attributes however you like. But in real-world compiler engineering, you face a brutal practical constraint:

> **You cannot afford to build a 10-million-node concrete parse tree in RAM and perform multiple slow traversal passes over it.**

A production compiler wants to compile **on-the-fly in a single pass**, as tokens stream off the disk or network.

When a parser is running:
- An **LR (Bottom-Up) parser** maintains a stack of symbols.
- An **LL (Top-Down) recursive-descent parser** executes a hierarchy of function calls.

How can semantic rules execute naturally within these parsing engines without freezing, crashing, or running in circles?

Compiler designers solved this by defining two mathematically guaranteed, cycle-free classes of Syntax-Directed Definitions:
1. **S-Attributed Definitions:** Designed for **Bottom-Up LR Parsing Stack** execution.
2. **L-Attributed Definitions:** Designed for **Top-Down LL Recursive-Descent** execution.

---

---

---

---

---

## Developing the Idea

### Formal Definition:
An SDD is **S-attributed** if **every single attribute** of every grammar symbol is a **[[Synthesized and Inherited Attributes|Synthesized Attribute]]**. There are zero inherited attributes.

```mermaid
flowchart BT
    subgraph S_Attributed ["S-Attributed Execution: Native LR Parser Stack Reduction"]
        direction BT
        Child1["val[top-2]: Child X"] --> Rule["Semantic Rule executes upon REDUCE"]
        Child2["val[top-1]: Child Y"] --> Rule
        Child3["val[top]:   Child Z"] --> Rule
        Rule --> Parent["val[top-2]: Parent Head A"]
    end
```

### Why S-Attributed is Pure Engineering Elegance:
Consider an LR parser (like Yacc or Bison). When an LR parser recognizes the right-hand side of a production $A \longrightarrow X \; Y \; Z$, where are the symbols $X, Y, Z$?

**They are sitting right on top of the parser stack!**

The compiler expands each stack slot to hold a pair: `(state, val)`.
When the parser performs a reduction by $A \to X \; Y \; Z$:
1. The values of $X, Y, Z$ are located at `val[top - 2]`, `val[top - 1]`, and `val[top]`.
2. The semantic action fires immediately:
   $$\text{temp} = \text{val}[top - 2] + \text{val}[top]$$
3. The parser pops 3 entries off the stack and pushes $A$ with attribute `temp`.
4. **Zero parse tree in RAM. Zero extra traversal passes.** The entire computation finishes on-the-fly in a single bottom-up sweep!

---

---

---

---

---

## Definition

### The Intuition: What Does the "L" Stand For?
The letter **"L" stands for Left-to-Right**.

Think about how a human reads code, or how a recursive-descent LL(1) parser executes. It moves strictly from left to right. When the parser is currently examining the $j$-th symbol $X_j$ in a production:
$$A \longrightarrow X_1 \; X_2 \; \dots \; X_{j-1} \; \mathbf{X_j} \; X_{j+1} \; \dots \; X_n$$
- It has already visited the parent $A$.
- It has already completely parsed the sibling symbols to the left: $X_1, X_2, \dots, X_{j-1}$.
- It has **NEVER SEEN** the symbols to the right: $X_{j+1}, \dots, X_n$. They exist only in the unread future!

Therefore, if $X_j$ needs inherited context to do its job, where can that context legally come from?
- It can come from the parent $A$.
- It can come from its older siblings to the left ($X_1 \dots X_{j-1}$).
- It **CAN NEVER** come from its unborn siblings to the right ($X_{j+1} \dots X_n$)!

```mermaid
flowchart TD
    ParentA["Parent A (Inherited)"] -->|"LEGAL: Context from above"| TargetXj["Target Symbol Xj (Inherited)"]
    LeftSib["Left Siblings: X1, ..., Xj-1 (All Attributes)"] -->|"LEGAL: Information from the past"| TargetXj
    RightSib["Right Siblings: Xj+1, ..., Xn"] -.->|"ILLEGAL! Cannot depend on the unparsed future!"| TargetXj
    ParentAsyn["Parent A (Synthesized)"] -.->|"ILLEGAL! Creates parent-child circular trap!"| TargetXj
```

### Formal Definition:
An SDD is **L-attributed** if each attribute of every grammar symbol is either:
1. A **synthesized attribute**, OR
2. An **inherited attribute** such that, for every production:
   $$A \longrightarrow X_1 X_2 \dots X_n$$
   the inherited attribute $X_j.i$ depends **only** on:
   - The inherited attributes of the head $A$.
   - The attributes (synthesized or inherited) of the symbols $X_1, X_2, \dots, X_{j-1}$ appearing to the **left** of $X_j$.
   - Attributes of $X_j$ itself, provided that no cycles are created between attributes of $X_j$.

> [!CAUTION] The Subtle Restriction on Parent Attributes
> Notice carefully: $X_j.i$ can depend on **inherited** attributes of $A$ ($A.inh$), but **NOT on synthesized attributes of $A$ ($A.syn$)**! Why? Because $A.syn$ is computed from its children. If child $X_j$ depended on $A.syn$, you would create an immediate circular dependency: $X_j$ needs $A.syn$, but $A.syn$ needs $X_j$!

---

---

---

---

---

## How It Works

### How It Works

### How It Works

### How It Works

### The Recursive-Descent Mapping (Why L-Attributed Feels Natural)

In a top-down recursive-descent parser, every non-terminal symbol $X$ is written as a programming language function: `parse_X()`.

How do attributes map into code? It is a thing of pure beauty:
- **Inherited Attributes** become **Input Arguments** to the function!
- **Synthesized Attributes** become **Return Values** of the function!

```python
# Production: A -> X Y
# Semantic Rules:
#   X.inh = A.inh + 1        (Inherited: passed as argument to X)
#   Y.inh = X.syn * 2        (Inherited: depends on left sibling X's synthesized value!)
#   A.syn = Y.syn + 5        (Synthesized: returned to caller)

def parse_A(A_inh):
    # 1. Compute X's inherited attribute from our argument
    X_inh = A_inh + 1
    
    # 2. Call left child function, passing X_inh, and capture its synthesized return value
    X_syn = parse_X(X_inh)
    
    # 3. Compute Y's inherited attribute using X's synthesized result!
    Y_inh = X_syn * 2
    
    # 4. Call right child function, passing Y_inh, and capture its synthesized return value
    Y_syn = parse_Y(Y_inh)
    
    # 5. Synthesize our own return value
    A_syn = Y_syn + 5
    return A_syn
```

Look at that code. Does it feel natural? **It is impossible to write this function if $X$ depended on $Y$**, because line 2 would require `Y_syn`, which hasn't been called yet! That is why L-attribution is the natural mathematical law of top-down computing.

---
### The Fundamental Subset Hierarchy

```mermaid
flowchart TD
    All["All Possible SDDs (Arbitrary Dependencies - NP-Complete Cycle Risk)"]
    L["L-Attributed SDDs (Cycle-Free · Top-Down Compatible)"]
    S["S-Attributed SDDs (Cycle-Free · Pure Bottom-Up LR Stack)"]
    
    All --> L
    L --> S
```

> **Theorem:** $\text{S-Attributed SDDs} \subset \text{L-Attributed SDDs}$.  
> **Every S-attributed definition is trivially an L-attributed definition!**  
> *Proof:* An S-attributed definition contains *zero* inherited attributes. Since it has no inherited attributes, the restrictions on inherited dependencies are vacuously satisfied for all productions. $\blacksquare$

---
### Summary Comparison for Quick Recall

| Metric | S-Attributed SDD | L-Attributed SDD |
| :--- | :--- | :--- |
| **Allowed Attributes** | Synthesized **ONLY** | Synthesized **AND** Restricted Inherited |
| **Information Flow** | Bottom-up (Leaves $\to$ Root) | Bottom-up **AND** Left-to-Right |
| **Underlying Parser Type** | **Bottom-Up LR Parsers** (SLR, LALR, LR(1)) | **Top-Down LL Parsers** (LL(1), Recursive Descent) |
| **Implementation Vehicle** | Parser Stack values during reduction | Function parameters (inh) and return values (syn) |
| **LR Compatibility** | $100\%$ native (zero code modifications) | Requires marker non-terminals $\epsilon$ and stack offset tricks |

---

---
### Technical Details

Target architecture and ABI specifications govern low-level alignment and register assignments.

---
### Important Properties and Why They Hold

### The Mathematical Proof of Cycle-Freeness

> **Theorem:** Every L-attributed Syntax-Directed Definition is guaranteed to be **acyclic** (contains no circular dependencies for ANY parse tree).
>
> **Proof:**  
> We prove that a valid topological evaluation order always exists by constructing a depth-first, left-to-right traversal order that evaluates every attribute instance:
> 1. For any node $N$ labeled with non-terminal $A$:
>    - When control first arrives at $N$ from its parent, all inherited attributes $A.inh$ have already been computed by the parent.
> 2. The traversal visits the children $X_1, X_2, \dots, X_n$ strictly in order from left to right ($1 \le j \le n$):
>    - Before descending into child $X_j$, the rule for $X_j.i$ evaluates. By the L-attributed definition, $X_j.i$ depends only on $A.inh$ (already evaluated in Step 1) and attributes of $X_1, \dots, X_{j-1}$ (already fully evaluated because those subtrees were completely traversed).
>    - Thus, all arguments for $X_j.i$ are strictly available.
>    - The subtree at $X_j$ is recursively traversed, evaluating all its internal attributes and its synthesized attributes $X_j.syn$.
> 3. After all children $X_1 \dots X_n$ have returned, the synthesized attributes $A.syn$ evaluate. Their operands are attributes of children $X_1 \dots X_n$, which are all fully computed.
> 
> Because this order visits every attribute instance and every dependency points backwards to an already-evaluated attribute, no directed cycle can exist. $\blacksquare$

---

---
### Related Concepts

- [[Syntax-Directed Definitions and Translation Schemes]]
- [[Intermediate Representations and Three-Address Code]]
- [[Basic Blocks and Control Flow Graphs]]

---
### Prerequisites

- [[Syntax-Directed Definitions and Translation Schemes]]

---
### Problems

- [[Problem — Desk Calculator SDD and Annotated Parse Tree]]
- [[Problem — Array Reference Three-Address Code Generation]]

---

---
### Technical Details

Target architecture and ABI specifications govern low-level alignment and register assignments.

---
### Important Properties and Why They Hold

- **Semantic Soundness:** Preserves program execution equivalence.
- **Algorithmic Efficiency:** Operates in low polynomial or linear time over the program structure.

---
### Related Concepts

- [[Syntax-Directed Definitions and Translation Schemes]]
- [[Intermediate Representations and Three-Address Code]]
- [[Basic Blocks and Control Flow Graphs]]

---
### Prerequisites

- [[Syntax-Directed Definitions and Translation Schemes]]

---
### Problems

- [[Problem — Desk Calculator SDD and Annotated Parse Tree]]
- [[Problem — Array Reference Three-Address Code Generation]]

---

---
### Technical Details

Target architecture and ABI specifications govern low-level alignment and register assignments.

---
### Important Properties and Why They Hold

- **Semantic Soundness:** Preserves program execution equivalence.
- **Algorithmic Efficiency:** Operates in low polynomial or linear time over the program structure.

---
### Related Concepts

- [[Syntax-Directed Definitions and Translation Schemes]]
- [[Intermediate Representations and Three-Address Code]]
- [[Basic Blocks and Control Flow Graphs]]

---
### Prerequisites

- [[Syntax-Directed Definitions and Translation Schemes]]

---
### Problems

- [[Problem — Desk Calculator SDD and Annotated Parse Tree]]
- [[Problem — Array Reference Three-Address Code Generation]]

---

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

### Common Mistakes

### Common Mistakes

### Common Mistakes

- Confusing syntactic validity with semantic correctness.
- Overlooking variable scoping or memory aliasing side effects.

---

---

---

---

## Exam Relevance

### Example

Detailed walkthroughs and traces are provided in the corresponding example and problem notes.

---
### Exam Relevance

### Example

Detailed walkthroughs and traces are provided in the corresponding example and problem notes.

---
### Exam Relevance

### Example

Detailed walkthroughs and traces are provided in the corresponding example and problem notes.

---
### Exam Relevance

Tested regularly in compiler examinations via syntax-directed translation proofs, activation record diagrams, and control flow optimization problems.

---

---

---

---

## Related Concepts

- [[Syntax-Directed Definitions and Translation Schemes]]
- [[Intermediate Representations and Three-Address Code]]
- [[Basic Blocks and Control Flow Graphs]]

---

## Prerequisites

- [[Syntax-Directed Definitions and Translation Schemes]]

---

## Problems

- [[Problem — Desk Calculator SDD and Annotated Parse Tree]]
- [[Problem — Array Reference Three-Address Code Generation]]

---

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Chapter 5 (Slides 31–40).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 5.2.
