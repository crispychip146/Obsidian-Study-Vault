---
type: formula
course: cse313
status: active
order: 9
---

# CPU Multiprogramming Utilization Formula

> 📖 **Reading Order:** Step 09 of 34 | **Module 2:** Processes & Threads  
> ◄ **Previous:** [[Threads and Multithreading Models]] | ► **Next:** [[Process Forking and Zombie Orphan Example]]

---

## Mathematical Statement

Let $n$ denote the **degree of multiprogramming** (the number of independent processes loaded concurrently into main memory). Let $p$ denote the average fraction of time each process spends waiting for I/O operations to complete (where $0 \le p \le 1$).

Under the probabilistic assumption that process I/O requests are statistically independent, the **CPU Utilization** (the fraction of time the processor is actively executing instructions) is given by:

$$\text{CPU Utilization} = 1 - p^n$$

Equivalently, the probability that the CPU is completely idle (zero processes ready to compute) is:
$$P(\text{CPU Idle}) = p^n$$

---

## Intuitive Derivation & Probabilistic Logic

1. Consider a single process running in isolation ($n = 1$). By definition, it spends fraction $p$ of its time blocked on I/O. Therefore:
   $$\text{CPU Utilization} = 1 - p$$
   If an I/O-intensive process spends $80\%$ of its time waiting for disk reads ($p = 0.80$), the expensive CPU core sits completely idle for $80\%$ of the day!

2. Now suppose $n$ independent processes are kept resident in physical memory simultaneously.
3. If each process blocks on I/O with probability $p$, and their behaviors are mutually independent, the probability that **all $n$ processes are simultaneously blocked on I/O** is:
   $$P(\text{All } n \text{ processes blocked}) = p \times p \times \dots \times p = p^n$$
4. The CPU is idle if and only if every single process is blocked.
5. Therefore, by the complement rule of probability, the CPU has at least one ready process to execute with probability:
   $$\text{CPU Utilization} = 1 - P(\text{All } n \text{ processes blocked}) = 1 - p^n$$
$\blacksquare$

---

## Numerical Analysis: The Power of Multiprogramming

Consider a workload where processes spend $p = 0.80$ ($80\%$) of their total lifecycle waiting for I/O:

| Degree of Multiprogramming ($n$) | Idle Probability ($p^n = 0.80^n$) | CPU Utilization ($1 - p^n$) | Marginal Gain ($\Delta$) |
|---|---|---|---|
| **$n = 1$ (Uniprogrammed)** | $0.8000$ ($80.0\%$) | **$20.00\%$** | Baseline |
| **$n = 2$** | $0.6400$ ($64.0\%$) | **$36.00\%$** | $+16.00\%$ |
| **$n = 3$** | $0.5120$ ($51.2\%$) | **$48.80\%$** | $+12.80\%$ |
| **$n = 4$** | $0.4096$ ($41.0\%$) | **$59.04\%$** | $+10.24\%$ |
| **$n = 6$** | $0.2621$ ($26.2\%$) | **$73.79\%$** | $+14.75\%$ |
| **$n = 8$** | $0.1678$ ($16.8\%$) | **$83.22\%$** | $+9.43\%$ |
| **$n = 10$** | $0.1074$ ($10.7\%$) | **$89.26\%$** | $+6.04\%$ |
| **$n = 15$** | $0.0352$ ($3.5\%$) | **$96.48\%$** | $+7.22\%$ |
| **$n = 20$** | $0.0115$ ($1.2\%$) | **$98.85\%$** | $+2.37\%$ |

```mermaid
xychart-beta
    title "CPU Utilization vs. Degree of Multiprogramming (p = 0.8)"
    x-axis [1, 2, 3, 4, 6, 8, 10, 15, 20]
    y-axis "CPU Utilization (%)" 0 --> 100
    bar [20.0, 36.0, 48.8, 59.0, 73.8, 83.2, 89.3, 96.5, 98.9]
```

---

## Practical Hardware Design Implication: Sizing RAM

This formula guides physical memory capacity planning in operating systems:
- Suppose a computer has $2\text{ GB}$ of RAM, with the OS taking $512\text{ MB}$ and each user process requiring $256\text{ MB}$.
- The maximum degree of multiprogramming is:
  $$n = \frac{2048 - 512}{256} = 6 \text{ processes}$$
  With $p = 0.80$, CPU utilization is $1 - 0.8^6 \approx 73.8\%$.
- Upgrading RAM to $4\text{ GB}$ allows $n = \frac{4096 - 512}{256} \approx 14$ processes.
  CPU utilization surges from $73.8\%$ to $1 - 0.8^{14} \approx 95.6\%$, yielding a **$21.8\%$ increase in total computational throughput** simply by adding memory!
- **Law of Diminishing Returns:** Upgrading beyond $15$ processes produces negligible CPU gains ($< 2\%$), while consuming expensive memory and increasing scheduling overhead.

---

## Model Limitations & Assumptions

While invaluable for conceptual modeling, the formula makes simplifying assumptions that break down in extreme cases:
1. **Assumes Independent I/O:** Processes are rarely purely independent. In reality, multiple processes compete for the same physical disk controller or network interface, causing queueing bottlenecks.
2. **Ignores Context Switching Overhead:** Context switches consume non-zero CPU time. If $n$ becomes excessively large, physical memory is exhausted, triggering paging/thrashing where the CPU spends $99\%$ of its time swapping pages to disk.

---

## Cross-Topic Connections / Exam Relevance

- **Next Step:** See concrete C code demonstrations of process spawning and state tracing (see [[Process Forking and Zombie Orphan Example]]).
- **Scheduling:** The degree of multiprogramming determines the length of the Ready Queue in [[CPU Scheduling Principles and Criteria]].
- **Exam Testing:** Common numerical question on midterms:
  - "Given that processes spend 70% of their time waiting for I/O, how many processes must be resident in memory to achieve at least 90% CPU utilization?"
  - *Calculation:* $1 - 0.70^n \ge 0.90 \implies 0.70^n \le 0.10 \implies n \ln(0.70) \le \ln(0.10) \implies n(-0.3567) \le -2.3026 \implies n \ge 6.45 \implies n = 7 \text{ processes}$.

---

## Sources & Traceability

- **Lectures:** `cse313/01 - Sources/Lectures/2. ProcessAndThread-week2-RRR.pdf` (Slides 16–21)
- **Textbook:** Andrew S. Tanenbaum & Herbert Bos, *Modern Operating Systems* (4th Edition), Chapter 2 (Section 2.1.4: Modeling Multiprogramming)
