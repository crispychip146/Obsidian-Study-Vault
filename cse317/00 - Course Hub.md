# CSE 317 — Artificial Intelligence

> **Credits:** 3.0 | **Contact Hours:** 3 hours/week  
> **Course Scope:** Foundations and history of AI, rational agents, PEAS task environments, agent architectures, state-space search, uninformed search (BFS, UCS, DFS, DLS, IDS, Bidirectional), heuristic informed search (Greedy Best-First, A*, IDA*, RBFS, SMA*), admissibility and consistency, local search and optimization (Hill-Climbing, Simulated Annealing, Local Beam Search, Genetic Algorithms), adversarial search and two-player games (Minimax, Alpha-Beta Pruning, evaluation functions), and Constraint Satisfaction Problems (CSPs, AC-3 arc consistency, Backtracking with MRV/Degree/LCV, Forward Checking, Min-Conflicts).

---

## 🗺️ Master Sequential Reading Roadmap (Steps 01 – 43)

Read these notes in strict chronological order to build cumulative mastery from core agent foundations to complex heuristic search, game theory, and constraint propagation:

### Module 1: AI Foundations & History
- [ ] **Step 01 (Concept):** [[Definition and Foundations of Artificial Intelligence]] — The four approaches to AI (Thinking/Acting Humanly vs Rationally), Turing Test, interdisciplinary foundations, and historical eras from Dartmouth 1956 to modern deep learning.

### Module 2: Intelligent Agents & Architectures
- [ ] **Step 02 (Concept):** [[Intelligent Agents and Rationality]] — Formal agent definition, sensors, actuators, percept sequence, agent function $f: P^* \to A$ vs agent program, rationality vs omniscience, and autonomy.
- [ ] **Step 03 (Concept):** [[PEAS Framework]] — Performance measure, Environment, Actuators, and Sensors with case studies for automated taxis, medical diagnosis, satellite analysis, and part-picking robots.
- [ ] **Step 04 (Concept):** [[Environment Characterization in AI]] — Formal environment taxonomy: Observable, Multi-agent, Deterministic, Episodic, Static, and Discrete dimensions.
- [ ] **Step 05 (Concept):** [[Agent Architectures]] — The four classic structures: Simple Reflex, Model-Based Reflex, Goal-Based, and Utility-Based agents with state transitions and MEU theory.
- [ ] **Step 06 (Concept):** [[Learning Agents]] — Critic, Learning Element, Performance Element, and Problem Generator; exploration vs exploitation dilemma, feedback mechanisms, and pitfalls.
- [ ] **Step 07 (Concept):** [[Agentic AI and Autonomous Systems]] — Modern autonomous task-execution systems, the Four Pillars of Agency, ReAct / Plan-and-Solve workflows, and multi-agent systems.

### Module 3: Problem Solving & Uninformed Search
- [ ] **Step 08 (Concept):** [[Problem-Solving Agents and State Space Formulation]] — 5-tuple problem formulation ($s_0, A, \text{Result}, G, c$), state space vs search tree, node data structures, explored sets, and cycle prevention.
- [ ] **Step 09 (Concept):** [[Uninformed Search Strategies]] — Performance evaluation dimensions: completeness, optimality, time and space complexity with parameters $b, d, m, C^*, \epsilon$, and comparative analysis.
- [ ] **Step 10 (Algorithm):** [[Breadth-First Search Algorithm]] — FIFO queue mechanics, early goal test on node generation, proof of optimality for uniform costs, and $O(b^d)$ exponential memory analysis.
- [ ] **Step 11 (Algorithm):** [[Uniform-Cost Search Algorithm]] — Priority queue ordered by $g(n)$, late goal test on expansion, completeness condition $c \ge \epsilon > 0$, and rigorous mathematical proof of optimality.
- [ ] **Step 12 (Algorithm):** [[Depth-First Search and Depth-Limited Search Algorithm]] — LIFO stack / recursive search, $O(bm)$ linear memory advantage, non-optimality, tree-search loopy traps, and depth cutoff $l$.
- [ ] **Step 13 (Algorithm):** [[Iterative Deepening Search Algorithm]] — Combining BFS optimality with DFS $O(bd)$ linear memory; mathematical derivation proving asymptotic overhead ratio $\frac{b}{b-1}$.
- [ ] **Step 14 (Algorithm):** [[Bidirectional Search Algorithm]] — Simultaneous forward and backward frontiers, intersection detection, $O(b^{d/2})$ complexity reduction, and predecessor generation issues.
- [ ] **Step 15 (Example):** [[8-Puzzle and Vacuum World State Space Example]] — Complete 5-tuple formulations, 8-state vacuum graph, and 8-puzzle parity-restricted state space of $9!/2 = 181,440$ states.

### Module 4: Informed (Heuristic) Search & A* Search
- [ ] **Step 16 (Concept):** [[Heuristic Functions and Properties]] — Definition of $h(n)$, admissibility $h \le h^*$, consistency $h(n) \le c + h(n')$, triangle inequality, proof that consistency implies admissibility, heuristic dominance, and relaxed problems.
- [ ] **Step 17 (Algorithm):** [[Greedy Best-First Search Algorithm]] — Evaluation function $f(n) = h(n)$, priority queue mechanics, non-optimality, dead-end traps, and worst-case $O(b^m)$ behavior.
- [ ] **Step 18 (Algorithm):** [[A-Star Search Algorithm]] — Evaluation function $f(n) = g(n) + h(n)$, graph search with closed list and frontier updating, pseudocode, and execution mechanics.
- [ ] **Step 19 (Concept):** [[Optimality of A-Star Search]] — Complete mathematical proofs: Tree Search optimality under admissibility, Graph Search optimality under consistency, $f$-contour geometry, and pruning theorems.
- [ ] **Step 20 (Algorithm):** [[Memory-Bounded Heuristic Search Algorithms]] — Resolving the memory bottleneck: IDA* ($f$-contour cutoffs), RBFS (linear space with alternative path backups), and SMA* (memory-bounded leaf dropping).
- [ ] **Step 21 (Example):** [[Romania Travel Routing A-Star Search Example]] — Complete step-by-step trace from Arad to Bucharest comparing Greedy BFS (suboptimal cost 450) with A* Search (optimal cost 418).

### Module 5: Local Search, Optimization & Genetic Algorithms
- [ ] **Step 22 (Concept):** [[Local Search and Optimization Landscape]] — Complete-state formulation, state-space topography: global/local maxima, plateaus, shoulders, and ridges.
- [ ] **Step 23 (Algorithm):** [[Hill-Climbing Search Algorithm]] — Steepest-ascent greedy search, local maximum traps, plateau wandering, stochastic and first-choice variants, and Random-Restart completeness proof $1 - (1-p)^k$.
- [ ] **Step 24 (Algorithm):** [[Simulated Annealing Algorithm]] — Metallurgy annealing analogy, controlled downhill moves via Boltzmann probability $P = e^{\Delta E / T}$, cooling schedule $T(t)$, and asymptotic convergence theorem.
- [ ] **Step 25 (Algorithm):** [[Local Beam Search Algorithm]] — Maintaining $k$ states, information sharing among parallel beams vs independent random restarts, and stochastic beam search.
- [ ] **Step 26 (Algorithm):** [[Genetic Algorithm]] — Biological evolution model: chromosomes, fitness function, roulette wheel and tournament selection, single/two-point crossover, mutation, and Schema Theorem intuition.
- [ ] **Step 27 (Example):** [[8-Queens Problem Local Search and Genetic Algorithm Example]] — Formulating 8-queens with $h$ attacking pairs, steepest-ascent trace failing at local maximum $h=1$, and full Genetic Algorithm generational reproduction trace.

### Module 6: Adversarial Search & Game Playing
- [ ] **Step 28 (Concept):** [[Adversarial Search and Two-Player Games]] — Deterministic, turn-taking, two-player, zero-sum games of perfect information; 6-tuple formulation ($S_0, \text{Player}, \text{Actions}, \text{Result}, \text{Terminal-Test}, \text{Utility}$) and game trees.
- [ ] **Step 29 (Algorithm):** [[Minimax Algorithm]] — Optimal decision against an optimal opponent, recursive max/min value propagation, completeness, optimality, $O(b^m)$ time, and $O(bm)$ space complexity.
- [ ] **Step 30 (Algorithm):** [[Alpha-Beta Pruning Algorithm]] — Alpha (MAX guaranteed minimum) and Beta (MIN guaranteed maximum) parameters, pruning condition $\alpha \ge \beta$, mathematical equivalence to Minimax, and $O(b^{m/2})$ search depth doubling.
- [ ] **Step 31 (Concept):** [[Evaluation Functions and Cutting Off Search]] — Imperfect real-time decisions, cutoff tests, weighted linear evaluation functions $\sum w_i f_i(s)$, horizon effect, and quiescence search.
- [ ] **Step 32 (Example):** [[Minimax and Alpha-Beta Game Tree Pruning Example]] — Step-by-step 3-ply game tree trace demonstrating exact $[lpha, eta]$ updates and explicit identification of all pruned subtrees.

### Module 7: Constraint Satisfaction Problems (CSPs)
- [ ] **Step 33 (Concept):** [[Constraint Satisfaction Problems]] — Factored representation $(X, D, C)$, unary, binary, and global constraints, constraint graphs, and commutative assignment property.
- [ ] **Step 34 (Concept):** [[Constraint Propagation and Arc Consistency]] — Node consistency, directed arc consistency $X_i \to X_j$, directional nature of arcs, path consistency, and $k$-consistency.
- [ ] **Step 35 (Algorithm):** [[AC-3 Algorithm]] — Queue of directed arcs, `REVISE` subroutine, propagation of domain reductions, and complete mathematical proof of $O(c d^3)$ worst-case time complexity.
- [ ] **Step 36 (Algorithm):** [[Backtracking Search for CSPs Algorithm]] — Depth-first search with single-variable assignment, recursive backtracking algorithm, and early branch pruning.
- [ ] **Step 37 (Concept):** [[CSP Search Heuristics and Inference]] — Variable ordering: Minimum Remaining Values (MRV / fail-first) and Degree Heuristic; Value ordering: Least Constraining Value (LCV / succeed-first); Inference: Forward Checking and Maintaining Arc Consistency (MAC).
- [ ] **Step 38 (Algorithm):** [[Min-Conflicts Algorithm for CSPs]] — Local search complete-state formulation, repairing conflicted variables, and $O(1)$ empirical performance on $N$-queens.
- [ ] **Step 39 (Example):** [[Australia Map Coloring CSP Example]] — Complete trace on 7 Australian territories with 3 colors demonstrating MRV, Degree Heuristic, Forward Checking, and zero-backtrack solution.

### Module 8: Exam Problems & Rigorous Proofs
- [ ] **Step 40 (Problem):** [[Problem — Search Strategy Completeness and Complexity Analysis]] — Quantitative comparison of BFS vs IDS node generations, ratio derivation, and loopy graph DFS failure analysis.
- [ ] **Step 41 (Problem):** [[Problem — Admissible and Consistent Heuristic Verification]] — Mathematical proofs of admissibility for $\max(h_1, h_2)$ and $\text{avg}(h_1, h_2)$, dominance verification, and a 3-node counterexample proving admissibility does not imply consistency.
- [ ] **Step 42 (Problem):** [[Problem — Alpha-Beta Pruning Trace and Node Evaluation]] — Complete hand trace of 3-ply game tree with exact $[lpha, eta]$ interval bounds and classification of alpha/beta cutoffs.
- [ ] **Step 43 (Problem):** [[Problem — CSP Arc Consistency and Backtracking Trace]] — Running AC-3 on chain inequality network $X < Y < Z$ proving AC-3 alone completely solves the CSP without search.

---

# Sources & Course Materials

- Primary Lecture Slides: `cse317/01 - Sources/Lectures/MMi/`
  - [[01 - Sources/Lectures/MMi/Chap1-IntroAI.ppt|Chap1-IntroAI.ppt]] & [[01 - Sources/Lectures/MMi/AIHistory.ppt|AIHistory.ppt]]
  - [[01 - Sources/Lectures/MMi/Chap2-IntAgent.pptx|Chap2-IntAgent.pptx]]
  - [[01 - Sources/Lectures/MMi/learning_agents_elaborate_presentation.pptx|learning_agents_elaborate_presentation.pptx]]
  - [[01 - Sources/Lectures/MMi/Agentic_AI_Complete_Presentation.pptx|Agentic_AI_Complete_Presentation.pptx]]
  - [[01 - Sources/Lectures/MMi/Chap3-ProbSol.ppt|Chap3-ProbSol.ppt]] & [[01 - Sources/Lectures/MMi/UnInformedSearch.ppt|UnInformedSearch.ppt]]
  - [[01 - Sources/Lectures/MMi/Chap4-InformedSearch.ppt|Chap4-InformedSearch.ppt]] & [[01 - Sources/Lectures/MMi/A-starSearch.ppt|A-starSearch.ppt]]
  - [[01 - Sources/Lectures/MMi/Chap4-LocalSearch.ppt|Chap4-LocalSearch.ppt]] & [[01 - Sources/Lectures/MMi/Genetic Algorithm.ppt|Genetic Algorithm.ppt]]
  - [[01 - Sources/Lectures/MMi/Chap5-AdvSearch.ppt|Chap5-AdvSearch.ppt]]
  - [[01 - Sources/Lectures/MMi/CSP.pptx|CSP.pptx]]
- Authoritative Textbook:
  - [[01 - Sources/Lectures/MMi/Artificial Intelligence A Modern Approach 3rd Edition.pdf|Artificial Intelligence: A Modern Approach (3rd Edition) — Stuart Russell & Peter Norvig]]
