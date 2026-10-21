---
type: algorithm
course: cse309
status: active
order: 10
---

# Elimination of Left Recursion and Left Factoring Algorithms

## Starting Point and the Problem

Top-Down parsers (including Recursive-Descent parsers and predictive LL(1) parsers) build parse trees starting from the root nonterminal down to the leaves. When attempting to expand a nonterminal $A$, the parser inspects the current lookahead token to decide which production rule to apply.

However, two major structural patterns in standard Context-Free Grammars cause top-down parsing to fail catastrophically:
1. **Left Recursion:** If a grammar contains a production of the form $A \to A \alpha$, a top-down parser attempting to expand $A$ repeatedly calls the procedure for $A$ without consuming any input characters, entering an **infinite recursive loop** that crashes the compiler with a stack overflow!
2. **Common Prefixes (Non-Determinism):** If two alternate productions for a nonterminal share a common prefix (e.g., $A \to \alpha \beta_1 \mid \alpha \beta_2$), a predictive parser seeing lookahead token in $\text{FIRST}(\alpha)$ cannot determine which alternative to expand without unbounded backtracking.

To make a grammar suitable for top-down predictive parsing, compilers apply two formal grammar transformation algorithms: **Elimination of Left Recursion** and **Left Factoring**.

---

## Part 1: Eliminating Immediate (Direct) Left Recursion

### The Mechanism
A production is **immediately left-recursive** if the nonterminal on the left-hand side appears as the very first symbol on the right-hand side:
$$A \to A \alpha \mid \beta$$
where $\beta$ does not begin with $A$.

This production generates strings of the form:
$$\beta, \quad \beta \alpha, \quad \beta \alpha \alpha, \quad \beta \alpha \alpha \alpha, \quad \dots, \quad \beta \alpha^*$$
Notice that any valid string begins with a non-left-recursive base string $\beta$, followed by zero or more copies of $\alpha$ accumulating from the left.

We can generate the exact same language using **right recursion** by introducing a new auxiliary nonterminal $A'$:
$$\begin{aligned}
A &\to \beta A' \\
A' &\to \alpha A' \mid \epsilon
\end{aligned}$$

### General Formula for Multiple Productions
If nonterminal $A$ has several left-recursive alternatives and several non-left-recursive alternatives:
$$A \to A \alpha_1 \mid A \alpha_2 \mid \dots \mid A \alpha_m \mid \beta_1 \mid \beta_2 \mid \dots \mid \beta_n$$
where no $\beta_i$ begins with $A$.

The transformed right-recursive rules are:
$$\begin{aligned}
A &\to \beta_1 A' \mid \beta_2 A' \mid \dots \mid \beta_n A' \\
A' &\to \alpha_1 A' \mid \alpha_2 A' \mid \dots \mid \alpha_m A' \mid \epsilon
\end{aligned}$$

#### Concrete Example: Arithmetic Expressions
In the classic unambiguous expression grammar:
$$E \to E + T \mid T$$
Here, $A = E$, $\alpha = + T$, and $\beta = T$. Applying the formula yields:
$$\begin{aligned}
E &\to T E' \\
E' &\to + T E' \mid \epsilon
\end{aligned}$$
Similarly, for $T \to T * F \mid F$:
$$\begin{aligned}
T &\to F T' \\
T' &\to * F T' \mid \epsilon
\end{aligned}$$

---

## Part 2: The General Algorithm for Eliminating All Left Recursion

Direct left-recursion elimination alone is insufficient if the grammar contains **indirect (multi-step) left recursion**:
$$S \to A a \mid b, \qquad A \to S c \mid d$$
Notice that $S \Rightarrow A a \Rightarrow S c a$. Here, $S$ derives a sentential form beginning with $S$ in two derivation steps! No individual production has $S$ as its first right-hand symbol, yet the grammar is strictly left-recursive.

### The Systematic Algorithm
**Input:** Context-Free Grammar $G$ with no cycles ($A \Rightarrow^+ A$) and no $\epsilon$-productions ($A \to \epsilon$).  
**Output:** An equivalent grammar with zero left recursion.

```text
Algorithm Eliminate_Left_Recursion:
1. Arrange all nonterminals in some arbitrary fixed order: A_1, A_2, ..., A_n.
2. For i = 1 to n:
       For j = 1 to i - 1:
           // Replace each production of the form A_i -> A_j gamma
           // with A_i -> delta_1 gamma | delta_2 gamma | ... | delta_k gamma
           // where A_j -> delta_1 | delta_2 | ... | delta_k are the current rules for A_j.
       
       // Eliminate immediate left recursion among the rules for A_i.
```

### Walkthrough of Indirect Elimination
Consider the grammar:
$$\begin{aligned}
A_1 &\to A_2 d \mid A_1 a \mid a \\
A_2 &\to A_1 e \mid b
\end{aligned}$$
Ordered nonterminals: $A_1, A_2$.

1. **Iteration $i = 1$:** ($j$ loop does not run).
   - Rules for $A_1$: $A_1 \to A_1 a \mid A_2 d \mid a$.
   - Direct left recursion exists with $\alpha = a$, $\beta_1 = A_2 d$, $\beta_2 = a$.
   - Eliminate direct left recursion:
     $$\begin{aligned}
     A_1 &\to A_2 d A_1' \mid a A_1' \\
     A_1' &\to a A_1' \mid \epsilon
     \end{aligned}$$

2. **Iteration $i = 2$:**
   - **Inner loop $j = 1$:** Replace $A_2 \to A_1 e$ by substituting the current right-hand sides of $A_1$:
     Since $A_1 \to A_2 d A_1' \mid a A_1'$, substituting into $A_2 \to A_1 e$ yields:
     $$A_2 \to A_2 d A_1' e \mid a A_1' e \mid b$$
   - Notice that $A_2$ now has **immediate left recursion** on $A_2 d A_1' e$!
   - Here, $\alpha = d A_1' e$, and the non-left-recursive bases are $\beta_1 = a A_1' e$ and $\beta_2 = b$.
   - Eliminate immediate left recursion from $A_2$:
     $$\begin{aligned}
     A_2 &\to a A_1' e A_2' \mid b A_2' \\
     A_2' &\to d A_1' e A_2' \mid \epsilon
     \end{aligned}$$

The final grammar has zero direct and zero indirect left recursion!

---

## Part 3: Left Factoring Algorithm

### The Motivation
When a nonterminal has two or more productions whose bodies begin with the same string of symbols:
$$A \to \alpha \beta_1 \mid \alpha \beta_2$$
A predictive top-down parser cannot decide whether to expand $A$ via the first or second production when it sees a token derived from $\alpha$. 

**Left Factoring** is a grammar transformation that defers the decision until enough of the input has been consumed to make the choice deterministically.

### The Algorithm
**Input:** Grammar $G$.  
**Output:** An equivalent left-factored grammar.

```text
Algorithm Left_Factor:
For each nonterminal A:
    1. Find the longest prefix alpha common to two or more of its alternatives.
    2. If alpha != epsilon (a non-trivial common prefix exists):
       Replace all productions A -> alpha beta_1 | alpha beta_2 | ... | alpha beta_m | gamma
       (where gamma represents all alternatives that do NOT begin with alpha)
       with:
           A  -> alpha A' | gamma
           A' -> beta_1 | beta_2 | ... | beta_m
    3. Repeat Step 1 and 2 until no nonterminal has alternatives sharing a common prefix.
```

### Concrete Example: Conditional Statements
Consider the dangling-else grammar rules:
$$S \to \mathbf{i} E \mathbf{t} S \mid \mathbf{i} E \mathbf{t} S \mathbf{e} S \mid a$$
Both alternatives begin with common prefix $\alpha = \mathbf{i} E \mathbf{t} S$.
- $\beta_1 = \epsilon$
- $\beta_2 = \mathbf{e} S$
- $\gamma = a$

Applying Left Factoring:
$$\begin{aligned}
S &\to \mathbf{i} E \mathbf{t} S S' \mid a \\
S' &\to \mathbf{e} S \mid \epsilon
\end{aligned}$$
Now, when the parser encounters $\mathbf{i}$, it expands $S \to \mathbf{i} E \mathbf{t} S S'$. Only after consuming $\mathbf{i} E \mathbf{t} S$ does it inspect the lookahead token to decide whether to expand $S' \to \mathbf{e} S$ (if lookahead is $\mathbf{e}$) or $S' \to \epsilon$.

---

## Important Properties and Guarantees

- **Language Equivalence:** Both algorithms strictly preserve language equivalence: $L(G_{\text{new}}) = L(G_{\text{orig}})$.
- **Termination Guarantee:** The left-recursion algorithm is guaranteed to terminate because the nested loops over $i$ and $j$ strictly eliminate nonterminal references with lower indices ($j < i$).

---

## Common Mistakes

- **Forgetting $\epsilon$ in the Transformed Auxiliaries:** Omitting the $\epsilon$ alternative in $A' \to \alpha A' \mid \epsilon$. If $\epsilon$ is omitted, $A$ would be forced to derive at least one $\alpha$, breaking equivalence!
- **Swapping $\beta$ and $\alpha$:** Writing $A \to \alpha A'$ instead of $A \to \beta A'$. Remember: the non-left-recursive base $\beta$ must come *first*!

---

## Exam Relevance

In BUET CSE 309 examinations:
1. Executing direct left-recursion elimination on expression grammars.
2. Executing the two-pass general left-recursion algorithm on indirect grammars (e.g., $A \to B d \dots, B \to A e \dots$).
3. Performing left factoring on conditional statement or declaration grammars.

---

## Related Concepts

- [[Context-Free Grammars and Syntax Analysis]]
- [[FIRST and FOLLOW Sets Formulas]]
- [[Top-Down and Predictive Parsing]]
- [[LL(1) Parsing Table Construction and Execution Algorithm]]

---

## Prerequisites

- [[Context-Free Grammars and Syntax Analysis]]

---

## Problems

- [[Problem — LL(1) Grammar Verification and Parsing Table Construction]]

---

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/Rumi Sir Merged.pdf|Rumi Sir Merged.pdf]], Chapter 4: Syntax Analysis (Slides 339, 368–409).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 4.3 (pp. 211–216).

---

## Navigation

> 📖 **Reading Order:** Step 10 of 76 | **Module 3: Syntax Analysis & Top-Down Parsing**  
> ◄ **Previous:** [[Context-Free Grammars and Syntax Analysis]] | ► **Next:** [[FIRST and FOLLOW Sets Formulas]]
