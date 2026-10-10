---
type: concept
course: cse317
status: active
order: 1
---

# Definition and Foundations of Artificial Intelligence

> 📖 **Reading Order:** Step 01 of 43 | **Module 1:** AI Foundations & History  
> ◄ **Previous:** *Start of Course* | ► **Next:** [[Intelligent Agents and Rationality]]

---

## Starting Point and the Problem

For centuries, philosophers, mathematicians, and engineers pondered whether a machine could possess an artificial mind. Early mechanical automata could calculate arithmetic tables or play fixed music rolls, but they lacked adaptability: they could neither perceive novel surroundings nor make autonomous, rational judgments. The central challenge of Artificial Intelligence (AI) is to understand the computational mechanisms underlying intelligence and to construct artificial entities capable of perceiving, reasoning, learning, and acting effectively in complex, dynamic environments.

---

## Developing the Idea

To define artificial intelligence rigorously, researchers historical categorized definitions along two primary dimensions:
1. **Thought processes vs. Behavior:** Does intelligence reside in the internal reasoning process (thinking) or the observable external actions (acting)?
2. **Human fidelity vs. Rationality:** Is the benchmark human performance (with all our cognitive quirks, emotions, and heuristic biases), or an idealized standard of correctness called *rationality* (doing the "right thing" given available information)?

```
                         THOUGHT PROCESSES / REASONING
                                       ▲
                                       │
                Thinking Humanly       │       Thinking Rationally
             (Cognitive Science /      │      (Laws of Thought /
               Brain Modeling)         │       Formal Logic)
                                       │
    HUMAN PERFORMANCE ─────────────────┼───────────────── IDEAL RATIONALITY
                                       │
                 Acting Humanly        │        Acting Rationally
                 (Turing Test /        │       (Rational Agents /
              Empirical Behavior)      │     Maximizing Utility)
                                       │
                                       ▼
                             EXTERNAL BEHAVIOR / ACTION
```

1. **Acting Humanly (The Turing Test Approach):** Alan Turing (1950) proposed an operational definition of intelligence: if a human interrogator communicating via text terminal cannot reliably distinguish whether the conversational partner is a human or a machine, the machine exhibits intelligent behavior.
2. **Thinking Humanly (The Cognitive Modeling Approach):** Requires understanding *how* human minds actually operate through psychological experiments or neuroimaging, writing computer programs that replicate identical step-by-step cognitive processes.
3. **Thinking Rationally (The "Laws of Thought" Approach):** Rooted in Aristotle's syllogisms and mathematical logic. Every problem is translated into formal logic statements, and deductions produce provably correct conclusions.
4. **Acting Rationally (The Rational Agent Approach):** An entity that acts to achieve the best outcome or, when uncertainty exists, the best expected outcome. This is the modern, standard view in computer science and Russell & Norvig because it subsumes logic, handles uncertainty, and remains mathematically tractable.

---

## Definition

**Artificial Intelligence** is the study and construction of agents that perceive their environment and take actions that maximize their chance of successfully achieving their goals (rational action).

A **Rational Agent** is one that acts so as to achieve the best outcome or, when there is uncertainty, the best expected outcome based on its prior knowledge and current percepts.

---

## How It Works

### The Interdisciplinary Foundations of AI

Modern AI rests on foundations built by multiple disciplines over millennia:

1. **Philosophy (428 BCE – Present):**
   - Can formal rules be used to draw valid conclusions? (Aristotle, propositional logic).
   - How does the mental mind arise from a physical brain? (Dualism vs. Materialism).
   - Where does knowledge originate? (Empiricism of Francis Bacon and John Locke vs. Rationalism of Descartes).
   - What connects knowledge to action? (Utilitarianism of Bentham and Mill).

2. **Mathematics (c. 800 – Present):**
   - **Logic & Formal Systems:** Boole, Frege, Gödel's Incompleteness Theorem (establishing fundamental limits on what formal systems can prove).
   - **Computation & Algorithms:** Turing's universal computing machine, Church-Turing thesis, computability, and the distinction between polynomial ($P$) and non-deterministic polynomial ($NP$) time complexity.
   - **Probability & Statistics:** Thomas Bayes, Fermat, Pascal; quantifying uncertainty and reasoning under incomplete information.

3. **Economics (1776 – Present):**
   - Decision Theory: Combining probability with utility theory (von Neumann and Morgenstern, 1944) to model how rational decision-makers choose actions under risk.
   - Game Theory: Modeling strategic choices when multiple rational actors interact.
   - Operations Research: Sequential decision problems and Markov Decision Processes (Bellman).

4. **Neuroscience (1861 – Present):**
   - Investigation of how biological nervous systems process information.
   - Human brains contain $\sim 10^{11}$ neurons, each connected to $10^3$ to $10^4$ synapses, performing massively parallel electrochemical processing.
   - McCulloch and Pitts (1943) established the first mathematical model of artificial neurons.

5. **Psychology & Cognitive Science (1879 – Present):**
   - Shifting from behaviorism (stimulus-response) to cognitive psychology: viewing the mind as an information-processing system.

6. **Computer Engineering (1940 – Present):**
   - Development of electronic digital computers (ENIAC, von Neumann architecture) providing the physical hardware execution engine required for AI search and computation.

7. **Control Theory & Cybernetics (1948 – Present):**
   - Norbert Wiener, self-regulating feedback loops, objective-driven control systems, and dynamic minimization of error signals.

---

## Historical Eras of AI

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│  1943 - 1955 │  →  │  1956 - 1969 │  →  │  1966 - 1973 │  →  │  1969 - 1979 │  →  │  1980 - 1988 │
│  Gestation   │     │ Early Boom & │     │ First AI     │     │ Knowledge-   │     │ Expert Sys   │
│  McCulloch & │     │ Dartmouth    │     │ Winter       │     │ Based Boom   │     │ Industrial   │
│  Pitts,      │     │ Workshop     │     │ Combinatorial│     │ (DENDRAL,    │     │ Boom         │
│  Turing 1950 │     │ (Birth of AI)│     │ Explosion    │     │  MYCIN)      │     │ (LISP Mach)  │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                                            │
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐             │
│ 2011 - Pres  │  ◄  │ 2000 - 2010  │  ◄  │ 1987 - Pres  │  ◄  │  1987 - 1993 │  ◄──────────┘
│ Deep Learning│     │ Big Data &   │     │ Scientific   │     │ Second AI    │
│ LLMs, Agents │     │ Statistical  │     │ Method &     │     │ Winter       │
│ Scaled Comp  │     │ Learning     │     │ Probabilistic│     │ Collapse of  │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
```

1. **Gestation of AI (1943–1955):**
   - 1943: Warren McCulloch and Walter Pitts proposed the first model of artificial neural networks using threshold logic.
   - 1950: Alan Turing published *"Computing Machinery and Intelligence"*, proposing the Turing Test, machine learning, and genetic algorithms.
2. **The Birth of AI (1956):**
   - The Dartmouth Summer Research Project on Artificial Intelligence organized by John McCarthy, Marvin Minsky, Nathaniel Rochester, and Claude Shannon. McCarthy coined the term "Artificial Intelligence".
3. **Early Enthusiasm and Great Expectations (1952–1969):**
   - Newell and Simon's Logic Theorist and General Problem Solver (GPS).
   - Arthur Samuel's checkers player that learned by playing against itself.
   - Minsky and Papert's early work on perceptrons.
4. **The First AI Winter (1966–1973):**
   - Early systems succeeded only in tiny "toy worlds". When scaled to realistic domains, search spaces exploded exponentially (combinatorial explosion).
   - Inability of single-layer perceptrons to solve non-linear problems like XOR (Minsky & Papert, 1969).
   - Lighthill Report (1973) in the UK and DARPA funding cuts in the US halted major AI investment.
5. **Knowledge-Based Systems & Expert Systems Boom (1969–1988):**
   - Recognition that general-purpose search is weak without domain-specific knowledge.
   - Feigenbaum's DENDRAL (molecular structure inference) and Buchanan/Shortliffe's MYCIN (blood infection diagnosis).
   - Commercial boom of expert systems using inference engines and rule bases (e.g., DEC's R1/XCON saving millions). Specialized hardware companies built LISP machines.
6. **The Second AI Winter (1987–1993):**
   - The expert systems market collapsed. Specialized LISP hardware was overtaken by general-purpose personal workstations; rule bases proved brittle, unmaintainable, and unable to learn.
7. **Adoption of Scientific Method, Probabilistic Reasoning & Deep Learning (1987–Present):**
   - AI reunited with formal probability (Judea Pearl's Bayesian Networks) and statistical machine learning (SVMs, hidden Markov models).
   - 1997: IBM Deep Blue defeated World Chess Champion Garry Kasparov.
   - 2010s: Breakthroughs in Deep Learning fueled by GPUs and massive datasets (ImageNet 2012, AlphaGo 2016, Transformer architecture 2017, Large Language Models and Agentic AI 2020s).

---

## Technical Details

### The Turing Test Components

To pass the comprehensive Total Turing Test, an AI must demonstrate capabilities across six key subdisciplines:
- **Natural Language Processing (NLP):** Communicate fluently in natural human languages.
- **Knowledge Representation:** Store information systematically before and during the interrogation.
- **Automated Reasoning:** Use stored knowledge to answer questions, deduce logical consequences, and draw inferences.
- **Machine Learning:** Adapt to new scenarios, detect patterns, and update behaviors.
- **Computer Vision:** (For Total Turing Test) Perceive objects and video inputs from the physical world.
- **Robotics:** (For Total Turing Test) Manipulate physical objects and navigate physical environments.

---

## Important Properties and Why They Hold

- **Rationality $
eq$ Omniscience:** Omniscience requires knowing the actual outcome of an action in advance (impossible in non-deterministic environments). Rationality requires maximizing *expected* performance given available historical percepts and prior knowledge.
- **Rationality $
eq$ Perfection:** Rationality is about choosing the best decision under uncertainty, not guaranteeing that an unexpected external event won't cause failure.

---

## Common Mistakes

- **Confusing Acting Humanly with Acting Rationally:** Humans exhibit cognitive biases, emotional reactions, fatigue, and computational limits. Rational agents do not aim to simulate human flaws; they aim to compute optimal actions.
- **Assuming AI is pure logic:** Pure first-order logic struggles with uncertainty, noisy sensor inputs, and partial observability. Real-world AI relies fundamentally on probability theory and utility theory.

---

## Exam Relevance

Common examination topics include:
- The 4-quadrant categorization of AI definitions (Thinking vs. Acting, Human vs. Rational).
- The Turing Test requirements and criticisms (John Searle's Chinese Room argument).
- Historical causes of AI Winters (combinatorial explosion, brittleness of rule-based expert systems).
- The foundational disciplines of AI and their core contributions.

---

## Related Concepts

- [[Intelligent Agents and Rationality]]
- [[Environment Characterization in AI]]
- [[Agent Architectures]]

---

## Prerequisites

- None (Foundational introductory concept).

---

## Navigation

◄ **Previous:** *Start of Course* | ► **Next:** [[Intelligent Agents and Rationality]]
