---
type: concept
course: cse313
status: active
order: 67
---

# Linux System Architecture and Remote Procedure Calls (RPC)

> 📖 **Reading Order:** Step 67 of 68 | **Module 10: Advanced Kernel Systems & Multiprocessors**  
> ◄ **Previous:** [[Multiprocessor Operating System Architectures]] | ► **Next:** [[Problem — Multiprocessor Memory Latency and Kernel Memory Allocation]]

---

## Linux System Architecture Layers

As presented in Tanenbaum's Modern Operating Systems (slides 428–435), the modern Linux operating system is structured into five hierarchical layers:

```
Linux System Hierarchy:
+-----------------------------------------------------------------------+
| 1. User Applications & Utilities: bash, gcc, grep, cp, ssh, web apps   |
+-----------------------------------------------------------------------+
| 2. Standard System Libraries: POSIX C Library (glibc)                 |
+-----------------------------------------------------------------------+
| 3. System Call Interface: sysenter / syscall / int 0x80 trap handler  |
+-----------------------------------------------------------------------+
| 4. Linux Kernel Core Subsystems:                                      |
|    - Process Management (scheduler, signals, clone, task_struct)     |
|    - Memory Management (paging, TLB flushes, slab allocator)         |
|    - Virtual File System (VFS abstraction over ext4, NFS, FAT)       |
|    - I/O Subsystem (block/char drivers, elevator queue, buffer cache) |
|    - Networking Subsystem (BSD socket API, TCP/IP stack)             |
+-----------------------------------------------------------------------+
| 5. Hardware Platform: CPU cores, MMU, RAM, storage controllers, NICs  |
+-----------------------------------------------------------------------+
```

### Process Creation in Linux: `clone()` and Flag Masking
Unlike traditional UNIX that distinguished rigidly between processes and threads, Linux unifies both abstractions under the same kernel descriptor: `struct task_struct`.

The fundamental process creation system call is **`clone()`**:
```c
int clone(int (*fn)(void *), void *child_stack, int flags, void *arg);
```
The `flags` bitmask allows fine-grained specification of what the child shares with the parent:

| Clone Flag | Resource Controlled | Behavior when Set ($=1$) |
|---|---|---|
| **`CLONE_VM`** | Virtual Memory | Child shares parent's page tables and address space (Creates a Thread!). |
| **`CLONE_FS`** | File System Root / CWD | Child shares root directory, working directory, and umask. |
| **`CLONE_FILES`** | Open File Descriptors | Child shares the same file descriptor table (closing a fd affects both). |
| **`CLONE_SIGHAND`**| Signal Handlers | Child shares signal action handlers. |
| **`CLONE_THREAD`** | Thread Group ID | Child is placed in the same thread group (shares TGID / PID). |

- **Traditional `fork()`:** Implemented internally as `clone()` with all sharing flags set to **zero** (`flags = SIGCHLD`). The child receives a separate Copy-on-Write (COW) address space.
- **POSIX `pthread_create()`:** Implemented internally as `clone()` with `CLONE_VM | CLONE_FS | CLONE_FILES | CLONE_SIGHAND | CLONE_THREAD` asserted, creating a lightweight thread sharing the parent's memory space.

---

## Inter-System Communication: The Need for Distributed Primitives

## Developing the Idea: The Remote Procedure Call (RPC)

The goal of RPC is to make a procedure call on a **remote machine across a network look identical to a local function call in code**.

```c
// Client code looks like a normal local function call!
int total = calculate_sum(a, b);
```

Under the hood, the RPC runtime transparently packages the parameters into a network packet, transmits them to the server, executes the function on the server CPU, and returns the result to the caller!

---

## How It Works: The Five-Component RPC Architecture

```mermaid
sequenceDiagram
    participant Client as Client Application
    participant CStub as Client Stub
    participant Net as Network Transport (UDP/TCP)
    participant SStub as Server Stub
    participant Server as Server Function

    Client->>CStub: 1. Local Call: calculate_sum(a, b)
    Note over CStub: 2. Marshalling:<br/>Pack args into network buffer
    CStub->>Net: 3. Send message across socket
    Net->>SStub: 4. Deliver network packet
    Note over SStub: 5. Unmarshalling:<br/>Extract args from buffer
    SStub->>Server: 6. Local Call: calculate_sum(a, b)
    Server-->>SStub: 7. Return Result
    Note over SStub: 8. Marshall Result into buffer
    SStub->>Net: 9. Send reply packet
    Net->>CStub: 10. Deliver reply packet
    Note over CStub: 11. Unmarshall Result
    CStub-->>Client: 12. Return Result to caller!
```

### The Step-by-Step Execution Sequence:
1. **Client Application:** Calls the target function normally (e.g., `calculate_sum(a, b)`).
2. **Client Stub:** A generated wrapper function that intercepts the call. It performs **Marshalling (Serialization)**: packing the function identifier and parameters into a canonical, machine-independent network byte stream (such as XDR - External Data Representation, JSON, or Protocol Buffers).
3. **Transport Layer:** The client stub transmits the marshalled packet across the network using UDP or TCP.
4. **Server Stub:** Intercepts the incoming network packet. It performs **Unmarshalling (Deserialization)**: decoding the byte stream into native data types.
5. **Server Function:** The server stub calls the actual local server function with the unpacked arguments.
6. **Return Path:** The server function returns its result to the server stub; the stub marshalls the result and transmits it back across the network to the client stub, which unmarshalls the value and returns it to the client application!

---

## Interface Definition Language (IDL) and Stub Generation

How are client and server stubs created? Manually writing socket packing for dozens of functions is impractically bug-prone.

Systems use an **Interface Definition Language (IDL)** and a compiler (such as `rpcgen` in UNIX/Linux, or `protoc` for gRPC):
```c
// Example: math.x (IDL specification)
program MATH_PROG {
    version MATH_VERS {
        int ADD(pair_of_ints) = 1;
    } = 1;
} = 0x20000001;
```
Running `rpcgen math.x` automatically compiles this specification into:
- `math_clnt.c` (Client Stub)
- `math_svc.c` (Server Stub / Skeleton)
- `math_xdr.c` (Marshalling / Unmarshalling routines)

---

## Network Transport Choices: RPC over UDP vs. TCP

| Metric | RPC over UDP | RPC over TCP |
|---|---|---|
| **Connection Overhead** | Zero connection setup ($0$ handshakes) | 3-way handshake required ($1.5$ RTT) |
| **Packet Overhead** | Tiny 8-byte UDP header | 20-byte TCP header + sequence numbers |
| **Reliability** | Unreliable; caller must implement timeout/retry | Guaranteed reliable delivery & ordering |
| **Ideal Workloads** | Lightweight, **Idempotent** operations (NFS reads, DNS lookups) | Large payloads, non-idempotent operations (e.g., fund transfers) |

### The Idempotency Principle:
An operation is **Idempotent** if executing it multiple times produces the exact same outcome as executing it once (e.g., `Read Block 5`, `Set Temperature to 22`).
- Idempotent operations can safely use lightweight UDP with simple retry timeouts.
- Non-idempotent operations (e.g., `Append $500 to account balance`) require TCP or strict transaction IDs to prevent double execution upon network packet retransmission!

### The "Implicit ACK" Strategy in RPC over UDP (Slide 445)
Why do systems frequently choose UDP over TCP for remote procedure calls despite UDP being unreliable?
- Under TCP, a client sending a request receives a TCP ACK, waits for the server computation, receives the response, and sends another TCP ACK—requiring multiple round-trips and connection setup handshakes.
- **The Implicit ACK Optimization:** In RPC over UDP:
  1. The client transmits the RPC request packet and starts a retransmission timer.
  2. The server processes the request and sends the **Function Return Value** back to the client.
  3. **The function return value packet ITSELF acts as the implicit acknowledgement (ACK) for the request!**
  4. If the client receives the return value before its timer expires, it knows the request arrived and executed successfully with zero separate ACK packets sent across the wire.
  5. If the timer expires without a reply, the client assumes packet loss and simply retransmits the request.

---

## Important Properties and Guarantees

- **Transparency Boundary Limit:** RPC achieves syntactic transparency (calling syntax matches local calls), but cannot achieve semantic transparency. Local calls never fail with network timeouts, packet drops, or server reboots, whereas remote calls must always handle partial failure modes.
- **Canonical Endianness Invariant:** Marshalling converts host byte order (whether Big-Endian or Little-Endian) into standard **Network Byte Order (Big-Endian)** using `htonl()` and `htons()`, guaranteeing platform interoperability.

---

## Common Mistakes

- **Passing Memory Pointers Across RPC:** A pointer is an address within the *local process virtual address space*. Passing `int *ptr` across a network to a remote machine is meaningless and crashes the server! Pointers must be dereferenced and the actual data payload marshalled.
- **Ignoring Network Partial Failures:** Assuming an RPC call either succeeds or fails cleanly. A server may execute the function, crash *right before* transmitting the reply, causing the client to believe the call failed even though it succeeded!

---

## Exam Relevance

Frequently tested in Operating Systems and Distributed Systems exams:
- Drawing and explaining the complete 12-step RPC architectural timeline.
- Defining Marshalling, Unmarshalling, and the role of Interface Definition Languages (IDL).
- Comparing RPC over UDP vs RPC over TCP and explaining why memory pointers cannot be passed directly across RPC calls.

---

## Related Concepts

- [[Message Passing and IPC Models]]
- [[Multiprocessor Operating System Architectures]]
- [[Dual-Mode Operation and System Calls]]

---

## Prerequisites

- [[Message Passing and IPC Models]]
- [[Dual-Mode Operation and System Calls]]

---

## Problems

- [[Problem — Multiprocessor Memory Latency and Kernel Memory Allocation]]

---

## Sources

- **Lecture Slides:** [[cse313/01 - Sources/Lectures/CSE313_KRV_Merged.pdf|CSE313_KRV_Merged.pdf]], Tanenbaum Linux & RPC (Slides 428–445).
- **Textbook:** Tanenbaum & Bos, *Modern Operating Systems (3rd/4th Ed.)*, Chapter 10 (UNIX and Linux).
