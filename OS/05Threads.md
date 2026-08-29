# 8. Threads

## 1. Single-Threaded Process

A **single-threaded process** has only one thread of execution.

* The process performs **one task at a time**.
* It has a single **program counter (PC)**.
* If the thread blocks, the **entire process waits**.
* Simple to create and manage.
* Example: A simple program that performs one sequence of operations.

```text
Process
   |
   └── Thread
        ├── Program Counter
        ├── Registers
        └── Stack
```

**Point to Remember:** One process → One thread → One flow of execution.

---

## 2. Multithreaded Process

A **multithreaded process** contains two or more threads executing within the same process.

* Threads share the process's:

  * Code
  * Data
  * Files
  * Other process resources
* Each thread has its own:

  * Program Counter
  * Registers
  * Stack
* Threads can execute different tasks concurrently.
* If one thread blocks, another thread can continue execution.

```text
Process
   |
   ├── Thread 1 → Task 1
   ├── Thread 2 → Task 2
   └── Thread 3 → Task 3
```

**Point to Remember:** One process → Multiple threads → Multiple flows of execution.

---

## 3. Thread vs Process

| Feature           | Process                                | Thread                                          |
| ----------------- | -------------------------------------- | ----------------------------------------------- |
| Definition        | Program in execution                   | Smallest unit of CPU execution within a process |
| Address Space     | Has its own address space              | Shares address space with other threads         |
| Resources         | Owns resources                         | Shares process resources                        |
| Creation          | Relatively expensive                   | Relatively less expensive                       |
| Context Switching | More costly                            | Less costly                                     |
| Communication     | IPC is required                        | Can communicate through shared memory           |
| Failure           | Failure usually affects that process   | Failure can affect the entire process           |
| Execution         | Contains one or more threads           | Executes within a process                       |
| Stack             | Each process has its own address space | Each thread has its own stack                   |

**Point to Remember:** A process is a **resource container**, while a thread is a **unit of execution**.

---

## 4. Merits of Threads

### 1. Responsiveness

If one thread is blocked, other threads can continue execution.

### 2. Resource Sharing

Threads share code, data, and resources of the same process.

### 3. Economy

Creating and switching between threads is generally cheaper than processes.

### 4. Scalability

Multiple threads can run in parallel on multiple CPU cores.

### 5. Better CPU Utilization

While one thread waits for I/O, another thread can use the CPU.

**Points to Remember:**

* Threads improve **responsiveness**.
* Threads provide **resource sharing**.
* Threads are generally **less expensive than processes**.

---

## 5. Demerits of Threads

* Threads share memory, so incorrect synchronization can cause **race conditions**.
* A failure in one thread can potentially affect the **entire process**.
* Multithreaded programs are harder to design and debug.
* Synchronization introduces additional complexity.
* Too many threads can increase **overhead** and reduce performance.

**Point to Remember:** Threads improve performance but increase **programming complexity**.

---

## 6. Thread Scheduling

**Thread scheduling** is the process of selecting which ready thread should execute on the CPU.

* The scheduler selects a thread from the **ready queue**.
* In a multithreaded system, multiple threads may compete for CPU time.
* Scheduling can be performed at:

  * User level
  * Kernel level
* Common scheduling approaches include:

  * FCFS
  * Round Robin
  * Priority Scheduling

```text
Ready Threads
     |
     v
 Thread Scheduler
     |
     v
   CPU
```

**Point to Remember:** Thread scheduling decides **which thread gets the CPU next**.

---

## 7. Thread States

A thread can move through different states during its execution.

### 1. New

The thread is being created.

### 2. Ready

The thread is ready to execute but waiting for the CPU.

### 3. Running

The thread is currently executing on the CPU.

### 4. Waiting/Blocked

The thread is waiting for an event, resource, or I/O operation.

### 5. Terminated

The thread has completed execution or has been cancelled.

```text
             dispatch
   Ready ----------------> Running
    ^                         |
    |                         |
    |                     wait/block
    |                         |
    |                         v
    +--------------------- Waiting
                              |
                              | event occurs
                              v
                            Ready

Running → Terminated
```

**Point to Remember:**
`New → Ready → Running → Waiting → Ready → Running → Terminated`

---

## 8. Thread Management

**Thread management** involves creating, scheduling, synchronizing, and terminating threads.

### Thread Creation

Creates a new thread to perform a task.

### Thread Scheduling

Selects a thread for CPU execution.

### Thread Synchronization

Coordinates threads when they access shared resources.

### Thread Cancellation

Stops a thread before it completes normally.

### Thread Termination

Ends a thread after it completes its execution.

### Thread Joining

Allows one thread to wait for another thread to finish.

**Point to Remember:** Thread management controls the **lifecycle and execution of threads**.

---

## 9. Thread Cancellation

**Thread cancellation** means terminating a thread before it finishes its normal execution.

There are two main types:

### 1. Asynchronous Cancellation

The target thread is terminated immediately by another thread.

```text
Thread A
   |
   | cancels
   v
Thread B → Terminated
```

**Advantage:** Fast.

**Disadvantage:** The thread may be terminated while holding resources or locks, causing problems.

### 2. Deferred Cancellation

The target thread periodically checks whether it has been requested to terminate.

```text
Thread B
   |
   ├── Execute
   ├── Check cancellation request
   ├── Execute
   └── Terminate safely
```

**Advantage:** Safer because the thread can terminate at an appropriate point.

**Point to Remember:** Deferred cancellation is generally safer than asynchronous cancellation.

---

## 10. Thread Termination

**Thread termination** occurs when a thread finishes execution or is stopped.

A thread can terminate when:

* It completes its task.
* It explicitly calls an exit/termination operation.
* Another thread cancels it.
* An unrecoverable error occurs.

After termination:

* The thread no longer executes.
* Its execution resources can be released.
* Other threads may wait for its completion using a **join** operation.

```text
Running
   |
   v
Completed
   |
   v
Terminated
```

**Point to Remember:** Termination means the thread's execution has ended.

---

## 11. Thread Issues

Important issues related to multithreading include:

### 1. fork() and exec()

In a multithreaded process, `fork()` and `exec()` require careful handling.

* `fork()` creates a new process.
* `exec()` replaces the current process image with a new program.
* The behavior of `fork()` in a multithreaded process depends on the system/API.

### 2. Signal Handling

Signals notify a process or thread that an event has occurred.

In a multithreaded process, the system must determine **which thread should receive or handle the signal**.

### 3. Thread Cancellation

A thread may need to be stopped before completing its task.

* Asynchronous cancellation
* Deferred cancellation

### 4. Thread Pools

Creating a new thread for every task can create overhead. A thread pool keeps a collection of reusable threads.

### 5. Thread-Specific Data

Sometimes each thread needs its own copy of certain data instead of sharing it with other threads.

**Point to Remember:** Multithreading introduces issues related to **fork/exec, signals, cancellation, thread pools, and thread-specific data**.

---

## 12. Thread Safety

**Thread safety** means that code or a data structure can be safely used by multiple threads concurrently without producing incorrect results.

### Example of Unsafe Code

Suppose two threads execute:

```text
counter = counter + 1
```

If both threads access `counter` at the same time, a **race condition** may occur.

### Making Code Thread-Safe

Synchronization mechanisms can be used, such as:

* Mutex
* Lock
* Semaphore
* Monitor
* Atomic operations

```text
Multiple Threads
       |
       v
 Synchronization
       |
       v
 Shared Resource
```

**Point to Remember:** Thread safety prevents **incorrect results caused by concurrent access**.

---

## 13. Thread-Specific Data

**Thread-specific data (TSD)** is data that belongs to an individual thread rather than being shared by all threads.

Each thread gets its own copy of the data.

```text
Process
   |
   ├── Thread 1 → TSD₁
   ├── Thread 2 → TSD₂
   └── Thread 3 → TSD₃
```

### Why Thread-Specific Data is Used

* Prevents unwanted sharing of data.
* Allows each thread to maintain its own state.
* Reduces synchronization requirements for that data.
* Useful when the same variable is needed independently by multiple threads.

**Point to Remember:** TSD = **private data for each thread**.

---

## 14. Thread Pools

A **thread pool** is a collection of pre-created worker threads that are available to execute tasks.

Instead of creating a new thread for every task:

```text
Tasks → Thread Pool → Available Threads → Execute Tasks
```

### Working

1. A set of threads is created in advance.
2. Tasks are placed into a task queue.
3. An available thread takes a task.
4. The thread executes the task.
5. After completion, the thread returns to the pool.
6. The thread can execute another task.

### Advantages

* Reduces thread creation overhead.
* Improves response time.
* Controls the number of concurrent threads.
* Allows efficient reuse of threads.

**Point to Remember:** Thread Pool = **Reusable threads + Task queue**.

---

## 15. ULT State vs Process State

**ULT (User-Level Thread)** states are managed by the user-level thread library, while **process states** are managed by the operating system.

| Feature        | ULT State                                                   | Process State                                        |
| -------------- | ----------------------------------------------------------- | ---------------------------------------------------- |
| Managed By     | User-level thread library                                   | Operating System                                     |
| Visibility     | Usually invisible to kernel                                 | Visible to kernel                                    |
| States         | Ready, Running, Blocked, etc.                               | New, Ready, Running, Waiting, Terminated             |
| Scheduling     | User-level scheduler                                        | OS scheduler                                         |
| Blocking       | One ULT blocking in many-to-one can block the whole process | Process enters waiting state when it cannot continue |
| Context Switch | Usually faster                                              | Usually more expensive                               |
| Relationship   | Multiple ULTs exist inside one process                      | Process contains one or more threads                 |

### Important Relationship

```text
Process
   |
   ├── ULT 1 → Ready
   ├── ULT 2 → Running
   └── ULT 3 → Blocked
```

The **process has its own state**, while individual ULTs can have **different states**.

**Point to Remember:**
**Process state = OS-level view**
**ULT state = User-level thread view**
