---
type: problem
course: cse309
status: active
order: 55
---

# Problem — Quicksort Loop Induction Variable Strength Reduction

> 📖 **Reading Order:** Step 55 of 55 | **Module 6:** Machine-Independent Optimization  
> ◄ **Previous:** [[Quicksort Partition Loop Complete Optimization Example]] | ► **Next:** [[cse309/00 - Course Hub|Course Hub]]

---

## Problem

Consider the following basic block $B$ representing the inner step of an array traversal loop:

```
B:
    i = i + 1
    t1 = 8 * i
    t2 = b[t1]
    sum = sum + t2
    if i < 100 goto B
```

Assume $i$ is initialized before the loop to $0$ ($i = 0$), and array elements are 8-byte double-precision floating-point numbers.

### Tasks:
1. Identify the **Basic Induction Variable** and the **Derived Induction Variable** in this loop.
2. Apply **Strength Reduction** to replace the multiplication $8 * i$ with an addition. Show the code to be inserted into the loop pre-header and the updated loop body.
3. Apply **Induction Variable Elimination** to eliminate the basic induction variable $i$ from the loop entirely, transforming the termination condition.
4. Compare the instruction count and hardware cycle savings per iteration before and after optimization.

---

## Solution

The loop increments i before loading, so its first access uses i=1, not i=0. That timing determines the strength-reduction initialization.

Initialize an offset t to 8·0=0 in the preheader. In the body, replace `i=i+1; t1=8*i` with `t=t+8`; the first load uses byte offset 8. The invariant t=8i then holds after each paired update. This is the argument from [[Loop Optimizations and Strength Reduction]], specialized to eight-byte elements.

If i has no other required use, replace `i<100` with `t<800` under the stated integer-range assumptions. Do not delete i if its final value is needed after the loop. Also note that the original body accesses the element corresponding to index 100 before testing whether to repeat; preserving semantics is separate from validating the array's declared capacity.

Any cycle-savings calculation uses the question's instruction-cost assumptions, not a universal claim that multiplication has a fixed cost on all CPUs.

### Part 1: Identifying Induction Variables
- **Basic Induction Variable:** Variable $i$, because it is modified exclusively by the constant increment $i = i + 1$ ($c = 1$).
- **Derived Induction Variable:** Variable $t_1$, because it is a linear function of $i$:
  $$t_1 = 8 \times i + 0$$
  where $c_1 = 8, c_2 = 0$.

---

### Part 2: Applying Strength Reduction

1. **Pre-Header Initialization:**
   Before entering the loop, $i = 0$. The initial value of $t_1$ is:
   $$t_1 = 8 \times 0 = 0$$
   Insert into pre-header:
   ```
   i = 0
   t1 = 0
   ```
2. **Loop Body Transformation:**
   Because $i$ increases by $1$ each iteration, $t_1$ increases by:
   $$\Delta t_1 = 8 \times \Delta i = 8 \times 1 = 8$$
   Replace $t_1 = 8 * i$ inside the loop with an addition:
   $$t_1 = t_1 + 8$$

```
Updated Loop Body (after Strength Reduction):
B:
    i = i + 1
    t1 = t1 + 8       /* Multiplication replaced with fast addition! */
    t2 = b[t1]
    sum = sum + t2
    if i < 100 goto B
```

---

### Part 3: Applying Induction Variable Elimination

Notice that the only remaining use of variable $i$ inside the entire loop is the termination comparison:
$$\text{if } i < 100 \text{ goto } B$$

Since $t_1 = 8 * i$, multiplying both sides of the inequality by 8 preserves the relational truth:
$$i < 100 \iff 8 * i < 8 * 100 \iff t_1 < 800$$

1. **Transform Loop Exit Condition:**
   $$\text{if } t_1 < 800 \text{ goto } B$$
2. **Dead Code Elimination:**
   Now variable $i$ is **never read anywhere in the loop**!
   The statement `i = i + 1` is completely dead code. **Delete `i = i + 1`!**

---

### Final Optimized Code:

```
; Pre-header:
    t1 = 0

; Loop Body:
B:
    t1 = t1 + 8
    t2 = b[t1]
    sum = sum + t2
    if t1 < 800 goto B
```

---
### Quantitative Performance Analysis

| Metric | Original Code | Fully Optimized Code | Improvement |
| :--- | :---: | :---: | :---: |
| **Instructions per Iteration** | 5 | **4** | **20% fewer instructions** |
| **Multiplications per Iteration** | 1 (`MUL`) | **0** | **100% eliminated** |
| **Additions per Iteration** | 2 | **2** | Identical |
| **Registers Required** | 4 (`i, t1, t2, sum`) | **3 (`t1, t2, sum`)** | **1 register freed!** |
| **Estimated Clock Cycles** | ~8 cycles (MUL is slow) | **~3 cycles** | **~60% faster inner loop!** |

---

## What to carry forward

Check the first iteration, the last accessed offset, and the exit condition. They catch a misplaced offset update more effectively than comparing only a middle iteration. Preserve live-out values and overflow behavior when eliminating an induction variable.

## Related notes

- [[Loop Optimizations and Strength Reduction]]

## Source

- **Lecture Slides:** [[cse309/01 - Sources/Lectures/KMS Merged.pdf|KMS Merged.pdf]], Chapter 9 (Slides 558–566).
- **Textbook:** Aho, Lam, Sethi, Ullman, *Compilers: Principles, Techniques, & Tools* (2nd Ed.), Section 9.1.3 & 9.1.4.
