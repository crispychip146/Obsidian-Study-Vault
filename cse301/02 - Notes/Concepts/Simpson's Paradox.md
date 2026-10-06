---
type: concept
course: cse301
status: active
order: 27
---

# Simpson's Paradox

> 📖 **Reading Order:** Step 27 of 103 | **Module 3:** Conditional Probability and Conditioning  
> ◄ **Previous:** [[Conditional Probability and Independence]] | ► **Next:** [[Conditional Expectation]]

---

## Starting Point and the Problem

When comparing two treatments, algorithms, or decision-makers, intuition suggests that if Option $A$ outperforms Option $B$ across every distinct category of a population, then Option $A$ must also outperform Option $B$ when all categories are pooled together.

However, aggregate probability distributions can completely contradict subgroup probabilities. This counter-intuitive statistical phenomenon is known as **Simpson's Paradox**. It occurs when a confounding variable influences both the group assignment and the outcome, distorting or completely reversing the overall conclusion.

In computing and data science, failing to detect Simpson's paradox leads to erroneous conclusions in A/B testing, machine learning fairness audits, medical diagnostic models, and performance benchmarks.

---

## Developing the Idea

Consider a clinical scenario from lecture comparing two surgeons, **Dr. Hibbert** and **Dr. Nick**, across two types of procedures: difficult heart surgeries and routine bandage treatments.

### The Concrete Situation

1. **Subgroup 1: Difficult Heart Surgeries**
   - **Dr. Hibbert:** Performs 90 heart surgeries; 70 succeed, 20 fail.  
     $$\text{Success Rate} = \frac{70}{90} \approx 77.8\%$$
   - **Dr. Nick:** Performs only 10 heart surgeries; 0 succeed, 10 fail.  
     $$\text{Success Rate} = \frac{0}{10} = 0.0\%$$
   - *Subgroup Conclusion:* Dr. Hibbert is strictly superior on heart surgeries ($77.8\% > 0.0\%$).

2. **Subgroup 2: Routine Bandage Dressings**
   - **Dr. Hibbert:** Performs 10 bandage dressings; 10 succeed, 0 fail.  
     $$\text{Success Rate} = \frac{10}{10} = 100.0\%$$
   - **Dr. Nick:** Performs 90 bandage dressings; 81 succeed, 9 fail.  
     $$\text{Success Rate} = \frac{81}{90} = 90.0\%$$
   - *Subgroup Conclusion:* Dr. Hibbert is strictly superior on bandage dressings ($100.0\% > 90.0\%$).

### The Reversal Upon Aggregation

Now combine all 100 patients treated by each doctor:
- **Dr. Hibbert Overall:** $70 + 10 = 80$ successes out of $90 + 10 = 100$ operations:
  $$\text{Overall Success Rate} = \frac{80}{100} = 80.0\%$$
- **Dr. Nick Overall:** $0 + 81 = 81$ successes out of $10 + 90 = 100$ operations:
  $$\text{Overall Success Rate} = \frac{81}{100} = 81.0\%$$

Dr. Nick has a **higher overall success rate** ($81\% > 80\%$), despite being substantially worse at both individual surgical procedures!

### Why Did This Happen?

The type of procedure is a **confounding variable**. Dr. Nick primarily performs easy bandage dressings ($90\%$ of his caseload), where everyone has a high success probability. Dr. Hibbert primarily takes on severe heart surgeries ($90\%$ of his caseload), where even the best surgeon experiences fatalities. 

The aggregate success rate is a **weighted average** of the subgroup rates. Because the weights (case difficulty allocation) differ drastically between doctors, the aggregate comparison is confounded.

---

## Definition

**Simpson's Paradox** is a statistical phenomenon where a trend, correlation, or inequality that appears across every mutually exclusive subpopulation is reversed or eliminated when the subpopulations are aggregated.

Formally, let $A$ denote the success event, $B$ denote assignment to group 1 (versus $B^c$ for group 2), and $\{C_1, C_2, \dots, C_k\}$ denote a partition of the population (the confounding variable). Simpson's paradox occurs when:

$$P(A \mid B, C_i) > P(A \mid B^c, C_i) \quad \text{for all } i \in \{1, 2, \dots, k\}$$

yet the unconditional or aggregate probabilities satisfy:

$$P(A \mid B) < P(A \mid B^c)$$

---

## How It Works

The mathematical mechanics are governed directly by the [[Law of Total Probability and Bayes' Rule]]:

1. **Expanding Aggregate Probability via Total Probability:**
   $$P(A \mid B) = \sum_{i=1}^k P(A \mid B, C_i) P(C_i \mid B)$$
   $$P(A \mid B^c) = \sum_{i=1}^k P(A \mid B^c, C_i) P(C_i \mid B^c)$$

2. **The Mechanism of Reversal:**
   Even though $P(A \mid B, C_i) > P(A \mid B^c, C_i)$ term-by-term, the weights $P(C_i \mid B)$ and $P(C_i \mid B^c)$ differ:
   - If group $B$ is disproportionately assigned to categories $C_i$ where the baseline success rate $P(A \mid C_i)$ is low (e.g., severe heart surgeries), its weighted average is dragged downward.
   - If group $B^c$ is predominantly assigned to categories where baseline success is high (e.g., routine bandages), its weighted average is lifted upward.

3. **Geometric Interpretation (Vector Slopes):**
   In vector arithmetic, it is completely possible for two vectors $\vec{v}_1 = (x_1, y_1)$ and $\vec{v}_2 = (x_2, y_2)$ to have lower slopes than $\vec{u}_1 = (w_1, z_1)$ and $\vec{u}_2 = (w_2, z_2)$ individually ($\frac{y_1}{x_1} < \frac{z_1}{w_1}$ and $\frac{y_2}{x_2} < \frac{z_2}{w_2}$), yet have their vector sum $\vec{v}_1 + \vec{v}_2 = (x_1+x_2, y_1+y_2)$ achieve a strictly higher slope than $\vec{u}_1 + \vec{u}_2$:
   $$\frac{y_1 + y_2}{x_1 + x_2} > \frac{z_1 + z_2}{w_1 + w_2}$$

---

## Example

### Worked Example: Machine Learning Model A/B Test

An e-commerce firm compares Model 1 and Model 2 for recommendation click-through rate (CTR) across Mobile and Desktop users:

| Platform | Model 1 (Clicks / Impressions) | Model 1 CTR | Model 2 (Clicks / Impressions) | Model 2 CTR | Winner |
|---|---|---|---|---|---|
| **Mobile** | $180 / 1000$ | **$18.0\%$** | $15 / 100$ | $15.0\%$ | Model 1 |
| **Desktop** | $40 / 100$ | **$40.0\%$** | $350 / 1000$ | $35.0\%$ | Model 1 |
| **Total** | $220 / 1100$ | **$20.0\%$** | $365 / 1100$ | **$33.18\%$** | **Model 2** |

- **Subgroup Verification:** Model 1 has a strictly higher CTR on Mobile ($18\% > 15\%$) and on Desktop ($40\% > 35\%$).
- **Aggregate Verification:** Model 2 wins the aggregate comparison ($33.18\% > 20.00\%$) because Desktop has a much higher baseline CTR ($35\text{--}40\%$) and Model 2 received $91\%$ of its traffic on Desktop, while Model 1 was primarily evaluated on low-converting Mobile users.
- **Resolution:** A randomized experiment must stratify or control for user device platform; the aggregated comparison is invalid.

---

## Technical Details

### When Does Simpson's Paradox Disappear?
Simpson's paradox cannot occur if either of the following conditions is satisfied:

1. **Equal Subgroup Proportions (Independence of Assignment):**
   If the confounding variable $C$ is conditionally independent of group assignment $B$:
   $$P(C_i \mid B) = P(C_i \mid B^c) = P(C_i) \quad \text{for all } i$$
   Then both weighted sums use identical weights, ensuring:
   $$P(A \mid B) = \sum_i P(A \mid B, C_i) P(C_i) > \sum_i P(A \mid B^c, C_i) P(C_i) = P(A \mid B^c)$$
   *(This is the theoretical justification for **Randomized Controlled Trials (RCTs)**: randomization breaks the dependence between treatment assignment and confounders).*

2. **Equal Subgroup Performance Across Groups:**
   If the baseline performance across all strata $C_i$ is identical, weighting differences cannot flip the outcome.

---

## Important Properties and Why They Hold

- **No Mathematical Contradiction:** Simpson's paradox is not a mathematical flaw; it is a valid property of conditional probabilities and weighted averages. The "paradox" is psychological—humans instinctively conflate conditional probabilities $P(A \mid B)$ with causal effects $P(A \mid \operatorname{do}(B))$.
- **Causal DAG Structure:** A confounder $C$ acts as a common cause of both group assignment $B$ and outcome $A$ ($B \leftarrow C \rightarrow A$). Aggregating over $C$ opens a non-causal backdoor path.

---

## Common Mistakes

- **Selecting the Aggregate Winner in A/B Testing:** Choosing a treatment based on pooled conversion numbers without checking for unequal cohort distribution.
- **Assuming Probability Inequalities Add Directly:** Believing that $\frac{a}{b} > \frac{c}{d}$ and $\frac{e}{f} > \frac{g}{h}$ implies $\frac{a+e}{b+f} > \frac{c+g}{d+h}$. Fractions with different denominators do not add this way.
- **Failing to Stratify Data:** Reporting naive observational averages without conditioning on obvious confounding covariates (e.g., severity of illness, platform type, demographics).

---

## Exam Relevance

In CSE 301 examinations, questions on Simpson's Paradox typically test:
1. **Verifying Reversal via Law of Total Probability:** Given a contingency table (e.g., surgeons or treatments), computing the two conditional probabilities within strata, then computing the pooled marginals to demonstrate inequality reversal.
2. **Identifying the Confounder:** Explaining why the aggregate winner won (identifying the category with heavy weighting and high baseline success).
3. **Condition for Prevention:** Proving why random assignment ($P(C \mid B) = P(C \mid B^c)$) prevents Simpson's paradox.

---

## Related Concepts

- [[Conditional Probability and Independence]] — Formal definition of conditional events.
- [[Law of Total Probability and Bayes' Rule]] — Partition theorem underpinning the weighted average expansions.
- [[Hypothesis Testing Framework]] — Stratified and paired hypothesis testing to control for confounders.

---

## Prerequisites

- [[Probability Axioms and Naive Probability]] — Sample spaces, events, and axiomatic foundations.
- [[Conditional Probability and Independence]] — Conditioning mechanics and sample space restriction.

---

## Sources

- [[cse301/01 - Sources/Lectures/HT Final Note - Suchi.pdf|HT Final Note: Lecture 6 (pp. 22–24, CT-syllabus)]]
- [[cse301/01 - Sources/Lectures/ITP.pdf|Textbook: Introduction to Probability (Blitzstein & Hwang, Section 2.3: Simpson's Paradox)]]
