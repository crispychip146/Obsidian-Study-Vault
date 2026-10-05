---
type: algorithm
course: cse309
status: active
order: 46
---

# Chaitin's Graph Coloring Register Allocation Algorithm

> 📖 **Reading Order:** Step 46 of 55 | **Module 5:** Register Allocation  
> ◄ **Previous:** [[Linear Scan Register Allocation Algorithm]] | ► **Next:** [[Linear Scan Register Allocation Step-by-Step Example]]

---

## Building the idea

Graph coloring allocates ranges by conflicts rather than one linear order. Build the interference graph, remove nodes of degree less than K onto a stack, and later reinsert them in reverse order with available colors.

The degree argument in [[Register Interference Graphs and Graph Coloring Principles]] explains why a proven removable node can be colored if the remaining graph can. When simplification stalls, choose a spill candidate according to the implementation's heuristic. In optimistic coloring, remove it provisionally and attempt to color it later; a candidate need not become an actual spill.

If no color is available during selection, rewrite the program with spill storage and loads/stores. That rewrite creates new ranges and instructions, so allocation analysis must be updated. Deleting a node from the graph is not enough to produce executable spill code.

Coalescing removes copies when safe, but joining nodes can increase conflicts. The exact algorithm variant determines when and how those choices are made.

## How It Works

The algorithm transitions through defined phases.

---
## Complexity

- **Building RIG:** $O(V \cdot E)$ or $O(V^2)$ where $V$ is the number of live ranges.
- **Simplification / Coloring:** $O(V + E)$ using adjacency lists and degree buckets.
- **Repeated allocation:** Spilling inserts loads/stores and changes live ranges. Recompute liveness and the graph as needed; there is no general guarantee of one to three passes.
- **Space Complexity:** $O(V^2)$ storing the interference matrix.

---

## Limitations

### What Happens When an Actual Spill Occurs? (Chaitin Reloaded)

If a variable $x$ cannot be colored and must be spilled:
1. The compiler allocates a private stack slot in the procedure's activation record for $x$ (e.g., `[ebp - 12]`).
2. Immediately after every instruction that defines $x$, emit a store:
   $$\text{ST } [\text{ebp} - 12], R_{\text{temp}}$$
3. Immediately before every instruction that reads/uses $x$, emit a load:
   $$\text{LD } R_{\text{temp}}, [\text{ebp} - 12]$$
4. **The Dramatic Effect on Live Ranges:**
   - Instead of one massive live range spanning the entire program, $x$ now has **multiple tiny live ranges** that exist for only a single instruction!
5. **Re-run the Algorithm:** Recompute liveness, rebuild the RIG, and repeat! Splitting spilled values can reduce pressure, but inserted temporaries and target constraints can still require further decisions. Recheck the rewritten program rather than assuming the next graph will color.

---

## Exam Relevance

---

### Exam Relevance

Frequently tested on final examinations via hand-simulation of Chaitin's Graph Coloring Register Allocation Algorithm on given code fragments or graphs.

---

## What to carry forward

[[Chaitin's Graph Coloring Register Allocation Example]] traces the stack and selection. Distinguish the original Chaitin method from optimistic variants, and never treat a stalled low-degree search alone as proof that a spill is mathematically necessary.

## Related notes

- [[Register Interference Graphs and Graph Coloring Principles]]
- [[Chaitin's Graph Coloring Register Allocation Example]]

## Sources

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Register Allocation (Slides 444–531).
- **Textbook / Papers:** Chaitin, "Register Allocation & Spilling via Graph Coloring", ACM SIGPLAN Notices 1982; Briggs et al., "Improvements to Graph Coloring Register Allocation", ACM TOPLAS 1994.
