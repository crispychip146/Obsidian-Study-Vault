---
type: example
course: cse317
status: active
order: 39
---

# Australia Map Coloring CSP Example

> 📖 **Reading Order:** Step 39 of 43 | **Module 7:** Constraint Satisfaction Problems (CSPs)  
> ◄ **Previous:** [[Min-Conflicts Algorithm for CSPs]] | ► **Next:** [[Problem — Search Strategy Completeness and Complexity Analysis]]

---

## Starting Point and the Problem

To demonstrate how CSP algorithms operate in practice, we examine the principal benchmark problem from Russell & Norvig: coloring the map of **Australia** using three colors ($\{\text{Red}, \text{Green}, \text{Blue}\}$) such that no two neighboring territories share the same color.

---

## CSP Formulation

```
                                  [ NT ] ────── [ Q ]
                                 /   │            │
                                /    │            │
                         [ WA ] ─────┼── [ SA ] ──┼── [ NSW ]
                                     │            │     │
                                     └────────────┴── [ V ]
                                                         
                                         [ T ]
```

1. **Variables ($7$ territories):**
   $$X = \{\text{WA}, \text{NT}, \text{SA}, \text{Q}, \text{NSW}, \text{V}, \text{T}\}$$
   - Western Australia (WA), Northern Territory (NT), South Australia (SA), Queensland (Q), New South Wales (NSW), Victoria (V), Tasmania (T).
2. **Domains:**
   $$D_i = \{\text{Red}, \text{Green}, \text{Blue}\} \quad \forall i$$
3. **Constraints (Binary inequality constraints between adjacent territories):**
   - $\text{SA} \neq \text{WA}, \quad \text{SA} \neq \text{NT}, \quad \text{SA} \neq \text{Q}, \quad \text{SA} \neq \text{NSW}, \quad \text{SA} \neq \text{V}$
   - $\text{WA} \neq \text{NT}$
   - $\text{NT} \neq \text{Q}$
   - $\text{Q} \neq \text{NSW}$
   - $\text{NSW} \neq \text{V}$
   *(Notice: Tasmania (T) has no neighbors; it has zero binary constraints).*

---

## Walkthrough: Backtracking with Heuristics

### Step 1: First Variable Selection
- Domains are all size $3$ (MRV tie across all 7 variables).
- **Degree Heuristic Tie-Breaker:** Count degrees (constraints with unassigned neighbors):
  - $\text{degree}(\text{SA}) = 5$ (neighbors: WA, NT, Q, NSW, V)
  - $\text{degree}(\text{WA}) = 2$
  - $\text{degree}(\text{NT}) = 3$
  - $\text{degree}(\text{Q}) = 3$
  - $\text{degree}(\text{NSW}) = 3$
  - $\text{degree}(\text{V}) = 2$
  - $\text{degree}(\text{T}) = 0$
- **SA has the highest degree (5)!** Select **SA**.
- Assign: $\mathbf{SA = \text{Red}}$.

### Step 2: Forward Checking after $\text{SA} = \text{Red}$
Forward checking removes Red from all neighbors of SA:
- $D_{\text{WA}} = \{\text{Green}, \text{Blue}\}$ (size 2)
- $D_{\text{NT}} = \{\text{Green}, \text{Blue}\}$ (size 2)
- $D_{\text{Q}} = \{\text{Green}, \text{Blue}\}$ (size 2)
- $D_{\text{NSW}} = \{\text{Green}, \text{Blue}\}$ (size 2)
- $D_{\text{V}} = \{\text{Green}, \text{Blue}\}$ (size 2)
- $D_{\text{T}} = \{\text{Red}, \text{Green}, \text{Blue}\}$ (size 3)

### Step 3: Second Variable Selection
- MRV ties among WA, NT, Q, NSW, V (all size 2).
- Degree tie-breaker (constraints with *unassigned* neighbors):
  - NT is adjacent to unassigned WA and Q $\implies \text{degree} = 2$.
  - Q is adjacent to unassigned NT and NSW $\implies \text{degree} = 2$.
  - NSW is adjacent to unassigned Q and V $\implies \text{degree} = 2$.
- Pick **NT**. Assign: $\mathbf{NT = \text{Green}}$.

### Step 4: Forward Checking after $\text{NT} = \text{Green}$
- $D_{\text{WA}}$ was $\{\text{Green}, \text{Blue}\}$; Green removed $\implies D_{\text{WA}} = \{\mathbf{\text{Blue}}\}$ (size 1).
- $D_{\text{Q}}$ was $\{\text{Green}, \text{Blue}\}$; Green removed $\implies D_{\text{Q}} = \{\mathbf{\text{Blue}}\}$ (size 1).

### Step 5: Third Variable Selection (MRV in Action!)
- MRV immediately identifies WA and Q as having domain size **$1$**.
- Select **WA**: only choice is $\mathbf{WA = \text{Blue}}$.
- Select **Q**: only choice is $\mathbf{Q = \text{Blue}}$.

### Step 6: Remaining Variables
- Now NSW is adjacent to Q (Blue) and SA (Red).
  $D_{\text{NSW}}$ has Blue and Red removed $\implies D_{\text{NSW}} = \{\mathbf{\text{Green}}\}$.
- Victoria (V) is adjacent to SA (Red) and NSW (Green).
  $D_{\text{V}}$ has Red and Green removed $\implies D_{\text{V}} = \{\mathbf{\text{Blue}}\}$.
- Tasmania (T) is independent: choose any color (e.g., $\mathbf{T = \text{Red}}$).

---

## Final Valid Solution

$$\text{WA} = \text{Blue}, \quad \text{NT} = \text{Green}, \quad \text{SA} = \text{Red}, \quad \text{Q} = \text{Blue}, \quad \text{NSW} = \text{Green}, \quad \text{V} = \text{Blue}, \quad \text{T} = \text{Red}$$

Notice that with MRV and Degree heuristics, the search progressed directly to the solution **with ZERO backtracks**!

---

## Exam Relevance

- Reproducing the degrees and domain reductions during forward checking.
- Explaining how MRV avoids backtracking on the Australian map.

---

## Related Concepts

- [[Constraint Satisfaction Problems]]
- [[CSP Search Heuristics and Inference]]
- [[Backtracking Search for CSPs Algorithm]]

---

## Navigation

◄ **Previous:** [[Min-Conflicts Algorithm for CSPs]] | ► **Next:** [[Problem — Search Strategy Completeness and Complexity Analysis]]
