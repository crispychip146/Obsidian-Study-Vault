# Guided teaching rewrite — CHG-026

Date: 2026-10-05. Scope: all 181 existing learning notes under `02 - Notes` in CSE301 (92), CSE309 (55), and CSE313 (34). CSE315 and CSE317 have no learning notes in that directory structure. Course hubs, maps, revision sheets, question catalogs, and original source directories are outside this rewrite.

The repository agent and its required AI rules, note schema, and workflow governed the rewrite. Existing note content and course relationships supplied the working context. This was a teaching rewrite with targeted consistency corrections, not a new extraction from every original PDF or slide deck.

## What changed

Each note now develops its own question and mechanism: what we are trying to compute or control, why the previous idea is useful, which quantities or states change, and why the consequential steps work. Worked solutions retain their problem data and formal calculation structure while explaining the method at the point of use. Consolidation paragraphs explain what transfers to related topics.

Repeated template introductions, irrelevant register-allocation or generic mathematical claims, empty sections, and duplicated prose were removed. Important distinctions were clarified, including bounds versus approximations, coverage versus posterior probability, current requests versus maximum needs, mode switches versus context switches, and structural value numbering versus complete semantic equivalence.

Specific retained material required correction: the moving-threshold CLT proof, a consistency/MSE implication, mixed syscall architectures, generalized PC boot assumptions, semaphore count conventions, Mesa monitor pseudocode, dining-philosopher ring proofs, a contradictory semispace example, and Bison contextual stack indices. Implementation clarifications link to the official Linux syscall documentation, UEFI specification, and GNU Bison manual in the relevant notes. No universal runtime percentages or exact finite-run lottery shares are assumed.

## Validation

- All 181 expected files were edited; no learning note was renamed or deleted.
- YAML type, course, status, and order values were preserved. Reading navigation was retained; the left-recursion note's displayed step was corrected from 55 to 5.
- All wikilink targets in the rewritten notes resolve to existing vault files using course-relative or filename resolution. This checks targets, not every heading anchor or Obsidian rendering detail.
- Fenced code and displayed-math delimiters are balanced. No unexpected control characters were introduced. Git whitespace checks pass.
- Original source-reference entries were retained. Two OS algorithm bodies that had been placed under “Sources” were moved into dedicated algorithm sections while keeping their actual reference entries.
- The changed-path list contains learning notes and the four state/review files only. No original source, source-processing record, test attempt, or student-performance record was changed.

## Numerical checks

Independent calculations used the stated assumptions, rather than simply matching the printed answer.

| Check | Result |
|---|---|
| Four-process FCFS/SJF/SRTF/RR workload | Completion times and mean turnaround/wait/response match the retained calculation tables. |
| Five-process workload, including priority and RR arrival-before-requeue ties | FCFS means 15.2/10.8/10.8 ms; SJF 12.6/8.2/8.2; SRTF 8.8/4.4/2.0; priority 12.2/7.8/6.6; RR 14.4/10.0/4.4. |
| Four-resource Banker's exercise | Initial state and P1's tentative request admit sequence P0, P2, P1, P3, P4. P4's request fails current availability before safety testing. |
| Weather transition matrix | Four-step rainy probability from rain is 0.5749; stationary rainy weight is 4/7. |
| Paired Wald calculation | Sample difference variance 53.75/499; statistic approximately -3.40656; two-sided normal p-value approximately 0.00065787. |
| Poisson(20) event X ≥ 40 | Chernoff bound approximately 0.000441255; exact Poisson tail approximately 0.000053202. |
| Binomial(100, 0.5) event X ≥ 75 | Exact tail approximately 0.000000281814, below the discussed bounds. |
| Mendel categorical example | Chi-square upper-tail p-value approximately 0.925426 with three degrees of freedom. |
| Independent Beta(31,21), Beta(41,11) difference | Quadrature gives superiority probability approximately 0.984766 and equal-tailed 95% endpoints approximately 0.018424 and 0.361966; the note labels these separately from its normal approximation. |
| Finite M/M/1/3 example | Probabilities (8,12,18,27)/65 normalize; admitted throughput 228/65 agrees with service throughput; mean population 129/65. |

For the beta-difference check, integrate `BetaPDF(x;31,21) * BetaCDF(x+t;41,11)` over x from zero to one and invert the result at 0.025 and 0.975. Other checks use direct distribution tails, matrix powers, resource-vector comparisons, or event-based scheduling under zero overhead. These checks cover representative sensitive calculations; they do not certify every retained source attribution or every possible implementation of classroom pseudocode.

The rewrite adds no extracted questions, source-processing completions, practice-test results, or claims about the student's understanding. Existing note and question records reflect explanation updates only.
