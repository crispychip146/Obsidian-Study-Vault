---
type: problem
course: cse309
status: active
order: 8
---

# Problem — Lexical Token Recognition and Buffer Simulation

## Problem Statement

### Context
In the lexical analyzer of a C compiler, input characters are loaded into a buffer pair where each buffer has a capacity of $N = 16$ characters, augmented with an `EOF` sentinel at the end of each buffer half (total size $16 + 1 = 17$ bytes per half). 

The source program begins with the C code fragment:
```c
long long int x = 3.14159e+02;
```

---

### Question 1: Buffer Pair and Pointer Trace
Suppose the 31-character source code fragment is loaded from disk into memory. 
1. Draw the exact contents of Buffer 1 and Buffer 2, including the sentinel `EOF` positions.
2. Trace the movements of pointers `lexemeBegin` and `forward` as the scanner recognizes the first two tokens (`long` and `long`). State the exact character index of `lexemeBegin` and `forward` when the first token is emitted.
3. Explain what happens when `forward` encounters the `EOF` sentinel at the end of Buffer 1.

---

### Question 2: Token-Attribute Sequence
For the entire statement `long long int x = 3.14159e+02;`:
1. Write the exact sequential stream of tokens and attribute values emitted to the syntax analyzer.
2. Distinguish between keywords, user identifiers, operators, and numeric literals in the attribute representation.

---

### Question 3: Pointer Retraction ($*$) Analysis
In the transition diagram for numbers and identifiers:
1. Explain why the scanner must perform pointer retraction ($*$) when scanning the identifier `x` followed by ` = `.
2. Explain why the scanner must perform pointer retraction ($*$) when scanning the numeric literal `3.14159e+02` followed by `;`.
3. What catastrophic error would occur in the syntax analyzer if pointer retraction were omitted by the scanner?

---

### Question 4: Lex Conflict Resolution
Consider the inputs:
- Input A: `integrate`
- Input B: `int x`

Given the Lex specification:
```lex
"int"       { return INT; }
{letter}+   { return ID; }
```
Explain using formal Lex conflict resolution rules (**Longest Match** and **First Rule Match**) why Input A is returned as `ID` while in Input B the substring `int` is returned as keyword `INT`.

---

## Detailed Solution and Walkthrough

### Part 1: Buffer Layout and Pointer Simulation

#### 1. Buffer Pair Memory Layout ($N = 16$ characters)
The source string has 30 characters:
`l o n g <sp> l o n g <sp> i n t <sp> x <sp> = <sp> 3 . 1 4 1 5 9 e + 0 2 ;`

- **Buffer 1 (Indices 0 to 16):** Receives the first 16 characters followed by sentinel `EOF`:
  ```
  Index:   0   1   2   3   4   5   6   7   8   9  10  11  12  13  14  15   16
  Char:  | l | o | n | g |   | l | o | n | g |   | i | n | t |   | x |   | EOF |
  ```
- **Buffer 2 (Indices 17 to 33):** Receives the next 14 characters followed by sentinel `EOF`:
  ```
  Index:  17  18  19  20  21  22  23  24  25  26  27  28  29  30   31   32   33
  Char:  | = |   | 3 | . | 1 | 4 | 1 | 5 | 9 | e | + | 0 | 2 | ; |    |    | EOF |
  ```

#### 2. Tracing Pointers for First Token (`long`)
1. **Initial State:** `lexemeBegin = 0`, `forward = 0`.
2. `forward` scans `l` (idx 0), `o` (idx 1), `n` (idx 2), `g` (idx 3).
3. In state 10 (identifying letters/digits), `forward` advances to index 4 (`' '`, space).
4. Since space is neither a letter nor a digit, the state machine transitions to accepting State 11$*$.
5. **Retraction:** `forward` is retracted by 1 position (from index 4 back to index 3).
6. **Emission:** The lexeme between `lexemeBegin` (0) and `forward` (3) is `"long"`.
   - The scanner looks up `"long"` in the symbol table, discovers it is a reserved word, and emits `⟨LONG, NULL⟩`.
7. **Preparation for Next Token:** `lexemeBegin` is advanced to index 4, whitespace is skipped, and `lexemeBegin` lands at index 5.

#### 3. Encountering Sentinel `EOF`
At index 15, `forward` points to `' '`. Next, `forward` advances to index 16 where it reads `EOF`.
- Because `forward` is at the end of Buffer 1, the scanner recognizes this `EOF` as a buffer boundary sentinel, NOT the end of the file.
- It immediately invokes the operating system read to ensure Buffer 2 is populated and sets `forward = 17` (the first byte of Buffer 2).
- The transition loop continues seamlessly without interrupting token recognition.

---

### Part 2: Emitted Token-Attribute Sequence

For `long long int x = 3.14159e+02;`:

| Step | Lexeme | Token Name | Attribute Value | Semantic Interpretation |
|:---:|---|---|---|---|
| **1** | `"long"` | `LONG` | `NULL` | Primitive type keyword |
| **2** | `"long"` | `LONG` | `NULL` | Type modifier keyword |
| **3** | `"int"` | `INT` | `NULL` | Base integer type keyword |
| **4** | `"x"` | `ID` | `ptr_to_symtab("x")` | User-defined variable identifier |
| **5** | `"="` | `ASSIGN_OP` | `NULL` | Assignment operator |
| **6** | `"3.14159e+02"` | `NUMBER` | `314.159` (float) | Floating-point literal ($3.14159 \times 10^2$) |
| **7** | `";"` | `;` | `NULL` | Statement delimiter |

---

### Part 3: Pointer Retraction ($*$) Analysis

#### 1. Retraction on Identifier `x`
- After scanning `x`, `forward` reads the space character `' '`.
- The identifier state machine cannot know that `x` has ended until it sees a non-identifier character (the space).
- The space character belongs to the delimiter stream, not the variable name.
- Thus, the scanner must **retract** `forward` by one position back to `x` so that the space is not consumed by the token.

#### 2. Retraction on Number `3.14159e+02`
- In state 18 (accumulating digits of the exponent), `forward` reads the semicolon `;` at index 30.
- A semicolon is not a digit. The scanner concludes that the numeric literal has terminated and transitions to accepting State 19$*$.
- `forward` must be retracted by one character (from index 30 back to 29). This ensures that the semicolon `;` remains in the input buffer to be read as the subsequent statement-terminating token.

#### 3. Catastrophic Effect if Retraction is Omitted
If retraction were omitted:
- The semicolon `;` would be consumed as part of the number token.
- When the parser calls `getNextToken()` expecting a semicolon to complete the declaration, the input stream would already be empty (`EOF`).
- The parser would trigger an unexpected syntax error: `"Syntax Error: expected ';' before end of file"`.

---

### Part 4: Lex Conflict Resolution Principles

```lex
"int"       { return INT; }
{letter}+   { return ID; }
```

#### 1. Analysis of Input A: `integrate`
- Pattern `"int"` matches a prefix of length 3 (`int`).
- Pattern `{letter}+` matches all 9 characters (`integrate`).
- **Conflict Resolution:** **Rule 1 (Longest Match / Maximal Munch)** takes absolute precedence over rule ordering. Because 9 characters is strictly longer than 3 characters, Lex chooses `{letter}+` and returns token `ID`.

#### 2. Analysis of Input B: `int x`
- When scanning the first word `int`:
  - Pattern `"int"` matches 3 characters (`int`).
  - Pattern `{letter}+` also matches 3 characters (`int`).
- **Conflict Resolution:** Since both patterns match the exact same longest prefix length (3 characters), Lex applies **Rule 2 (First Rule Match / Priority)**.
- Because rule `"int"` appears *before* `{letter}+` in the specification file, Lex chooses `"int"` and emits keyword token `INT`.

---

## Important Properties and Key Takeaways

1. **Sentinels Turn Two Checks into One:** In every dual-buffer system, testing `*forward == EOF` eliminates the need to check `forward >= buffer_end` on every single character.
2. **Retraction is Inevitable for Delimiter-Free Tokens:** Whenever the language grammar allows identifiers or numbers to be immediately followed by operators (e.g., `x+1` or `x=y`), lookahead retraction is mandatory.
3. **Lex Order Matters for Disambiguation:** Always place specific keywords *above* generic identifier expressions in Lex/Flex specifications.

---

## Related Concepts

- [[Lexical Analysis Fundamentals]]
- [[Input Buffering and Sentinel Techniques]]
- [[Specification and Recognition of Tokens]]
- [[Lexical Analyzer Generation with Lex]]

---

## Prerequisites

- [[Input Buffering and Sentinel Techniques]]
- [[Specification and Recognition of Tokens]]
- [[Lexical Analyzer Generation with Lex]]

---

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/Rumi Sir Merged.pdf|Rumi Sir Merged.pdf]], Chapter 3: Lexical Analysis (Slides 166–210, 245–282).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 3.2–3.5 (pp. 115–151).

---

## Navigation

> 📖 **Reading Order:** Step 08 of 76 | **Module 2: Lexical Analysis**  
> ◄ **Previous:** [[Token Recognition and Lex Specification Example]] | ► **Next:** [[Context-Free Grammars and Syntax Analysis]]
