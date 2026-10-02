---
type: example
course: cse301
status: active
order: 22
---

# Monty Hall Problem Example

> 📖 **Reading Order:** Step 22 of 92 | **Module 3:** Conditional Probability and Conditioning  
> ◄ **Previous:** [[Eve's Law (Law of Total Variance)]] | ► **Next:** [[Random Number of Random Variables Sum Example]]

---

## Problem Context & Setup

On a game show, you are presented with three closed doors ($1, 2, 3$):
- Behind one door is a car (the grand prize).
- Behind the other two doors are goats.

The game proceeds as follows:
1. You pick an initial door (say, Door 1).
2. The host, Monty Hall, who knows what is behind every door, opens one of the remaining two doors (Door 2 or Door 3) to reveal a goat.
3. If Monty has a choice of two goats to reveal (which happens if your initial pick has the car), he chooses between them with equal probability ($1/2$).
4. Monty then offers you a choice: **"Do you want to stick with Door 1, or switch to the remaining closed door?"**

**Question:** Does switching increase your probability of winning the car? If so, what is the winning probability under switching?

---

## Step-by-Step Solution

### Method 1: The Intuitive Complement / Partition Argument
Let $C_i$ be the event that the car is behind Door $i$ ($i \in \{1, 2, 3\}$).
Prior to Monty opening any door:
$$P(C_1) = \frac{1}{3}, \quad P(C_2 \cup C_3) = \frac{2}{3}$$

- **Strategy: Stick with Door 1**
  You win if and only if the car was behind Door 1 originally.
  $$P(\text{Win by Sticking}) = P(C_1) = \frac{1}{3}$$

- **Strategy: Always Switch**
  - If the car is behind Door 1 (probability $1/3$), switching makes you pick a goat $\implies$ you lose.
  - If the car is behind Door 2 (probability $1/3$), Monty is **forced** to open Door 3. Switching takes you to Door 2 $\implies$ you win!
  - If the car is behind Door 3 (probability $1/3$), Monty is **forced** to open Door 2. Switching takes you to Door 3 $\implies$ you win!

Therefore:
$$P(\text{Win by Switching}) = 0 \cdot \frac{1}{3} + 1 \cdot \frac{1}{3} + 1 \cdot \frac{1}{3} = \frac{2}{3}$$

**Switching doubles your chances of winning from $1/3$ to $2/3$!**

---

### Method 2: Rigorous Application of Bayes' Rule

Without loss of generality, suppose you choose Door 1, and Monty opens Door 2 to reveal a goat (call this evidence event $M_2$).
We want to evaluate $P(C_1 \mid M_2)$ vs. $P(C_3 \mid M_2)$.

1. **Prior Probabilities:**
   $$P(C_1) = P(C_2) = P(C_3) = \frac{1}{3}$$

2. **Likelihoods of Monty Opening Door 2 ($P(M_2 \mid C_i)$):**
   - If car is behind Door 1: Monty can open Door 2 or 3. By the rules, he chooses randomly:
     $$P(M_2 \mid C_1) = \frac{1}{2}$$
   - If car is behind Door 2: Monty cannot reveal the car!
     $$P(M_2 \mid C_2) = 0$$
   - If car is behind Door 3: Monty cannot reveal the car (Door 3) and cannot open your pick (Door 1). He is **forced** to open Door 2:
     $$P(M_2 \mid C_3) = 1$$

3. **Marginal Probability of Evidence $P(M_2)$ (via LTP):**
   $$P(M_2) = P(M_2 \mid C_1)P(C_1) + P(M_2 \mid C_2)P(C_2) + P(M_2 \mid C_3)P(C_3)$$
   $$P(M_2) = \left(\frac{1}{2}\right)\left(\frac{1}{3}\right) + (0)\left(\frac{1}{3}\right) + (1)\left(\frac{1}{3}\right) = \frac{1}{6} + \frac{1}{3} = \frac{1}{2}$$

4. **Posterior Probabilities via Bayes' Rule:**
   - **For Door 1 (Sticking):**
     $$P(C_1 \mid M_2) = \frac{P(M_2 \mid C_1)P(C_1)}{P(M_2)} = \frac{(1/2)(1/3)}{1/2} = \frac{1/6}{1/2} = \frac{1}{3}$$
   - **For Door 3 (Switching):**
     $$P(C_3 \mid M_2) = \frac{P(M_2 \mid C_3)P(C_3)}{P(M_2)} = \frac{(1)(1/3)}{1/2} = \frac{1/3}{1/2} = \frac{2}{3}$$

---

## Why Common Intuition Fails (The 50/50 Fallacy)

People instinctively assume: *"Two doors are left, so the probability must be 50/50."*
The fallacy overlooks the **host's protocol and knowledge**:
- Monty is not opening a door at random (which might reveal a car).
- Monty acts as an informational funnel: all $2/3$ probability that the car was in $\{ \text{Door 2, Door 3} \}$ gets concentrated into whichever of those two doors Monty did **not** open!

---

## Related Notes

- [[Conditional Probability and Independence]] — Sample space reduction.
- [[Law of Total Probability and Bayes' Rule]] — The mathematical machinery used in Method 2.

---

## Sources & Traceability

- **Lectures:** `cse301/01 - Sources/Lectures/Lecture_Notes_Complete.pdf` (Lecture 5, pages 15–16)
- **Practice Sets:** `cse301/01 - Sources/Lectures/strategic_practice_and_homework_3.pdf`
- **Textbook:** Blitzstein & Hwang, *Introduction to Probability* (Section 2.4: Monty Hall)
