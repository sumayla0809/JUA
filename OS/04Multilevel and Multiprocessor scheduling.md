# 🔴 6. Multilevel Scheduling

## 1. Multilevel Queue Scheduling ⭐⭐⭐

**Multilevel Queue Scheduling** divides the ready queue into **multiple separate queues** based on process characteristics.

Each queue can have its own scheduling algorithm.

```text
                    Ready Queue
                        |
        ┌───────────────┼───────────────┐
        ↓               ↓               ↓
   System Queue    Interactive      Batch Queue
        ↓               ↓               ↓
       FCFS          Round Robin       FCFS
```

### Common Queue Types

* System processes
* Interactive processes
* Interactive editing processes
* Batch processes

### Important Points

* A process is **permanently assigned** to one queue.
* Different queues can use different scheduling algorithms.
* Scheduling is required **between the queues** as well.

### Queue Scheduling

Two common methods are:

#### Fixed-Priority Scheduling

A higher-priority queue is always served before a lower-priority queue.

```text
Q1 → Highest Priority
Q2 → Medium Priority
Q3 → Lowest Priority

Q1 > Q2 > Q3
```

**Problem:** Lower-priority queues may suffer from starvation.

#### Time Slicing Between Queues

CPU time can be divided among different queues.

Example:

```text
Q1 → 80% CPU
Q2 → 20% CPU
```

**Point to Remember:**
**Multilevel Queue = Multiple ready queues + Different scheduling policies + Permanent queue assignment.**

---

## 2. Multilevel Queue Example ⭐⭐⭐

Suppose there are three queues:

```text
Q1 → System Processes
Q2 → Interactive Processes
Q3 → Batch Processes
```

Scheduling algorithms:

```text
Q1 → FCFS
Q2 → Round Robin
Q3 → FCFS
```

```text
                  Ready Queues
                       |
       ┌───────────────┼───────────────┐
       ↓               ↓               ↓
      Q1              Q2              Q3
   System         Interactive        Batch
      |               |               |
     FCFS             RR             FCFS
```

If **fixed priority** is used:

```text
Q1 > Q2 > Q3
```

The CPU first checks Q1. If Q1 has processes, they are selected before Q2 and Q3.

### Example

```text
Q1: P1, P2
Q2: P3, P4
Q3: P5, P6
```

With fixed priority:

```text
P1 → P2 → P3 → P4 → P5 → P6
```

If Q1 continuously receives new processes, Q2 and Q3 may wait indefinitely.

**Point to Remember:**
**Processes do not move between queues in a traditional Multilevel Queue system.**

---

## 3. Multilevel Feedback Queue (MLFQ) ⭐⭐⭐

**Multilevel Feedback Queue** is a scheduling algorithm with multiple queues where processes **can move between queues** based on their behavior and CPU usage.

```text
                    Q1
              Round Robin
                   ↓
             CPU-intensive
                   ↓
                    Q2
              Round Robin
                   ↓
                    Q3
                  FCFS
```

### Main Idea

* New processes generally enter a higher-priority queue.
* A process that uses too much CPU may be moved to a lower-priority queue.
* A process that waits for a long time may be promoted to a higher-priority queue.
* Different queues can use different scheduling algorithms.

### Example Structure

```text
Q1 → Highest Priority → RR, small quantum
Q2 → Medium Priority  → RR, larger quantum
Q3 → Lowest Priority  → FCFS
```

### Advantages

* Flexible.
* Gives fast response to interactive processes.
* CPU-intensive processes gradually move to lower-priority queues.
* Can reduce starvation using aging/promotion.

### Disadvantages

* More complex to implement.
* Choosing queue priorities and time quantum is difficult.
* Incorrect configuration can cause starvation.

**Point to Remember:**
**MLFQ = Multiple queues + Processes can move between queues.**

---

## 4. MLFQ Example ⭐⭐⭐

Suppose:

```text
Q1 → RR, Time Quantum = 4 ms
Q2 → RR, Time Quantum = 8 ms
Q3 → FCFS
```

```text
              Q1
       RR, 4 ms quantum
              ↓
       Process uses full
       quantum repeatedly
              ↓
              Q2
       RR, 8 ms quantum
              ↓
       Process remains CPU-bound
              ↓
              Q3
             FCFS
```

Suppose process `P1` requires a long CPU burst.

```text
P1 enters Q1
   ↓
Uses entire 4 ms quantum
   ↓
Moves to Q2
   ↓
Uses entire 8 ms quantum
   ↓
Moves to Q3
   ↓
Runs using FCFS
```

A short/interactive process that frequently waits for I/O may remain in a higher-priority queue.

### Aging in MLFQ

A long-waiting process can be moved upward:

```text
Q3
 ↓
Q2
 ↓
Q1
```

This helps prevent starvation.

**Point to Remember:**
**CPU-bound process → tends to move down**
**Long-waiting/interactive process → can move up**

---

## 5. Fixed-Priority Scheduling ⭐⭐

**Fixed-Priority Scheduling** assigns a fixed priority to each queue or process.

The scheduler selects the highest-priority eligible queue/process first.

```text
Priority 1 → Highest
Priority 2
Priority 3
Priority 4 → Lowest
```

```text
       CPU
        ↑
       Q1  ← Highest
        ↑
       Q2
        ↑
       Q3  ← Lowest
```

### Advantages

* Simple scheduling policy.
* Important processes can receive CPU first.
* Predictable priority ordering.

### Disadvantage

A low-priority process may suffer **starvation** if higher-priority processes continuously require the CPU.

**Point to Remember:**
**Fixed Priority = Priority does not normally change during scheduling.**

---

## 6. Q1 — RR Foreground

In a multilevel scheduling system, the **foreground queue** commonly contains interactive processes.

It can use **Round Robin (RR)** to provide quick and fair response.

```text
Foreground Queue
       |
       ↓
   Round Robin
       |
   ┌───┼───┐
   ↓   ↓   ↓
  P1 → P2 → P3
   ↑         |
   └─────────┘
```

### Working

Suppose:

```text
Time Quantum = 4 ms

P1 → 4 ms
P2 → 4 ms
P3 → 4 ms
P1 → 4 ms
...
```

Each process gets a fixed amount of CPU time in rotation.

### Why RR for Foreground?

* Good response time.
* Suitable for interactive processes.
* Provides fair CPU sharing.
* Prevents one process from monopolizing the CPU.

**Point to Remember:**
**Foreground → Interactive → Round Robin → Fast response**

---

## 7. Q2 — FCFS Background

The **background queue** commonly contains batch processes that do not require immediate user interaction.

It can use **FCFS** scheduling.

```text
Background Queue
       |
       ↓
      FCFS
       |
       ↓
P1 → P2 → P3 → P4
```

The process that arrives first is executed first.

### Why FCFS for Background?

* Simple.
* Low scheduling overhead.
* Suitable for batch jobs.
* Interactive response is less important for background processes.

**Point to Remember:**
**Background → Batch → FCFS → Simple execution**

---

# 🔴 7. Multiprocessor Scheduling

## 8. Multiprocessor Scheduling ⭐⭐

**Multiprocessor scheduling** is the process of scheduling tasks/threads when a system has **two or more CPUs or processing cores**.

```text
             Ready Queue
                  |
          ┌───────┼───────┐
          ↓       ↓       ↓
        CPU 1   CPU 2   CPU 3
          ↓       ↓       ↓
        Task 1  Task 2  Task 3
```

### Goals

* Keep all processors busy.
* Improve CPU utilization.
* Improve throughput.
* Reduce response time.
* Balance workload among processors.
* Reduce scheduling overhead.

### Types

* **Symmetric Multiprocessing (SMP)**
* **Asymmetric Multiprocessing**

**Point to Remember:**
**Multiprocessor Scheduling = Scheduling processes/threads across multiple CPUs/cores.**

---

## 9. Symmetric Multiprocessing (SMP) ⭐⭐⭐

In **Symmetric Multiprocessing (SMP)**, each processor can perform scheduling and execute processes.

All processors have roughly equal status.

```text
             Shared Ready Queue
              /      |      \
             ↓       ↓       ↓
          CPU 1    CPU 2    CPU 3
             ↕       ↕       ↕
          Scheduling available
          on each processor
```

### Characteristics

* All processors are peers.
* Processes/threads can execute on any available processor.
* Scheduling is distributed or coordinated among processors.
* Provides good load balancing when properly managed.

### Advantages

* Better processor utilization.
* Good scalability.
* No single processor must handle all scheduling work.
* Supports parallel execution.

### Disadvantages

* More complex synchronization.
* Scheduling overhead can increase.
* Load balancing is required.

**Point to Remember:**
**SMP = All processors are peers and can participate in scheduling.**

---

## 10. Asymmetric Multiprocessing ⭐⭐⭐

In **Asymmetric Multiprocessing**, one processor is assigned special responsibility for system activities such as scheduling, while other processors execute assigned tasks.

```text
             Master Processor
                   |
             Scheduling
                   |
        ┌──────────┼──────────┐
        ↓          ↓          ↓
      CPU 1      CPU 2      CPU 3
      Worker     Worker     Worker
```

### Characteristics

* One processor acts as the **master**.
* Other processors act as workers/slaves.
* The master may handle scheduling and system activities.
* Work is assigned to other processors.

### Advantages

* Simpler to implement.
* Easier coordination.
* Less synchronization between processors.

### Disadvantages

* Master processor can become a bottleneck.
* Less flexible than SMP.
* Failure or overload of the master can affect scheduling.

**Point to Remember:**
**Asymmetric Multiprocessing = Master processor + Worker processors.**

---

## 11. Push Migration ⭐⭐

**Push migration** is a load-balancing technique where a processor or scheduler **actively moves tasks from an overloaded processor to a less-loaded processor**.

```text
CPU 1
[ P1 ][ P2 ][ P3 ][ P4 ]
          |
          | Push P4
          ↓
CPU 2
[ P5 ][ P4 ]
```

### Working

1. A processor becomes overloaded.
2. The system detects the load imbalance.
3. A task is selected.
4. The task is pushed to another less-loaded processor.

**Point to Remember:**
**Push Migration = Overloaded CPU pushes work away.**

---

## 12. Pull Migration ⭐⭐

**Pull migration** occurs when an **idle or lightly loaded processor pulls a task** from another processor.

```text
CPU 1
[ P1 ][ P2 ][ P3 ]
          |
          | Task available
          ↓
CPU 2
   Idle
     |
     | Pull P3
     ↓
[ P3 ]
```

### Working

1. A processor becomes idle.
2. It looks for work on another processor.
3. It takes a task.
4. It starts executing the task.

**Point to Remember:**
**Pull Migration = Idle CPU pulls work toward itself.**

---

## 13. Symmetric Multithreading (SMT) ⭐⭐

**Symmetric Multithreading (SMT)** is a processor technique that allows a **single physical CPU core to execute instructions from multiple threads** concurrently by maintaining multiple hardware thread contexts.

It improves utilization of processor execution resources when one thread cannot fully use them.

```text
          Physical CPU Core
                 |
        ┌────────┴────────┐
        ↓                 ↓
    Hardware          Hardware
    Thread 1          Thread 2
        ↓                 ↓
      Thread A          Thread B
```

### Characteristics

* Multiple hardware threads share one physical core.
* Threads share many processor resources.
* Improves utilization of execution resources.
* Does **not** mean that each hardware thread is equivalent to a separate physical core.

### Example

A processor may have:

```text
4 Physical Cores
        ↓
8 Hardware Threads
```

This can allow up to 8 hardware threads to be scheduled concurrently, depending on the processor architecture.

**Point to Remember:**
**SMT = Multiple hardware threads execute on a single physical CPU core.**

---

# Quick Revision

| Topic                      | Key Point                                                  |
| -------------------------- | ---------------------------------------------------------- |
| Multilevel Queue           | Multiple queues; processes normally stay in assigned queue |
| MLFQ                       | Multiple queues; processes can move between queues         |
| Fixed Priority             | Higher-priority queue/process selected first               |
| Foreground                 | Commonly uses Round Robin                                  |
| Background                 | Commonly uses FCFS                                         |
| Multiprocessor Scheduling  | Scheduling across multiple CPUs/cores                      |
| SMP                        | All processors are peers                                   |
| Asymmetric Multiprocessing | One master processor coordinates work                      |
| Push Migration             | Overloaded CPU pushes tasks away                           |
| Pull Migration             | Idle CPU pulls tasks                                       |
| SMT                        | Multiple hardware threads on one physical core             |

### Most Important Differences

```text
Multilevel Queue
→ Processes are permanently assigned to queues.

MLFQ
→ Processes can move between queues.

SMP
→ All processors can participate in scheduling.

Asymmetric Multiprocessing
→ One master processor handles important scheduling/system work.

Push Migration
→ Busy CPU pushes work.

Pull Migration
→ Idle CPU pulls work.

SMT
→ Multiple hardware threads share one physical CPU core.
```
