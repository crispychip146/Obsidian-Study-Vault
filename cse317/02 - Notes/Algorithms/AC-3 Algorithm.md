---
type: algorithm
course: cse317
status: active
order: 35
---

# AC-3 Algorithm

> 📖 **Reading Order:** Step 35 of 43 | **Module 7:** Constraint Satisfaction Problems (CSPs)  
> ◄ **Previous:** [[Constraint Propagation and Arc Consistency]] | ► **Next:** [[Backtracking Search for CSPs Algorithm]]

---

## Starting Point and the Problem

As defined in [[Constraint Propagation and Arc Consistency]], enforcing arc consistency requires verifying that every value in domain $D_i$ has a valid supporting value in domain $D_j$. However, if deleting a value from $D_i$ causes a domain reduction, that reduction may break the arc consistency of *other* variables connected to $X_i$! We need an algorithm that systematically propagates domain reductions until the entire network reaches a fixed point of arc consistency. That algorithm is **AC-3** (Mackworth, 1977).

---

## Developing the Idea

AC-3 maintains a **Queue of Arcs** to be tested:
1. Initialize the queue with all directed binary arcs in the CSP: for every binary constraint between $X_i$ and $X_j$, add both $(X_i, X_j)$ and $(X_j, X_i)$ to the queue.
2. Pop an arc $(X_i, X_j)$ and call subroutine `REVISE(Xi, Xj)` to prune unsupported values from $D_i$.
3. If $D_i$ was modified:
   - All neighboring variables $X_k$ that constrain $X_i$ must have their arcs $(X_k, X_i)$ **re-inserted into the queue** (for all $k \neq j$)!
4. Repeat until the queue is empty (network is arc consistent) or some domain becomes empty (problem has no solution).

```
               [ Pop Arc (Xᵢ, Xⱼ) from Queue ]
                             │
                             ▼
               [ Call REVISE(Xᵢ, Xⱼ) ]
                             │
            ┌────────────────┴────────────────┐
            │ Was Dᵢ modified?                │
            ▼                                 ▼
           YES                                NO
            │                                 │
   Is Dᵢ now EMPTY?                           Do nothing;
   ├── YES ──► RETURN FAILURE                 continue loop
   └── NO  ──► Re-enqueue all (Xₖ, Xᵢ)
               for all neighbors Xₖ (k ≠ j)
```

---

## Algorithm Specification

```python
def AC3(csp):
    # Initialize queue with ALL directed binary arcs in the CSP
    queue = Queue([(Xi, Xj) for Xi in csp.VARIABLES 
                            for Xj in csp.NEIGHBORS(Xi)])

    while not queue.is_empty():
        (Xi, Xj) = queue.pop()
        
        if REVISE(csp, Xi, Xj):
            # If domain became empty, no solution exists!
            if len(csp.DOMAINS[Xi]) == 0:
                return False
                
            # Re-enqueue all incoming arcs from neighbors of Xi (except Xj)
            for Xk in csp.NEIGHBORS(Xi):
                if Xk != Xj:
                    queue.push((Xk, Xi))
                    
    return True

def REVISE(csp, Xi, Xj):
    revised = False
    for x in set(csp.DOMAINS[Xi]):
        # Check if there exists ANY y in D_j that satisfies constraint with x
        has_support = any(csp.SATISFIES_CONSTRAINT(Xi, x, Xj, y) 
                          for y in csp.DOMAINS[Xj])
        if not has_support:
            csp.DOMAINS[Xi].remove(x)
            revised = True
    return revised
```

---

## Complexity Analysis

Let $c$ be the number of binary constraints (edges in constraint graph), and $d$ be the maximum domain size ($|D_i| \le d$).

1. **Queue Invocations:**
   - There are $2c$ initial directed arcs in the queue.
   - An arc $(X_k, X_i)$ is re-inserted into the queue *only* when a value is deleted from $D_i$.
   - Since $D_i$ has at most $d$ values, a value can be deleted from $D_i$ at most $d$ times.
   - Variable $X_i$ has at most degree$(X_i)$ neighbors.
   - Therefore, arcs pointing into $X_i$ can be inserted into the queue at most $O(d \cdot \text{degree}(X_i))$ times.
   - Summing over all variables: $\sum_i d \cdot \text{degree}(X_i) = d \sum \text{degree} = O(c \cdot d)$.
   - Thus, an arc is popped from the queue at most $O(c \cdot d)$ times!

2. **Work per Arc (`REVISE`):**
   - For an arc $(X_i, X_j)$, `REVISE` compares each of the $\le d$ values in $D_i$ against each of the $\le d$ values in $D_j$.
   - Work per `REVISE` call is $O(d^2)$.

3. **Total Worst-Case Time Complexity:**
   $$\text{Total Time} = O(c \cdot d) \times O(d^2) = O\left(c \cdot d^3\right)$$

---

## Common Mistakes

- Forgetting to re-enqueue arcs $(X_k, X_i)$ when $D_i$ is modified.
- Re-enqueuing $(X_j, X_i)$ when revising $(X_i, X_j)$ (unnecessary because $X_j$ was the cause of the revision).

---

## Exam Relevance

- Proving the $O(c d^3)$ worst-case time complexity bound of AC-3.
- Tracing AC-3 on small constraint networks step by step.

---

## Related Concepts

- [[Constraint Propagation and Arc Consistency]]
- [[Backtracking Search for CSPs Algorithm]]
- [[CSP Search Heuristics and Inference]]

---

## Prerequisites

- [[Constraint Propagation and Arc Consistency]]

---

## Navigation

◄ **Previous:** [[Constraint Propagation and Arc Consistency]] | ► **Next:** [[Backtracking Search for CSPs Algorithm]]
