---
type: formula
course: cse309
status: active
order: 11
---

# FIRST and FOLLOW Sets Formulas

## Starting Point and the Problem

To build a predictive parser that operates deterministically without backtracking, a top-down parser must know:
1. When looking at nonterminal $A$ and seeing lookahead terminal $a$, which production $A \to \alpha$ should be selected?
2. If production $A \to \alpha$ can derive the empty string $\epsilon$, what tokens can legally follow $A$ in the grammar so we know when to expand $A$ into $\epsilon$?

To answer these questions mathematically, compiler theory defines two foundational set-valued functions: **$\text{FIRST}$** and **$\text{FOLLOW}$**. These sets are essential not only for top-down LL(1) parsers, but also for constructing Bottom-Up LR/SLR parsing tables.

---

## Definitions and Intuition

### 1. $\text{FIRST}(\alpha)$
For any grammar symbol string $\alpha \in (V \cup \Sigma)^*$:
$$\text{FIRST}(\alpha) = \{ a \in \Sigma \mid \alpha \Rightarrow^* a \beta \text{ for some } \beta \in (V \cup \Sigma)^* \}$$
- **Intuition:** $\text{FIRST}(\alpha)$ is the set of all terminal symbols that can appear as the very first symbol of a string derived from $\alpha$.
- If $\alpha \Rightarrow^* \epsilon$, then $\epsilon \in \text{FIRST}(\alpha)$.

### 2. $\text{FOLLOW}(A)$
For any nonterminal $A \in V$:
$$\text{FOLLOW}(A) = \{ a \in \Sigma \cup \{\$\} \mid S \Rightarrow^* \alpha A a \beta \text{ for some } \alpha, \beta \in (V \cup \Sigma)^* \}$$
- **Intuition:** $\text{FOLLOW}(A)$ is the set of all terminal symbols that can appear immediately to the right of $A$ in *some* sentential form derived from the start symbol $S$.
- Here, `$` is the special endmarker representing the end of the input stream. If $A$ can be the rightmost symbol in a sentence ($S \Rightarrow^* \alpha A$), then $\$ \in \text{FOLLOW}(A)$.
- **Crucial Rule:** $\text{FOLLOW}$ sets are defined **only for nonterminals**, and $\text{FOLLOW}(A)$ **never contains $\epsilon$**.

---

## Formula 1: Computation of $\text{FIRST}(X)$

To compute $\text{FIRST}(X)$ for all grammar symbols $X \in (V \cup \Sigma)$, apply the following rules iteratively until no more symbols or $\epsilon$ can be added:

### Rule 1 (Terminal Base Case)
If $X$ is a terminal ($X \in \Sigma$):
$$\text{FIRST}(X) = \{X\}$$

### Rule 2 ($\epsilon$ Base Case)
If $X \to \epsilon$ is a production rule:
$$\epsilon \in \text{FIRST}(X)$$

### Rule 3 (Nonterminal Expansion and $\epsilon$-Propagation)
If $X \in V$ is a nonterminal and $X \to Y_1 Y_2 \dots Y_k$ is a production:
1. For some $i \le k$, if $\epsilon \in \text{FIRST}(Y_1), \dots, \epsilon \in \text{FIRST}(Y_{i-1})$ (meaning all preceding symbols $Y_1, \dots, Y_{i-1}$ can derive the empty string $\epsilon$), then:
   $$(\text{FIRST}(Y_i) \setminus \{\epsilon\}) \subseteq \text{FIRST}(X)$$
2. If **all** symbols $Y_1, Y_2, \dots, Y_k$ can derive $\epsilon$ (i.e., $\epsilon \in \text{FIRST}(Y_j)$ for all $j \in \{1, \dots, k\}$):
   $$\epsilon \in \text{FIRST}(X)$$

### Extension to Strings of Symbols: $\text{FIRST}(X_1 X_2 \dots X_n)$
Given an arbitrary sentential form $\alpha = X_1 X_2 \dots X_n$:
1. Add all non-$\epsilon$ symbols of $\text{FIRST}(X_1)$ to $\text{FIRST}(\alpha)$.
2. If $\epsilon \in \text{FIRST}(X_1)$, add all non-$\epsilon$ symbols of $\text{FIRST}(X_2)$, and continue sequentially.
3. If $\epsilon \in \text{FIRST}(X_i)$ for all $1 \le i \le n$, add $\epsilon$ to $\text{FIRST}(\alpha)$.

---

## Formula 2: Computation of $\text{FOLLOW}(A)$

To compute $\text{FOLLOW}(A)$ for all nonterminals $A \in V$, apply the following rules iteratively until no new terminals can be added to any set:

### Rule 1 (Start Symbol Initialization)
For the start symbol $S$:
$$\$ \in \text{FOLLOW}(S)$$

### Rule 2 (Symbols to the Right)
If there is a production of the form:
$$A \to \alpha B \beta$$
where $B$ is a nonterminal and $\beta$ is a non-empty string of grammar symbols:
Everything in $\text{FIRST}(\beta)$ (except $\epsilon$) is in $\text{FOLLOW}(B)$:
$$(\text{FIRST}(\beta) \setminus \{\epsilon\}) \subseteq \text{FOLLOW}(B)$$

### Rule 3 ($\epsilon$-Derivation / End of Production)
If there is a production:
$$A \to \alpha B \quad \text{or} \quad A \to \alpha B \beta \text{ where } \epsilon \in \text{FIRST}(\beta)$$
Then everything currently in $\text{FOLLOW}(A)$ is also in $\text{FOLLOW}(B)$:
$$\text{FOLLOW}(A) \subseteq \text{FOLLOW}(B)$$

---

## Complete Concrete Calculation Example

Consider the standard unambiguous, left-recursion-free expression grammar:
$$\begin{aligned}
E &\to T E' \\
E' &\to + T E' \mid \epsilon \\
T &\to F T' \\
T' &\to * F T' \mid \epsilon \\
F &\to (E) \mid \mathbf{id}
\end{aligned}$$

### 1. Calculating $\text{FIRST}$ Sets (Bottom-Up)
1. For $F \to (E) \mid \mathbf{id}$:
   $$\text{FIRST}(F) = \{(, \mathbf{id}\}$$
2. For $T' \to * F T' \mid \epsilon$:
   $$\text{FIRST}(T') = \{*, \epsilon\}$$
3. For $T \to F T'$:
   Since $\epsilon \notin \text{FIRST}(F)$, $\text{FIRST}(T) = \text{FIRST}(F) = \{(, \mathbf{id}\}$.
4. For $E' \to + T E' \mid \epsilon$:
   $$\text{FIRST}(E') = \{+, \epsilon\}$$
5. For $E \to T E'$:
   Since $\epsilon \notin \text{FIRST}(T)$, $\text{FIRST}(E) = \text{FIRST}(T) = \{(, \mathbf{id}\}$.

**Summary of $\text{FIRST}$ Sets:**
- $\text{FIRST}(E) = \{(, \mathbf{id}\}$
- $\text{FIRST}(E') = \{+, \epsilon\}$
- $\text{FIRST}(T) = \{(, \mathbf{id}\}$
- $\text{FIRST}(T') = \{*, \epsilon\}$
- $\text{FIRST}(F) = \{(, \mathbf{id}\}$

---

### 2. Calculating $\text{FOLLOW}$ Sets
1. **Start Symbol Rule:** $E$ is start symbol $\implies \$ \in \text{FOLLOW}(E)$.
   From $F \to (E)$, the symbol following $E$ is $)\implies ) \in \text{FOLLOW}(E)$.
   $$\text{FOLLOW}(E) = \{), \$\}$$

2. **Propagating to $E'$:**
   From $E \to T E'$, $E'$ is at the end $\implies \text{FOLLOW}(E) \subseteq \text{FOLLOW}(E')$.
   From $E' \to + T E'$, $E'$ is at the end $\implies \text{FOLLOW}(E') \subseteq \text{FOLLOW}(E')$.
   $$\text{FOLLOW}(E') = \text{FOLLOW}(E) = \{), \$\}$$

3. **Propagating to $T$:**
   From $E \to T E'$, $E'$ follows $T \implies (\text{FIRST}(E') \setminus \{\epsilon\}) \subseteq \text{FOLLOW}(T) \implies + \in \text{FOLLOW}(T)$.
   Since $\epsilon \in \text{FIRST}(E')$, by Rule 3: $\text{FOLLOW}(E) \subseteq \text{FOLLOW}(T) \implies \{), \$\} \subseteq \text{FOLLOW}(T)$.
   $$\text{FOLLOW}(T) = \{+, ), \$\}$$

4. **Propagating to $T'$:**
   From $T \to F T'$, $T'$ is at the end $\implies \text{FOLLOW}(T) \subseteq \text{FOLLOW}(T')$.
   $$\text{FOLLOW}(T') = \text{FOLLOW}(T) = \{+, ), \$\}$$

5. **Propagating to $F$:**
   From $T \to F T'$, $T'$ follows $F \implies (\text{FIRST}(T') \setminus \{\epsilon\}) \subseteq \text{FOLLOW}(F) \implies * \in \text{FOLLOW}(F)$.
   Since $\epsilon \in \text{FIRST}(T')$, by Rule 3: $\text{FOLLOW}(T) \subseteq \text{FOLLOW}(F) \implies \{+, ), \$\} \subseteq \text{FOLLOW}(F)$.
   $$\text{FOLLOW}(F) = \{+, *, ), \$\}$$

---

## Important Properties and Guarantees

- **Monotonicity and Finite Termination:** The computation operates over a finite set of terminals $\Sigma \cup \{\$\}$. Each iterative pass either strictly enlarges at least one set or reaches a fixed point. Therefore, the computation is strictly monotonic and guaranteed to terminate in at most $|V|$ passes.
- **$\text{FOLLOW}$ Sets Depend on Global Sentential Structure:** While $\text{FIRST}$ can be computed locally from a nonterminal's own productions, $\text{FOLLOW}(A)$ depends on every production where $A$ appears on the *right-hand side*.

---

## Common Mistakes

- **Adding $\epsilon$ to a $\text{FOLLOW}$ Set:** $\epsilon$ can *never* appear in $\text{FOLLOW}$. $\text{FOLLOW}$ contains only physical tokens or `$`.
- **Forgetting Endmarker `$` in Start Symbol:** Omitting `$` from $\text{FOLLOW}(S)$. Without `$`, the parser cannot detect legal end-of-input termination.
- **Stopping at the First Symbol When Computing $\text{FIRST}$:** In $A \to B C$, assuming $\text{FIRST}(A) = \text{FIRST}(B)$. If $B \Rightarrow^* \epsilon$, then all non-$\epsilon$ symbols of $\text{FIRST}(C)$ *must* also be added to $\text{FIRST}(A)$!

---

## Exam Relevance

In BUET CSE 309 examinations:
1. Calculating $\text{FIRST}$ and $\text{FOLLOW}$ sets is a guaranteed, mandatory 8-to-12 mark problem.
2. Explaining and applying the three formal rules for $\text{FOLLOW}$.
3. Using $\text{FIRST}$ and $\text{FOLLOW}$ to construct predictive LL(1) tables and SLR(1) tables.

---

## Related Concepts

- [[Context-Free Grammars and Syntax Analysis]]
- [[Elimination of Left Recursion and Left Factoring Algorithms]]
- [[Top-Down and Predictive Parsing]]
- [[LL(1) Parsing Table Construction and Execution Algorithm]]
- [[SLR(1) Parsing Table Construction and LR Parser Algorithm]]

---

## Prerequisites

- [[Context-Free Grammars and Syntax Analysis]]
- [[Elimination of Left Recursion and Left Factoring Algorithms]]

---

## Problems

- [[Problem — LL(1) Grammar Verification and Parsing Table Construction]]
- [[Problem — SLR(1) Table Construction and Conflict Analysis]]

---

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/Rumi Sir Merged.pdf|Rumi Sir Merged.pdf]], Chapter 4: Syntax Analysis (Slides 416–451).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 4.4 (pp. 220–222).

---

## Navigation

> 📖 **Reading Order:** Step 11 of 76 | **Module 3: Syntax Analysis & Top-Down Parsing**  
> ◄ **Previous:** [[Elimination of Left Recursion and Left Factoring Algorithms]] | ► **Next:** [[Top-Down and Predictive Parsing]]
