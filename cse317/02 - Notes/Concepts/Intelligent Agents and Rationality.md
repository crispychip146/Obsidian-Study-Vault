---
type: concept
course: cse317
status: active
order: 2
---

# Intelligent Agents and Rationality

> 📖 **Reading Order:** Step 02 of 43 | **Module 2:** Intelligent Agents & Architectures  
> ◄ **Previous:** [[Definition and Foundations of Artificial Intelligence]] | ► **Next:** [[PEAS Framework]]

---

## Starting Point and the Problem

In classical computing, software processes static inputs to generate static outputs without ongoing awareness of the external world. However, automated systems—from robotic vacuum cleaners to medical diagnosis tools—operate within dynamic, evolving environments. Such systems continuously observe external states and execute actions that modify those states. To analyze and design such systems, AI unifies them under the abstraction of an **agent**. The fundamental challenge is: how do we mathematically define and design an agent whose actions are provably or measurably *rational*?

---

## Developing the Idea

Consider a thermostat or a robotic vacuum cleaner. At every time step, it observes sensor readings (temperature or dirt detection) and initiates an action (turn on heating, or turn right and vacuum).

```
                      ┌─────────────────────────────────────────┐
                      │               ENVIRONMENT               │
                      └─────────────┬───────────────────────────▲
                                    │                           │
                                    │ Percepts                  │ Actions
                                    ▼                           │
                              ┌───────────┐               ┌───────────┐
                              │  SENSORS  │               │ ACTUATORS │
                              └─────┬─────┘               └─────▲─────┘
                                    │                           │
                                    ▼                           │
                      ┌─────────────────────────────────────────┴─┐
                      │                   AGENT                   │
                      │                                           │
                      │   Percept Sequence: [p₁, p₂, ..., pₜ]     │
                      │                   ↓                       │
                      │         Agent Function: f(P*) → A         │
                      │         Implemented by: Program           │
                      └───────────────────────────────────────────┘
```

1. **Percept:** The agent's perceptual inputs at any given instant.
2. **Percept Sequence:** The complete history of everything the agent has ever perceived: $P^* = (p_1, p_2, \dots, p_t)$.
3. **Agent Function:** An abstract mathematical mapping from any percept sequence to an action:
   $$f: P^* \to A$$
4. **Agent Program:** The concrete software implementation executing on a physical architecture (hardware):
   $$\text{Agent} = \text{Architecture} + \text{Program}$$

---

## Definition

An **Agent** is anything that can be viewed as perceiving its environment through sensors and acting upon that environment through actuators.

A **Rational Agent** is one that, for each possible percept sequence, selects an action that is expected to maximize its performance measure, given the evidence provided by the percept sequence and whatever built-in knowledge the agent possesses.

---

## How It Works

### The Four Pillars of Rationality

Rationality is formally evaluated by four independent factors:

1. **The Performance Measure:** An objective, external criterion defining what constitutes success in the environment.
2. **Prior Knowledge:** The agent's initial background knowledge about the environment.
3. **The Actions:** The set of all legal actions the agent can physically execute.
4. **The Percept Sequence to Date:** The historical sequence of all sensory observations received up to the current instant.

### Rationality vs. Omniscience

It is crucial to distinguish between rationality and omniscience:
- **Omniscient Agent:** Knows the actual outcome of its actions and can act accordingly. In physical and stochastic environments, omniscience is impossible because the future is uncertain and sensors are incomplete.
- **Rational Agent:** Maximizes *expected* success based on available percepts. Rationality does not require clairvoyance; it requires making the best possible calculation given existing evidence.

### Information Gathering and Exploration

Rationality does not merely mean acting on current knowledge. If an agent does not know what lies behind a closed door, the rational action may be to open the door and look. This is called **information gathering** or **exploration**. A rational agent must explore to avoid being trapped by incomplete initial models.

### Autonomy

An agent is **autonomous** to the extent that its behavior is determined by its own experience (its ability to learn from percepts), rather than solely by the initial knowledge endowed by its designer.
- An agent with zero autonomy relies strictly on pre-programmed rules. If the environment shifts slightly, it fails.
- A truly autonomous agent learns, adapts, and builds its own internal models over time.

---

## Example: The Two-State Vacuum Cleaner World

```
        ┌───────────────┬───────────────┐
        │   Square A    │   Square B    │
        │    [Agent]    │               │
        │    (Dirty)    │    (Dirty)    │
        └───────────────┴───────────────┘
```

- **Percepts:** `[Location, Status]` (e.g., `[A, Dirty]` or `[B, Clean]`).
- **Actions:** `{Left, Right, Suck, NoOp}`.
- **Agent Function Table:**
  - `[A, Clean]` $\to$ `Right`
  - `[A, Dirty]` $\to$ `Suck`
  - `[B, Clean]` $\to$ `Left`
  - `[B, Dirty]` $\to$ `Suck`

If the performance measure gives $+1$ point for every clean square per time step, this simple mapping behaves rationally in a cleanable, static world.

---

## Technical Details

### Tabular Agent vs. Algorithmic Agent

While an agent function can theoretically be represented as a lookup table mapping every possible percept sequence $P^*$ to an action $A$, this is practically infeasible:
- If there are $|P|$ possible percepts and an episode lasts $T$ steps, the table requires $|P|^T$ entries.
- For a simple chess player or vacuum cleaner, $|P|^T$ exceeds the number of atoms in the observable universe.
- Hence, AI design focuses on writing compact **agent programs** that compute actions algorithmically using state representations, search, logic, and neural networks.

---

## Important Properties and Why They Hold

- **Performance Measure Location:** The performance measure must be designed according to what one actually wants in the environment, NOT according to how the agent ought to behave.
  - *Counterexample:* If a vacuum cleaner is rewarded $+1$ for every piece of dirt cleaned, it can rationally dump dirt back onto the floor and clean it again to maximize score. The correct measure is rewarding clean floors continuously.

---

## Common Mistakes

- Confusing an agent's internal state with the true state of the environment.
- Assuming rational behavior implies perfect outcomes (a rational poker player can make the optimal bet and still lose due to chance).
- Hardcoding rules and assuming the agent is intelligent without autonomy or adaptability.

---

## Exam Relevance

Frequently examined concepts include:
- Explaining the difference between an Agent Function and an Agent Program.
- Deriving why an agent function table suffers from exponential blowup.
- Rationality vs. Omniscience distinctions.
- Designing performance measures that prevent reward hacking.

---

## Related Concepts

- [[Definition and Foundations of Artificial Intelligence]]
- [[PEAS Framework]]
- [[Environment Characterization in AI]]
- [[Agent Architectures]]
- [[Learning Agents]]

---

## Prerequisites

- [[Definition and Foundations of Artificial Intelligence]]

---

## Navigation

◄ **Previous:** [[Definition and Foundations of Artificial Intelligence]] | ► **Next:** [[PEAS Framework]]
