---
type: algorithm
course: cse317
status: active
order: 24
---

# Simulated Annealing Algorithm

> 📖 **Reading Order:** Step 24 of 43 | **Module 5:** Local Search, Optimization & Genetic Algorithms  
> ◄ **Previous:** [[Hill-Climbing Search Algorithm]] | ► **Next:** [[Local Beam Search Algorithm]]

---

## Starting Point and the Problem

As established in [[Hill-Climbing Search Algorithm]], standard hill-climbing gets trapped in local maxima because it strictly refuses to take any step that decreases the objective function. Conversely, a purely random walk is complete but hopelessly inefficient. We need an algorithm that combines the **efficiency of hill-climbing** with the **ability to escape local maxima by occasionally accepting downhill moves in a controlled manner**. That algorithm is **Simulated Annealing** (Kirkpatrick et al., 1983).

---

## Developing the Idea: The Metallurgy Analogy

The algorithm is inspired by the physical process of **annealing in metallurgy**:
- In physics, to produce a crystal with minimum energy (flawless molecular structure), a metal is heated to a very high temperature where atoms bounce around freely and violently in random states.
- The temperature is then **cooled very gradually (annealed)**.
- At high temperatures, large disruptions occur, allowing atoms to break out of suboptimal microscopic alignments.
- As temperature cools, atoms settle into stable, low-energy configurations. If cooling is too fast (quenching), the metal forms brittle, defective glass (trapped in a local optimum).

```
   High Temperature T                          Low Temperature T
┌────────────────────────────┐              ┌────────────────────────────┐
│ Atoms move wildly          │    Cooling   │ Atoms settle into optimal  │
│ Downhill moves freely      │  Schedule T  │ crystalline structure      │
│ accepted (Exploration)     │ ───────────► │ Only improvements accepted │
└────────────────────────────┘              │ (Greedy Exploitation)      │
                                            └────────────────────────────┘
```

---

## Algorithm Specification

```python
import math, random

def SIMULATED_ANNEALING(problem, schedule):
    current = problem.INITIAL_STATE
    
    for t in range(1, float('inf')):
        # Get temperature according to cooling schedule T(t)
        T = schedule(t)
        if T <= 0:
            return current  # Completely cooled
            
        next_state = random.choice(problem.NEIGHBORS(current))
        delta_E = problem.VALUE(next_state) - problem.VALUE(current)
        
        # If the move improves the state, ALWAYS accept it
        if delta_E > 0:
            current = next_state
        else:
            # If the move is worse, accept it with Boltzmann probability!
            probability = math.exp(delta_E / T)
            if random.random() < probability:
                current = next_state
```

*(Note: If minimizing cost, $\Delta E = \text{Cost}(\text{current}) - \text{Cost}(\text{next})$, or use $e^{-\Delta C / T}$).*

---

## The Boltzmann Acceptance Probability

When a proposed move is worse ($\Delta E < 0$):
$$P(\text{accept bad move}) = e^{\frac{\Delta E}{T}}$$

Notice how this formula mathematically balances exploration and exploitation:
1. **Influence of Temperature $T$:**
   - When $T$ is very large ($T \to \infty$): $\frac{\Delta E}{T} \approx 0 \implies e^0 = 1$. The algorithm accepts almost every bad move, behaving like a **random walk** and escaping any local maximum.
   - When $T$ approaches zero ($T \to 0$): $\frac{\Delta E}{T} \to -\infty \implies e^{-\infty} = 0$. The algorithm accepts zero bad moves, behaving identically to **pure greedy hill-climbing**.
2. **Influence of $\Delta E$ (Severity of the Bad Move):**
   - A slightly worse move ($\Delta E$ close to $0$) has a relatively high probability of acceptance.
   - A disastrously worse move ($\Delta E$ strongly negative) has a near-zero probability of acceptance.

---

## Convergence Theorem

**Theorem (Geman & Geman, 1984):**
If the temperature schedule $T(t)$ cools sufficiently slowly (specifically, at a rate $T(t) \ge \frac{C}{\log(1 + t)}$ for an appropriate constant $C$), Simulated Annealing is guaranteed to converge to the **global optimum with probability 1**.

*Practical Reality:* Logarithmic cooling schedules are too slow for real-time engineering. Practitioners typically use geometric cooling ($T_{t+1} = \alpha T_t$ with $\alpha \in [0.8, 0.99]$), which provides exceptional optimization in practice even without the theoretical infinite-time guarantee.

---

## Common Mistakes

- Cooling the temperature too rapidly (quenching), causing the search to get trapped in a local optimum.
- Confusing $\Delta E > 0$ with $\Delta E < 0$ when formulating maximization vs. minimization.

---

## Exam Relevance

- Calculating the exact numerical acceptance probability $P = e^{\Delta E / T}$ given $\Delta E$ and $T$.
- Explaining the physical metallurgy analogy.
- Explaining how the algorithm transitions from random exploration to greedy exploitation.

---

## Related Concepts

- [[Hill-Climbing Search Algorithm]]
- [[Local Beam Search Algorithm]]
- [[Genetic Algorithm]]

---

## Prerequisites

- [[Hill-Climbing Search Algorithm]]

---

## Sources

- **Lectures:** [[cse317/01 - Sources/Lectures/MMi/Chap4-LocalSearch.ppt|Chap4-LocalSearch.ppt]] (Slides 23–30)
- **Textbook:** [[cse317/01 - Sources/Lectures/MMi/Artificial Intelligence A Modern Approach 3rd Edition.pdf|Artificial Intelligence: A Modern Approach (3rd Edition)]], Chapter 4: Beyond Classical Search (Section 4.1.2)

---

## Navigation

◄ **Previous:** [[Hill-Climbing Search Algorithm]] | ► **Next:** [[Local Beam Search Algorithm]]
