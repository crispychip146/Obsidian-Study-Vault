---
type: example
course: cse301
status: active
order: 33
---

# Ace of Spades Conditioning Paradox Example

> 📖 **Reading Order:** Step 33 of 103 | **Module 3:** Conditional Probability and Conditioning  
> ◄ **Previous:** [[Monty Hall Problem Example]] | ► **Next:** [[Random Number of Random Variables Sum Example]]

---

## Problem

A player is dealt a 2-card hand uniformly at random from a standard, shuffled 52-card deck.

1. What is the probability that both cards are aces given that **at least one card is an ace**?
2. What is the probability that both cards are aces given that **one of the cards is the Ace of Spades ($A\spadesuit$)**?

---

## Given

- A standard deck contains $52$ cards, consisting of:
  - $4$ aces: $\{A\spadesuit, A\heartsuit, A\diamondsuit, A\clubsuit\}$
  - $48$ non-ace cards.
- A 2-card hand is an unordered selection of 2 cards. Total possible hands:
  $$|S| = \binom{52}{2} = \frac{52 \times 51}{2} = 1326$$
- Let $A$ denote the event: "Both cards are aces."
  $$|A| = \binom{4}{2} = \frac{4 \times 3}{2} = 6$$

---

## Required

Calculate:
1. $P(A \mid \text{at least one ace})$
2. $P(A \mid \text{have } A\spadesuit)$

Compare the results and explain why naming the specific suit nearly doubles the conditional probability.

---

## Understanding the Problem and Choosing the Method

### The Intuitive Puzzle
Many people find it shocking that the two answers differ. They argue:
> *"The Ace of Spades is just an ace like any other. Why should knowing the suit change anything? All four aces are symmetric!"*

### The Conditioning Mechanism
Conditional probability is defined as:
$$P(A \mid B) = \frac{P(A \cap B)}{P(B)} = \frac{|A \cap B|}{|B|}$$

The two conditioning events $B_1 = \{\text{at least one ace}\}$ and $B_2 = \{\text{have } A\spadesuit\}$ do not reduce the sample space in the same proportion:
- Knowing that *some* ace is present leaves $198$ possible hands, the vast majority of which have only one ace.
- Specifying the exact card ($A\spadesuit$) drastically shrinks the denominator to $51$ possible hands, eliminating hands with other aces that do not contain $A\spadesuit$.

---

## Solution

### Part 1: Conditioned on "At least one ace" ($B_1$)

1. **Size of Conditioning Event $B_1$:**
   Using the complement of drawing zero aces:
   $$|B_1^c| = \binom{48}{2} = \frac{48 \times 47}{2} = 1128$$
   $$|B_1| = \binom{52}{2} - \binom{48}{2} = 1326 - 1128 = 198$$

   *(Breakdown: $B_1$ contains $\binom{4}{1}\binom{48}{1} = 192$ hands with exactly 1 ace, plus $\binom{4}{2} = 6$ hands with 2 aces: $192 + 6 = 198$)*.

2. **Intersection $A \cap B_1$:**
   Having both aces is a subset of having at least one ace:
   $$|A \cap B_1| = |A| = 6$$

3. **Conditional Probability:**
   $$P(A \mid B_1) = \frac{|A \cap B_1|}{|B_1|} = \frac{6}{198} = \frac{1}{33} \approx \mathbf{0.0303} \quad (3.03\%)$$

---

### Part 2: Conditioned on "Hand contains the Ace of Spades" ($B_2$)

1. **Size of Conditioning Event $B_2$:**
   Since the hand must contain $A\spadesuit$, the second card can be any of the remaining $51$ cards in the deck:
   $$|B_2| = 51$$

2. **Intersection $A \cap B_2$:**
   For both cards to be aces when one is $A\spadesuit$, the second card must be one of the other $3$ aces ($\{A\heartsuit, A\diamondsuit, A\clubsuit\}$):
   $$|A \cap B_2| = 3$$

3. **Conditional Probability:**
   $$P(A \mid B_2) = \frac{|A \cap B_2|}{|B_2|} = \frac{3}{51} = \frac{1}{17} \approx \mathbf{0.0588} \quad (5.88\%)$$

---

## Result

$$\boxed{P(\text{both aces} \mid \text{at least one ace}) = \frac{1}{33} \approx 3.03\%}$$
$$\boxed{P(\text{both aces} \mid \text{have } A\spadesuit) = \frac{1}{17} \approx 5.88\%}$$

**Conditioning on the Ace of Spades nearly doubles the probability of having two aces** (from $3.03\%$ to $5.88\%$).

---

## Why This Works

To understand the mechanics, compare the ratios of (Two Aces) to (Single Ace) under both conditions:

1. **Under $B_1$ (Generic Ace):**
   - Single-ace hands: $192$
   - Two-ace hands: $6$
   - Ratio: $\frac{6}{192 + 6} = \frac{6}{198} = \frac{1}{33}$

2. **Under $B_2$ (Specific Ace $A\spadesuit$):**
   - Single-ace hands containing $A\spadesuit$: $48$ (pairing $A\spadesuit$ with any non-ace).
   - Two-ace hands containing $A\spadesuit$: $3$ (pairing $A\spadesuit$ with another ace).
   - Ratio: $\frac{3}{48 + 3} = \frac{3}{51} = \frac{1}{17}$

Notice what happened to the numbers:
- The single-ace hands were divided by $4$ (from $192$ down to $48$).
- But the two-ace hands were only divided by $2$ (from $6$ down to $3$), because hands with two aces have twice as many opportunities to contain $A\spadesuit$!

Because two-ace hands are **twice as likely to survive the condition** "contains $A\spadesuit$" as single-ace hands, specifying the suit enriches the proportion of two-ace hands in the conditioned sample space.

---

## Common Mistakes

- **Assuming Suit Irrelevance:** Assuming that because suits are symmetric, conditioning on a specific suit must yield the same answer as conditioning on the union of all suits. Conditioning on a specific outcome reduces the sample space much more aggressively.
- **Forgetting Hand Overlap:** Double-counting the two-ace hand $\{A\spadesuit, A\heartsuit\}$ when partitioning into individual suits.

---

## Related Concepts

- [[Conditional Probability and Independence]] — Sample space restriction.
- [[Combinatorics and Counting Principles]] — Hand combinations and sampling without replacement.
- [[Monty Hall Problem Example]] — Another famous instance where conditioning on specific information alters posterior probabilities.

---

## Sources

- [[cse301/01 - Sources/Lectures/HT Final Note - Suchi.pdf|HT Final Note: Lecture 5 (pp. 17–18)]]
- [[cse301/01 - Sources/Lectures/ITP.pdf|Textbook: Introduction to Probability (Blitzstein & Hwang, Section 2.4: Conditioning on Specific Information)]]
