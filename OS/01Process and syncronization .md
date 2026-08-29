# 1. Process & Synchronization

## 1. Process Concept

A **process** is a program that is currently in execution.

A program is a passive entity stored on disk, while a process is an **active entity** being executed by the CPU.

### Components of a Process

A process generally contains:

* **Program Code (Text Section)** — Instructions of the program.
* **Data Section** — Global and static variables.
* **Heap** — Dynamically allocated memory.
* **Stack** — Function calls, parameters, and local variables.
* **Program Counter (PC)** — Address of the next instruction.
* **CPU Registers** — Store temporary execution information.

```text
Process
   |
   ├── Code / Text
   ├── Data
   ├── Heap
   ├── Stack
   ├── Program Counter
   └── CPU Registers
```

**Point to Remember:**
**Program = Passive**
**Process = Program in execution**

---

## 2. Process State ⭐

A **process state** represents the current condition of a process during its execution.

### Main Process States

#### 1. New

The process is being created.

#### 2. Ready

The process is waiting for the CPU.

#### 3. Running

The process is currently executing on the CPU.

#### 4. Waiting / Blocked

The process is waiting for an event, I/O operation, or resource.

#### 5. Terminated

The process has finished execution.

```text
              dispatch
     Ready ----------------> Running
       ↑                       |
       |                       |
       |                    I/O wait
       |                       |
       |                       ↓
       +------------------- Waiting
                              |
                              | I/O completed
                              ↓
                            Ready

Running ───────────────> Terminated
```

### Important Transitions

* **New → Ready:** Process is admitted.
* **Ready → Running:** CPU is assigned.
* **Running → Ready:** CPU is taken away/preemption.
* **Running → Waiting:** Process requests I/O or waits for an event.
* **Waiting → Ready:** Required event/I/O completes.
* **Running → Terminated:** Process completes execution.

**Point to Remember:**
`New → Ready → Running → Waiting → Ready → Running → Terminated`

---

## 3. Process Context

**Process context** is the complete information about the current state of a process that is required to resume its execution correctly.

It includes:

* Program Counter (PC)
* CPU registers
* Stack Pointer
* Process state
* Memory-management information
* Scheduling information
* Other CPU execution information

During a **context switch**, the OS saves the context of the current process and loads the context of another process.

```text
Process P1 Running
       |
       | Save P1 Context
       ↓
   Context Switch
       |
       | Load P2 Context
       ↓
Process P2 Running
```

**Point to Remember:**
**Context switch = Save old process context + Load new process context**

---

## 4. Process Control Block (PCB) ⭐

A **Process Control Block (PCB)** is a data structure maintained by the operating system to store information about a process.

### Contents of PCB

| Information                   | Description                                   |
| ----------------------------- | --------------------------------------------- |
| Process ID (PID)              | Unique identification number                  |
| Process State                 | Current state of process                      |
| Program Counter               | Address of next instruction                   |
| CPU Registers                 | Saved register values                         |
| CPU Scheduling Information    | Priority, queue information, etc.             |
| Memory Management Information | Information about memory allocated to process |
| Accounting Information        | CPU usage and other resource information      |
| I/O Status Information        | I/O devices and files allocated to process    |

```text
          PCB
┌─────────────────────────┐
│ Process ID              │
│ Process State           │
│ Program Counter         │
│ CPU Registers           │
│ Scheduling Information  │
│ Memory Information      │
│ Accounting Information  │
│ I/O Information         │
└─────────────────────────┘
```

**Point to Remember:**
**PCB = Complete OS information about a process**

---

## 5. Process Synchronization ⭐⭐⭐

**Process synchronization** is the coordination of multiple processes or threads when they access **shared data or resources**.

It is required to ensure that concurrent processes produce **correct and consistent results**.

### Why Synchronization is Needed

When multiple processes access shared data simultaneously:

* Data inconsistency may occur.
* Race conditions may occur.
* Incorrect results may be produced.

```text
Process P1 ──┐
             ├──> Shared Resource
Process P2 ──┘
                  ↓
            Synchronization
```

### Common Synchronization Mechanisms

* Mutex
* Semaphore
* Monitor
* Locks
* Atomic instructions such as Compare-and-Swap

**Point to Remember:**
**Synchronization = Coordinating processes/threads to safely access shared resources.**

---

## 6. Race Condition ⭐⭐⭐

A **race condition** occurs when multiple processes or threads access and modify shared data concurrently, and the final result depends on the **order of execution**.

### Example

Suppose:

```text
counter = 5
```

Two threads execute:

```text
counter = counter + 1
```

Both may read `5` before either writes the result.

```text
Thread 1 → Read 5 → Calculate 6
Thread 2 → Read 5 → Calculate 6

Thread 1 → Write 6
Thread 2 → Write 6

Final value = 6
Expected value = 7
```

This produces an incorrect result.

### Solution

Race conditions can be prevented using:

* Mutual exclusion
* Locks
* Semaphores
* Monitors
* Atomic operations

**Point to Remember:**
**Race condition = Result depends on the timing/order of concurrent execution.**

---

## 7. Critical Section ⭐⭐⭐

A **critical section** is the part of a program where a process/thread accesses or modifies **shared resources or shared data**.

Only one process/thread should execute its critical section at a time when accessing the same shared resource.

### Structure

```text
do {
    // Entry Section

    // Critical Section
    // Access shared resource

    // Exit Section

    // Remainder Section
} while (true);
```

### Four Requirements of a Critical-Section Solution

#### 1. Mutual Exclusion

Only one process can execute its critical section at a time.

#### 2. Progress

If no process is in its critical section, the selection of the next process should not be postponed indefinitely.

#### 3. Bounded Waiting

A process should have a limit on the number of times other processes can enter their critical sections before it gets its turn.

#### 4. No Unnecessary Delay

A process outside its critical section should not prevent another process from entering its critical section.

**Point to Remember:**
**Critical Section = Code section that accesses shared data/resource.**

---

## 8. Mutual Exclusion ⭐⭐⭐

**Mutual exclusion** is a property that ensures **only one process/thread can enter a critical section at a time**.

```text
Process P1 ──> Critical Section
                   ↑
                   │
              Only one
                   │
Process P2 ──> Waiting
```

If P1 is executing its critical section, P2 must wait until P1 exits.

### Common Mechanisms

* Mutex locks
* Semaphores
* Monitors
* Atomic instructions

**Point to Remember:**
**Mutual Exclusion = One process/thread at a time in the critical section.**

---

## 9. Bounded Waiting ⭐⭐

**Bounded waiting** means there must be a limit on the number of times other processes can enter their critical sections after a process has requested entry and before that process is allowed to enter.

### Example

```text
P1 requests Critical Section
        ↓
P2 enters
        ↓
P3 enters
        ↓
P1 gets its turn
```

P1 should not wait forever while P2 and P3 repeatedly enter.

### Purpose

* Prevents indefinite waiting.
* Helps prevent starvation.
* Provides fairness in access to shared resources.

**Point to Remember:**
**Bounded Waiting = A process cannot be postponed forever.**

---

## 10. Shared Memory

**Shared memory** is a memory region that can be accessed by multiple processes.

It provides a mechanism for processes to communicate by reading and writing shared data.

```text
Process P1 ──┐
             │
             ↓
        Shared Memory
             ↑
             │
Process P2 ──┘
```

### Advantages

* Fast communication.
* Processes can directly access shared data.
* Less overhead than some message-passing mechanisms.

### Problem

Multiple processes accessing shared memory simultaneously can cause:

* Race conditions
* Data inconsistency

Therefore, **synchronization is required**.

**Point to Remember:**
**Shared Memory = Common memory region accessible by multiple processes.**

---

## 11. Producer-Consumer Problem ⭐⭐⭐

The **Producer-Consumer Problem** is a classic synchronization problem where:

* **Producer** produces data/items.
* **Consumer** consumes data/items.
* Both use a **shared buffer**.

```text
Producer
    |
    | Produces items
    ↓
┌───────────────┐
│ Shared Buffer │
└───────────────┘
    |
    | Consumes items
    ↓
Consumer
```

### Problems

#### Buffer Full

Producer must wait because there is no free space.

#### Buffer Empty

Consumer must wait because there is no item to consume.

#### Concurrent Access

Producer and consumer must not modify the buffer simultaneously in an unsafe manner.

### Synchronization Requirements

* Producer must wait when the buffer is full.
* Consumer must wait when the buffer is empty.
* Only one process/thread should modify the shared buffer at a time.

### Semaphore Solution

Common semaphores:

```text
mutex = 1     // Controls access to buffer
empty = N     // Number of empty slots
full = 0      // Number of filled slots
```

#### Producer

```text
wait(empty);      // Wait for an empty slot
wait(mutex);      // Enter critical section

// Add item to buffer

signal(mutex);    // Leave critical section
signal(full);     // Increase filled slots
```

#### Consumer

```text
wait(full);       // Wait for an available item
wait(mutex);      // Enter critical section

// Remove item from buffer

signal(mutex);    // Leave critical section
signal(empty);    // Increase empty slots
```

**Point to Remember:**
**Producer → waits for empty space**
**Consumer → waits for available item**
**Mutex → protects the shared buffer**

---

## 12. Bounded Buffer Problem ⭐⭐⭐

The **Bounded Buffer Problem** is a specific form of the Producer-Consumer Problem where the shared buffer has a **fixed size**.

```text
        Producer
            |
            ↓
      ┌───────────┐
      │   Buffer  │
      │ [ ][ ][ ] │
      │ [ ][ ][ ] │
      └───────────┘
            |
            ↓
        Consumer
```

If the buffer size is `N`:

```text
empty = N     // Empty slots
full = 0      // Filled slots
mutex = 1     // Mutual exclusion
```

### Producer

```text
wait(empty);      // Check for empty slot
wait(mutex);      // Lock buffer

// Insert item

signal(mutex);    // Unlock buffer
signal(full);     // One more item available
```

### Consumer

```text
wait(full);       // Check for available item
wait(mutex);      // Lock buffer

// Remove item

signal(mutex);    // Unlock buffer
signal(empty);    // One more empty slot available
```

### Important Cases

```text
Buffer Full  → Producer waits
Buffer Empty → Consumer waits
Buffer Access → Mutual exclusion required
```

**Point to Remember:**
**Bounded Buffer = Fixed-size shared buffer + Producer + Consumer + Synchronization**

---

## 13. Compare-and-Swap ⭐⭐

**Compare-and-Swap (CAS)** is an atomic hardware instruction used for synchronization and implementing lock-free data structures.

It compares a memory location with an expected value. If they are equal, it replaces the value with a new value.

### Concept

```text
Compare:
Memory value == Expected value?

       Yes
        ↓
Replace with New value

       No
        ↓
Do not change value
```

### Pseudocode

```text
CompareAndSwap(address, expected, new)
{
    old = *address;

    if (old == expected)
        *address = new;

    return old;
}
```

* `address` → Memory location.
* `expected` → Value that is expected.
* `new` → New value to store.
* `return` → Previous value.

### Example: Lock

```text
lock = 0;             // 0 = unlocked
```

```text
while (CompareAndSwap(&lock, 0, 1) != 0)
{
    // Keep trying
}

// Critical Section

lock = 0;             // Unlock
```

**Point to Remember:**
**CAS performs comparison and update as one atomic operation.**

---

## 14. Parent-Child Process Ordering

**Parent-child process ordering** refers to controlling the execution order between a parent process and its child process.

A parent process can create a child process using `fork()`.

```text
          Parent Process
                |
              fork()
             /      \
            ↓        ↓
        Parent      Child
```

Without synchronization, the parent and child may execute in an unpredictable order.

### Parent Waits for Child

The parent can use `wait()` to wait for the child process to finish.

```text
Parent
   |
   | fork()
   ↓
Child
   |
   | executes
   ↓
Child terminates
   |
   ↓
Parent continues
```

Example:

```c
pid = fork();

if (pid == 0)
{
    // Child process
}
else
{
    wait(NULL);       // Parent waits for child
    // Parent continues after child terminates
}
```

**Point to Remember:**
`fork()` → Creates child process
`wait()` → Allows parent to wait for child completion
