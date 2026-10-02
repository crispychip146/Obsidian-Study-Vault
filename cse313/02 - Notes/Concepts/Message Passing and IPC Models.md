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

## 1. Architectural Overview & Motivation

In previous notes, synchronization relied on **shared memory** (shared variables, semaphores, monitors). However, shared memory has two critical limitations:
1. It is prone to race conditions if developers omit locking primitives.
2. It cannot function across distributed systems or computer networks where CPUs do not share physical RAM.

**Message Passing** provides both communication and synchronization through explicit message exchanges managed by the OS kernel or network subsystem.

```
+------------------------------------------------------------+
|                    SHARED MEMORY MODEL                     |
|  Process A [Write] ----> [ Shared RAM ] <---- [Read] Process B|
|  (Zero OS intervention for transfers; locks required)      |
+------------------------------------------------------------+
|                   MESSAGE PASSING MODEL                    |
|  Process A ---> [OS Kernel / Network / Mailbox] ---> Process B |
|  send(B, msg)                                 receive(A, &msg)|
+------------------------------------------------------------+
```

---

## 2. Core Message Passing Primitives

The interface is centered around two fundamental system calls:
- **`send(destination, &message)`**
- **`receive(source, &message)`**

---

## 3. Key Design Dimensions of Message Passing Systems

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

## 4. Producer-Consumer Implementation with Message Passing

Message passing eliminates shared counters and semaphores. Instead, the consumer feeds empty message packets to the producer, and the producer sends back filled message packets:

```c
#define N 100 // Buffer capacity

void producer(void) {
    message m;
    while (TRUE) {
        item data = produce_item();
        receive(consumer, &m);      // Wait for an empty message token from consumer
        build_message(&m, data);
        send(consumer, &m);         // Send filled message to consumer
    }
}

void consumer(void) {
    message m;
    // Step 1: Send N empty messages to the producer initially
    for (int i = 0; i < N; i++) {
        send(producer, &m);
    }

    while (TRUE) {
        receive(producer, &m);      // Wait for a filled message from producer
        item data = extract_item(&m);
        send(producer, &m);         // Return empty message token back to producer
        consume_item(data);
    }
}
```

---

## 5. UNIX IPC Mechanisms Overview

Modern POSIX operating systems provide concrete message-passing primitives:

| Mechanism | Scope | Directionality | Characteristics |
|---|---|---|---|
| **Anonymous Pipe (`pipe()`)** | Parent-child related processes | Half-duplex (unidirectional byte stream) | Uses standard file descriptors; data destroyed once read. |
| **Named Pipe / FIFO (`mkfifo()`)** | Unrelated processes on same host | Bidirectional or Unidirectional | Exists as a filesystem node; survives process termination. |
| **UNIX Domain Socket (`AF_UNIX`)** | Any process on the same OS host | Full-duplex (bidirectional) | High performance, avoids network stack overhead. |
| **Network Sockets (`AF_INET`)** | Across distributed network hosts | Full-duplex byte stream / datagrams | Operates over TCP/IP network protocol stack. |

---

## Source Traceability & Metadata
- **Source Material:** `4. IPC-week-4-5-RRR.pptx` (Slides 53–60: Message Passing, Addressing, Buffering, Pipes and Network Sockets).
- **Previous Topic:** [[Monitors and Condition Variables]] (Step 21).
- **Next Topic:** [[Classic Synchronization Solutions]] (Step 23).
