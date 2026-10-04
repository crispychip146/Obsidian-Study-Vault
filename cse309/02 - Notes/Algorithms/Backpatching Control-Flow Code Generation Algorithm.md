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

---

---

---

---

## The Problem and Earlier Tools

The **Backpatching Algorithm** generates intermediate Three-Address Code (represented as quadruples or tuples indexed by integers called **quads**) for boolean expressions and control-flow statements in a **single pass**.

### 1.1 Global State & Primitives
The compiler maintains:
1. `quad[]`: An array of instruction tuples `(op, arg1, arg2, target)`.
2. `nextquad`: An integer counter initialized to `1` (or `100`), pointing to the array slot where the next instruction will be placed.

```python
class BackpatchEngine:
    def __init__(self, start_quad=100):
        self.quad = {}           # quad_index -> instruction_string
        self.nextquad = start_quad

    def emit(self, instruction_template: str) -> int:
        """Emits an instruction with an optional placeholder '_' for target.
        Returns the quad index where the instruction was recorded."""
        q = self.nextquad
        self.quad[q] = instruction_template
        self.nextquad += 1
        return q

    def makelist(self, i: int) -> list:
        """Creates and returns a new list containing single instruction quad i."""
        return [i]

    def merge(self, p1: list, p2: list) -> list:
        """Concatenates two lists of quads. Cost: O(1) if using linked lists."""
        return p1 + p2

    def backpatch(self, p: list, target_quad: int) -> None:
        """Patches target_quad into every instruction on list p."""
        for q in p:
            # Replace placeholder '_' with the concrete target quad
            self.quad[q] = self.quad[q].replace('_', str(target_quad))
```

---

---

---

---

---

## Developing the Core Idea

Why must the grammar include marker non-terminals $M$ and $N$?
- In a bottom-up LR parser, semantic actions execute only when a production **reduces**.
- If an action needs to fire in the middle of a production (for instance, recording `nextquad` right before statement $S_1$ begins), an ordinary production cannot execute code until the entire statement finishes!
- Inserting an $\epsilon$-marker non-terminal $M \to \epsilon$ forces the parser to reduce $M$ the exact moment the tokens preceding $M$ are consumed.

### The Two Standard Markers:
1. **Address Snapshot Marker $M$:**
   $$M \longrightarrow \epsilon \quad \{ M.quad = nextquad; \}$$
2. **Unconditional Escape Marker $N$:**
   $$N \longrightarrow \epsilon \quad \{ N.nextlist = \text{makelist}(nextquad); \; \text{emit}(\text{'goto _'}); \}$$

---

---

---

---

---

## Inputs

- Intermediate representation (Three-Address Code instructions, parse tree nodes, live intervals, or interference graph).

---

## Outputs

- Partitioned blocks, DAG nodes, allocated physical registers, or evacuated memory blocks.

---

## How It Works

### Inputs

- Intermediate representation (Three-Address Code instructions, parse tree nodes, live intervals, or interference graph).

---
### Outputs

- Partitioned blocks, DAG nodes, allocated physical registers, or evacuated memory blocks.

---
### How It Works

### Inputs

- Intermediate representation (Three-Address Code instructions, parse tree nodes, live intervals, or interference graph).

---
### Outputs

- Partitioned blocks, DAG nodes, allocated physical registers, or evacuated memory blocks.

---
### How It Works

### Inputs

- Intermediate representation (Three-Address Code instructions, parse tree nodes, live intervals, or interference graph).

---
### Outputs

- Partitioned blocks, DAG nodes, allocated physical registers, or evacuated memory blocks.

---
### How It Works

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

---
### Properties

- **Termination:** Provably terminates on all well-formed compiler inputs.
- **Correctness:** Preserves the underlying language semantics and program data dependencies.

---
### Related Concepts

- [[Basic Blocks and Control Flow Graphs]]
- [[Live Ranges and Live Intervals in Register Allocation]]
- [[Register Interference Graphs and Graph Coloring Principles]]

---
### Prerequisites

- [[Basic Blocks and Control Flow Graphs]]

---
### Problems

- [[Problem — Linear Scan Register Allocation Simulation]]
- [[Problem — Chaitin Graph Coloring Register Allocation]]

---

---
### Properties

- **Termination:** Provably terminates on all well-formed compiler inputs.
- **Correctness:** Preserves the underlying language semantics and program data dependencies.

---
### Related Concepts

- [[Basic Blocks and Control Flow Graphs]]
- [[Live Ranges and Live Intervals in Register Allocation]]
- [[Register Interference Graphs and Graph Coloring Principles]]

---
### Prerequisites

- [[Basic Blocks and Control Flow Graphs]]

---
### Problems

- [[Problem — Linear Scan Register Allocation Simulation]]
- [[Problem — Chaitin Graph Coloring Register Allocation]]

---

---
### Properties

- **Termination:** Provably terminates on all well-formed compiler inputs.
- **Correctness:** Preserves the underlying language semantics and program data dependencies.

---
### Related Concepts

- [[Basic Blocks and Control Flow Graphs]]
- [[Live Ranges and Live Intervals in Register Allocation]]
- [[Register Interference Graphs and Graph Coloring Principles]]

---
### Prerequisites

- [[Basic Blocks and Control Flow Graphs]]

---
### Problems

- [[Problem — Linear Scan Register Allocation Simulation]]
- [[Problem — Chaitin Graph Coloring Register Allocation]]

---

---

## Pseudocode

### Pseudocode

### Pseudocode

### Pseudocode

The complete algorithmic procedure is detailed in the sections above.

---

---

---

---

## Example

Concrete step-by-step simulations and traces are cataloged in the associated Example and Problem notes.

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

---

---

---

## Properties

- **Termination:** Provably terminates on all well-formed compiler inputs.
- **Correctness:** Preserves the underlying language semantics and program data dependencies.

---

## Limitations

### Limitations

### Limitations

### Limitations

- Conservative heuristics may yield suboptimal allocations or require register spilling when demand exceeds hardware resources.

---

---

---

---

## Common Mistakes

### Common Mistakes

### Common Mistakes

### Common Mistakes

- Forgetting to update liveness information or next-use pointers.
- Misinterpreting index bounds during stack or interval scans.

---

---

---

---

## Exam Relevance

### Example

Concrete step-by-step simulations and traces are cataloged in the associated Example and Problem notes.

---
### Exam Relevance

### Example

Concrete step-by-step simulations and traces are cataloged in the associated Example and Problem notes.

---
### Exam Relevance

### Example

Concrete step-by-step simulations and traces are cataloged in the associated Example and Problem notes.

---
### Exam Relevance

Frequently tested on final examinations via hand-simulation of Backpatching Control-Flow Code Generation Algorithm on given code fragments or graphs.

---

---

---

---

## Related Concepts

- [[Basic Blocks and Control Flow Graphs]]
- [[Live Ranges and Live Intervals in Register Allocation]]
- [[Register Interference Graphs and Graph Coloring Principles]]

---

## Prerequisites

- [[Basic Blocks and Control Flow Graphs]]

---

## Problems

- [[Problem — Linear Scan Register Allocation Simulation]]
- [[Problem — Chaitin Graph Coloring Register Allocation]]

---

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Chapter 6 (Slides 186–193).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 6.7 (Backpatching).
