---
type: concept
course: cse317
status: active
order: 7
---

# Agentic AI and Autonomous Systems

> 📖 **Reading Order:** Step 07 of 43 | **Module 2:** Intelligent Agents & Architectures  
> ◄ **Previous:** [[Learning Agents]] | ► **Next:** [[Problem-Solving Agents and State Space Formulation]]

---

## Starting Point and the Problem

Traditional Artificial Intelligence and modern generative foundation models (such as LLMs) operate primarily in a single-turn, passive paradigm: a user provides a prompt, and the model outputs text or an image. While impressive, these systems cannot independently accomplish long-horizon tasks—such as conducting research, debugging a multi-file software codebase, or booking complex travel itineraries. They lack continuous environmental perception, autonomous planning, iterative error correction, and external tool execution. **Agentic AI** represents the paradigm shift from passive predictive models to autonomous task-execution systems.

---

## Developing the Idea

```
  PASSIVE / GENERATIVE AI                     AGENTIC AI SYSTEMS
┌─────────────────────────┐         ┌─────────────────────────────────────┐
│  Single-turn Prompt     │         │ High-Level Strategic Goal           │
│           ↓             │         │                  ↓                  │
│ Direct Text Generation  │         │ Autonomous Plan Decomposition       │
│           ↓             │         │                  ↓                  │
│ Static Output to User   │         │ Tool Use (APIs, Code Execution, DB) │
└─────────────────────────┘         │                  ↓                  │
                                    │ Self-Reflection & Critic Loop       │
                                    │                  ↓                  │
                                    │ Re-plan upon failure                │
                                    │                  ↓                  │
                                    │ Verified Multi-step Execution       │
                                    └─────────────────────────────────────┘
```

---

## Definition

**Agentic AI** refers to autonomous computational systems that utilize reasoning models, memory, and external tools to execute multi-step, goal-directed tasks over extended horizons with minimal human intervention.

Unlike standard foundation models, an Agentic AI system acts as a complete **rational agent**: it perceives its digital or physical environment, maintains state, plans sequences of actions, interacts with tools, verifies intermediate outcomes, and adapts its strategy upon encountering errors.

---

## The Four Pillars of Agency

Modern Agentic AI architectures are built upon four fundamental capabilities:

```
                          ┌───────────────────────────┐
                          │   FOUR PILLARS OF AGENCY  │
                          └─────────────┬─────────────┘
                                        │
        ┌───────────────────┬───────────┴───────┬───────────────────┐
        ▼                   ▼                   ▼                   ▼
┌───────────────┐   ┌───────────────┐   ┌───────────────┐   ┌───────────────┐
│  01. AUTONOMY │   │02. REASONING  │   │  03. TOOL     │   │  04. MEMORY   │
│               │   │   & PLANNING  │   │  UTILIZATION  │   │ & REFLECTION  │
│ Operates over │   │ Decomposes    │   │ Executes APIs,│   │ Retains state,│
│ long intervals│   │ goals into    │   │ bash scripts, │   │ evaluates own │
│ without human │   │ structured sub│   │ web searches, │   │ errors, and   │
│ intervention  │   │ tasks         │   │ databases     │   │ self-corrects │
└───────────────┘   └───────────────┘   └───────────────┘   └───────────────┘
```

1. **Autonomy:** Operates independently across extended time horizons without requiring constant user prompting at every step.
2. **Reasoning & Planning:** Breaks down complex, ambiguous objectives into ordered sequences of manageable sub-tasks and dynamically re-plans when sub-tasks fail.
3. **Tool Utilization:** Calls external tools, web search engines, calculators, code execution sandboxes, and APIs to overcome the static knowledge limitations of closed weights.
4. **Memory & Reflection:** Maintains short-term operational state (working memory) and long-term knowledge (vector databases / RAG), inspecting its own outputs through critic loops to catch hallucinations and logic flaws.

---

## How It Works

### Core Operational Workflows

```
 User Goal ──► [ Planner / Reasoner ] ──► [ Tool Execution ]
                      ▲                         │
                      │                         ▼
               [ Critic / Evaluator ] ◄── [ Observation ]
```

1. **ReAct (Reasoning + Acting):**
   - Interleaves thought traces with actionable tool calls:
     $$\text{Thought} \to \text{Action} \to \text{Observation} \to \text{Thought} \to \dots$$
   - Prevents ungrounded reasoning by grounding thoughts in real tool feedback.
2. **Plan-and-Solve:**
   - First devises a complete multi-step plan, then executes each step sequentially, verifying each sub-goal before moving to the next.
3. **Self-Correction & Reflection:**
   - The agent inspects the exit code, syntax errors, or logical validity of its output. If an error is detected, it formulates an alternative hypothesis and re-runs the action.

---

## Multi-Agent Systems (MAS)

In complex production workflows, a single monolithic agent is often replaced by a **specialized multi-agent architecture**:
- **Manager / Orchestrator Agent:** Decomposes the master objective and assigns tasks to specialized workers.
- **Domain Worker Agents:** Agents specialized in narrow tasks (e.g., Code Researcher, Backend Implementer, Unit Tester).
- **Critic / Reviewer Agent:** Independent agent that audits code quality and verifies that solutions satisfy constraints before final delivery.

---

## Comparison Matrix

| Dimension | Traditional Generative AI | Agentic AI Systems |
|---|---|---|
| **User Input** | Highly constrained, explicit prompts | Broad, high-level strategic objectives |
| **Execution Cycle** | Single-turn feedforward | Iterative perception-action feedback loop |
| **Tool Integration** | Isolated / none | Native API, terminal, and database execution |
| **Error Handling** | Hallucinates or yields failure | Diagnoses errors, adjusts plans, and retries |
| **State Tracking** | Context window only | Epistemic state, persistent memory, and vector RAG |

---

## Common Mistakes

- Assuming Agentic AI is immune to infinite loops (without loop detection, an agent can endlessly alternate between failed actions).
- Granting unbounded actuator permissions without sandboxing or safety guardrails.

---

## Exam Relevance

- Contrasting traditional static models with autonomous agentic systems.
- Detailing the Four Pillars of Agency.
- Explaining ReAct and Reflection feedback loops.

---

## Related Concepts

- [[Intelligent Agents and Rationality]]
- [[Agent Architectures]]
- [[Learning Agents]]

---

## Prerequisites

- [[Agent Architectures]]
- [[Learning Agents]]

---

## Navigation

◄ **Previous:** [[Learning Agents]] | ► **Next:** [[Problem-Solving Agents and State Space Formulation]]
