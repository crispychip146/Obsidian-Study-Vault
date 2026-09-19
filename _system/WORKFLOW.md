# Learning System Workflow

This vault is a learning system first and a note-taking system second.

The objective is to help the student progress from beginner understanding
to advanced understanding and exam-level problem-solving ability.

The AI must continuously maintain and improve this learning system.

---

# 1. Vault Architecture

Each course contains:

- `01 - Sources` — original course material
- `02 - Notes` — structured knowledge
- `03 - Maps` — relationships and exam intelligence
- `04 - Revision` — revision material
- `05 - Testing` — practice, tests, simulations, and performance

The `_system` directory contains the global rules and processing state.

Never modify original source material.

---

# 2. Source Types

Sources may include:

- lecture slides
- lecture notes
- textbooks
- textbook chapters
- tutorials
- assignments
- class notes
- question papers
- final examinations
- reference material

The source type must be identified before processing.

---

# 3. When a New Source Is Added

When a new file appears inside a course's `01 - Sources` directory:

## Step 1 — Identify

Determine:

- course
- source type
- title
- academic topic
- source file
- whether it has already been processed

Do not process a source that has already been completely processed
unless the source has changed or the user explicitly requests
reprocessing.

---

## Step 2 — Register the Source

Record the source in:

`_system/STATE/sources.md`

Track:

- source
- course
- type
- status
- processing date
- notes created
- notes updated
- questions extracted
- questions solved

---

## Step 3 — Analyze Before Writing

Read the source and identify:

- concepts
- definitions
- principles
- formulas
- algorithms
- procedures
- examples
- problems
- prerequisites
- relationships
- important terminology
- exam-relevant material

Do not immediately create notes.

First determine how the information fits into the existing knowledge base.

---

# 4. Existing Knowledge Check

Before creating anything:

1. Search existing notes.
2. Identify equivalent concepts.
3. Identify related concepts.
4. Identify prerequisite concepts.
5. Identify existing examples.
6. Identify existing problems.
7. Identify existing questions.

Never create a duplicate simply because the new source uses different
terminology.

---

# 5. Knowledge Decision

For every important piece of information, choose one:

## New Knowledge

The concept does not exist.

→ Create a new note.

## Deeper Knowledge

The concept already exists but the source adds useful information.

→ Update the existing note.

## Confirmation

The source confirms information already represented.

→ Do not create unnecessary content.

## Conflict

The source disagrees with existing material.

→ Record the disagreement and identify both sources.

Do not silently overwrite existing knowledge.

---

# 6. Learning-First Note Generation

When creating or updating a note, optimize for learning rather than
summarization.

The explanation must be suitable for a complete beginner.

The reader should be able to progress through:

1. Why the concept exists
2. Intuition
3. Basic explanation
4. Formal definition
5. Step-by-step mechanism
6. Technical details
7. Mathematical formulation
8. Worked examples
9. Edge cases
10. Common mistakes
11. Relationships with other concepts
12. Exam/problem-solving techniques
13. Advanced understanding

Do not assume prerequisite knowledge without linking to it.

If a prerequisite is necessary, create or reference the prerequisite
note.

---

# 7. Relationship Discovery

After creating or updating notes, analyze relationships.

Look for:

- prerequisites
- dependencies
- applications
- comparisons
- examples
- generalizations
- special cases
- related algorithms
- related formulas
- commonly confused concepts

Add meaningful Obsidian `[[wikilinks]]`.

Do not create links merely because two concepts occur in the same source.

---

# 8. Lecture Processing

For lecture material:

1. Identify the topics covered.
2. Identify individual concepts.
3. Identify prerequisites.
4. Create or update concept notes.
5. Extract formulas and algorithms when relevant.
6. Extract important examples.
7. Link concepts together.
8. Record the lecture as a source.
9. Update the course knowledge map.
10. Record processing state.

The lecture itself should remain in `01 - Sources`.

---

# 9. Textbook Processing

Textbooks should not automatically be converted into massive notes.

Use textbooks to:

- deepen existing concepts
- clarify difficult concepts
- provide alternative explanations
- provide examples
- provide mathematical derivations
- identify advanced details
- fill gaps in lecture material

Prefer enriching existing notes over creating duplicates.

If a textbook introduces genuinely new material relevant to the course,
create new notes.

---

# 10. Tutorial Processing

For tutorials:

1. Extract every meaningful problem.
2. Identify the concepts required.
3. Link problems to concepts.
4. Preserve the problem statement.
5. Solve the problem when appropriate.
6. Explain the solving method.
7. Record common mistakes.
8. Add the problem to the question bank.
9. Record its processing state.

Tutorials should strengthen the connection between knowledge and
problem-solving.

---

# 11. Assignment Processing

For assignments:

1. Extract problems.
2. Identify concepts tested.
3. Identify prerequisite concepts.
4. Add problems to the question bank.
5. Preserve the original problem.
6. Provide solutions when appropriate.
7. Identify techniques required.
8. Link problems to relevant notes.

Assignments should contribute to the learning and testing system.

---

# 12. Final and Exam Processing

Final examinations and other exam papers are especially important.

When an exam paper is added:

1. Extract every question.
2. Separate multi-part questions.
3. Identify concepts tested.
4. Identify prerequisite concepts.
5. Identify question type.
6. Identify required solving techniques.
7. Solve each question.
8. Link the question to relevant knowledge notes.
9. Add the question to the question bank.
10. Record the question in exam intelligence.
11. Record processing state.

---

# 13. Exam Intelligence

Use the available historical exam papers to identify observable patterns.

Track:

- topics appearing in exams
- number of questions involving each topic
- years in which topics appeared
- question types
- numerical vs theoretical questions
- recurring problem structures
- recurring concepts
- prerequisite concepts
- difficulty when reasonably inferable from the question
- marks allocated when available

Do not claim that a topic will appear in a future examination.

Instead use factual descriptions such as:

"This topic appeared in 4 of the 5 available final examinations."

---

# 14. Focus Recommendations

The system may recommend study priorities using evidence such as:

- frequency in available exams
- marks represented by the topic
- number of distinct question types
- prerequisite importance
- student's test performance
- unresolved mistakes
- incomplete understanding
- course coverage

Every recommendation should explain why it was made.

Example:

> Prioritize Deadlock because it appears frequently in the available
> final papers, has several associated problem types, and the student's
> recent test performance on the topic is weak.

Do not pretend that historical frequency guarantees future examination
appearance.

---

# 15. Question Bank

Every meaningful tutorial, assignment, and exam question should
eventually become part of the course question bank.

Each question should track:

- source
- course
- topic
- concepts tested
- difficulty when appropriate
- question type
- status
- solution status
- student's performance when attempted

Avoid storing duplicate questions.

Equivalent questions may be grouped by problem pattern.

---

# 16. Question Patterns

For recurring exam questions, identify the underlying problem pattern.

Example:

Original questions:

- calculate CPU scheduling metrics for process set A
- calculate CPU scheduling metrics for process set B

Underlying pattern:

`CPU Scheduling Calculation`

The testing system may later generate a new problem using the same
reasoning pattern with different inputs.

Never simply reproduce an original exam question when generating
practice material unless explicitly requested.

---

# 17. Testing System

The testing system should eventually support:

## Topic Practice

Questions focused on one topic.

## Mixed Practice

Questions spanning multiple related topics.

## Weak-Area Practice

Questions selected based on the student's previous mistakes.

## Exam-Pattern Practice

New questions inspired by structures found in historical exams.

## Final Simulation

A complete new examination modeled on the structure and topic
distribution observed in available course examinations.

---

# 18. Test Generation

Generated questions should:

- test actual course concepts
- be solvable from the knowledge base
- resemble legitimate academic problems
- vary numbers and scenarios
- test reasoning rather than memorization
- include multiple difficulty levels
- avoid copying source questions
- include solutions separately from the initial question

The system should prefer questions that expose weaknesses.

---

# 19. Testing Loop

The intended learning loop is:

    Learn
      ↓
    Practice
      ↓
    Attempt independently
      ↓
    Evaluation
      ↓
    Error analysis
      ↓
    Identify weak concepts
      ↓
    Review knowledge
      ↓
    Targeted practice
      ↓
    Retest

Do not immediately reveal solutions when the student is attempting
a test unless requested.

---

# 20. Attempt Tracking

After a test attempt, record:

- test
- date
- score
- questions attempted
- questions correct
- questions incorrect
- concepts tested
- weak concepts
- recurring mistakes
- recommended follow-up

Update:

`_system/STATE/tests.md`

and the course's:

`05 - Testing/Attempt History/`

---

# 21. Weak Area Detection

A weak area may be identified from:

- repeated incorrect answers
- repeated conceptual mistakes
- low test performance
- inability to solve increasingly difficult problems
- missing prerequisite knowledge
- inability to explain a concept
- repeated requests for clarification

Weak areas should be linked to the relevant concept notes.

---

# 22. Adaptive Learning

The system should change what it recommends based on demonstrated
performance.

If a student repeatedly succeeds at a topic:

→ gradually increase difficulty.

If a student repeatedly fails:

→ return to prerequisites and simpler problems.

If the student understands theory but fails calculations:

→ provide calculation-focused practice.

If the student can calculate but cannot explain:

→ provide conceptual/theoretical questions.

---

# 23. Avoiding Repeated Work

Before performing any substantial operation, check the persistent
state.

Check:

`_system/STATE/sources.md`

`_system/STATE/notes.md`

`_system/STATE/questions.md`

`_system/STATE/tests.md`

If the work has already been completed and nothing has changed:

→ do not repeat it.

If the source has changed:

→ process only the relevant changes.

If the user explicitly asks for reprocessing:

→ reprocess as requested.

---

# 24. Change Tracking

When the AI modifies an existing note, record the meaningful change in:

`_system/STATE/changes.md`

Track:

- date
- note
- source responsible for change
- type of change
- brief description

Example:

| Date | Note | Source | Change |
|---|---|---|---|
| 2026-09-19 | [[Paging]] | Lecture 06 | Added page-table example |

---

# 25. No Unnecessary Regeneration

Never regenerate the entire course simply because a new source was added.

Only update what is affected.

The existing knowledge base is persistent.

---

# 26. Source Authority

When sources disagree, prefer:

1. Official course material
2. Lecturer-provided material
3. Course textbook
4. Tutorials and assignments
5. External references

However, preserve meaningful disagreements.

Do not silently alter course-specific definitions to match an external
source.

---

# 27. Completion Standard

A course is not considered complete merely because every lecture has
been summarized.

A topic is well covered when appropriate material exists for:

- beginner explanation
- formal understanding
- deeper technical understanding
- examples
- formulas
- algorithms
- relationships
- common mistakes
- practice problems
- exam questions
- worked solutions

The exact components depend on the topic.

---

# 28. Primary Objective

The primary objective is:

    Help the student learn the course deeply enough to solve
    unfamiliar problems and perform effectively in examinations.

Notes are a tool.

The knowledge graph is a tool.

The question bank is a tool.

Exam analysis is a tool.

Testing is a tool.

The student's learning and problem-solving ability is the final objective.
