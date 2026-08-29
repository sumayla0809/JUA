# 2. Inter-Process Communication (IPC)

## 1. IPC ⭐⭐⭐

**Inter-Process Communication (IPC)** is a mechanism that allows processes to **exchange data and communicate with each other**.

Processes normally have separate address spaces, so IPC provides controlled methods for communication.

### Main IPC Methods

* Shared Memory
* Message Passing
* Pipes
* Named Pipes
* Sockets
* Remote Procedure Call (RPC)

```text id="m8g2r7"
Process P1  ────────>  IPC  ────────>  Process P2
                       |
                  Data Exchange
```

### Why IPC is Needed

* Exchange information between processes.
* Coordinate process execution.
* Share data.
* Synchronize processes.
* Improve modularity and performance.

**Point to Remember:**
**IPC = Communication + Data Exchange + Synchronization between processes**

---

## 2. Message Passing ⭐⭐⭐

**Message passing** is an IPC mechanism in which processes communicate by **sending and receiving messages**.

The two basic operations are:

```text id="q4e3st"
send(message)      // Send a message
receive(message)   // Receive a message
```

```text id="k5bqcz"
Process P1
    |
    | send(message)
    ↓
 Message System
    |
    | receive(message)
    ↓
Process P2
```

### Advantages

* Processes do not need to share the same memory.
* Suitable for distributed systems.
* Provides communication and synchronization.
* Easier to protect process address spaces.

### Disadvantages

* Message passing can have communication overhead.
* Data may need to be copied between processes.
* Generally slower than direct access through shared memory.

**Point to Remember:**
**Message Passing = `send()` + `receive()`**

---

## 3. Direct Communication

In **direct communication**, the sender and receiver explicitly identify each other.

```text id="0htr19"
send(P2, message)
receive(P1, message)
```

Here:

* `P1` directly sends a message to `P2`.
* `P2` directly receives a message from `P1`.

### Characteristics

* Sender must know the receiver.
* Receiver must know the sender.
* Communication is directly between processes.

```text id="q1p8ih"
P1 ───────────────> P2
       Message
```

**Point to Remember:**
**Direct communication = Process names the other process explicitly.**

---

## 4. Indirect Communication

In **indirect communication**, processes communicate through a **mailbox or port**.

```text id="g0f1kx"
P1 ──> Mailbox ──> P2
```

Example:

```text id="d9zq1j"
send(A, message)
receive(A, message)
```

Here `A` represents a mailbox.

### Characteristics

* Processes do not need to directly identify each other.
* Messages are stored in a mailbox.
* Multiple processes may communicate through the same mailbox, depending on the IPC design.

**Point to Remember:**
**Indirect communication = Communication through a mailbox/port.**

---

## 5. Unidirectional Communication

**Unidirectional communication** allows data to flow in **only one direction**.

```text id="g9j8y5"
Process P1 ─────────> Process P2
              Data
```

* P1 sends data.
* P2 receives data.
* Communication occurs in one direction.

**Example:** A pipe configured for one-way communication.

**Point to Remember:**
**Unidirectional = One-way communication**

---

## 6. Bidirectional Communication

**Bidirectional communication** allows data to flow in **both directions** between processes.

```text id="9jjwna"
Process P1 <─────────> Process P2
            Data
```

* P1 can send data to P2.
* P2 can send data to P1.

**Point to Remember:**
**Bidirectional = Two-way communication**

---

## 7. Pipes ⭐⭐⭐

A **pipe** is an IPC mechanism that provides a communication channel through which one process can send data to another process.

```text id="jcbv1x"
Process P1
    |
    | Write
    ↓
┌─────────┐
│  Pipe   │
└─────────┘
    |
    | Read
    ↓
Process P2
```

### Basic Operations

```text
write()      // Write data into pipe
read()       // Read data from pipe
```

### Characteristics

* Provides communication between processes.
* Commonly used between related processes.
* A pipe usually has a kernel-managed buffer.
* Traditional anonymous pipes are commonly used for communication between a parent and child process.
* Pipes can be used as a stream of bytes.

### Example

```text id="n1jv5n"
Parent Process
      |
    write()
      ↓
     Pipe
      |
     read()
      ↓
Child Process
```

**Point to Remember:**
**Pipe = Communication channel used to transfer data between processes.**

---

## 8. Named Pipes ⭐⭐

A **named pipe (FIFO)** is a pipe that has a name in the file system or IPC namespace and can be used by processes that are not necessarily related.

```text id="j9j91m"
Process P1
     |
     | write
     ↓
┌──────────────┐
│ Named Pipe   │
│    (FIFO)    │
└──────────────┘
     ↑
     | read
     |
Process P2
```

### Characteristics

* Has a specific name.
* Can be accessed by unrelated processes.
* Exists independently of the processes using it, subject to OS semantics.
* Supports communication through a kernel-managed pipe buffer.

### Difference from Anonymous Pipe

| Feature   | Anonymous Pipe                                | Named Pipe                                  |
| --------- | --------------------------------------------- | ------------------------------------------- |
| Name      | No persistent name                            | Has a name                                  |
| Processes | Commonly related processes                    | Can communicate between unrelated processes |
| Access    | Usually through inherited descriptors/handles | Can be opened by name                       |
| Use       | Simple process communication                  | Communication between independent processes |

**Point to Remember:**
**Named Pipe = FIFO with a name, allowing unrelated processes to communicate.**

---

## 9. Client-Server Communication

**Client-server communication** is a communication model in which a **client requests a service** and a **server provides the service**.

```text id="5y8xkt"
Client
  |
  | Request
  ↓
Server
  |
  | Response
  ↓
Client
```

### Working

1. Client sends a request.
2. Server receives the request.
3. Server processes the request.
4. Server sends a response.
5. Client receives the response.

### IPC Mechanisms Used

* Sockets
* Pipes
* Message passing
* RPC

**Example:**

```text id="a8j9fl"
Web Browser → Request → Web Server
Web Browser ← Response ← Web Server
```

**Point to Remember:**
**Client = Requests service**
**Server = Provides service**

---

## 10. Remote Procedure Call (RPC) ⭐⭐⭐

**Remote Procedure Call (RPC)** is a communication mechanism that allows a program to call a procedure/function located in another process or machine as if it were a local function.

```text id="v9bqpy"
Client
   |
   | RPC Request
   ↓
Server
   |
   | Execute Procedure
   ↓
Server Result
   |
   | RPC Response
   ↓
Client
```

### Main Components

* **Client** — Makes the procedure call.
* **Client Stub** — Packages/marshals the request.
* **RPC Runtime** — Handles communication.
* **Server Stub** — Unpacks/unmarshals the request.
* **Server** — Executes the requested procedure.

**Point to Remember:**
**RPC = Calling a remote procedure as if it were a local procedure.**

---

## 11. Execution of RPC ⭐⭐⭐

RPC execution involves several steps.

```text id="f4x1de"
Client
   |
   | 1. Call procedure
   ↓
Client Stub
   |
   | 2. Marshal arguments
   ↓
RPC Runtime
   |
   | 3. Send request
   ↓
Network / IPC
   |
   ↓
Server Stub
   |
   | 4. Unmarshal arguments
   ↓
Server Procedure
   |
   | 5. Execute
   ↓
Server Stub
   |
   | 6. Marshal result
   ↓
RPC Runtime
   |
   | 7. Send response
   ↓
Client Stub
   |
   | 8. Unmarshal result
   ↓
Client
```

### Step-by-Step

1. **Client calls the remote procedure.**
2. **Client stub** receives the call.
3. The stub **marshals** arguments into a message format.
4. RPC runtime sends the request to the server.
5. **Server stub** receives and **unmarshals** the request.
6. Server executes the required procedure.
7. Server stub packages the result.
8. Result is sent back to the client.
9. Client stub receives and unmarshals the result.
10. Client receives the returned result.

### Important Terms

**Marshalling:** Converting procedure arguments/data into a format suitable for transmission.

**Unmarshalling:** Converting received data back into usable arguments/data.

**Stub:** A piece of code that hides the communication details from the application.

**Point to Remember:**

```text id="2x6u8b"
Client
  ↓
Client Stub
  ↓
RPC Runtime
  ↓
Server Stub
  ↓
Server Procedure
  ↓
Result returns through the same path
```

---

## Quick Revision

| Topic                  | Key Point                                     |
| ---------------------- | --------------------------------------------- |
| IPC                    | Communication between processes               |
| Message Passing        | `send()` and `receive()`                      |
| Direct Communication   | Processes directly name each other            |
| Indirect Communication | Communication through mailbox/port            |
| Unidirectional         | One-way communication                         |
| Bidirectional          | Two-way communication                         |
| Pipe                   | IPC communication channel                     |
| Named Pipe             | Named FIFO usable by unrelated processes      |
| Client                 | Requests a service                            |
| Server                 | Provides a service                            |
| RPC                    | Calls a procedure on a remote process/machine |
| Marshalling            | Packages data for transmission                |
| Unmarshalling          | Reconstructs received data                    |
| Stub                   | Handles RPC communication details             |
