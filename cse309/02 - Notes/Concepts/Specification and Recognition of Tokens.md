---
type: concept
course: cse309
status: active
order: 5
---

# Specification and Recognition of Tokens

## Starting Point and the Problem

To build an automated scanner, we must formally answer two questions:
1. **Specification:** How do we mathematically define the set of valid character sequences for tokens like identifiers, integers, floats, and operators?
2. **Recognition:** How do we systematically convert those mathematical specifications into executable state transitions that consume characters and emit token-attribute pairs?

Because tokens have simple grammatical structures, we do not need heavy Context-Free Grammars to specify them. Instead, compiler writers use **Regular Expressions** for formal specification and **Transition Diagrams (Finite Automata)** for recognition.

---

## Developing the Core Idea: Mathematical Foundations

### 1. Alphabets, Strings, and Languages
- An **alphabet** $\Sigma$ is any finite set of symbols (e.g., ASCII or UTF-8).
- A **string** $s$ over an alphabet is a finite sequence of symbols chosen from $\Sigma$. The empty string is denoted $\epsilon$. The length $|s|$ is the number of symbols in $s$.
- A **language** $L$ over $\Sigma$ is any countable set of strings over $\Sigma$.

### 2. Operations on Languages
Given two languages $L$ and $M$:
1. **Union:** $L \cup M = \{s \mid s \in L \text{ or } s \in M\}$.
2. **Concatenation:** $LM = \{st \mid s \in L \text{ and } t \in M\}$.
3. **Kleene Closure:** $L^* = \bigcup_{i=0}^{\infty} L^i$ (zero or more occurrences of strings from $L$, where $L^0 = \{\epsilon\}$).
4. **Positive Closure:** $L^+ = \bigcup_{i=1}^{\infty} L^i = L L^*$ (one or more occurrences of strings from $L$).

---

## Regular Expressions and Regular Definitions

### 1. Regular Expressions (RE)
A regular expression $r$ describes a regular language $L(r)$:
- **Base Cases:**
  - $\epsilon$ is an RE denoting the language $\{\epsilon\}$.
  - For each $a \in \Sigma$, $a$ is an RE denoting the language $\{a\}$.
- **Inductive Steps:** If $r$ and $s$ are REs denoting $L(r)$ and $L(s)$:
  - $(r) \mid (s)$ denotes $L(r) \cup L(s)$.
  - $(r)(s)$ denotes $L(r)L(s)$.
  - $(r)^*$ denotes $(L(r))^*$.
  - $(r)$ denotes $L(r)$ (parentheses enforce precedence: $*$ binds tightest, then concatenation, then $\mid$).

### 2. Extension Shorthands
- **One or More ($+$):** $r^+ = r r^*$.
- **Zero or One ($?$):** $r? = r \mid \epsilon$.
- **Character Classes:** $[a-z]$ represents $a \mid b \mid \dots \mid z$. $[0-9]$ represents $0 \mid 1 \mid \dots \mid 9$.

### 3. Regular Definitions
To prevent writing unreadably huge regular expressions, a compiler defines named sub-patterns sequentially:
$$\begin{aligned}
d_1 &\to r_1 \\
d_2 &\to r_2 \\
&\;\;\vdots \\
d_n &\to r_n
\end{aligned}$$
where each $r_i$ is a regular expression over the alphabet $\Sigma \cup \{d_1, d_2, \dots, d_{i-1}\}$.

#### Example: Identifiers and Numbers in C
$$\begin{aligned}
\text{letter\_} &\to [a-zA-Z\_] \\
\text{digit} &\to [0-9] \\
\text{id} &\to \text{letter\_} (\text{letter\_} \mid \text{digit})^* \\
\text{digits} &\to \text{digit}^+ \\
\text{optionalFraction} &\to (\,.\, \text{digits})? \\
\text{optionalExponent} &\to ([eE] [+-]? \text{digits})? \\
\text{number} &\to \text{digits} \; \text{optionalFraction} \; \text{optionalExponent}
\end{aligned}$$

---

## Transition Diagrams: Visualizing State Machines

A **Transition Diagram** is a stylized flowchart depicting a finite automaton used by the scanner:
- **States:** Represented by circles. State $0$ is the designated start state.
- **Edges:** Directed arrows labeled by the character or character class that triggers the transition.
- **Accepting (Final) States:** Represented by double concentric circles. When reached, a complete token has been recognized.
- **Retraction Asterisk ($*$):** Placed adjacent to an accepting state to indicate that the lookahead pointer `forward` moved one character too far and must be retracted before returning the token.

### 1. Transition Diagram for Relational Operators (`relop`)

```
          '<'               '='
  ( 0 ) -------> ( 1 ) -----------> (( 2 ))  --> return(relop, LE)
    |              |
    |              |       '>'
    |              +--------------> (( 3 ))  --> return(relop, NE)
    |              |
    |              |      other
    |              +--------------> (( 4* )) --> return(relop, LT)
    |
    |     '='
    +------------> (( 5 ))          --> return(relop, EQ)
    |
    |     '>'               '='
    +------------> ( 6 ) ----------> (( 7 ))  --> return(relop, GE)
                   |
                   |      other
                   +--------------> (( 8* )) --> return(relop, GT)
```

- In State 1: If the next character is `=`, we reach accepting State 2 (`<=`). If `>`, we reach State 3 (`<>`). If any other character is read, we reach State 4 (`<`), which is marked with an asterisk ($*$) because that "other" character belongs to the *next* token and `forward` must be retracted.

---

## Transition Diagram for Identifiers and Keywords

```
          letter_                 letter_ or digit
  ( 9 ) -----------> ( 10 ) -----------------------------+
                       ^                                 |
                       +---------------------------------+
                       |
                       | other
                       v
                    (( 11* )) --> Check Keyword or return(id, entry_ptr)
```

### The Keyword Conflict: Is `then` an Identifier or a Keyword?
Because the regular definition for an identifier (`letter_ (letter_ | digit)*`) matches reserved keywords like `if`, `else`, `while`, and `then`, a scanner must prevent keywords from being classified as user variables.

There are two primary architectural strategies:

#### Strategy 1: Hardcoded Transition Diagrams
- Create separate, dedicated paths in the state machine for every keyword before branching to the generic identifier state.
- **Drawback:** Greatly increases the number of states and transitions in the finite automaton.

#### Strategy 2: Pre-seeded Symbol Table Lookup (Standard Approach)
- The scanner uses the single generic transition diagram above to recognize any sequence of letters and digits.
- When accepting State 11 is reached, the scanner looks up the recognized string in the **Symbol Table**.
- The symbol table is **pre-populated at compiler startup** with all language reserved words (their table records indicate they are keywords).
- If the lexeme is found in the table as a reserved word, the scanner returns the specific keyword token (e.g., `⟨if, NULL⟩` or `⟨then, NULL⟩`).
- If not found, it is a genuine user identifier; the scanner inserts it into the symbol table and returns `⟨id, ptr⟩`.

---

## Transition Diagram for Numbers

Handling unsigned integers, floating-point decimals, and scientific exponential notation requires handling multiple optional components:

```
          digit                  digit
  ( 12 ) -------> ( 13 ) ----------------------+
                    |    ^                     |
                    |    +---------------------+
                    |
                    |  '.'            digit
                    +--------> ( 14 ) ------> ( 15 ) ----+  digit
                    |                           |        |  (loop)
                    |                           +--------+
                    |  'E' or 'e'               |
                    +---------------------------+  'E' or 'e'
                    |                           |
                    v                           v
                  ( 16 ) <----------------------+
                    |
                    |  '+' or '-'         digit
                    +------------> ( 17 ) ------> ( 18 ) ----+
                    |                               ^        |  digit
                    |  digit                        +--------+  (loop)
                    +-------------------------------+
                                                    |
                                                    | other
                                                    v
                                                 (( 19* )) --> return(number, val)
```

- Notice State 19 has an asterisk ($*$) because the transition out of the digit-loop in State 18 occurs upon reading a non-digit delimiter (such as a semicolon, space, or operator).

---

## Programmatic Implementation of Transition Diagrams

How are transition diagrams translated into production code? A transition diagram is mechanically converted into a state machine function (e.g., `getRelop()`) using an integer `state` variable and a `switch-case` construct:

```cpp
TOKEN getRelop() {
    int state = 0;
    char c;
    while (true) {
        switch (state) {
            case 0:
                c = nextchar();
                if (c == '<')      state = 1;
                else if (c == '=') state = 5;
                else if (c == '>') state = 6;
                else fail(); // Not a relop; reset pointer and try next diagram
                break;
            case 1:
                c = nextchar();
                if (c == '=') return TOKEN(RELOP, LE);      // State 2 (no retraction)
                else if (c == '>') return TOKEN(RELOP, NE); // State 3 (no retraction)
                else {
                    retract();                              // State 4* (retract 1 char)
                    return TOKEN(RELOP, LT);
                }
            case 6:
                c = nextchar();
                if (c == '=') return TOKEN(RELOP, GE);      // State 7 (no retraction)
                else {
                    retract();                              // State 8* (retract 1 char)
                    return TOKEN(RELOP, GT);
                }
            // Additional states ...
        }
    }
}
```

### The Function of `fail()` and Pointer Resetting
If the current character cannot transition from state 0 to any valid edge in `getRelop()`, the scanner invokes `fail()`:
1. `fail()` resets the `forward` pointer back to `lexemeBegin`:
   $$\text{forward} = \text{lexemeBegin}$$
2. The scanner then activates the *next* candidate transition diagram (e.g., trying identifiers or numbers).
3. If all candidate transition diagrams fail to match, `fail()` invokes lexical error recovery (e.g., character skipping in panic mode).

---

## Architectural Strategies for Orchestrating Multiple Transition Diagrams

In a full compiler, the lexical analyzer must recognize dozens of token types (identifiers, numbers, relational operators, arithmetic operators, keywords). There are three fundamental architectural strategies to organize these diagrams:

1. **Sequential Trial:**
   - The scanner attempts transition diagrams one by one (e.g., first test `relop`, if `fail()` then test `id`, if `fail()` then test `number`).
   - *Limitation:* Inefficient; requires repeatedly resetting the `forward` pointer back to `lexemeBegin` and re-reading characters from the buffer.
2. **Parallel Trial:**
   - The scanner feeds the current input character simultaneously into multiple active transition diagrams, tracking which ones remain in valid states.
   - *Limitation:* Requires maintaining multiple active state threads or bitmaps.
3. **Combined Single Transition Diagram (Standard / Lex Approach):**
   - The compiler merges all individual transition diagrams into a single, unified Deterministic Finite Automaton (DFA) with a single shared start state.
   - Because no two token patterns in the merged DFA have conflicting transitions from the start state (or conflicts are resolved via maximal munch and priority rules), the scanner reads each character **strictly once** without resetting pointers.

---

## Important Properties and Guarantees

- **Maximal Munch Property:** When navigating transition diagrams, scanners greedily consume characters as long as transitions exist to find the longest matching lexeme (e.g., recognizing `==` rather than two separate `=` tokens).
- **Correctness Invariant:** Retraction must preserve the integrity of `lexemeBegin`. The distance retracted cannot exceed the start of the current token.

---

## Common Mistakes

- **Forgetting the Retraction on Accepting States:** Emitting an identifier or `<` without retracting `forward`. If `x = 5` is scanned, omitting retraction consumes the `=` character into the identifier token, causing a syntax error on the subsequent token!
- **Hardcoding Keywords as Fixed Tokens:** Attempting to write regular expressions that match "any identifier except keywords" using negative lookahead. This results in monstrously complex regular expressions. Using Symbol Table lookup is vastly simpler and asymptotically identical in speed.

---

## Exam Relevance

In BUET CSE 309 examinations:
1. Writing regular definitions for identifiers, unsigned numbers, and string literals.
2. Drawing exact transition diagrams (with labeled states, start states, accepting states, and $*$ retraction marks) for relational operators and numeric constants.
3. Describing the pre-seeded symbol table method for keyword disambiguation.

---

## Related Concepts

- [[Lexical Analysis Fundamentals]]
- [[Input Buffering and Sentinel Techniques]]
- [[Lexical Analyzer Generation with Lex]]
- [[Token Recognition and Lex Specification Example]]

---

## Prerequisites

- [[Lexical Analysis Fundamentals]]
- [[Input Buffering and Sentinel Techniques]]

---

## Problems

- [[Problem — Lexical Token Recognition and Buffer Simulation]]

---

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/Rumi Sir Merged.pdf|Rumi Sir Merged.pdf]], Chapter 3: Lexical Analysis (Slides 181–244).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 3.3–3.4 (pp. 120–140).

---

## Navigation

> 📖 **Reading Order:** Step 05 of 76 | **Module 2: Lexical Analysis**  
> ◄ **Previous:** [[Input Buffering and Sentinel Techniques]] | ► **Next:** [[Lexical Analyzer Generation with Lex]]
