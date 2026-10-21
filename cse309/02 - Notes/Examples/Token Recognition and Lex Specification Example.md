---
type: example
course: cse309
status: active
order: 7
---

# Token Recognition and Lex Specification Example

## Starting Point and the Problem

To solidify how the theoretical principles of regular definitions, transition diagrams, and Lex are applied in real compiler engineering, we explore two concrete implementations for token recognition:
1. **A Hand-Coded C++ Scanner Routine:** Simulating state transitions with explicit character reading and lookahead retraction.
2. **A Production Lex/Flex Specification:** Implementing an automated scanner that processes relational operators, keywords, identifiers, and numeric literals with symbol table integration.

---

## Part 1: Hand-Coded Transition Diagram Simulation in C++

Consider the transition diagram for relational operators (`relop`):
- `<` $\to$ State 1 $\to$ if `=` then State 2 (return `LE`); if `>` then State 3 (return `NE`); if other then State 4 (retract and return `LT`).
- `=` $\to$ State 5 (return `EQ`).
- `>` $\to$ State 6 $\to$ if `=` then State 7 (return `GE`); if other then State 8 (retract and return `GT`).

Here is the robust C++ implementation simulating the diagram using a `switch` state machine:

```cpp
#include <iostream>
#include <string>

enum TokenType { RELOP, ID, NUMBER, ERROR, END_OF_FILE };
enum RelopAttr { LT, LE, EQ, NE, GT, GE };

struct Token {
    TokenType type;
    int attribute;
};

// Global input buffer pointer functions
char nextChar();
void retract();

Token getRelop() {
    int state = 0;
    char c;

    while (true) {
        switch (state) {
            case 0:
                c = nextChar();
                if (c == '<') state = 1;
                else if (c == '=') state = 5;
                else if (c == '>') state = 6;
                else return { ERROR, 0 };
                break;

            case 1:
                c = nextChar();
                if (c == '=') return { RELOP, LE }; // State 2: no retraction needed
                else if (c == '>') return { RELOP, NE }; // State 3: no retraction needed
                else {
                    retract(); // State 4: read one character too far!
                    return { RELOP, LT };
                }

            case 5:
                return { RELOP, EQ };

            case 6:
                c = nextChar();
                if (c == '=') return { RELOP, GE }; // State 7: no retraction needed
                else {
                    retract(); // State 8: read one character too far!
                    return { RELOP, GT };
                }

            default:
                return { ERROR, 0 };
        }
    }
}
```

### Trace of Retraction
Suppose the input stream contains:
```text
x < y
```
1. After scanning `x`, the scanner enters `getRelop()` in state 0.
2. It skips spaces and reads `<` $\implies$ transitions to `state = 1`.
3. In state 1, it reads `nextChar()`, which is `' '` (space).
4. Since `' '` is neither `'='` nor `'>'`, it enters the `else` branch:
   - Calls `retract()`, pushing the space pointer back so the next token scan will see it.
   - Returns `{ RELOP, LT }`.

---

## Part 2: Complete Runnable Lex Specification (`scanner.l`)

Below is the complete, self-contained Lex specification file recognizing relational operators, keywords (`if`, `then`, `else`), identifiers, and numbers.

```lex
%{
/* =========================================================================
   Declarations Section: C headers, Token Codes, and Data Structures
   ========================================================================= */
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define LT      301
#define LE      302
#define EQ      303
#define NE      304
#define GT      305
#define GE      306
#define IF      310
#define THEN    311
#define ELSE    312
#define ID      320
#define NUMBER  330

// Value carrier for semantic attributes
typedef union {
    int intVal;
    double floatVal;
    char* strVal;
    int attrVal;
} YYSTYPE;

YYSTYPE yylval;

// Prototype for symbol table helper
int installID();
int installNum();
%}

/* =========================================================================
   Regular Definitions
   ========================================================================= */
delim       [ \t\n]
ws          {delim}+
letter      [A-Za-z_]
digit       [0-9]
id          {letter}({letter}|{digit})*
number      {digit}+(\.{digit}+)?([eE][+-]?{digit}+)?

/* =========================================================================
   Translation Rules
   ========================================================================= */
%%

{ws}        { /* No action: whitespace is silently discarded */ }

"if"        { return IF; }
"then"      { return THEN; }
"else"      { return ELSE; }

"<"         { yylval.attrVal = LT; return RELOP; }
"<="        { yylval.attrVal = LE; return RELOP; }
"="         { yylval.attrVal = EQ; return RELOP; }
"<>"        { yylval.attrVal = NE; return RELOP; }
">"         { yylval.attrVal = GT; return RELOP; }
">="        { yylval.attrVal = GE; return RELOP; }

{id}        { 
                yylval.strVal = strdup(yytext); 
                installID(); 
                return ID; 
            }

{number}    { 
                yylval.floatVal = atof(yytext); 
                return NUMBER; 
            }

.           { printf("Lexical Error at line: illegal character '%s'\n", yytext); }

%%

/* =========================================================================
   Auxiliary Functions: Symbol Table Management & Driver
   ========================================================================= */

#define HASH_SIZE 1024

typedef struct SymbolNode {
    char* name;
    int tokenType;
    struct SymbolNode* next;
} SymbolNode;

SymbolNode* symbolTable[HASH_SIZE];

unsigned int hash(const char* s) {
    unsigned int h = 0;
    while (*s) h = (h << 5) + *s++;
    return h % HASH_SIZE;
}

int installID() {
    unsigned int h = hash(yytext);
    SymbolNode* node = symbolTable[h];
    while (node) {
        if (strcmp(node->name, yytext) == 0) {
            return h; // Already in table
        }
        node = node->next;
    }
    // Insert new identifier
    SymbolNode* newNode = (SymbolNode*)malloc(sizeof(SymbolNode));
    newNode->name = strdup(yytext);
    newNode->tokenType = ID;
    newNode->next = symbolTable[h];
    symbolTable[h] = newNode;
    return h;
}

int yywrap() {
    return 1; // Indicate end of input on EOF
}

int main() {
    printf("Lexical Analyzer Initialized. Enter source code (Ctrl+D to end):\n");
    int token;
    while ((token = yylex())) {
        switch (token) {
            case IF:     printf("TOKEN: <IF>\n"); break;
            case THEN:   printf("TOKEN: <THEN>\n"); break;
            case ELSE:   printf("TOKEN: <ELSE>\n"); break;
            case RELOP:  printf("TOKEN: <RELOP, attr=%d>\n", yylval.attrVal); break;
            case ID:     printf("TOKEN: <ID, name=\"%s\">\n", yylval.strVal); break;
            case NUMBER: printf("TOKEN: <NUMBER, val=%f>\n", yylval.floatVal); break;
        }
    }
    return 0;
}
```

---

## Step-by-Step Lex Execution Trace

Suppose the above Lex program receives the input:
```c
if count <= 3.14159e2 then
```

### Trace Walkthrough

| Step | Remaining Input | Matched Lexeme (`yytext`) | Applicable Rules | Conflict Resolution | Emitted Token |
|:---:|---|---|---|---|---|
| **1** | `if count <= ...` | `"if"` | `"if"` vs `{id}` | **First Rule Match:** `"if"` appears before `{id}` | `⟨IF⟩` |
| **2** | ` count <= ...` | `" "` | `{ws}` | Discarded | *(None)* |
| **3** | `count <= ...` | `"count"` | `{id}` | Only `{id}` matches | `⟨ID, "count"⟩` |
| **4** | ` <= ...` | `" "` | `{ws}` | Discarded | *(None)* |
| **5** | `<= 3.14159e2...` | `"<="` | `"<"` vs `"<="` | **Longest Match:** `"<="` is 2 chars, `"<"` is 1 char | `⟨RELOP, LE⟩` |
| **6** | ` 3.14159e2...` | `" "` | `{ws}` | Discarded | *(None)* |
| **7** | `3.14159e2 then` | `"3.14159e2"` | `{number}` | Longest match absorbs digits, dot, digits, exp | `⟨NUMBER, 314.159⟩` |
| **8** | ` then` | `" "` | `{ws}` | Discarded | *(None)* |
| **9** | `then` | `"then"` | `"then"` vs `{id}` | **First Rule Match:** `"then"` appears before `{id}` | `⟨THEN⟩` |

---

## Important Properties and Insights

- **Greedy Matching is Essential:** In Step 5, both `<` and `<=` could match. The Longest Match rule prevents splitting `<=` into `<` and `=`.
- **Order Preservation for Keywords:** In Step 1 and Step 9, placing `"if"` and `"then"` above `{id}` guarantees that keywords are intercepted and never erroneously installed as variables in the Symbol Table.

---

## Common Mistakes

- **Memory Leaks in `strdup(yytext)`:** In production compilers, strings should be stored in an arena allocator or symbol table rather than blindly allocated on the heap without freeing.
- **Forgetting Exponential Signs:** Defining numbers as `[0-9]+(\.[0-9]+)?([eE][0-9]+)?` misses signed exponents like `1.5e-4` or `2.0E+10`. Always include `[+-]?` after `[eE]`.

---

## Exam Relevance

In BUET CSE 309 examinations:
1. Writing complete Lex specifications for custom token sets with pattern-action pairs.
2. Explaining step-by-step why an input like `if_flag` is recognized as an identifier while `if` is recognized as a keyword.
3. Tracing the state transitions of hand-coded lexical routines.

---

## Related Concepts

- [[Lexical Analysis Fundamentals]]
- [[Specification and Recognition of Tokens]]
- [[Lexical Analyzer Generation with Lex]]
- [[Problem — Lexical Token Recognition and Buffer Simulation]]

---

## Prerequisites

- [[Lexical Analysis Fundamentals]]
- [[Specification and Recognition of Tokens]]
- [[Lexical Analyzer Generation with Lex]]

---

## Problems

- [[Problem — Lexical Token Recognition and Buffer Simulation]]

---

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/Rumi Sir Merged.pdf|Rumi Sir Merged.pdf]], Chapter 3: Lexical Analysis (Slides 215–282).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 3.5 (pp. 140–151).

---

## Navigation

> 📖 **Reading Order:** Step 07 of 76 | **Module 2: Lexical Analysis**  
> ◄ **Previous:** [[Lexical Analyzer Generation with Lex]] | ► **Next:** [[Problem — Lexical Token Recognition and Buffer Simulation]]
