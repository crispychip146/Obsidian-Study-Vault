---
type: concept
course: cse313
status: active
order: 22
---

# Message Passing and IPC Models

> 📖 **Reading Order:** Step 22 of 34 | **Module 4:** Inter-Process Communication & Synchronization  
> ◄ **Previous:** [[Monitors and Condition Variables]] | ► **Next:** [[Classic Synchronization Solutions]]

---

---

## Starting Point and the Problem

Shared-memory synchronization primitives (mutexes, semaphores, monitors) assume that all communicating threads share a single, unified physical address space.

We want a communication mechanism that operates uniformly whether communicating processes run on the same physical CPU, on a microkernel operating system with disjoint address spaces, or across different computers connected via a local network. The central obstacle is that in distributed environments or isolated address spaces, one process cannot dereference pointers or write to memory belonging to another process.

---

## Developing the Idea

The operating system provides an alternative IPC architecture: **Message Passing**.

Instead of sharing memory, processes communicate by explicitly transmitting self-contained data packets via two standardized kernel primitives:
1. **`send(destination, &message)`**
2. **`receive(source, &message)`**

The operating system kernel handles the data copying, synchronization, queuing, and network serialization transparently. Message passing unifies local inter-process communication (pipes, message queues, sockets) with distributed computing.

---

## Definition



---

## How It Works

### 2. Core Message Passing Primitives

The interface is centered around two fundamental system calls:
- **`send(destination, &message)`**
- **`receive(source, &message)`**

---

---

### 5. UNIX IPC Mechanisms Overview

Modern POSIX operating systems provide concrete message-passing primitives:

| Mechanism | Scope | Directionality | Characteristics |
|---|---|---|---|
| **Anonymous Pipe (`pipe()`)** | Parent-child related processes | Half-duplex (unidirectional byte stream) | Uses standard file descriptors; data destroyed once read. |
| **Named Pipe / FIFO (`mkfifo()`)** | Unrelated processes on same host | Bidirectional or Unidirectional | Exists as a filesystem node; survives process termination. |
| **UNIX Domain Socket (`AF_UNIX`)** | Any process on the same OS host | Full-duplex (bidirectional) | High performance, avoids network stack overhead. |
| **Network Sockets (`AF_INET`)** | Across distributed network hosts | Full-duplex byte stream / datagrams | Operates over TCP/IP network protocol stack. |

---

---

## Example

Producer-Consumer implemented via message passing with mailboxes:
```c
// Producer Process:
while (1) {
    item_t item = produce_item();
    message_t msg = { .data = item };
    send(mailbox_id, &msg); // Blocks if mailbox is full
}

// Consumer Process:
while (1) {
    message_t msg;
    receive(mailbox_id, &msg); // Blocks if mailbox is empty
    consume_item(msg.data);
}
```

---

## Technical Details

### 3. Key Design Dimensions of Message Passing Systems

Operating systems implement message passing across three fundamental architectural dimensions:

### 1. Direct vs Indirect Addressing (Naming)

| Model | Addressing Mechanism | Characteristics & Trade-offs |
|---|---|---|
| **Direct (Symmetric)** | `send(Process_B, &msg);`<br/>`receive(Process_A, &msg);` | Communication link is established automatically between exactly two processes. High coupling: altering a process ID breaks code. |
| **Direct (Asymmetric)** | `send(Process_B, &msg);`<br/>`receive(&sender_id, &msg);` | The receiver accepts messages from *any* sender and receives the sender's identity as an output parameter. |
| **Indirect (Mailboxes / Ports)** | `send(Mailbox_M, &msg);`<br/>`receive(Mailbox_M, &msg);` | Messages are sent to and received from named storage objects (mailboxes, message queues, or ports). Decouples processes: many senders and receivers can share one mailbox. |

---

### 2. Synchronization Disciplines (Blocking vs Non-blocking)

Communication can be either synchronous or asynchronous:

- **Blocking Send (Synchronous):** The sending process is suspended until the message is received by the destination process or successfully deposited into a mailbox.
- **Non-blocking Send (Asynchronous):** The sender deposits the message and resumes execution immediately without waiting.
- **Blocking Receive:** The receiver is suspended until a message is available.
- **Non-blocking Receive:** The receiver either retrieves a valid message or immediately receives a null indicator if no message is pending.

> [!NOTE] Rendezvous
> When **both** `send()` and `receive()` are blocking, the synchronization point is called a **Rendezvous**. Sender and receiver meet at the exact moment of message handover.

---

### 3. Buffering Capacity (Queue Sizing)

Every message channel has an internal buffer maintained by the operating system:

1. **Zero Capacity (No Buffering):**
   - The queue length is 0.
   - The sender **must** block until the receiver executes `receive()`. Forces strict rendezvous.
2. **Bounded Capacity:**
   - The queue has a finite capacity of $k$ messages.
   - If the queue is not full, the sender continues without blocking. If full, the sender is suspended until space is cleared.
3. **Unbounded Capacity:**
   - The queue can theoretically hold infinite messages.
   - The sender never blocks.

---

---

## Important Properties and Why They Hold

- **Zero-Sharing Memory Isolation:** Communicating processes do not share any state; memory corruption in one process cannot directly alter memory in the peer process.
- **Synchronization Coupling Dimensions:**
  - *Blocking (Synchronous):* `send()` blocks until receiver acknowledges receipt; creates a rendezvous.
  - *Non-Blocking (Asynchronous):* `send()` copies message to kernel buffer and returns immediately.
- **Buffering Invariant:** Systems with zero-capacity buffers require rendezvous; bounded/unbounded buffers allow producer to run ahead of consumer.

---

## Common Mistakes

- Assuming user mode code can execute privileged instructions directly without a system call trap.
- Overlooking race conditions in shared variables without explicit synchronization.

---

## Exam Relevance

Frequently examined through conceptual comparison questions, trace diagrams, and architectural trade-off evaluations.

---

## Related Concepts

- [[Semaphores and Synchronization Primitives]]
- [[Classic Synchronization Solutions]]
- [[Dual-Mode Operation and System Calls]]

---

## Prerequisites

- [[Process Concepts and Memory Layout]]
- [[Operating System Structures and Functions]]

---

## Problems

- [[Problem — Dining Philosophers Deadlock-Free Synchronization]]

---

## Sources

- **Source Material:** `4. IPC-week-4-5-RRR.pptx` (Slides 53–60: Message Passing, Addressing, Buffering, Pipes and Network Sockets).
- **Previous Topic:** [[Monitors and Condition Variables]] (Step 21).
- **Next Topic:** [[Classic Synchronization Solutions]] (Step 23).
