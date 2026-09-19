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

# 5. LEARNING DEPTH

Knowledge should normally be developed progressively.

Use this progression when the subject requires it:

1. Why does this exist?
2. Intuition
3. Core concept
4. Technical explanation
5. Worked example
6. Advanced understanding
7. Problem solving
8. Exam application

Not every note requires every section.

However, important concepts should normally progress beyond a short definition.

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

## Definition

Clearly define the concept.

Use technically correct terminology.

---

## Intuition

Explain the idea in simple language.

Assume the student may be seeing the concept for the first time.

Use analogies when they genuinely improve understanding.

---

## Why It Exists

Explain:

- What problem does it solve?
- Why was it introduced?
- What limitation or need motivated it?

---

## How It Works

Explain the mechanism step by step.

Break complicated processes into smaller parts.

---

## Example

Provide a concrete example.

Use diagrams, tables, calculations, traces, or pseudocode when useful.

---

## Technical Details

Include important technical information that the student needs for
advanced understanding or exams.

---

## Important Properties

List important characteristics, conditions, assumptions, or guarantees.

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

## Purpose

What problem does the algorithm solve?

---

## Core Idea

Explain the central idea in simple language.

---

## Inputs

Describe the required inputs.

---

## Outputs

Describe the expected output.

---

## How It Works

Explain the algorithm step by step.

---

## Pseudocode

algorithm

---

## Example

Walk through a concrete example.

Show intermediate steps when they matter.

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

## Formula

[Formula goes here]

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

Explain what the formula is actually telling us.

Do not leave the formula as unexplained notation.

---

## Derivation

Provide the derivation when useful or required by the course.

---

## Example

Show a worked example.

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

## Concepts Used

- [[Concept]]
- [[Formula]]
- [[Algorithm]]

---

## Solution

### Step 1

Explain the first step.

### Step 2

Explain the next step.

### Step 3

Continue until the problem is solved.

Do not skip important reasoning.

---

## Result

State the final answer clearly.

---

## Why This Works

Explain the reasoning behind the solution.

---

## Common Mistakes

Explain alternative incorrect approaches when useful.

---

## General Method

Extract the reusable problem-solving technique.

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

Provide the complete solution.

Show reasoning rather than only the final answer.

---

## Key Idea

Explain the main insight required to solve the problem.

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

Solutions should prioritize understanding.

A complete solution should normally contain:

1. What is being asked?
2. What information is given?
3. Which concept applies?
4. Why does that concept apply?
5. What steps are performed?
6. Why does each step work?
7. What is the final result?
8. How could the problem be recognized in an exam?

Avoid answers that provide only a final result without reasoning.

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


