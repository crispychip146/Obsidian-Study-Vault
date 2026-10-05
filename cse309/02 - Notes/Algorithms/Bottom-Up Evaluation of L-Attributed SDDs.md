---
type: algorithm
course: cse309
status: active
order: 6
---

# Bottom-Up Evaluation of L-Attributed SDDs

> 📖 **Reading Order:** Step 6 of 55 | **Module 1:** Syntax-Directed Translation  
> ◄ **Previous:** [[Eliminating Left Recursion from SDTs]] | ► **Next:** [[Arithmetic Expression Desk Calculator SDD Example]]

---

## Building the idea

An LR parser normally acts on completed right-hand sides. An inherited value, however, may be needed **before** a child finishes parsing. Bottom-up evaluation must therefore make that value available at the appropriate earlier point.

For a declaration `D -> T L`, T's type is known when T has been reduced. A marker nonterminal with an empty production can execute an action before L, copying the available type into a place L's actions can access. In applicable schemes, an action can also read already available attribute values at known parser-stack offsets.

The marker does not consume input. Its role is scheduling a computation between grammar symbols. [[S-Attributed and L-Attributed SDDs]] explains the dependency constraint; the transformed grammar and LR states determine whether the required actions can actually be scheduled without parsing conflicts.

Draw the stack immediately before the action. Then locate the occurrence carrying the needed value. A memorized negative offset without that stack picture is easy to apply to the wrong occurrence.

## Deriving the stack accesses

Consider `D -> T L`, `T -> int | float`, and `L -> L , id | id`. T has a synthesized type; each identifier in L must receive it. The notation `top` below indexes semantic-value entries only, rather than an implementation's interleaved parser states.

Immediately before reducing `L -> id`, the relevant stack is:

| Relative index | Symbol | Available value |
|---|---|---|
| `top - 1` | T | The declaration type |
| `top` | id | The identifier entry |

The action needs the identifier at `top` and type at `top-1`. In Bison, however, dollar references are indexed relative to the **current right-hand side**, not directly by distance from the top. For this one-symbol rule, `$1` is id and `$0` is the symbol just before it, T. `$-1` would go one symbol farther back and is wrong for this stack.

For `L -> L , id`, the previously reduced L can carry the type as a synthesized value. Then `$1` is the earlier L and `$3` is the new identifier. An educational action scheme is:

```yacc
/* Schematic semantic values; declarations/types are omitted. */
L : ID         { $$ = $0; addType($1, $$); }
  | L ',' ID   { $$ = $1; addType($3, $1); }
  ;
```

The `$0` access is valid only when every relevant application of the base rule has the specified T immediately before it. Context-dependent stack access must be verified across all uses of L. This is why an attribute dependency is not safely implemented by memorizing a negative index.

## What an empty marker does

An action needed before a child is parsed can sometimes be represented by an empty marker production. In `S -> A B M L`, the stack before reducing `M -> epsilon` ends in A, B. At that point `$0` denotes B and `$-1` denotes A. The action can set M's new synthesized value to a value obtained from A; reducing M pushes that value rather than overwriting B.

Once M has been pushed, a later base reduction within L can access it at the justified contextual position. Draw the stack at the actual action time: when reducing a multi-symbol production for L, the distance from the top also depends on that production's right-hand-side length.

Markers schedule computations without consuming input. They can introduce LR conflicts, so attribute availability and parser compatibility must both be checked. The technique handles suitable L-attributed schemes; it is not an unconditional guarantee for every LR grammar/SDD combination.

## Complexity

### Time Complexity
With a valid evaluation schedule and constant-time actions, attribute work is linear in the number of production occurrences. Marker actions and stack offsets must preserve the required dependencies.

### Space Complexity
Attribute/parser-stack storage depends on the parse depth and retained values.

---

## Exam Relevance

---

### Exam Relevance

Frequently tested on final examinations via hand-simulation of Bottom-Up Evaluation of L-Attributed SDDs on given code fragments or graphs.

---

## What to carry forward

Check both attribute availability and parser compatibility. Adding empty markers can introduce LR conflicts, so the technique is not an unconditional conversion of every L-attributed definition to any bottom-up parser.

## Related notes

- [[S-Attributed and L-Attributed SDDs]]

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Chapter 5 (Slides 76–80).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 5.5.

- **Implementation clarification:** [GNU Bison semantic action indexing](https://www.gnu.org/software/bison/manual/html_node/Actions.html).
