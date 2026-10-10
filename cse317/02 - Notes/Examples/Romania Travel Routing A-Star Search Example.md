---
type: example
course: cse317
status: active
order: 21
---

# Romania Travel Routing A-Star Search Example

> 📖 **Reading Order:** Step 21 of 43 | **Module 4:** Informed (Heuristic) Search & A* Search  
> ◄ **Previous:** [[Memory-Bounded Heuristic Search Algorithms]] | ► **Next:** [[Local Search and Optimization Landscape]]

---

## Starting Point and the Problem

To demonstrate how A* search operates and compare it against Greedy Best-First Search, we analyze the classic navigation problem from Russell & Norvig: finding the optimal driving route from **Arad** to **Bucharest** on a simplified Romanian road map.

---

## Problem Data

### 1. Road Graph Step Costs $c(u, v)$
- $\text{Arad} \leftrightarrow \text{Zerind}: 75$
- $\text{Arad} \leftrightarrow \text{Sibiu}: 140$
- $\text{Arad} \leftrightarrow \text{Timisoara}: 118$
- $\text{Zerind} \leftrightarrow \text{Oradea}: 71$
- $\text{Oradea} \leftrightarrow \text{Sibiu}: 151$
- $\text{Sibiu} \leftrightarrow \text{Fagaras}: 99$
- $\text{Sibiu} \leftrightarrow \text{Rimnicu Vilcea}: 80$
- $\text{Fagaras} \leftrightarrow \text{Bucharest}: 211$
- $\text{Rimnicu Vilcea} \leftrightarrow \text{Pitesti}: 97$
- $\text{Rimnicu Vilcea} \leftrightarrow \text{Craiova}: 146$
- $\text{Pitesti} \leftrightarrow \text{Bucharest}: 101$
- $\text{Pitesti} \leftrightarrow \text{Craiova}: 138$

### 2. Straight-Line Distance to Bucharest Heuristic $h_{SLD}(n)$
- $\text{Arad}: 366$
- $\text{Bucharest}: 0$
- $\text{Craiova}: 160$
- $\text{Fagaras}: 176$
- $\text{Oradea}: 380$
- $\text{Pitesti}: 100$
- $\text{Rimnicu Vilcea}: 193$
- $\text{Sibiu}: 253$
- $\text{Timisoara}: 329$
- $\text{Zerind}: 374$

*(Note: $h_{SLD}$ is admissible and consistent because straight line is the shortest possible physical distance between two geographical points).*

---

## Comparison 1: Greedy Best-First Search ($f(n) = h(n)$)

1. **Step 1 (Start Arad):**
   - Expand Arad ($h=366$).
   - Children: Sibiu ($h=253$), Timisoara ($h=329$), Zerind ($h=374$).
   - Greedy chooses **Sibiu** ($h=253$).
2. **Step 2 (Expand Sibiu):**
   - Children: Arad ($366$), Fagaras ($h=176$), Oradea ($380$), Rimnicu Vilcea ($h=193$).
   - Greedy chooses **Fagaras** ($h=176$).
3. **Step 3 (Expand Fagaras):**
   - Children: Sibiu ($253$), Bucharest ($h=0$).
   - Greedy chooses **Bucharest** ($h=0$).
4. **Result:**
   - Path found: $\text{Arad} \to \text{Sibiu} \to \text{Fagaras} \to \text{Bucharest}$.
   - Total Cost: $140 + 99 + 211 = \mathbf{450}$.
   - **This solution is SUBOPTIMAL!**

---

## Comparison 2: A* Search ($f(n) = g(n) + h(n)$)

### Step-by-Step Execution Trace

1. **Initial State:**
   - Frontier: `[Arad: g=0, h=366, f=366]`
2. **Iteration 1:**
   - Pop Arad ($f=366$).
   - Generate successors:
     - Sibiu: $g = 0 + 140 = 140$, $h = 253 \implies f = 393$
     - Timisoara: $g = 0 + 118 = 118$, $h = 329 \implies f = 447$
     - Zerind: $g = 0 + 75 = 75$, $h = 374 \implies f = 449$
   - Frontier: `[Sibiu(393), Timisoara(447), Zerind(449)]`
3. **Iteration 2:**
   - Pop **Sibiu** ($f=393$).
   - Generate successors from Sibiu:
     - Rimnicu Vilcea: $g = 140 + 80 = 220$, $h = 193 \implies f = 413$
     - Fagaras: $g = 140 + 99 = 239$, $h = 176 \implies f = 415$
     - Oradea: $g = 140 + 151 = 291$, $h = 380 \implies f = 671$
   - Frontier: `[Rimnicu Vilcea(413), Fagaras(415), Timisoara(447), Zerind(449), Oradea(671)]`
4. **Iteration 3:**
   - Pop **Rimnicu Vilcea** ($f=413$).
   - Generate successors from Rimnicu Vilcea:
     - Pitesti: $g = 220 + 97 = 317$, $h = 100 \implies f = 417$
     - Craiova: $g = 220 + 146 = 366$, $h = 160 \implies f = 526$
   - Frontier: `[Fagaras(415), Pitesti(417), Timisoara(447), Zerind(449), Craiova(526), Oradea(671)]`
5. **Iteration 4:**
   - Pop **Fagaras** ($f=415$).
   - Generate successors from Fagaras:
     - Bucharest: $g = 239 + 211 = 450$, $h = 0 \implies f = 450$
   - *(Notice: Bucharest is generated with $f=450$, but is NOT popped yet!)*
   - Frontier: `[Pitesti(417), Timisoara(447), Zerind(449), Bucharest(450), Craiova(526), Oradea(671)]`
6. **Iteration 5:**
   - Pop **Pitesti** ($f=417$).
   - Generate successors from Pitesti:
     - Bucharest: $g = 317 + 101 = 418$, $h = 0 \implies f = 418$
   - Since $418 < 450$, the frontier entry for Bucharest is **updated to the cheaper path**!
   - Frontier: `[Bucharest(418), Timisoara(447), Zerind(449), Craiova(526), Oradea(671)]`
7. **Iteration 6:**
   - Pop **Bucharest** ($f=418$).
   - Goal Test returns True! Return optimal solution.

---

## Result Summary

$$\text{Optimal Path: } \text{Arad} \to \text{Sibiu} \to \text{Rimnicu Vilcea} \to \text{Pitesti} \to \text{Bucharest}$$
$$\text{Optimal Path Cost: } 140 + 80 + 97 + 101 = \mathbf{418}$$

Notice that A* discovered the route through Pitesti ($418$), while Greedy Best-First Search settled for the suboptimal route through Fagaras ($450$).

---

## Exam Relevance

- Tracing A* step by step on this exact graph.
- Showing why late goal testing is required (Bucharest was first generated via Fagaras at cost 450, but popped via Pitesti at cost 418).

---

## Related Concepts

- [[A-Star Search Algorithm]]
- [[Greedy Best-First Search Algorithm]]
- [[Optimality of A-Star Search]]

---

## Sources

- **Lectures:** [[cse317/01 - Sources/Lectures/MMi/Chap4-InformedSearch.ppt|Chap4-InformedSearch.ppt]] (Slides 15–28), [[cse317/01 - Sources/Lectures/MMi/A-starSearch.ppt|A-starSearch.ppt]]
- **Textbook:** [[cse317/01 - Sources/Lectures/MMi/Artificial Intelligence A Modern Approach 3rd Edition.pdf|Artificial Intelligence: A Modern Approach (3rd Edition)]], Chapter 3: Solving Problems by Searching (Sections 3.4–3.5)

---

## Navigation

◄ **Previous:** [[Memory-Bounded Heuristic Search Algorithms]] | ► **Next:** [[Local Search and Optimization Landscape]]
