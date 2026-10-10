---
type: concept
course: cse317
status: active
order: 5
---

# Agent Architectures

> 📖 **Reading Order:** Step 05 of 43 | **Module 2:** Intelligent Agents & Architectures  
> ◄ **Previous:** [[Environment Characterization in AI]] | ► **Next:** [[Learning Agents]]

---

## Starting Point and the Problem

An agent's behavior is dictated by its **agent program**, which translates sensory percepts into actions. A naive agent could attempt to store a massive lookup table mapping every conceivable percept sequence to an action. However, as established in [[Intelligent Agents and Rationality]], this lookup table grows exponentially ($|P|^T$) and is impossible to store or learn. Therefore, we must design structured software architectures that compress the logic into manageable, generalizable computational models.

---

## Developing the Idea

As we move from simple environments to complex, uncertain worlds, an agent requires progressively richer internal representations:
1. React directly to immediate sensations: **Simple Reflex Agent**.
2. Maintain memory of unobserved parts of the world: **Model-Based Reflex Agent**.
3. Deliberate about desired future states: **Goal-Based Agent**.
4. Balance competing objectives and quantify preferences: **Utility-Based Agent**.

```
    SIMPLE REFLEX          MODEL-BASED REFLEX          GOAL-BASED             UTILITY-BASED
┌──────────────────┐      ┌──────────────────┐    ┌──────────────────┐    ┌──────────────────┐
│ Percept → Action │      │ State + Model    │    │ State + Goals    │    │ State + Utility  │
│ Condition-Action │      │ Tracks hidden    │    │ Search & Planning│    │ Trade-offs &     │
│ rules            │      │ aspects of world │    │ to reach target  │    │ Probabilities    │
└──────────────────┘      └──────────────────┘    └──────────────────┘    └──────────────────┘
```

---

## The Four Classic Architectures

### 1. Simple Reflex Agent

Operates solely on the current percept, completely ignoring the historical percept sequence.

```
                  ┌───────────────────────────────┐
                  │          ENVIRONMENT          │
                  └──────┬─────────────────▲──────┘
                         │ Percept         │ Action
                         ▼                 │
                   ┌───────────┐     ┌───────────┐
                   │  SENSORS  │     │ ACTUATORS │
                   └─────┬─────┘     └─────▲─────┘
                         │                 │
                         ▼                 │
                   ┌───────────────────────┴─────┐
                   │ What the world is like now  │
                   └─────────────┬───────────────┘
                                 │
                                 ▼
                   ┌─────────────────────────────┐
                   │   Condition-Action Rules    │
                   │   "IF car_in_front_braking  │
                   │    THEN initiate_braking"   │
                   └─────────────────────────────┘
```

- **Mechanism:** Implements condition-action rules:
  $$\text{if } \text{condition} \text{ then } \text{action}$$
- **Limitations:** Only works if the environment is **fully observable**. If even a small part of the state is hidden, infinite loops frequently occur (e.g., a vacuum cleaner that cannot sense its location can bounce between two dirty rooms indefinitely).

---

### 2. Model-Based Reflex Agent

Handles partial observability by maintaining an **internal state** that tracks aspects of the environment that cannot be viewed right now.

```
                  ┌───────────────────────────────┐
                  │          ENVIRONMENT          │
                  └──────┬─────────────────▲──────┘
                         │ Percept         │ Action
                         ▼                 │
                   ┌───────────┐     ┌───────────┐
                   │  SENSORS  │     │ ACTUATORS │
                   └─────┬─────┘     └─────▲─────┘
                         │                 │
                         ▼                 │
                   ┌───────────────────────┴─────┐
                   │       INTERNAL STATE        │
                   │   How world evolves: T(s,a) │
                   │   How sensors work: O(s)    │
                   └─────────────┬───────────────┘
                                 │
                                 ▼
                   ┌─────────────────────────────┐
                   │   Condition-Action Rules    │
                   └─────────────────────────────┘
```

- **Mechanism:** Uses two internal models:
  1. **Transition Model:** Knowledge of how the world changes independently of the agent and how the agent's own actions affect the world: $s_{t+1} = T(s_t, a_t)$.
  2. **Sensor Model:** Knowledge of how the state of the world is reflected in the agent's percepts: $p_t = O(s_t)$.
- **Update Rule:**
  $$\text{State} \leftarrow \text{Update-State}(\text{State}, \text{Action}, \text{Percept}, \text{Model})$$

---

### 3. Goal-Based Agent

Knowing the current state is not always enough; an agent must also know what states are desirable.

```
                  ┌───────────────────────────────┐
                  │          ENVIRONMENT          │
                  └──────┬─────────────────▲──────┘
                         │ Percept         │ Action
                         ▼                 │
                   ┌───────────┐     ┌───────────┐
                   │  SENSORS  │     │ ACTUATORS │
                   └─────┬─────┘     └─────▲─────┘
                         │                 │
                         ▼                 │
                   ┌───────────────────────┴─────┐
                   │       INTERNAL STATE        │
                   └─────────────┬───────────────┘
                                 │
                                 ▼
                   ┌─────────────────────────────┐
                   │   What will it be like if   │
                   │       I do action A?        │
                   └─────────────┬───────────────┘
                                 │
                                 ▼
                   ┌─────────────────────────────┐
                   │            GOALS            │
                   │      Search & Planning      │
                   └─────────────────────────────┘
```

- **Mechanism:** Combines state information with explicit goal descriptions. It deliberates over future sequences of actions using **search** and **planning** algorithms (e.g., A* search, STRIPS planning).
- **Advantage:** Highly flexible. If destination changes from City A to City B, only the goal description is updated; the underlying code does not need rewriting.

---

### 4. Utility-Based Agent

Goals alone provide only a binary distinction between success and failure (goal reached vs. not reached). In real scenarios, multiple competing goals exist (e.g., speed vs. fuel efficiency vs. safety).

```
                  ┌───────────────────────────────┐
                  │          ENVIRONMENT          │
                  └──────┬─────────────────▲──────┘
                         │ Percept         │ Action
                         ▼                 │
                   ┌───────────┐     ┌───────────┐
                   │  SENSORS  │     │ ACTUATORS │
                   └─────┬─────┘     └─────▲─────┘
                         │                 │
                         ▼                 │
                   ┌───────────────────────┴─────┐
                   │       INTERNAL STATE        │
                   └─────────────┬───────────────┘
                                 │
                                 ▼
                   ┌─────────────────────────────┐
                   │   How happy will I be in    │
                   │       such a state?         │
                   └─────────────┬───────────────┘
                                 │
                                 ▼
                   ┌─────────────────────────────┐
                   │      UTILITY FUNCTION       │
                   │     U: State → Real Number  │
                   │   Maximize Expected Utility │
                   └─────────────────────────────┘
```

- **Mechanism:** Implements a **Utility Function**:
  $$U: S \to \mathbb{R}$$
  When outcomes are uncertain (stochastic environment with probability distribution $P(s' \mid s, a)$), the rational agent selects the action that maximizes **Expected Utility (MEU)**:
  $$a^* = \arg\max_a \sum_{s'} P(s' \mid s, a) U(s')$$
- **Advantage:** Enables principled trade-offs when goals conflict, and handles probabilistic uncertainty gracefully.

---

## Comparative Matrix

| Architecture | Memory Required? | Knowledge of World Physics? | Deliberation / Planning? | Optimization Metric |
|---|---|---|---|---|
| **Simple Reflex** | None (Stateless) | None | None | Instant rule trigger |
| **Model-Based Reflex** | Yes (Internal State) | Transition & Sensor models | None | Condition-action rules |
| **Goal-Based** | Yes | Transition model | Yes (Search/Planning) | Binary goal achievement |
| **Utility-Based** | Yes | Probabilistic transitions | Yes (Decision-theoretic) | Continuous scalar utility $U(s)$ |

---

## Common Mistakes

- Believing model-based reflex agents plan into the future. They only track the *current* state of the world; they do not perform multi-step forward lookahead search.
- Using simple reflex agents in partially observable worlds, causing infinite cyclical behaviors.

---

## Exam Relevance

- Identifying which architecture is required given a problem description.
- Drawing block diagrams of Model-Based Reflex, Goal-Based, and Utility-Based agents.
- Formulating the Maximum Expected Utility (MEU) decision rule.

---

## Related Concepts

- [[Intelligent Agents and Rationality]]
- [[Environment Characterization in AI]]
- [[Learning Agents]]
- [[Agentic AI and Autonomous Systems]]

---

## Prerequisites

- [[Intelligent Agents and Rationality]]

---

## Sources

- **Lectures:** [[cse317/01 - Sources/Lectures/MMi/Chap2-IntAgent.pptx|Chap2-IntAgent.pptx]] (Slides 25–32)
- **Textbook:** [[cse317/01 - Sources/Lectures/MMi/Artificial Intelligence A Modern Approach 3rd Edition.pdf|Artificial Intelligence: A Modern Approach (3rd Edition)]], Chapter 2: Intelligent Agents (Section 2.4)

---

## Navigation

◄ **Previous:** [[Environment Characterization in AI]] | ► **Next:** [[Learning Agents]]
