---
type: algorithm
course: cse317
status: active
order: 26
---

# Genetic Algorithm

> 📖 **Reading Order:** Step 26 of 43 | **Module 5:** Local Search, Optimization & Genetic Algorithms  
> ◄ **Previous:** [[Local Beam Search Algorithm]] | ► **Next:** [[8-Queens Problem Local Search and Genetic Algorithm Example]]

---

## Starting Point and the Problem

Stochastic beam search selects promising candidates probabilistically. However, generating new states purely by mutating a single parent neglects a powerful mechanism found in biological nature: sexual reproduction and recombination. In biology, complex organisms evolve by combining successful genetic traits from two distinct parent individuals. **Genetic Algorithms (GAs)** (John Holland, 1975; David Goldberg, 1989) model optimization after natural selection, genetics, and recombination.

---

## Developing the Idea

A Genetic Algorithm maintains a **population** of candidate solutions (called *individuals* or *chromosomes*). Successive generations evolve through selection, crossover, and mutation.

```
                  ┌─────────────────────────────────────────┐
                  │          INITIAL POPULATION             │
                  │   Random strings: [10110], [01101], ... │
                  └───────────────────┬─────────────────────┘
                                      │
                                      ▼
                  ┌─────────────────────────────────────────┐
                  │            FITNESS EVALUATION           │
                  │       f(x) evaluates each string        │
                  └───────────────────┬─────────────────────┘
                                      │
                                      ▼
                  ┌─────────────────────────────────────────┐
                  │            PARENT SELECTION             │
                  │   Roulette Wheel: P(i) ∝ Fitness        │
                  └───────────────────┬─────────────────────┘
                                      │
                                      ▼
                  ┌─────────────────────────────────────────┐
                  │          CROSSOVER (Recombination)      │
                  │   Parent 1: [101 | 10]                  │
                  │   Parent 2: [011 | 01]                  │
                  │   Child:    [101 | 01]                  │
                  └───────────────────┬─────────────────────┘
                                      │
                                      ▼
                  ┌─────────────────────────────────────────┐
                  │                MUTATION                 │
                  │   Random bit-flip with prob p_m         │
                  │   [10101] ──► [10111]                   │
                  └───────────────────┬─────────────────────┘
                                      │
                                      ▼
                  ┌─────────────────────────────────────────┐
                  │       REPLACE POPULATION & REPEAT       │
                  └─────────────────────────────────────────┘
```

---

## Core Components of Genetic Algorithms

### 1. Representation (Chromosomes)
States are encoded as strings over a finite alphabet—most commonly **binary strings** ($\{0, 1\}^L$), integer arrays, or permutations.
- *Example (8-Queens):* An 8-digit string where the $i$-th digit represents the row position of the queen in column $i$ (e.g., `24748552`).

### 2. Fitness Function
A function $f(x)$ that assigns a non-negative scalar score evaluating the quality of chromosome $x$. Higher fitness indicates higher survival probability.
- *Example (8-Queens):* $f(x) = 28 - h(x)$, where $h(x)$ is the number of mutually attacking pairs of queens, and $28 = \binom{8}{2}$ is the maximum possible non-attacking pairs.

### 3. Selection Operators
Selects individuals from the current generation to act as parents for the next generation.
- **Roulette Wheel Selection (Fitness-Proportionate):**
  The probability $P(i)$ of selecting individual $i$ is proportional to its fitness:
  $$P(i) = \frac{f(x_i)}{\sum_{j=1}^N f(x_j)}$$
- **Tournament Selection:** Randomly picks $k$ individuals and selects the one with the highest fitness.

### 4. Crossover Operators (Recombination)
Combines genetic material from two parents to produce offspring.
- **Single-Point Crossover:** A crossover point $c \in \{1, \dots, L-1\}$ is chosen at random. The child receives bits $1 \dots c$ from Parent 1, and bits $c+1 \dots L$ from Parent 2:
  $$\text{Parent 1: } \mathbf{1 1 0} \mid 1 0 \quad \text{Parent 2: } \mathbf{0 0 1} \mid 0 1 \implies \text{Child: } \mathbf{1 1 0} \mid 0 1$$
- **Two-Point Crossover:** Two points are chosen; the segment between them is swapped.
- **Uniform Crossover:** Each bit is chosen independently from either parent with probability $0.5$.

### 5. Mutation Operator
To prevent premature loss of genetic diversity, each position (gene) in a child has a small probability $p_m$ (e.g., $p_m \approx 0.001 - 0.01$) of undergoing a random modification (e.g., flipping a bit $0 \leftrightarrow 1$).

---

## The Schema Theorem Intuition

Why does crossover work better than pure mutation?
John Holland formalized this through the **Schema Theorem**:
- A **Schema** represents a template describing a subset of strings (e.g., `1*0**1`, where `*` is a wildcard).
- Short, low-order schemata with above-average fitness are called **Building Blocks**.
- Crossover allows the algorithm to combine high-fitness building blocks discovered independently in different parts of the population into a single super-individual!

---

## Common Mistakes

- Setting mutation rate $p_m$ too high: turns the algorithm into an aimless random walk.
- Setting mutation rate to zero: if an essential allele is absent in the initial population, it can never be discovered.
- Using fitness functions that return negative values without normalizing them for roulette wheel selection.

---

## Exam Relevance

- Performing a full step-by-step hand calculation of one GA generation (fitness calculation, selection probabilities, roulette selection, single-point crossover, and mutation).
- Explaining the Building Block Hypothesis and Schema Theorem intuition.

---

## Related Concepts

- [[Local Beam Search Algorithm]]
- [[8-Queens Problem Local Search and Genetic Algorithm Example]]
- [[Simulated Annealing Algorithm]]

---

## Prerequisites

- [[Local Search and Optimization Landscape]]

---

## Sources

- **Lectures:** [[cse317/01 - Sources/Lectures/MMi/Chap4-LocalSearch.ppt|Chap4-LocalSearch.ppt]] (Slides 36–45), [[cse317/01 - Sources/Lectures/MMi/Genetic Algorithm.ppt|Genetic Algorithm.ppt]]
- **Textbook:** [[cse317/01 - Sources/Lectures/MMi/Artificial Intelligence A Modern Approach 3rd Edition.pdf|Artificial Intelligence: A Modern Approach (3rd Edition)]], Chapter 4: Beyond Classical Search (Section 4.1.4)

---

## Navigation

◄ **Previous:** [[Local Beam Search Algorithm]] | ► **Next:** [[8-Queens Problem Local Search and Genetic Algorithm Example]]
