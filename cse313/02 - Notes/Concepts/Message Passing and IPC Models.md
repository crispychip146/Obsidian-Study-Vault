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

## Building the idea

[[Threads and Multithreading Models]] allows activities to communicate through shared memory. Separate processes can instead exchange messages: one sends a piece of information, and the other receives it through a communication channel. This changes the coordination problem from who may access a shared variable to who can send, where a message waits, and when each operation completes.

Keep three design choices independent. **Naming:** address a process directly or a mailbox shared by participants. **Blocking:** wait for completion or return immediately under the API's rules. **Buffering:** store messages, store a bounded number, or require sender and receiver to meet.

A zero-capacity channel illustrates rendezvous: the sender cannot leave a message behind, so handover needs a receiving participant. With a bounded mailbox, the sender can often return after enqueueing even if the receiver has not consumed the message. When it fills, capacity creates backpressure.

In the producer-consumer example, blocking receive replaces waiting for a nonempty shared buffer. Blocking send on a full mailbox replaces waiting for an empty slot. The channel implements that coordination; the application's protocol must still define valid messages and their meaning.

## How It Works

### 2. Core Message Passing Primitives

The interface is centered around two fundamental system calls:
- **`send(destination, &message)`**
- **`receive(source, &message)`**

---

### 5. UNIX IPC Mechanisms Overview

Modern POSIX operating systems provide concrete message-passing primitives:

| Mechanism | Scope | Directionality | Characteristics |
|---|---|---|---|
| **Anonymous Pipe (`pipe()`)** | Parent-child related processes | Half-duplex (unidirectional byte stream) | Uses standard file descriptors; data destroyed once read. |
| **Named Pipe / FIFO (`mkfifo()`)** | Unrelated processes on same host | Portably unidirectional; two channels can support two-way traffic | Exists as a filesystem node; survives process termination. |
| **UNIX Domain Socket (`AF_UNIX`)** | Any process on the same OS host | Full-duplex (bidirectional) | High performance, avoids network stack overhead. |
| **Network Sockets (`AF_INET`)** | Across distributed network hosts | Full-duplex byte stream / datagrams | Operates over TCP/IP network protocol stack. |

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
> A **zero-capacity synchronous channel** requires sender and receiver to rendezvous for handover. Both APIs being blocking is insufficient by itself: a buffered send may finish once enqueued, before receipt.

---

### 3. Buffering Capacity (Queue Sizing)

The communication model specifies a buffering policy:

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

## Important Properties and Why They Hold

- **Zero-Sharing Memory Isolation:** Communicating processes do not share any state; memory corruption in one process cannot directly alter memory in the peer process.
- **Synchronization Coupling Dimensions:**
  - *Blocking:* `send()` waits until its API-defined completion condition holds; this may mean enqueueing or synchronous receipt, depending on buffering and protocol.
  - *Non-Blocking (Asynchronous):* `send()` copies message to kernel buffer and returns immediately.
- **Buffering Invariant:** Systems with zero-capacity buffers require rendezvous; bounded/unbounded buffers allow producer to run ahead of consumer.

---

## What to carry forward

Blocking send does not universally mean acknowledged consumption; check the API's completion rule. Byte-stream pipes and sockets may not preserve application message boundaries, so protocols need framing. [[Classic Synchronization Solutions]] compares the same cooperation problems using shared-memory primitives.

## Related notes

- [[Threads and Multithreading Models]]
- [[Classic Synchronization Solutions]]

## Sources

- **Source Material:** `4. IPC-week-4-5-RRR.pptx` (Slides 53–60: Message Passing, Addressing, Buffering, Pipes and Network Sockets).
- **Previous Topic:** [[Monitors and Condition Variables]] (Step 21).
- **Next Topic:** [[Classic Synchronization Solutions]] (Step 23).
