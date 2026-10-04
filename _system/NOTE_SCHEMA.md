# NOTE SCHEMA

This file defines the structure, quality standards, and rules for knowledge notes
created inside the course learning system.

The purpose of notes is to build a connected learning system that takes the
student from beginner understanding to advanced understanding and exam-level
problem solving.

Notes are not summaries.

They are reusable knowledge units.

---

# 1. PRIMARY OBJECTIVE

The primary objective of every note is:

> Help the student understand, remember, connect, and apply the concept.

A note should prioritize:

1. Understanding
2. Intuition
3. Technical correctness
4. Connections to other concepts
5. Problem solving
6. Exam relevance
7. Source traceability

Do not create notes simply to increase the number of files.

---

# 2. NOTE TYPES

The system uses five primary knowledge note types:

1. Concept
2. Algorithm
3. Formula
4. Example
5. Problem

Choose the type that best represents the actual knowledge being stored.

---

# 3. GENERAL NOTE RULES

Every knowledge note should:

- Represent one coherent knowledge unit.
- Have a clear title.
- Be understandable without requiring the original source.
- Link to related concepts.
- Identify prerequisites where applicable.
- Include source references.
- Avoid unnecessary duplication.
- Build on existing notes instead of recreating them.
- Preserve useful existing explanations.
- Add deeper understanding incrementally.
- Clearly distinguish facts from examples or interpretation.
- Use precise terminology.
- Prefer explanation over unexplained jargon.

A note may link to many other notes.

A note should not contain unrelated topics merely because they appeared
together in a lecture.

---

# 4. FRONTMATTER

Use the following frontmatter for knowledge notes:

---
type:
course:
status: active
---

## type

One of:

- concept
- algorithm
- formula
- example
- problem

## course

The course code:

- cse301
- cse309
- cse313
- cse315
- cse317

## status

Possible values:

- active
- draft
- needs_review
- superseded

Use `needs_review` when the information is incomplete, ambiguous, or requires
verification.

---

# 5. LEARNING DEPTH AND GUIDED DEVELOPMENT

The student prefers to mentally follow what is happening, why each meaningful
step is useful and valid, and how the reasoning grows from earlier ideas.
Follow `_system/AI_RULES.md`, Section 3, for the full teaching standard.

Develop important notes from a concrete question and obstacle toward the key
insight, mechanism, and formal understanding. Do not begin with unexplained
heavy notation and postpone the motivation until after the answer.

The templates below are flexible guides. Sections may be merged or reordered
to preserve a connected explanation. Do not fill every heading mechanically,
repeat the same intuition in several sections, or add a proof to a topic that
does not require one. Keep clear results and rigorous formal arguments.

For each important prerequisite, explain the earlier idea briefly and state
what role it plays here. Use real existing notes when available. A link list
alone is not an explanation. Provide the local bridge needed for independent
reading; do not claim the student has mastered the prerequisite.

For a useful future connection, state the remaining question or new capability
and explain how another concept addresses it. Use known course order only when
supported; conceptual dependency does not establish lecture order.

Use a small running example, diagram, trace, or comparison when it reveals the
mechanism. Label AI-created teaching examples as illustrative. Show how the
example's reasoning generalizes, and distinguish intuition from proof.

The five primary note types and existing frontmatter remain unchanged.

---

# 6. CONCEPT NOTE

Use a Concept note for ideas, principles, definitions, mechanisms,
architectural concepts, theoretical topics, and relationships.

Template:

---
type: concept
course:
status: active
---

# Concept Name

## Starting Point and the Problem

Recall the relevant earlier idea and explain what it lets us do.
Establish a concrete situation, the target, and the difficulty or limitation
that motivates this concept. Explain important terminology as it appears.

---

## Developing the Idea

Use a revealing example to develop the central insight from that starting
point. Follow what happens to relevant objects or states. Explain how the
new idea addresses the difficulty. Use an analogy only if its mapping helps.

---

## Definition

State the technically correct definition and connect its terms to the idea
just developed. Keep the definition easy to locate for revision.

---

## How It Works

Explain the mechanism step by step. Track what changes and why each
consequential step helps. Connect the mechanism to the earlier idea and
justify important transitions using definitions or assumptions.

---

## Example

Provide a concrete example.

Use diagrams, tables, calculations, traces, or pseudocode when useful.

---

## Technical Details

Include important technical information that the student needs for
advanced understanding or exams.

---

## Important Properties and Why They Hold

Explain important characteristics, assumptions, and guarantees.
For claims requiring proof, develop the proof's key idea and then give the
rigorous argument, explaining why its steps establish the claim generally.

---

## Common Mistakes

Document common misunderstandings and incorrect approaches.

---

## Exam Relevance

Explain how the concept commonly appears in problems, questions,
comparisons, derivations, or applications when supported by the available
course material or exam evidence.

---

## Related Concepts

- [[Related Concept]]

---

## Prerequisites

- [[Prerequisite Concept]]

---

## Problems

- [[Problem]]

---

## Sources

- [[Source]]

---

# 7. ALGORITHM NOTE

Use an Algorithm note for algorithms, procedures, optimization methods,
parsing algorithms, scheduling algorithms, search algorithms, etc.

Template:

---
type: algorithm
course:
status: active
---

# Algorithm Name

## The Problem and Earlier Tools

Explain the situation, required output, and central obstacle. Recall earlier
tools or algorithms and explain the relevant capability or limitation.

---

## Developing the Core Idea

Show the observation that suggests the algorithm. Use a natural first attempt
or a small example when it reveals the difficulty. Explain why the chosen
approach makes progress without implying it is the only valid approach.

---

## Inputs

Describe the required inputs.

---

## Outputs

Describe the expected output.

---

## How It Works

Explain the algorithm step by step, showing the relevant state before and
after important operations. Explain why choices are made, how they move toward
the output, and what invariant or property is preserved where relevant.

---

## Pseudocode

algorithm

---

## Example

Walk through a concrete example using a trace, table, or diagram when useful.
Track actual state changes and explain important decisions. Relate the trace
to the pseudocode and extract the reasoning that carries to other inputs.

---

## Complexity

### Time Complexity

Explain the time complexity and why.

### Space Complexity

Explain the space complexity and why.

---

## Properties

Include relevant properties such as:

- Correctness
- Completeness
- Optimality
- Stability
- Determinism
- Termination

Only include properties that actually apply.
For correctness arguments, explain the key observation or invariant, establish
it rigorously, and show why termination produces the required result. Use the
appropriate proof strategy; an execution trace alone is not a correctness proof.

---

## Limitations

Explain when the algorithm performs poorly or cannot be used.

---

## Common Mistakes

List common implementation or exam mistakes.

---

## Exam Relevance

Describe common problem structures and applications when supported by
course material or exam evidence.

---

## Related Concepts

- [[Related Concept]]

---

## Prerequisites

- [[Prerequisite Concept]]

---

## Problems

- [[Problem]]

---

## Sources

- [[Source]]

---

# 8. FORMULA NOTE

Use a Formula note for important mathematical formulas, equations,
probability results, complexity formulas, statistical equations, and
other reusable mathematical relationships.

Template:

---
type: formula
course:
status: active
---

# Formula Name

## The Question and Earlier Knowledge

Explain the quantity or relationship we want to determine. Recall the earlier
ideas used to construct it and identify the obstacle to calculating it directly.

---

## Developing the Formula

Use a small situation to expose the structure. Explain what is being counted,
measured, combined, conditioned on, or averaged. Motivate the important
operations before introducing the complete expression.

---

## Formula

State the compact formula and connect its terms to the reasoning just developed.
Keep it easy to locate for reference.

---

## Variables

| Symbol | Meaning |
|---|---|
| x | Meaning of x |
| y | Meaning of y |

---

## Conditions

Explain when the formula can be used.

Include assumptions and restrictions.

---

## Intuition

Interpret the formula in the original situation. Explain what important
terms represent and how the expression behaves in a revealing simple or
limiting case. Merge this into the development if a separate section repeats it.

---

## Derivation

Provide the derivation when useful or required by the course. State the
assumptions, explain the key idea, and justify consequential transformations,
including important changes of index, sample space, or conditioning.
Explain why the argument applies generally; distinguish an illustrative
calculation from a formal derivation or proof.

---

## Example

Show a worked example. Interpret the givens, explain why the formula applies,
connect substitutions to their meanings, and interpret and check the result.

---

## Common Mistakes

Explain common substitution, algebraic, assumption, or interpretation
mistakes.

---

## Related Concepts

- [[Related Concept]]

---

## Prerequisites

- [[Prerequisite Concept]]

---

## Problems

- [[Problem]]

---

## Sources

- [[Source]]

---

# 9. EXAMPLE NOTE

Use an Example note for worked examples that demonstrate how concepts
are applied.

Examples should teach a reusable method rather than merely provide an answer.

Template:

---
type: example
course:
status: active
---

# Example Title

## Problem

State the problem clearly.

---

## Given

List the known information.

---

## Required

State what needs to be found or demonstrated.

---

## Understanding the Problem and Choosing the Method

Explain what the situation means and what makes the target difficult.
Develop the observation that suggests the method. Recall the earlier ideas
being used and explain their roles; include relevant links such as
[[Concept]], [[Formula]], or [[Algorithm]].

---

## Solution

Use descriptive steps that follow the actual reasoning. For each consequential
step, explain the current situation, the chosen action or inference, its purpose
and justification, and what it changes or establishes. Show meaningful
intermediate calculations or states without narrating every routine operation.

For a proof, first develop the key observation and strategy, then establish
the claim rigorously. Do not skip a logical bridge or replace proof with an
example.

---

## Result

State the final answer clearly. Explain what it means in the original
situation and verify it with an appropriate check when possible.

---

## Why This Works

Explain why the method reaches the required result and where its assumptions
matter. Reasoning must also appear alongside the solution steps; this section
is for consolidation, not delayed justification.

---

## Common Mistakes

Explain alternative incorrect approaches when useful.

---

## General Method

Extract the reusable problem-solving technique and the cues that suggest it.
Where useful, change one condition and explain what carries over or must change.

---

## Related Concepts

- [[Related Concept]]

---

## Sources

- [[Source]]

---

# 10. PROBLEM NOTE

Use a Problem note for questions extracted from:

- Final exams
- Midterms
- Tutorials
- Assignments
- Practice sheets
- Textbooks
- Question banks
- Other course sources

A Problem note should preserve the question and its solution separately.

Template:

---
type: problem
course:
status: active
---

# Problem Title

## Problem

Write the complete problem statement.

---

## Given

List the known information.

---

## Required

State what must be found, proved, explained, implemented, or calculated.

---

## Concepts Tested

- [[Concept]]
- [[Concept]]

---

## Prerequisites

- [[Prerequisite Concept]]

---

## Question Type

Examples:

- Numerical
- Derivation
- Proof
- Conceptual
- Algorithm
- Trace
- Code
- Design
- Comparison
- Explanation

---

## Solution

### Understanding the Situation

Interpret the original question without altering it. Explain what its objects
and givens mean, what is required, and the central difficulty.

### Developing the Key Idea

Recall the specific earlier result or mechanism we can reuse and explain its
connection. Develop the observation that suggests the approach, and explain
why the method's assumptions hold here.

### Working Through the Solution

Provide the complete solution with meaningful intermediate work. Explain the
purpose and justification of consequential steps and what each establishes.
For proofs, develop the strategy and then present a rigorous general argument.

### Result and Interpretation

Give the final answer and explain it in the problem's language. Check it when
possible. Add a compact exam-ready answer if useful without repeating the full
teaching explanation.

---

## Reusable Insight

Explain what reasoning carries to other problems and how to recognize its use.
Use a targeted variation when it reveals an important assumption or connection.

For unsolved questions and practice tests, omit the solution until appropriate
under the testing rules. Preserve the original question separately; do not put
solution-revealing guidance in the student's test question.

---

## Common Mistakes

Explain likely incorrect approaches.

---

## Exam Pattern

Describe the structure of the question.

Do not claim that the exact question will appear again.

---

## Related Problems

- [[Related Problem]]

---

## Related Concepts

- [[Related Concept]]

---

## Source

- [[Source]]

---

# 11. SOURCE TRACEABILITY

Every knowledge note should identify where the information came from.

Possible sources include:

- Lecture slides
- Lecture notes
- Textbooks
- Tutorials
- Assignments
- Exams
- Reference materials
- External references

Example:

## Sources

- [[01 - Sources/Lectures/Lecture 05]]
- [[01 - Sources/Textbooks/Operating Systems]]

For exam questions:

## Source

- [[01 - Sources/Exams/CSE313 Final 2025]]

Do not invent sources.

If information comes from external knowledge rather than the supplied course
materials, make that distinction clear.

---

# 12. WIKILINK RULES

Use Obsidian wikilinks whenever a meaningful related note exists.
Explain important relationships in the body: state what earlier idea is being
reused and how it helps the current reasoning. Link lists remain useful for
navigation but do not replace these explanatory bridges.

Example:

[[Virtual Memory]]
[[Paging]]
[[Page Fault]]
[[TLB]]

Do not create links merely for the sake of linking.

Prioritize links that represent:

- Prerequisites
- Dependencies
- Related concepts
- Algorithms
- Formulas
- Examples
- Problems
- Applications

---

# 13. PREREQUISITE RULE

When understanding a concept requires earlier knowledge, explicitly identify
the prerequisite.

Example:

## Prerequisites

- [[Probability Distribution]]
- [[Expected Value]]
- [[Conditional Probability]]

Prerequisites should reflect actual conceptual dependencies.

Do not list every remotely related topic.

---

# 14. DUPLICATE PREVENTION

Before creating a new note:

1. Search the vault.
2. Check existing notes.
3. Determine whether the concept already exists.
4. If it exists, update or expand it.
5. Only create a new note if it represents genuinely different knowledge.

Do not create multiple files for the same concept.

For example, do not create:

- Paging.md
- Paging 2.md
- Paging New.md
- Paging Final.md

when they represent the same concept.

Instead, maintain one authoritative knowledge note.

---

# 15. INCREMENTAL ENRICHMENT

Existing notes should be improved rather than repeatedly regenerated.

For example, an initial note may contain only:

## Definition

Paging divides memory into fixed-size pages.

After deeper sources are processed, the same note may grow to include:

- Why paging exists
- Page and frame structure
- Address translation
- Page tables
- TLB
- Page faults
- Multi-level page tables
- Advantages
- Limitations
- Exam problems

The system should preserve useful existing content while adding new knowledge.

---

# 16. CONFLICTING INFORMATION

If two sources disagree:

1. Do not silently overwrite existing information.
2. Identify the conflict.
3. Check source authority.
4. Determine whether the difference is due to context, terminology,
   assumptions, or actual disagreement.
5. Record the resolution when possible.

Example:

## Source Note

Lecture slides use X under assumption A.

Textbook defines X more generally under assumption B.

For this course, use the lecture definition when solving course-specific
questions.

---

# 17. EXAM INTEGRATION

Knowledge notes should connect to exam problems when evidence exists.

Example:

## Exam Applications

- [[CSE313 Final 2025 - Q3]]
- [[CSE313 Final 2024 - Q5]]

Exam frequency should never be fabricated.

If historical evidence is available, it may be recorded.

Example:

The concept appeared in 3 of the 5 analyzed final exams.

Historical frequency is evidence about past exams, not a guarantee about future
exams.

---

# 18. PROBLEM-SOLVING STANDARD

Solutions should develop understanding alongside the answer.

A complete learning solution should normally explain:

1. What situation does the question describe, and what is being asked?
2. What is given, and what earlier knowledge can we use?
3. What is difficult about reaching the target?
4. What observation suggests the chosen method?
5. Why does the method apply under these assumptions?
6. What does each meaningful step change or establish, and why is it valid?
7. What does the result mean, and how can we check it?
8. What reasoning can we reuse in a changed or unfamiliar problem?

For proof problems, state the claim and assumptions, develop the strategy,
justify the formal argument, and explain why all required cases are covered.
Examples and analogies support understanding but do not replace proof.

Use connected prose, relevant mathematics, and traces or diagrams where useful.
Do not mechanically answer these eight prompts as eight repeated sections.
Keep the final answer clear and easy to locate.

---

# 19. BEGINNER EXPLANATION STANDARD

The student may initially know nothing about a topic.

Therefore, important concepts should explain terminology before relying heavily
on it.

Preferred progression:

Simple explanation
        ↓
Correct terminology
        ↓
Technical understanding
        ↓
Advanced application

Do not replace technical correctness with oversimplification.

The goal is understanding, not memorization.

---

# 20. ADVANCED UNDERSTANDING

Important topics should eventually explain:

- Why the method works
- Underlying assumptions
- Edge cases
- Limitations
- Trade-offs
- Complexity
- Connections to other concepts
- Practical applications
- Exam applications

Do not force advanced material into every note.

Depth should match the importance and complexity of the topic.

---

# 21. EXAMPLE SELECTION

Examples should be selected intentionally.

Prefer examples that demonstrate:

- A fundamental idea
- A common exam pattern
- A common mistake
- A non-obvious edge case
- A practical application
- A connection between concepts

Avoid adding many nearly identical examples.

---

# 22. COMMON MISTAKES

Important notes should document common mistakes.

Examples:

- Confusing logical and physical addresses.
- Forgetting that page numbers and offsets have different roles.
- Applying a formula without checking its assumptions.
- Confusing similar algorithms.
- Memorizing a procedure without understanding why it works.

Mistakes should be specific and actionable.

---

# 23. NOTE QUALITY CHECKLIST

Before considering a note complete, verify:

- [ ] Correct note type
- [ ] Correct course
- [ ] Clear definition
- [ ] Beginner-friendly explanation where necessary
- [ ] Technical explanation where necessary
- [ ] Useful example
- [ ] Important properties included
- [ ] Common mistakes included where relevant
- [ ] Prerequisites identified
- [ ] Related concepts linked
- [ ] Problems linked
- [ ] Sources recorded
- [ ] No unnecessary duplication
- [ ] Existing knowledge preserved
- [ ] Exam relevance included where evidence exists
- [ ] No unsupported claims
- [ ] Concrete situation, target, and obstacle clear where relevant
- [ ] Key insight developed before relying on the method
- [ ] Important state changes or inferences mentally followable
- [ ] Consequential steps explained for both purpose and justification
- [ ] Earlier ideas recalled and their actual roles explained
- [ ] Symbols and important operations connected to their meanings
- [ ] Proof strategy and general justification explained where applicable
- [ ] Examples, analogies, and proofs correctly distinguished
- [ ] Result interpreted and checked where appropriate
- [ ] Useful future connection explained when available
- [ ] Connected flow without redundant sections or artificial discovery

---

# 24. NOTE MATURITY

A note does not need to be perfect immediately.

Use progressive maturity.

## Draft

Basic information has been captured.

## Active

The concept is sufficiently explained and usable.

## Needs Review

Important uncertainty, missing information, or conflicting sources remain.

## Superseded

The note has been replaced by another authoritative note.

The goal is continuous improvement rather than unnecessary regeneration.

---

# 25. KNOWLEDGE GRAPH PRINCIPLE

The vault should behave as a connected knowledge graph.

Preferred structure:

Prerequisite
    ↓
Concept
    ↓
Formula / Algorithm
    ↓
Example
    ↓
Problem
    ↓
Exam Question

For example:

[[Probability]]
      ↓
[[Conditional Probability]]
      ↓
[[Bayes Theorem]]
      ↓
[[Bayes Example]]
      ↓
[[Bayes Exam Problem]]

This allows the student to move from foundational knowledge to exam-level
application.

---

# 26. CROSS-COURSE CONNECTIONS

When concepts genuinely overlap between courses, link them.

Examples:

- [[Graph]]
- [[Probability]]
- [[Optimization]]
- [[Processes]]
- [[Algorithms]]

Do not duplicate the same fundamental knowledge across courses unless the
course-specific context genuinely requires it.

---

# 27. REVISION MATERIAL

Revision notes should be generated from the deeper knowledge notes.

Do not make the revision note the primary source of knowledge.

Preferred structure:

Deep Knowledge
      ↓
Quick Revision
      ↓
Practice
      ↓
Exam

The detailed concept note remains authoritative.

---

# 28. TESTING CONNECTION

Knowledge notes should eventually connect to the testing system.

For example:

## Practice

- [[05 - Testing/Question Bank/Paging Problem 01]]
- [[05 - Testing/Question Bank/Paging Problem 02]]

This allows the system to move from learning to assessment.

---

# 29. PERFORMANCE FEEDBACK

When test results show a weakness, the relevant knowledge note may be updated
with:

- Missing explanation
- Common mistake
- Clarification
- Additional example
- Prerequisite link
- Targeted problem

The testing system should therefore improve the knowledge base over time.

---

# 30. DO NOT OVERGENERATE

Do not create notes merely because a source contains many headings.

Ask:

> Does this information deserve to exist as a reusable knowledge unit?

If not, keep it within the relevant larger note.

Optimize for:

Understanding
+
Connections
+
Retrievability
+
Problem-solving ability

rather than:

Number of files

---

# 31. FINAL PRINCIPLE

The vault is a learning system.

Notes are the knowledge layer.

Questions are the application layer.

Tests are the assessment layer.

Performance is the feedback layer.

The system should continuously connect:

Sources
   ↓
Knowledge
   ↓
Examples
   ↓
Problems
   ↓
Exams
   ↓
Tests
   ↓
Performance
   ↓
Weaknesses
   ↓
Improved Knowledge

The ultimate goal is not to produce more notes.

The ultimate goal is to make the student capable of independently
understanding and solving increasingly difficult problems.


