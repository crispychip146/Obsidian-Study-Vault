---
type: example
course: cse317
status: active
order: 27
---

# 8-Queens Problem Local Search and Genetic Algorithm Example

> 📖 **Reading Order:** Step 27 of 43 | **Module 5:** Local Search, Optimization & Genetic Algorithms  
> ◄ **Previous:** [[Genetic Algorithm]] | ► **Next:** [[Adversarial Search and Two-Player Games]]

---

## Starting Point and the Problem

The **8-Queens Problem** requires placing 8 chess queens on an $8 \times 8$ chessboard such that no two queens attack each other (no two queens share the same row, column, or diagonal). We examine how this classic benchmark is solved using **Steepest-Ascent Hill-Climbing** and **Genetic Algorithms**.

---

## Local Search Formulation

- **Complete-State Representation:** One queen per column across all 8 columns. A state is represented by an array of 8 integers:
  $$s = [r_1, r_2, r_3, r_4, r_5, r_6, r_7, r_8], \quad r_i \in \{1, 2, \dots, 8\}$$
  Total complete states = $8^8 = 16,777,216$.
- **Heuristic Function $h(s)$:** The total number of pairs of queens attacking each other (directly or indirectly).
  The goal state has $h(s) = 0$.
  The maximum possible attacking pairs is $\binom{8}{2} = \frac{8 \times 7}{2} = 28$.
- **Successor Function:** Move any single queen to any other row in its own column.
  Each queen has 7 alternative rows $\implies 8 \times 7 = \mathbf{56\text{ possible neighbors}}$ per state.

---

## Walkthrough 1: Steepest-Ascent Hill-Climbing

Consider an initial state with $h = 17$:

```
               Board with h = 17 Attacking Pairs
    8  .  .  .  .  .  .  .  .
    7  .  .  .  .  .  .  .  .
    6  .  .  .  .  Q  .  .  .
    5  .  Q  .  .  .  .  .  Q
    4  .  .  .  .  .  Q  .  .
    3  .  .  .  Q  .  .  .  .
    2  Q  .  .  .  .  .  Q  .
    1  .  .  Q  .  .  .  .  .
       1  2  3  4  5  6  7  8
```

1. **Step 1:** Evaluate all 56 neighbors. Suppose the best neighbor reduces attacking pairs to $h = 12$. Move there.
2. **Step 2:** From $h = 12$, evaluate all 56 neighbors. Best neighbor gives $h = 8$. Move there.
3. **Step 3:** From $h = 8$, steepest descent finds a neighbor with $h = 3$, then $h = 1$.
4. **The Local Maximum Trap:**
   At $h = 1$, evaluate all 56 neighbors:
   - 8 neighbors have $h = 1$ (plateau).
   - 48 neighbors have $h \ge 2$ (worse).
   - **Zero neighbors have $h = 0$!**
   Steepest-ascent hill-climbing **terminates at $h = 1$ in failure**.

*Empirical Fact:* Pure steepest-ascent hill-climbing succeeds on 8-queens only **$14\%$ of the time**, getting trapped in local maxima or plateaus $86\%$ of the time. However, with **Random-Restart Hill-Climbing**, the expected number of restarts is $\frac{1}{0.14} \approx 7$, finding the optimal solution in under a millisecond!

---

## Walkthrough 2: Genetic Algorithm

### Step 1: Population and Representation
Consider a population of 4 individuals (chromosomes representing rows 1 to 8):
- Individual A: `2 4 7 4 8 5 5 2` (attacking pairs $h = 4 \implies \text{fitness } f = 28 - 4 = 24$)
- Individual B: `3 2 7 5 2 4 1 1` (attacking pairs $h = 5 \implies \text{fitness } f = 28 - 5 = 23$)
- Individual C: `2 4 4 1 5 1 2 4` (attacking pairs $h = 8 \implies \text{fitness } f = 28 - 8 = 20$)
- Individual D: `3 2 5 4 3 2 1 3` (attacking pairs $h = 17 \implies \text{fitness } f = 28 - 17 = 11$)

Total Fitness = $24 + 23 + 20 + 11 = \mathbf{78}$.

### Step 2: Selection Probabilities
- $P(A) = 24 / 78 \approx 31\%$
- $P(B) = 23 / 78 \approx 29\%$
- $P(C) = 20 / 78 \approx 26\%$
- $P(D) = 11 / 78 \approx 14\%$

Roulette wheel selection spins and selects Parents: suppose **A** and **B** are chosen to reproduce!

### Step 3: Single-Point Crossover
Crossover point randomly chosen after column 3:
- Parent A: `2 4 7 | 4 8 5 5 2`
- Parent B: `3 2 7 | 5 2 4 1 1`
Offspring Child 1 receives columns 1–3 from A and columns 4–8 from B:
$$\text{Child 1: } \mathbf{2\ 4\ 7}\ \mathbf{5\ 2\ 4\ 1\ 1}$$

### Step 4: Mutation
A random mutation occurs in column 7, changing row $1 \to 7$:
$$\text{Mutated Child: } 2\ 4\ 7\ 5\ 2\ 4\ \mathbf{7}\ 1$$
This mutated child is evaluated for fitness and added to the next generation.

---

## Exam Relevance

- Calculating $h$ (attacking queen pairs) for a given board layout.
- Performing the selection probability table and single-point crossover calculation.

---

## Related Concepts

- [[Local Search and Optimization Landscape]]
- [[Hill-Climbing Search Algorithm]]
- [[Genetic Algorithm]]

---

## Sources

- **Lectures:** [[cse317/01 - Sources/Lectures/MMi/Chap4-LocalSearch.ppt|Chap4-LocalSearch.ppt]] (Slides 12–18, 38–42), [[cse317/01 - Sources/Lectures/MMi/Genetic Algorithm.ppt|Genetic Algorithm.ppt]]
- **Textbook:** [[cse317/01 - Sources/Lectures/MMi/Artificial Intelligence A Modern Approach 3rd Edition.pdf|Artificial Intelligence: A Modern Approach (3rd Edition)]], Chapter 4: Beyond Classical Search (Section 4.1)

---

## Navigation

◄ **Previous:** [[Genetic Algorithm]] | ► **Next:** [[Adversarial Search and Two-Player Games]]
