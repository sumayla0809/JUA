# 🔴 12. Semaphores — MUST DO ⭐⭐⭐

## 1. Semaphore

A **semaphore** is a synchronization tool used to control access to **shared resources** by multiple processes or threads.

A semaphore is an integer variable that can be accessed only through two atomic operations:

* `wait()` — decreases the semaphore value.
* `signal()` — increases the semaphore value.

```text
wait(S):
    S = S - 1

signal(S):
    S = S + 1
```

### Types of Semaphores

#### 1. Binary Semaphore

A binary semaphore can have only two values:

```text
0 → Resource unavailable
1 → Resource available
```

It is commonly used for **mutual exclusion**.

#### 2. Counting Semaphore

A counting semaphore can have a value greater than 1.

It is used when multiple instances of a resource are available.

```text
S = 3

3 → Three resources available
2 → Two resources available
1 → One resource available
0 → No resource available
```

**Point to Remember:**
**Semaphore = Integer synchronization variable + `wait()` + `signal()`**

---

## 2. Semaphore Usage

Semaphores are mainly used for **process/thread synchronization** and **resource management**.

### 1. Mutual Exclusion

A binary semaphore can ensure that only one thread enters a critical section at a time.

```text
wait(mutex)

    // Critical Section

signal(mutex)
```

### 2. Resource Management

A counting semaphore can control access to a limited number of identical resources.

Example:

```text
S = 3

Thread 1 → Resource
Thread 2 → Resource
Thread 3 → Resource
Thread 4 → Waits
```

### 3. Process Synchronization

Semaphores can force one process/thread to wait until another process/thread completes a particular operation.

```text
Process P1
    |
    | signal(S)
    v
Process P2
    |
    | wait(S)
    v
 Continue
```

### 4. Producer-Consumer Problem

Semaphores can synchronize producers and consumers when accessing a shared buffer.

Common semaphores:

```text
mutex = 1     // Mutual exclusion
empty = N     // Empty buffer slots
full = 0      // Filled buffer slots
```

**Point to Remember:** Semaphores are used for **mutual exclusion, synchronization, and resource management**.

---

## 3. Semaphore Implementation

Semaphore operations must be **atomic**, meaning `wait()` and `signal()` must execute completely without interruption.

### Basic Implementation

```text
wait(S)
{
    while (S <= 0)
        ;              // Busy waiting

    S--;
}

signal(S)
{
    S++;
}
```

### Working of `wait()`

```text
wait(S)
{
    while (S <= 0)
        ;              // Keep waiting

    S--;               // Acquire resource
}
```

* If `S > 0`, the process/thread can continue.
* `S` is decremented after acquiring the resource.
* If `S == 0`, the process/thread must wait.

### Working of `signal()`

```text
signal(S)
{
    S++;               // Release resource
}
```

* Increases the semaphore value.
* Indicates that a resource has been released.
* Allows a waiting process/thread to proceed.

### Example

Suppose:

```text
S = 1
```

Thread A executes:

```text
wait(S)
```

Now:

```text
S = 0
```

Thread A enters the critical section.

When Thread A finishes:

```text
signal(S)
```

Now:

```text
S = 1
```

Another waiting thread can enter.

**Point to Remember:** `wait()` **acquires/decreases**, while `signal()` **releases/increases**.

---

## 4. Semaphore Without Busy Waiting

The basic semaphore implementation may use **busy waiting**, where a process continuously checks the semaphore.

```text
while (S <= 0)
    ;       // Busy waiting
```

This wastes CPU time.

To avoid this, the process is **blocked and placed into a waiting queue** when the semaphore is unavailable.

### Structure

```text
typedef struct {
    int value;
    queue waiting_queue;
} semaphore;
```

### `wait()` Without Busy Waiting

```text
wait(S)
{
    S.value--;

    if (S.value < 0) {
        add this process to S.waiting_queue;
        block();       // Process goes to waiting state
    }
}
```

### `signal()` Without Busy Waiting

```text
signal(S)
{
    S.value++;

    if (S.value <= 0) {
        remove a process P from S.waiting_queue;
        wakeup(P);     // Wake the waiting process
    }
}
```

### Working

```text
Semaphore unavailable
        |
        v
    Process waits
        |
        v
 Waiting Queue
        |
        | Resource becomes available
        v
     Wakeup
        |
        v
 Process continues
```

### Advantages

* Avoids wasting CPU cycles.
* Waiting processes are placed in a queue.
* CPU can execute other processes/threads.
* More efficient than busy waiting when the waiting time is significant.

**Point to Remember:**

> **Busy Waiting → CPU keeps checking.**
> **Without Busy Waiting → Process sleeps/blocks and is awakened when the resource becomes available.**

---

## Quick Revision

| Concept              | Key Point                                                |
| -------------------- | -------------------------------------------------------- |
| Semaphore            | Synchronization variable                                 |
| Binary Semaphore     | Value generally `0` or `1`                               |
| Counting Semaphore   | Controls multiple resource instances                     |
| `wait()`             | Decreases semaphore / acquires resource                  |
| `signal()`           | Increases semaphore / releases resource                  |
| Busy Waiting         | Continuously checks condition                            |
| Without Busy Waiting | Process blocks and waits in a queue                      |
| Main Purpose         | Synchronization + Mutual Exclusion + Resource Management |
