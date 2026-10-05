---
type: algorithm
course: cse309
status: active
order: 17
---

# Backpatching Control-Flow Code Generation Algorithm

> 📖 **Reading Order:** Step 17 of 55 | **Module 2:** Intermediate Code Generation  
> ◄ **Previous:** [[Backpatching in Intermediate Code Generation]] | ► **Next:** [[Array Reference and Boolean Control-Flow TAC Generation Example]]

---

## Building the idea

Treat every pending jump as an unfinished edge in the control-flow graph. `makelist` records the edge's instruction position; `merge` combines edges needing the same destination; `backpatch` supplies that destination when it becomes known.

The Boolean rules follow [[Control Flow Translation and Boolean Expressions]]. AND connects the left true path to the right test; OR connects the left false path. Statement rules connect normal continuation, alternatives, and loop-back edges according to the source construct.

Trace a marker when encountered, before translating the following subtree. Its `quad` is the first instruction of that subtree, not the last one eventually emitted. For a loop, the condition start and body start are different boundaries, and confusing them can skip a test or create the wrong cycle.

The linear-time argument depends on constant-time list concatenation and each pending edge being traversed only a bounded number of times. A copying merge implementation can repeatedly traverse growing lists and lose that bound.

## How It Works

### SDT Specification: Boolean Expressions

Each boolean non-terminal $B$ synthesizes two lists of incomplete jump quads:
- `B.truelist`: Quads to branch to when $B$ evaluates to true.
- `B.falselist`: Quads to branch to when $B$ evaluates to false.

```
Production                        Semantic Actions
-----------------------------------------------------------------------------------------------------------------
1. B -> E1 relop E2               B.truelist  = makelist(nextquad);
                                  B.falselist = makelist(nextquad + 1);
                                  emit('if ' E1.addr relop.op E2.addr ' goto _');
                                  emit('goto _');

2. B -> B1 || M B2                backpatch(B1.falselist, M.quad);
                                  B.truelist  = merge(B1.truelist, B2.truelist);
                                  B.falselist = B2.falselist;

3. B -> B1 && M B2                backpatch(B1.truelist, M.quad);
                                  B.truelist  = B2.truelist;
                                  B.falselist = merge(B1.falselist, B2.falselist);

4. B -> ! B1                      B.truelist  = B1.falselist;
                                  B.falselist = B1.truelist;

5. B -> ( B1 )                    B.truelist  = B1.truelist;
                                  B.falselist = B1.falselist;

6. B -> true                      B.truelist  = makelist(nextquad);
                                  emit('goto _');

7. B -> false                     B.falselist = makelist(nextquad);
                                  emit('goto _');
```

---
### SDT Specification: Control-Flow Statements

Each statement non-terminal $S$ synthesizes:
- `S.nextlist`: A list of jump quads whose destination is the instruction immediately following $S$.

```
Production                                      Semantic Actions
-----------------------------------------------------------------------------------------------------------------
1. S -> id = E ;                                S.nextlist = [];
                                                emit(id.entry '=' E.addr);

2. S -> if ( B ) M S1                           backpatch(B.truelist, M.quad);
                                                S.nextlist = merge(B.falselist, S1.nextlist);

3. S -> if ( B ) M1 S1 N else M2 S2             backpatch(B.truelist, M1.quad);
                                                backpatch(B.falselist, M2.quad);
                                                temp = merge(S1.nextlist, N.nextlist);
                                                S.nextlist = merge(temp, S2.nextlist);

4. S -> while M1 ( B ) M2 S1                    backpatch(S1.nextlist, M1.quad);
                                                backpatch(B.truelist, M2.quad);
                                                S.nextlist = B.falselist;
                                                emit('goto ' M1.quad);

5. S -> S1 M S2                                 backpatch(S1.nextlist, M.quad);
                                                S.nextlist = S2.nextlist;

6. S -> { L }                                   S.nextlist = L.nextlist;
```

---

## Complexity

### Time Complexity
Why does backpatching remain so fast, even for programs with hundreds of thousands of nested control flow jumps?

### Theorem:
*The total computational time spent across all `makelist`, `merge`, and `backpatch` calls during the compilation of a program generating $N$ intermediate instructions is strictly $O(N)$.*

### Proof:
1. **`makelist` Complexity:**
   - Every call to `makelist` allocates a single-element list.
   - A `makelist` is called only when an instruction is emitted (`emit`).
   - For $N$ emitted instructions, `makelist` is invoked at most $2N$ times.
   - Total time: $O(N)$.
2. **`merge` Complexity:**
   - By implementing lists as singly-linked lists storing pointers to both `head` and `tail`, concatenating two lists takes $O(1)$ pointer assignments:
     $$\text{tail}_1.\text{next} = \text{head}_2; \quad \text{combined.tail} = \text{tail}_2;$$
   - Each grammatical reduction performs at most a constant number of merges.
   - Total time across all merges: $O(N)$.
3. **`backpatch` Complexity:**
   - A `backpatch(p, target)` call performs work proportional to $|p|$, the number of elements in list $p$.
   - **Crucial Invariant:** Every quad $q \in \{1, \dots, N\}$ represents an instruction with a single jump target field.
   - Once instruction $q$ is backpatched, its target slot is filled, and $q$ is removed from the active list.
   - **No instruction index $q$ is ever backpatched more than once.**
   - Therefore, the sum of lengths of all lists passed to `backpatch` across the entire compilation is bounded by the total number of jump instructions:
     $$\sum_{\text{all backpatch calls } k} |p_k| \le N$$
4. Summing the costs:
   $$T_{\text{total}} = T_{\text{makelist}} + T_{\text{merge}} + T_{\text{backpatch}} \le O(N) + O(N) + O(N) = O(N)$$
Thus, the entire backpatching overhead is strictly linear in the size of the generated code. $\blacksquare$

---

### Space Complexity
$O(N)$ auxiliary memory for data structures.

---

## Exam Relevance

---

### Exam Relevance

Frequently tested on final examinations via hand-simulation of Backpatching Control-Flow Code Generation Algorithm on given code fragments or graphs.

---

## What to carry forward

Prove correctness through list meaning: every member represents an exit of the indicated kind, and patching sends it to the required continuation. A completed trace is evidence for one input; the list invariant explains the general method.

## Related notes

- [[Control Flow Translation and Boolean Expressions]]

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Chapter 6 (Slides 186–193).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 6.7 (Backpatching).
