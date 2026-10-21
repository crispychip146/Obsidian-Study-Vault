---
type: example
course: cse309
status: active
order: 14
---

# LL(1) Parser Construction and Simulation Example

## Starting Point and the Problem

To understand the complete mathematical and operational pipeline of top-down predictive parsing, we execute an end-to-end walkthrough on the standard arithmetic expression grammar:
$$\begin{aligned}
E &\to E + T \mid T \\
T &\to T * F \mid F \\
F &\to (E) \mid \mathbf{id}
\end{aligned}$$
We will:
1. Transform the grammar into an LL(1) compliant form by eliminating left recursion.
2. Construct the complete two-dimensional LL(1) predictive parsing table $M$.
3. Trace the non-recursive table-driven engine step-by-step on the input sentence:
   $$\mathbf{id} + \mathbf{id} * \mathbf{id} \; \$$$

---

## Step 1: Grammar Transformation (Eliminating Left Recursion)

The original grammar is immediately left-recursive on $E \to E + T$ and $T \to T * F$.
Applying the direct left-recursion elimination formula:
- $E \to T E'$
- $E' \to + T E' \mid \epsilon$
- $T \to F T'$
- $T' \to * F T' \mid \epsilon$
- $F \to (E) \mid \mathbf{id}$

The grammar is now completely free of left recursion and has no common prefixes.

---

## Step 2: $\text{FIRST}$ and $\text{FOLLOW}$ Sets

As derived in [[FIRST and FOLLOW Sets Formulas]]:

| Nonterminal ($A$) | $\text{FIRST}(A)$ | $\text{FOLLOW}(A)$ |
|:---:|:---:|:---:|
| $E$ | $\{(, \mathbf{id}\}$ | $\{), \$\}$ |
| $E'$ | $\{+, \epsilon\}$ | $\{), \$\}$ |
| $T$ | $\{(, \mathbf{id}\}$ | $\{+, ), \$\}$ |
| $T'$ | $\{*, \epsilon\}$ | $\{+, ), \$\}$ |
| $F$ | $\{(, \mathbf{id}\}$ | $\{+, *, ), \$\}$ |

---

## Step 3: LL(1) Parsing Table $M$ Construction

We apply the table construction rules:
1. **Rule $E \to T E'$:** $\text{FIRST}(T E') = \text{FIRST}(T) = \{(, \mathbf{id}\}$.
   $\implies$ Place $E \to T E'$ in $M[E, \mathbf{id}]$ and $M[E, (]$.
2. **Rule $E' \to + T E'$:** $\text{FIRST}(+ T E') = \{+\}$.
   $\implies$ Place $E' \to + T E'$ in $M[E', +]$.
3. **Rule $E' \to \epsilon$:** Since $\epsilon \in \text{FIRST}(\epsilon)$, we inspect $\text{FOLLOW}(E') = \{), \$\}$.
   $\implies$ Place $E' \to \epsilon$ in $M[E', )]$ and $M[E', \$]$.
4. **Rule $T \to F T'$:** $\text{FIRST}(F T') = \text{FIRST}(F) = \{(, \mathbf{id}\}$.
   $\implies$ Place $T \to F T'$ in $M[T, \mathbf{id}]$ and $M[T, (]$.
5. **Rule $T' \to * F T'$:** $\text{FIRST}(* F T') = \{*\}$.
   $\implies$ Place $T' \to * F T'$ in $M[T', *]$.
6. **Rule $T' \to \epsilon$:** Since $\epsilon \in \text{FIRST}(\epsilon)$, inspect $\text{FOLLOW}(T') = \{+, ), \$\}$.
   $\implies$ Place $T' \to \epsilon$ in $M[T', +]$, $M[T', )]$, and $M[T', \$]$.
7. **Rule $F \to (E)$:** $\text{FIRST}((E)) = \{(\}$.
   $\implies$ Place $F \to (E)$ in $M[F, (]$.
8. **Rule $F \to \mathbf{id}$:** $\text{FIRST}(\mathbf{id}) = \{\mathbf{id}\}$.
   $\implies$ Place $F \to \mathbf{id}$ in $M[F, \mathbf{id}]$.

### The Completed LL(1) Parsing Table $M$

| Nonterminal | $\mathbf{id}$ | $+$ | $*$ | $($ | $)$ | $\$$ |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **$E$** | $E \to T E'$ | | | $E \to T E'$ | `synch` | `synch` |
| **$E'$** | | $E' \to + T E'$ | | | $E' \to \epsilon$ | $E' \to \epsilon$ |
| **$T$** | $T \to F T'$ | `synch` | | $T \to F T'$ | `synch` | `synch` |
| **$T'$** | | $T' \to \epsilon$ | $T' \to * F T'$ | | $T' \to \epsilon$ | $T' \to \epsilon$ |
| **$F$** | $F \to \mathbf{id}$ | `synch` | `synch` | $F \to (E)$ | `synch` | `synch` |

*Verification:* Notice that **no cell contains more than one production**. Every entry is either empty (or marked `synch` for error recovery) or holds exactly one production rule. This proves conclusively that the grammar is **LL(1)**!

---

## Step 4: Step-by-Step Table-Driven Parser Simulation

Input String: $\mathbf{id} + \mathbf{id} * \mathbf{id} \; \$$  
Stack is initialized to: `[$, E]` (left is bottom, right is top).

| Step | Matched Prefix | Parsing Stack | Input Buffer | Action Taken |
|:---:|:---:|---|---|---|
| **1** | | `$ E` | `id + id * id $` | $M[E, \mathbf{id}]$: pop $E$, push $E' T$ |
| **2** | | `$ E' T` | `id + id * id $` | $M[T, \mathbf{id}]$: pop $T$, push $T' F$ |
| **3** | | `$ E' T' F` | `id + id * id $` | $M[F, \mathbf{id}]$: pop $F$, push $\mathbf{id}$ |
| **4** | | `$ E' T' id` | `id + id * id $` | **Match $\mathbf{id}$!** Pop $\mathbf{id}$, advance input |
| **5** | `id` | `$ E' T'` | `+ id * id $` | $M[T', +]$: pop $T'$, push $\epsilon$ (pop only) |
| **6** | `id` | `$ E'` | `+ id * id $` | $M[E', +]$: pop $E'$, push $E' T +$ |
| **7** | `id` | `$ E' T +` | `+ id * id $` | **Match $+$!** Pop $+$, advance input |
| **8** | `id +` | `$ E' T` | `id * id $` | $M[T, \mathbf{id}]$: pop $T$, push $T' F$ |
| **9** | `id +` | `$ E' T' F` | `id * id $` | $M[F, \mathbf{id}]$: pop $F$, push $\mathbf{id}$ |
| **10** | `id +` | `$ E' T' id` | `id * id $` | **Match $\mathbf{id}$!** Pop $\mathbf{id}$, advance input |
| **11** | `id + id` | `$ E' T'` | `* id $` | $M[T', *]$: pop $T'$, push $T' F *$ |
| **12** | `id + id` | `$ E' T' F *` | `* id $` | **Match $*$!** Pop $*$, advance input |
| **13** | `id + id *` | `$ E' T' F` | `id $` | $M[F, \mathbf{id}]$: pop $F$, push $\mathbf{id}$ |
| **14** | `id + id *` | `$ E' T' id` | `id $` | **Match $\mathbf{id}$!** Pop $\mathbf{id}$, advance input |
| **15** | `id + id * id` | `$ E' T'` | `$` | $M[T', \$]$: pop $T'$, push $\epsilon$ |
| **16** | `id + id * id` | `$ E'` | `$` | $M[E', \$]$: pop $E'$, push $\epsilon$ |
| **17** | `id + id * id` | `$` | `$` | **Match $\$$ on $\$$ $\implies$ ACCEPT!** |

---

## Analysis of the Parse Result

1. **Exact Derivation Recovered:**
   Writing out the sequence of productions emitted by the parser recovers the canonical **leftmost derivation**:
   $$E \Rightarrow T E' \Rightarrow F T' E' \Rightarrow \mathbf{id} T' E' \Rightarrow \mathbf{id} E' \Rightarrow \mathbf{id} + T E' \Rightarrow \mathbf{id} + F T' E' \Rightarrow \mathbf{id} + \mathbf{id} T' E' \Rightarrow \mathbf{id} + \mathbf{id} * F T' E' \Rightarrow \mathbf{id} + \mathbf{id} * \mathbf{id} T' E' \Rightarrow \mathbf{id} + \mathbf{id} * \mathbf{id}$$
2. **Determinism:** At no point did the parser guess, test alternatives, or backtrack. Every transition was dictated uniquely by $M[\text{StackTop}, \text{Lookahead}]$.
3. **Linearity:** The 5-token input string was completely parsed in exactly 17 constant-time steps.

---

## Common Mistakes

- **Incorrect Reverse Pushing:** In Step 1, pushing $E' T$ as $E'$ first and $T$ second. That places $E'$ on top! The correct stack state is `[$, E', T]` where $T$ is at the right (top).
- **Consuming Tokens on $\epsilon$-Productions:** In Step 5, seeing $M[T', +] = T' \to \epsilon$ and advancing the input pointer past `+`. Expanding an $\epsilon$-production *only pops the nonterminal*; the input token `+` must remain in the input buffer to be processed by $E'$!

---

## Exam Relevance

In BUET CSE 309 examinations:
1. Constructing the complete table $M$ from the transformed grammar (8 to 10 marks).
2. Writing the 17-step simulation table with columns `Stack`, `Input`, and `Action Taken` (frequent exam problem).

---

## Related Concepts

- [[Context-Free Grammars and Syntax Analysis]]
- [[FIRST and FOLLOW Sets Formulas]]
- [[Top-Down and Predictive Parsing]]
- [[LL(1) Parsing Table Construction and Execution Algorithm]]
- [[Problem — LL(1) Grammar Verification and Parsing Table Construction]]

---

## Prerequisites

- [[FIRST and FOLLOW Sets Formulas]]
- [[LL(1) Parsing Table Construction and Execution Algorithm]]

---

## Problems

- [[Problem — LL(1) Grammar Verification and Parsing Table Construction]]

---

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/Rumi Sir Merged.pdf|Rumi Sir Merged.pdf]], Chapter 4: Syntax Analysis (Slides 452–491).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 4.4.3–4.4.4 (pp. 222–228).

---

## Navigation

> 📖 **Reading Order:** Step 14 of 76 | **Module 3: Syntax Analysis & Top-Down Parsing**  
> ◄ **Previous:** [[LL(1) Parsing Table Construction and Execution Algorithm]] | ► **Next:** [[Problem — LL(1) Grammar Verification and Parsing Table Construction]]
