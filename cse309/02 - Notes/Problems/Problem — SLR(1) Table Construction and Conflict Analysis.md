---
type: problem
course: cse309
status: active
order: 21
---

# Problem — SLR(1) Table Construction and Conflict Analysis

## Problem Statement

### Context
In designing a syntax analyzer for an imperative programming language with pointer dereferencing, a compiler writer defines the following grammar fragment for assignment statements:
$$\begin{aligned}
(1)\; S &\to L = R \\
(2)\; S &\to R \\
(3)\; L &\to * R \\
(4)\; L &\to \mathbf{id} \\
(5)\; R &\to L
\end{aligned}$$
where $\{S, L, R\}$ are nonterminals, $\{=, *, \mathbf{id}\}$ are terminals, and $S$ is the start symbol ($L$ represents an l-value / memory location, and $R$ represents an r-value / evaluated value).

---

### Question 1: Canonical Collection of LR(0) Item Sets
Augment the grammar with $S' \to S$. Construct the complete canonical collection of sets of LR(0) items $\mathcal{C} = \{I_0, I_1, \dots, I_n\}$, showing the full derivations for $\text{CLOSURE}$ and $\text{GOTO}$.

---

### Question 2: FOLLOW Set Derivation
Compute the exact $\text{FOLLOW}$ sets for nonterminals $S$, $L$, and $R$:
$$\text{FOLLOW}(S), \quad \text{FOLLOW}(L), \quad \text{FOLLOW}(R)$$
Show the step-by-step mathematical justification, specifically proving why terminal $=$ belongs to $\text{FOLLOW}(R)$.

---

### Question 3: SLR(1) Table Construction and Conflict Analysis
1. Construct the SLR(1) parsing table ($\text{ACTION}$ and $\text{GOTO}$) from the item sets and $\text{FOLLOW}$ sets.
2. Identify the exact state and input terminal that encounters a **Shift/Reduce Conflict**.
3. State precisely which two actions collide in that table cell.

---

### Question 4: Ambiguity versus SLR(1) Power
1. Prove whether this grammar is ambiguous or unambiguous.
2. Explain the fundamental theoretical limitation of SLR(1) exposed by this grammar, and explain how Canonical LR(1) or LALR(1) resolves the conflict.

---

## Detailed Solution and Walkthrough

### Part 1: Canonical Collection of Sets of LR(0) Items

Augmented Grammar:
$$\begin{aligned}
(0)\; S' &\to S \\
(1)\; S &\to L = R \\
(2)\; S &\to R \\
(3)\; L &\to * R \\
(4)\; L &\to \mathbf{id} \\
(5)\; R &\to L
\end{aligned}$$

#### State $I_0 = \text{CLOSURE}(\{[S' \to \cdot S]\})$
- $S' \to \cdot S$
- $S \to \cdot L = R$
- $S \to \cdot R$
- $L \to \cdot * R$
- $L \to \cdot \mathbf{id}$
- $R \to \cdot L$

**Transitions from $I_0$:**
- $\text{GOTO}(I_0, S) = I_1$
- $\text{GOTO}(I_0, L) = I_2$
- $\text{GOTO}(I_0, R) = I_3$
- $\text{GOTO}(I_0, *) = I_4$
- $\text{GOTO}(I_0, \mathbf{id}) = I_5$

---

#### Constructing Remaining States ($I_1$ to $I_9$)

- **State $I_1 = \text{GOTO}(I_0, S)$:**
  - $S' \to S \cdot$ *(Accept state)*

- **State $I_2 = \text{GOTO}(I_0, L)$:**
  - $S \to L \cdot = R$
  - $R \to L \cdot$ *(Complete item!)*
  - Transition: $\text{GOTO}(I_2, =) = I_6$

- **State $I_3 = \text{GOTO}(I_0, R)$:**
  - $S \to R \cdot$ *(Reduce by rule 2)*

- **State $I_4 = \text{GOTO}(I_0, *) = \text{CLOSURE}(\{[L \to * \cdot R]\})$:**
  - $L \to * \cdot R$
  - $R \to \cdot L$
  - $L \to \cdot * R$
  - $L \to \cdot \mathbf{id}$
  - Transitions: $\text{GOTO}(I_4, R) = I_7$, $\text{GOTO}(I_4, L) = I_2$, $\text{GOTO}(I_4, *) = I_4$, $\text{GOTO}(I_4, \mathbf{id}) = I_5$

- **State $I_5 = \text{GOTO}(I_0, \mathbf{id})$:**
  - $L \to \mathbf{id} \cdot$ *(Reduce by rule 4)*

- **State $I_6 = \text{GOTO}(I_2, =) = \text{CLOSURE}(\{[S \to L = \cdot R]\})$:**
  - $S \to L = \cdot R$
  - $R \to \cdot L$
  - $L \to \cdot * R$
  - $L \to \cdot \mathbf{id}$
  - Transitions: $\text{GOTO}(I_6, R) = I_8$, $\text{GOTO}(I_6, L) = I_2$, $\text{GOTO}(I_6, *) = I_4$, $\text{GOTO}(I_6, \mathbf{id}) = I_5$

- **State $I_7 = \text{GOTO}(I_4, R)$:**
  - $L \to * R \cdot$ *(Reduce by rule 3)*

- **State $I_8 = \text{GOTO}(I_6, R)$:**
  - $S \to L = R \cdot$ *(Reduce by rule 1)*

- **Total States:** Exactly 9 states ($I_0$ through $I_8$).

---

### Part 2: Computation of FOLLOW Sets

1. **$\text{FOLLOW}(S)$:**
   - $S$ is start symbol $\implies \$ \in \text{FOLLOW}(S)$.
   - $\implies \text{FOLLOW}(S) = \{\$\}$.

2. **$\text{FOLLOW}(R)$:**
   - In production (1) $S \to L = R$, $R$ is at the end $\implies \text{FOLLOW}(S) \subseteq \text{FOLLOW}(R) \implies \$ \in \text{FOLLOW}(R)$.
   - In production (3) $L \to * R$, $R$ is at the end $\implies \text{FOLLOW}(L) \subseteq \text{FOLLOW}(R)$.
   - Consider sentential form: $S \Rightarrow L = R \Rightarrow * R = R$.
   - In this sentential form, terminal $=$ appears immediately after $R$!
   - Therefore, by the formal definition of $\text{FOLLOW}$, $=$ **belongs to $\text{FOLLOW}(R)$**!
   - $\implies \text{FOLLOW}(R) = \{=, \$\}$.

3. **$\text{FOLLOW}(L)$:**
   - In production (1) $S \to L = R$, terminal $=$ follows $L \implies = \in \text{FOLLOW}(L)$.
   - In production (5) $R \to L$, $L$ is at the end $\implies \text{FOLLOW}(R) \subseteq \text{FOLLOW}(L) \implies \text{FOLLOW}(R) \subseteq \text{FOLLOW}(L)$.
   - Since $\text{FOLLOW}(R) = \{=, \$\}$:
   - $\implies \text{FOLLOW}(L) = \{=, \$\}$.

---

### Part 3: SLR(1) Table Construction and Conflict Demonstration

Now apply SLR(1) table construction rules to **State 2 ($I_2$)**:
State $I_2$ contains two items:
1. Item $[S \to L \cdot = R]$:
   - The dot precedes terminal $=$.
   - Since $\text{GOTO}(I_2, =) = I_6$, Step 2a requires:
     $$\text{ACTION}[2, =] = \text{shift } 6 \quad (s_6)$$
2. Item $[R \to L \cdot]$:
   - This is a complete reduce item ($R \to L$ is production 5).
   - Step 2b requires adding reduce rule $r_5$ for **ALL terminals in $\text{FOLLOW}(R)$**:
   - Since $\text{FOLLOW}(R) = \{=, \$\}$, the algorithm sets:
     $$\text{ACTION}[2, \$] = r_5$$
     $$\text{ACTION}[2, =] = r_5$$

### The Collision in Cell $\text{ACTION}[2, =]$:
Look at cell $\text{ACTION}[2, =]$:
$$\text{ACTION}[2, =] = \{ s_6, \; r_5 \}$$
- Action $s_6$ demands **SHIFT** $=$.
- Action $r_5$ demands **REDUCE** via $R \to L$.
- The cell contains **both a shift and a reduce action** $\implies$ **Shift/Reduce Conflict!**

**Conclusion:** Because cell $\text{ACTION}[2, =]$ contains multiple conflicting entries, the grammar is **NOT SLR(1)**!

---

### Part 4: Ambiguity versus SLR(1) Inadequacy

#### 1. Is the Grammar Ambiguous?
**No. The grammar is 100% UNAMBIGUOUS.**
- Every string generated by the grammar (such as $\mathbf{id} = \mathbf{id}$ or $* \mathbf{id} = \mathbf{id}$) has exactly **one unique parse tree** and exactly one rightmost derivation.
- For example, for $\mathbf{id} = \mathbf{id}$:
  $$S \Rightarrow_{rm} L = R \Rightarrow_{rm} L = L \Rightarrow_{rm} L = \mathbf{id} \Rightarrow_{rm} \mathbf{id} = \mathbf{id}$$
  There is no alternative derivation.

#### 2. Why Does SLR(1) Fail?
The failure is entirely due to the **coarseness of the $\text{FOLLOW}$ set approximation in SLR(1)**:
- SLR(1) checks whether $=$ is in $\text{FOLLOW}(R)$ *anywhere in the entire grammar*.
- $=$ is indeed in $\text{FOLLOW}(R)$ because of strings like $* R = R$.
- However, when the stack contains just $L$ (which is State $I_2$, derived from $S \to L \dots$), the only valid right-sentential form is $L = R$. A reduction $R \to L$ in this specific state would yield $S \Rightarrow R = R$, which is **impossible** (the grammar cannot generate $R = R$ starting from $S$).
- Therefore, in the specific context of State 2, **$=$ can never legally follow $R$**!
- SLR(1) is too blunt to know this context because it discards the derivation history.

#### 3. How LR(1) and LALR(1) Resolve the Conflict
- **Canonical LR(1)** embeds an explicit lookahead token directly into each item:
  - In State 2, the items become:
    $$[S \to L \cdot = R, \; \$] \qquad [R \to L \cdot, \; \$]$$
  - The lookahead for $[R \to L \cdot]$ is strictly $\{\$\}$, NOT $\{=, \$\}$!
- Therefore, in an LR(1) table:
  - $\text{ACTION}[2, =] = s_6$ (Shift only!)
  - $\text{ACTION}[2, \$] = r_5$ (Reduce only!)
- The conflict completely vanishes! The grammar is provably **LR(1)** and **LALR(1)**.

---

## Important Properties and Key Takeaways

1. **Unambiguous Grammars Can Fail SLR(1):** Proving that an unambiguous grammar is not SLR(1) demonstrates that SLR(1) is strictly weaker than LR(1).
2. **Grammar Hierarchy:**
   $$\text{LL}(1) \subset \text{SLR}(1) \subset \text{LALR}(1) \subset \text{LR}(1) \subset \text{Unambiguous CFG}$$
3. **The Assignment Grammar Benchmark:** $S \to L = R \mid R$ is the canonical universal benchmark in compiler theory used to demonstrate why parser generators (like Yacc/Bison) use LALR(1) rather than SLR(1).

---

## Related Concepts

- [[Bottom-Up Parsing, Reductions, and Handles]]
- [[Shift-Reduce Parsing and Parsing Conflicts]]
- [[LR(0) Items and the LR(0) Automaton]]
- [[SLR(1) Parsing Table Construction and LR Parser Algorithm]]
- [[SLR(1) Automaton and Parsing Table Construction Example]]
- [[Syntax-Directed Definitions and Translation Schemes]]

---

## Prerequisites

- [[LR(0) Items and the LR(0) Automaton]]
- [[SLR(1) Parsing Table Construction and LR Parser Algorithm]]

---

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/Rumi Sir Merged.pdf|Rumi Sir Merged.pdf]], Chapter 4: Syntax Analysis (Slides 720–727).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 4.6.4 (pp. 257–259).

---

## Navigation

> 📖 **Reading Order:** Step 21 of 76 | **Module 4: Bottom-Up Parsing & LR Parsers**  
> ◄ **Previous:** [[SLR(1) Automaton and Parsing Table Construction Example]] | ► **Next:** [[Syntax-Directed Definitions and Translation Schemes]]
