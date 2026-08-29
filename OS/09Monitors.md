# 🔴 14. Monitors — VERY IMPORTANT

## 1. Monitor ⭐⭐⭐

A **monitor** is a high-level synchronization mechanism used to control access to **shared data** by multiple processes or threads.

* A monitor contains:

  * Shared variables
  * Procedures/functions that operate on the shared variables
  * Condition variables
* Only **one process/thread can be active inside a monitor at a time**.
* Mutual exclusion is automatically provided by the monitor.
* Threads that cannot continue can wait using **condition variables**.

```text
                    Monitor
        ┌──────────────────────────┐
        │      Shared Variables    │
        │            ↓             │
        │      Procedures         │
        │            ↓             │
        │    Condition Variables  │
        └──────────────────────────┘
                  ↑
                  │
          One thread at a time
```

### Basic Structure

```text
monitor MonitorName
{
    shared variables;

    procedure function1()
    {
        // Access shared data
    }

    procedure function2()
    {
        // Access shared data
    }

    condition x;
}
```

**Point to Remember:**
**Monitor = Shared data + Procedures + Automatic mutual exclusion + Condition variables**

---

## 2. Shared Variable Declaration

Shared variables are variables that can be accessed by multiple processes or threads.

Inside a monitor, shared variables are declared as part of the monitor's data.

```text
monitor SharedResource
{
    int count;          // Shared variable
    int buffer[N];      // Shared buffer

    procedure add()
    {
        // Modify shared variables
    }

    procedure remove()
    {
        // Access shared variables
    }
}
```

### Important Points

* Shared variables are **protected by the monitor**.
* They cannot normally be accessed directly from outside the monitor.
* Monitor procedures provide controlled access.
* Mutual exclusion prevents multiple threads from modifying shared data simultaneously.

**Point to Remember:** Shared variables inside a monitor are accessed through **monitor procedures**.

---

## 3. Symmetric View of Monitor

The **symmetric view** treats processes/threads that enter a monitor in the same way.

* No particular process/thread has permanent priority.
* Any waiting process/thread can enter when the monitor becomes available.
* The monitor provides **mutual exclusion** for all processes equally.
* Scheduling among waiting processes depends on the monitor's implementation.

```text
Thread 1 ──┐
Thread 2 ──┼──> Monitor ──> Shared Data
Thread 3 ──┤
Thread 4 ──┘

        One thread enters
        at a time
```

**Point to Remember:**
**Symmetric view = All threads are treated equally when accessing the monitor.**

---

## 4. Key Points of Monitor

* Monitor is a **high-level synchronization mechanism**.
* It provides **automatic mutual exclusion**.
* Only **one thread/process** can execute inside the monitor at a time.
* Shared variables are protected inside the monitor.
* Access is provided through monitor procedures.
* **Condition variables** allow threads to wait for specific conditions.
* `wait()` can temporarily release the monitor so another thread can enter.
* `signal()` can wake a waiting thread.
* Monitors make synchronization easier than directly using semaphores.

**Point to Remember:**
The main purpose of a monitor is to provide **safe and organized access to shared resources**.

---

## 5. Condition Variables ⭐⭐⭐

A **condition variable** is a synchronization mechanism used inside a monitor that allows a thread to **wait until a particular condition becomes true**.

A condition variable is associated with a waiting queue.

```text
condition x;

Thread
   |
   | wait(x)
   v
Waiting Queue
   |
   | signal(x)
   v
Thread becomes ready
```

### Why Condition Variables Are Needed

A thread may enter a monitor but find that it cannot continue.

For example, in a producer-consumer problem:

```text
if buffer is empty
    consumer must wait
```

The consumer can wait on a condition variable:

```text
wait(notEmpty);
```

When a producer adds an item, it can signal:

```text
signal(notEmpty);
```

**Point to Remember:**
Condition variable = **Allows a thread to wait for a specific condition.**

---

## 6. Condition Variable Operations ⭐⭐⭐

The two main operations on a condition variable are:

### 1. `wait()`

`wait()` causes the calling thread to wait until another thread signals the condition.

```text
wait(condition);
```

### Working

```text
Thread enters monitor
        ↓
Condition is false
        ↓
wait(condition)
        ↓
Thread blocks
        ↓
Thread releases monitor
        ↓
Another thread enters monitor
```

**Important:** `wait()` releases the monitor's mutual-exclusion lock while the thread is waiting, allowing another thread to enter.

---

### 2. `signal()`

`signal()` wakes one thread waiting on the specified condition variable.

```text
signal(condition);
```

### Working

```text
Thread changes shared data
        ↓
Condition becomes true
        ↓
signal(condition)
        ↓
Waiting thread is awakened
```

If no thread is waiting on the condition variable, the signal has no waiting thread to wake.

---

### Example

```text
monitor Buffer
{
    condition notEmpty;
    condition notFull;

    procedure consume()
    {
        if (buffer is empty)
            wait(notEmpty);

        // Remove item

        signal(notFull);
    }

    procedure produce()
    {
        if (buffer is full)
            wait(notFull);

        // Add item

        signal(notEmpty);
    }
}
```

### Meaning

```text
notEmpty → Consumer waits if buffer is empty
notFull  → Producer waits if buffer is full
```

**Point to Remember:**

> `wait()` → **Go to waiting state**
> `signal()` → **Wake a waiting thread**

---

## 7. Condition Variable Choice / Selection ⭐⭐

When multiple condition variables exist in a monitor, the programmer must choose the **appropriate condition variable** according to the condition on which the thread needs to wait.

### Example

For a bounded buffer:

```text
condition notEmpty;
condition notFull;
```

* Consumer waits on `notEmpty` when the buffer is empty.
* Producer waits on `notFull` when the buffer is full.

```text
Buffer Empty
     ↓
Consumer
     ↓
wait(notEmpty)

Buffer Full
     ↓
Producer
     ↓
wait(notFull)
```

### Selection Rule

Choose the condition variable that represents the **condition required for the thread to continue**.

```text
Need data?
   → wait(notEmpty)

Need free space?
   → wait(notFull)
```

**Point to Remember:**
**Condition variable selection depends on what condition the waiting thread needs to become true.**

---

## Quick Revision

| Concept            | Key Point                                             |
| ------------------ | ----------------------------------------------------- |
| Monitor            | High-level synchronization mechanism                  |
| Mutual Exclusion   | Only one thread executes inside monitor at a time     |
| Shared Variables   | Protected inside monitor                              |
| Condition Variable | Used to wait for a condition                          |
| `wait()`           | Blocks the calling thread and releases monitor access |
| `signal()`         | Wakes a waiting thread                                |
| Symmetric View     | Threads are treated equally                           |
| `notEmpty`         | Used when consumer waits for data                     |
| `notFull`          | Used when producer waits for free space               |

### One-Line Revision

```text
Monitor → Shared Data + Procedures + Mutual Exclusion
Condition Variable → Wait for a condition
wait() → Block and release monitor
signal() → Wake a waiting thread
Condition Selection → Choose according to the required condition
```
