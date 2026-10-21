---
type: problem
course: cse309
status: active
order: 15
---

# Problem — LL(1) Grammar Verification and Parsing Table Construction

## Problem Statement

### Context
A language designer proposes the following Context-Free Grammar $G$ for a command configuration language:
$$\begin{aligned}
S &\to a B D h \\
B &\to c C \\
C &\to b C \mid \epsilon \\
D &\to E F \\
E &\to g \mid \epsilon \\
F &\to f \mid \epsilon
\end{aligned}$$
where $\{S, B, C, D, E, F\}$ are nonterminals, $\{a, b, c, f, g, h\}$ are terminals, and $S$ is the start symbol.

---

### Question 1: FIRST and FOLLOW Set Derivations
1. Calculate the $\text{FIRST}$ set for every nonterminal: $\text{FIRST}(S)$, $\text{FIRST}(B)$, $\text{FIRST}(C)$, $\text{FIRST}(D)$, $\text{FIRST}(E)$, $\text{FIRST}(F)$.
2. Calculate the $\text{FOLLOW}$ set for every nonterminal: $\text{FOLLOW}(S)$, $\text{FOLLOW}(B)$, $\text{FOLLOW}(C)$, $\text{FOLLOW}(D)$, $\text{FOLLOW}(E)$, $\text{FOLLOW}(F)$. Show the explicit derivation justification for each set.

---

### Question 2: LL(1) Parsing Table Construction and Verification
1. Construct the complete two-dimensional predictive parsing table $M[A, t]$ for all nonterminals $A$ and terminals $t \in \{a, b, c, f, g, h, \text{\$}\}$. Include `synch` entries for error recovery.
2. Based strictly on the constructed table, determine whether grammar $G$ is **LL(1)**. State the precise formal condition.

---

### Question 3: Step-by-Step Parser Execution Trace
Trace the execution of the table-driven LL(1) parsing algorithm on the valid input string:
$$w = a \; c \; b \; g \; f \; h \; \text{\$}$$
Present the trace in a standard 5-column table: Step, Matched Prefix, Parsing Stack (top on right), Current Input, and Action Taken.

---

### Question 4: Panic-Mode Error Recovery Trace
Demonstrate panic-mode error recovery using the table's `synch` entries when presented with the syntactically invalid input:
$$w_{\text{err}} = a \; c \; g \; f \; h \; \text{\$}$$
Explain what error is reported and how the parser recovers to successfully finish analyzing the remainder of the sentence.

---

## Detailed Solution and Walkthrough

### Part 1: FIRST and FOLLOW Calculations

#### 1. $\text{FIRST}$ Set Calculations
- **$E \to g \mid \epsilon$:** Direct base case $\implies \text{FIRST}(E) = \{g, \epsilon\}$.
- **$F \to f \mid \epsilon$:** Direct base case $\implies \text{FIRST}(F) = \{f, \epsilon\}$.
- **$D \to E F$:**
  - $\text{FIRST}(E) \setminus \{\epsilon\} = \{g\} \subseteq \text{FIRST}(D)$.
  - Since $\epsilon \in \text{FIRST}(E)$, we propagate $\text{FIRST}(F) \setminus \{\epsilon\} = \{f\} \subseteq \text{FIRST}(D)$.
  - Since both $\epsilon \in \text{FIRST}(E)$ and $\epsilon \in \text{FIRST}(F)$, $\epsilon \in \text{FIRST}(D)$.
  - $\implies \text{FIRST}(D) = \{g, f, \epsilon\}$.
- **$C \to b C \mid \epsilon$:** Direct base case $\implies \text{FIRST}(C) = \{b, \epsilon\}$.
- **$B \to c C$:** Begins with terminal $c \implies \text{FIRST}(B) = \{c\}$.
- **$S \to a B D h$:** Begins with terminal $a \implies \text{FIRST}(S) = \{a\}$.

**Summary of $\text{FIRST}$ Sets:**
- $\text{FIRST}(S) = \{a\}$
- $\text{FIRST}(B) = \{c\}$
- $\text{FIRST}(C) = \{b, \epsilon\}$
- $\text{FIRST}(D) = \{g, f, \epsilon\}$
- $\text{FIRST}(E) = \{g, \epsilon\}$
- $\text{FIRST}(F) = \{f, \epsilon\}$

---

#### 2. $\text{FOLLOW}$ Set Calculations
- **Start Symbol $S$:**
  - Rule 1: $\$ \in \text{FOLLOW}(S)$. $S$ appears on no right-hand side.
  - $\implies \text{FOLLOW}(S) = \{\$\}$.
- **Nonterminal $D$:**
  - In $S \to a B D h$, the symbol immediately following $D$ is terminal $h$.
  - $\implies \text{FOLLOW}(D) = \{h\}$.
- **Nonterminal $F$:**
  - In $D \to E F$, $F$ is at the right end of the production.
  - By Rule 3: $\text{FOLLOW}(D) \subseteq \text{FOLLOW}(F) \implies h \in \text{FOLLOW}(F)$.
  - $\implies \text{FOLLOW}(F) = \{h\}$.
- **Nonterminal $E$:**
  - In $D \to E F$, $F$ follows $E$.
  - By Rule 2: $(\text{FIRST}(F) \setminus \{\epsilon\}) = \{f\} \subseteq \text{FOLLOW}(E)$.
  - Since $\epsilon \in \text{FIRST}(F)$, by Rule 3: $\text{FOLLOW}(D) \subseteq \text{FOLLOW}(E) \implies h \in \text{FOLLOW}(E)$.
  - $\implies \text{FOLLOW}(E) = \{f, h\}$.
- **Nonterminal $B$:**
  - In $S \to a B D h$, the string following $B$ is $D h$.
  - By Rule 2: $(\text{FIRST}(D) \setminus \{\epsilon\}) = \{g, f\} \subseteq \text{FOLLOW}(B)$.
  - Since $\epsilon \in \text{FIRST}(D)$, the terminal following $D$ is $h \implies h \in \text{FOLLOW}(B)$.
  - $\implies \text{FOLLOW}(B) = \{g, f, h\}$.
- **Nonterminal $C$:**
  - In $B \to c C$, $C$ is at the right end of the production $\implies \text{FOLLOW}(B) \subseteq \text{FOLLOW}(C)$.
  - In $C \to b C$, $C$ is also at the end $\implies \text{FOLLOW}(C) \subseteq \text{FOLLOW}(C)$.
  - $\implies \text{FOLLOW}(C) = \text{FOLLOW}(B) = \{g, f, h\}$.

**Summary of $\text{FOLLOW}$ Sets:**
- $\text{FOLLOW}(S) = \{\$\}$
- $\text{FOLLOW}(B) = \{g, f, h\}$
- $\text{FOLLOW}(C) = \{g, f, h\}$
- $\text{FOLLOW}(D) = \{h\}$
- $\text{FOLLOW}(E) = \{f, h\}$
- $\text{FOLLOW}(F) = \{h\}$

---

### Part 2: Parsing Table Construction and LL(1) Verification

Applying the construction algorithm:
1. $S \to a B D h$: Place in $M[S, a]$.
2. $B \to c C$: Place in $M[B, c]$.
3. $C \to b C$: Place in $M[C, b]$.
   $C \to \epsilon$: $\text{FOLLOW}(C) = \{g, f, h\} \implies$ Place in $M[C, g], M[C, f], M[C, h]$.
4. $D \to E F$: $\text{FIRST}(E F) = \{g, f, \epsilon\}$.
   - Add $D \to E F$ on $\text{FIRST} \setminus \{\epsilon\} = \{g, f\} \implies M[D, g]$ and $M[D, f]$.
   - Since $\epsilon \in \text{FIRST}(E F)$, inspect $\text{FOLLOW}(D) = \{h\} \implies$ Place $D \to E F$ in $M[D, h]$.
5. $E \to g$: Place in $M[E, g]$.
   $E \to \epsilon$: $\text{FOLLOW}(E) = \{f, h\} \implies$ Place in $M[E, f]$ and $M[E, h]$.
6. $F \to f$: Place in $M[F, f]$.
   $F \to \epsilon$: $\text{FOLLOW}(F) = \{h\} \implies$ Place in $M[F, h]$.

### Constructed Parsing Table $M$

| Nonterminal | $a$ | $b$ | $c$ | $f$ | $g$ | $h$ | `$` |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **$S$** | $S \to a B D h$ | | | | | | `synch` |
| **$B$** | | | $B \to c C$ | `synch` | `synch` | `synch` | |
| **$C$** | | $C \to b C$ | | $C \to \epsilon$ | $C \to \epsilon$ | $C \to \epsilon$ | |
| **$D$** | | | | $D \to E F$ | $D \to E F$ | $D \to E F$ | |
| **$E$** | | | | $E \to \epsilon$ | $E \to g$ | $E \to \epsilon$ | |
| **$F$** | | | | $F \to f$ | | $F \to \epsilon$ | |

#### Formal LL(1) Verification
- **Inspection:** We examine every cell of table $M$. No cell contains more than one production rule.
- **Conclusion:** Because table $M$ contains **zero multiply-defined entries (no collisions)**, the grammar $G$ is strictly and provably **LL(1)**!

---

### Part 3: Step-by-Step Simulation Trace on `a c b g f h $`

Stack initialized to `[$, S]`.

| Step | Matched Prefix | Parsing Stack | Input Buffer | Action Taken |
|:---:|:---:|---|---|---|
| **1** | | `$ S` | `a c b g f h $` | $M[S, a]$: pop $S$, push $h D B a$ |
| **2** | | `$ h D B a` | `a c b g f h $` | **Match $a$!** Pop $a$, advance input |
| **3** | `a` | `$ h D B` | `c b g f h $` | $M[B, c]$: pop $B$, push $C c$ |
| **4** | `a` | `$ h D C c` | `c b g f h $` | **Match $c$!** Pop $c$, advance input |
| **5** | `a c` | `$ h D C` | `b g f h $` | $M[C, b]$: pop $C$, push $C b$ |
| **6** | `a c` | `$ h D C b` | `b g f h $` | **Match $b$!** Pop $b$, advance input |
| **7** | `a c b` | `$ h D C` | `g f h $` | $M[C, g]$: pop $C$, push $\epsilon$ (pop only) |
| **8** | `a c b` | `$ h D` | `g f h $` | $M[D, g]$: pop $D$, push $F E$ |
| **9** | `a c b` | `$ h F E` | `g f h $` | $M[E, g]$: pop $E$, push $g$ |
| **10** | `a c b` | `$ h F g` | `g f h $` | **Match $g$!** Pop $g$, advance input |
| **11** | `a c b g` | `$ h F` | `f h $` | $M[F, f]$: pop $F$, push $f$ |
| **12** | `a c b g` | `$ h f` | `f h $` | **Match $f$!** Pop $f$, advance input |
| **13** | `a c b g f` | `$ h` | `h $` | **Match $h$!** Pop $h$, advance input |
| **14** | `a c b g f h` | `$` | `$` | **Match `$` on `$` $\implies$ ACCEPT!** |

---

### Part 4: Panic-Mode Error Recovery Trace on `a c g f h $`

Input: $w_{\text{err}} = a \; c \; g \; f \; h$ `$` (token `b` is missing!).

1. Steps 1 through 4 execute identically to Part 3, matching `a c`.
2. At Step 5:
   - **Stack:** `$ h D C`
   - **Input:** `g f h $` (lookahead is $g$).
3. Parser consults table at $M[C, g]$.
4. According to table $M$, $M[C, g] = C \to \epsilon$.
5. The parser pops $C$, advancing stack to `$ h D` without consuming $g$!
6. Next, stack top is $D$ and input lookahead is $g$:
   - $M[D, g] = D \to E F$.
   - Expands cleanly and continues to match $g, f, h$.
7. **Result:** The parser successfully recovered without crashing, correctly inferring that $C$ had derived $\epsilon$ due to the absence of $b$!

---

## Important Properties and Key Takeaways

1. **Epsilon Expansion Rules Rely on $\text{FOLLOW}$:** In LL(1) parsing, the decision to expand a nullable nonterminal $A \to \epsilon$ is guided strictly by whether the lookahead belongs to $\text{FOLLOW}(A)$.
2. **Reverse Stack Ordering:** Notice Step 1: $S \to a B D h$ was pushed as $h$, then $D$, then $B$, then $a$. This guarantees that $a$ is immediately available on top of the stack to match the first input character.

---

## Related Concepts

- [[Context-Free Grammars and Syntax Analysis]]
- [[Elimination of Left Recursion and Left Factoring Algorithms]]
- [[FIRST and FOLLOW Sets Formulas]]
- [[Top-Down and Predictive Parsing]]
- [[LL(1) Parsing Table Construction and Execution Algorithm]]

---

## Prerequisites

- [[FIRST and FOLLOW Sets Formulas]]
- [[LL(1) Parsing Table Construction and Execution Algorithm]]

---

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/Rumi Sir Merged.pdf|Rumi Sir Merged.pdf]], Chapter 4: Syntax Analysis (Slides 416–472).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 4.4 (pp. 220–228).

---

## Navigation

> 📖 **Reading Order:** Step 15 of 76 | **Module 3: Syntax Analysis & Top-Down Parsing**  
> ◄ **Previous:** [[LL(1) Parser Construction and Simulation Example]] | ► **Next:** [[Bottom-Up Parsing, Reductions, and Handles]]
