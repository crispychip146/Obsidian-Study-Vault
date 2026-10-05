---
type: algorithm
course: cse309
status: active
order: 11
---

# Value-Number Method for DAG Construction

> 📖 **Reading Order:** Step 11 of 55 | **Module 2:** Intermediate Code Generation  
> ◄ **Previous:** [[Intermediate Representations and Three-Address Code]] | ► **Next:** [[Type Expressions and Storage Layout]]

---

## Building the idea

Seeing the same text twice does not guarantee seeing the same value twice: an operand might have been reassigned. Value numbering gives names to **values** so the compiler can recognize repeated computations using the right identities.

For a pure binary operation, form a signature from its operator and the value numbers of its children. Look it up in a table. If the signature exists, reuse that node; otherwise allocate a node and number. When a variable is assigned, update which value its name denotes.

An illustrative `a+b` can reuse an earlier node only while a and b denote the same operand values and the operation's semantics permit reuse. This extends [[Abstract Syntax Tree Construction with SDDs]] into a DAG: two parents can refer to one computed child.

The correctness argument follows structure. Equal leaf identities denote equal values; matching operators applied to matching child values denote the same expression under the stated pure-operation model. Effects, memory changes, and exceptional behavior require additional checks.

## How It Works

### Advanced Compiler Extensions: Commutative Value Numbering

What happens if the code has:
$$x = a + b; \quad y = b + a;$$
In a naive value-number method:
- $a + b$ produces key `('+', 1, 2)` $\implies$ Value Number 3.
- $b + a$ produces key `('+', 2, 1)` $\implies$ Key not found! Creates redundant Value Number 4!

### Commutative normalization and its limits

For an operation known to be commutative under the IR semantics, canonicalize its operand value numbers, for example by placing the smaller first. This lets `a+b` and `b+a` share a signature in a suitable pure arithmetic model.

Do not reorder source-language short-circuit operands or computations with side effects. Floating-point, overflow, exception, and memory semantics must also be respected. The optimization acts on justified value identities, not arbitrary matching text.

### What the method proves

Under the deterministic pure-operation model, matching signatures imply equal expression values. The converse is not generally true: `x+0` and `x` can be equal mathematically while a purely structural algorithm assigns them different numbers. Additional algebraic rules can recognize more equalities.

**Proof strategy:** induct on expression structure. Equal leaf value identities refer to the same input value. At an interior node, matching signatures give the same operator and equal child values by the induction hypothesis. Applying the same deterministic operator gives equal parent values. This proves sound reuse for the expressions recognized, rather than completeness for all semantic equivalences.

The code below is an expression DAG builder. To use it across assignments, maintain a separate mapping from variable names to their current value numbers and update that mapping on every definition. Memory loads additionally need a valid memory-state or aliasing model.

### The Value-Number Construction Algorithm

```python
class ValueNumberDAGBuilder:
    def __init__(self):
        # 1-indexed list of nodes: node_array[val_num - 1]
        self.node_array = []
        # Hash map: signature tuple -> integer value number
        self.signature_map = {}

    def get_leaf(self, token_type: str, val: str) -> int:
        """Returns the value number for a leaf (variable or constant)."""
        signature = (token_type, val)
        if signature in self.signature_map:
            # Already exists! Reuse existing value number
            return self.signature_map[signature]
        
        # New leaf: allocate next available value number
        val_num = len(self.node_array) + 1
        node = {"type": "LEAF", "val": val, "id": val_num}
        self.node_array.append(node)
        self.signature_map[signature] = val_num
        return val_num

    def get_node(self, op: str, left_val: int, right_val: int) -> int:
        """Returns the value number for an interior operator node."""
        # Algebraic commutativity optimization (optional):
        # if op in ('+', '*'): left_val, right_val = sorted([left_val, right_val])
        
        signature = (op, left_val, right_val)
        if signature in self.signature_map:
            # Common subexpression eliminated!
            return self.signature_map[signature]
        
        # New operation: create interior node record
        val_num = len(self.node_array) + 1
        node = {"type": "INTERIOR", "op": op, "left": left_val, "right": right_val, "id": val_num}
        self.node_array.append(node)
        self.signature_map[signature] = val_num
        return val_num
```

---

## Complexity

- **Time Complexity:**
  - Hash lookup per token/node: $O(1)$ expected time.
  - For an expression with $N$ tokens: **$O(N)$ total time**.
- **Space Complexity:**
  - Hash table entries: $O(U)$ where $U \le N$ is the number of **unique** subexpressions.
  - Node array: $O(U)$ records.

---

## Exam Relevance

---

### Concrete Execution Trace: Step-by-Step

Let us trace $x = a + a \times (a - b) + (a - b) \times c$:

| Call # | Function Invocation | Signature Key Checked | Found in Hash? | Assigned Value Number | Action Taken |
| :---: | :--- | :--- | :---: | :---: | :--- |
| **1** | `get_leaf(ID, 'a')` | `('ID', 'a')` | **No** | **1** | Create Node 1: `(LEAF, a)` |
| **2** | `get_leaf(ID, 'a')` | `('ID', 'a')` | **YES** | **1** | Reused Node 1 |
| **3** | `get_leaf(ID, 'a')` | `('ID', 'a')` | **YES** | **1** | Reused Node 1 |
| **4** | `get_leaf(ID, 'b')` | `('ID', 'b')` | **No** | **2** | Create Node 2: `(LEAF, b)` |
| **5** | `get_node('-', 1, 2)` | `('-', 1, 2)` | **No** | **3** | Create Node 3: `('-', 1, 2)` representing $(a-b)$ |
| **6** | `get_node('*', 1, 3)` | `('*', 1, 3)` | **No** | **4** | Create Node 4: `('*', 1, 3)` representing $a * (a-b)$ |
| **7** | `get_node('+', 1, 4)` | `('+', 1, 4)` | **No** | **5** | Create Node 5: `('+', 1, 4)` representing $a + a*(a-b)$ |
| **8** | `get_leaf(ID, 'a')` | `('ID', 'a')` | **YES** | **1** | Reused Node 1 |
| **9** | `get_leaf(ID, 'b')` | `('ID', 'b')` | **YES** | **2** | Reused Node 2 |
| **10**| `get_node('-', 1, 2)` | `('-', 1, 2)` | **YES!** | **3** | **ELIMINATED!** Common subexpression $(a-b)$ reused! |
| **11**| `get_leaf(ID, 'c')` | `('ID', 'c')` | **No** | **6** | Create Node 6: `(LEAF, c)` |
| **12**| `get_node('*', 3, 6)` | `('*', 3, 6)` | **No** | **7** | Create Node 7: `('*', 3, 6)` representing $(a-b) * c$ |
| **13**| `get_node('+', 5, 7)` | `('+', 5, 7)` | **No** | **8** | Create Node 8: `('+', 5, 7)` Root node! |

### The Resulting Node Array:

```
Index | Type     | Op/Val | Left | Right | Mathematical Meaning
--------------------------------------------------------------
1     | LEAF     | a      | -    | -     | Variable a
2     | LEAF     | b      | -    | -     | Variable b
3     | INTERIOR | -      | 1    | 2     | (a - b)  [Reused by 4 & 7]
4     | INTERIOR | *      | 1    | 3     | a * (a - b)
5     | INTERIOR | +      | 1    | 4     | a + a * (a - b)
6     | LEAF     | c      | -    | -     | Variable c
7     | INTERIOR | *      | 3    | 6     | (a - b) * c
8     | INTERIOR | +      | 5    | 7     | Entire Expression
```

---

### Exam Relevance

Frequently tested on final examinations via hand-simulation of Value-Number Method for DAG Construction on given code fragments or graphs.

---

## What to carry forward

Hashing offers expected fast lookup, not guaranteed constant time in every implementation. Normalize commutative operands only when the language's operation and evaluation semantics allow it. [[DAG Construction and Local Optimization of Basic Blocks]] adds assignments and memory dependencies.

## Related notes

- [[Abstract Syntax Tree Construction with SDDs]]
- [[DAG Construction and Local Optimization of Basic Blocks]]

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Chapter 6 (Slides 93–101).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 6.1 (Directed Acyclic Graphs for Expressions).
