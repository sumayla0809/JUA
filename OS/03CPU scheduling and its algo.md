# 3. CPU Scheduling — VERY IMPORTANT

CPU Scheduling is the process of **selecting a process from the ready queue and allocating the CPU to it**.

```text
Ready Queue
    |
    ↓
CPU Scheduler
    |
    ↓
Selected Process
    |
    ↓
   CPU
```

---

## 1. Preemptive Scheduling ⭐⭐⭐

**Preemptive scheduling** allows the operating system to **take the CPU away from a running process** and allocate it to another process.

### When Preemption Can Occur

* A higher-priority process arrives.
* The current process's time quantum expires.
* A process becomes ready and should be scheduled.
* The current process is interrupted.

```text
P1 Running
    |
    | Preemption
    ↓
P1 → Ready
    |
    ↓
P2 → Running
```

### Advantages

* Better response time.
* Suitable for interactive systems.
* Prevents one process from keeping the CPU for too long.
* Supports time-sharing.

### Disadvantages

* More context switches.
* Higher overhead.
* Requires synchronization to handle shared data safely.

**Examples:** SRTF, Round Robin, preemptive Priority Scheduling.

**Point to Remember:**
**Preemptive = CPU can be taken from a running process.**

---

## 2. Non-Preemptive Scheduling ⭐⭐⭐

**Non-preemptive scheduling** does not forcibly take the CPU away from a running process.

Once a process gets the CPU, it keeps the CPU until it:

* Terminates, or
* Enters the waiting state.

```text
P1 → Running
       |
       | Complete / Wait
       ↓
     CPU Free
       ↓
      P2
```

### Advantages

* Simple to implement.
* Fewer context switches.
* Lower scheduling overhead.
* Easier to manage.

### Disadvantages

* Poor response time for interactive processes.
* A long process can make short processes wait.
* Can cause the convoy effect in FCFS.

**Examples:** FCFS, non-preemptive SJF, non-preemptive Priority Scheduling.

**Point to Remember:**
**Non-Preemptive = Once CPU is assigned, it is not forcibly taken away.**

---

## 3. Dispatcher ⭐⭐

The **dispatcher** is the OS component that gives control of the CPU to the process selected by the CPU scheduler.

### Main Functions

1. Performs context switching.
2. Switches the CPU from kernel mode to user mode when required.
3. Jumps to the proper location in the selected process so it can resume execution.

```text
CPU Scheduler
      |
      | Selects process
      ↓
 Dispatcher
      |
      ├── Context Switch
      ├── Mode Switch
      └── Start/Resume Process
      ↓
     CPU
```

### Dispatch Latency

**Dispatch latency** is the time required by the dispatcher to stop one process and start another process.

**Point to Remember:**
**Scheduler selects → Dispatcher gives CPU.**

---

## 4. Scheduling Criteria ⭐⭐⭐

Scheduling criteria are measures used to evaluate the performance of a CPU scheduling algorithm.

### 1. CPU Utilization

Percentage of time the CPU is busy.

```text
CPU Utilization = (CPU Busy Time / Total Time) × 100
```

**Goal:** Maximize CPU utilization.

### 2. Throughput

Number of processes completed per unit of time.

```text
Throughput = Number of completed processes / Unit time
```

**Goal:** Maximize throughput.

### 3. Turnaround Time

Total time taken by a process from submission to completion.

```text
Turnaround Time = Completion Time − Arrival Time
```

**Goal:** Minimize turnaround time.

### 4. Waiting Time

Total time a process spends waiting in the ready queue.

```text
Waiting Time = Turnaround Time − Burst Time
```

**Goal:** Minimize waiting time.

### 5. Response Time

Time from submitting a request until the process gets CPU for the first time.

```text
Response Time = First CPU Start Time − Arrival Time
```

**Goal:** Minimize response time.

### 6. Fairness

Every process should receive a reasonable opportunity to execute.

**Goal:** Avoid starvation.

**Point to Remember:**

```text
CPU Utilization → Maximum
Throughput      → Maximum
Turnaround Time → Minimum
Waiting Time    → Minimum
Response Time   → Minimum
Starvation      → Avoid
```

---

## 5. Scheduling Algorithms

A **CPU scheduling algorithm** determines which process from the ready queue should be allocated the CPU next.

Common algorithms:

* FCFS
* SJF
* SRTF
* Round Robin
* Priority Scheduling

---

## 6. Optimization Criteria ⭐⭐⭐

Optimization criteria define whether a scheduling algorithm is performing well.

| Criterion       | Desired Result  |
| --------------- | --------------- |
| CPU Utilization | Maximum         |
| Throughput      | Maximum         |
| Turnaround Time | Minimum         |
| Waiting Time    | Minimum         |
| Response Time   | Minimum         |
| Fairness        | Maximum         |
| Starvation      | Minimum/Avoided |

**Point to Remember:**
A good scheduling algorithm tries to **maximize CPU utilization and throughput while minimizing waiting, turnaround, and response time**.

---

## 7. Time Quantum ⭐⭐⭐

**Time quantum** is the maximum amount of CPU time allocated to a process before the CPU may be taken away from it in **Round Robin scheduling**.

```text
P1 → CPU → Quantum expires → P1 goes to Ready Queue
                         ↓
                         P2 gets CPU
```

### Example

Suppose:

```text
Time Quantum = 4 ms
```

A process can execute for at most 4 ms in one turn.

```text
P1 → 0–4 ms
P2 → 4–8 ms
P3 → 8–12 ms
P1 → 12–16 ms
```

### Effect of Time Quantum

**Very Small Quantum:**

* Better response time.
* More context switches.
* Higher overhead.

**Very Large Quantum:**

* Fewer context switches.
* Poorer response time.
* Round Robin starts behaving more like FCFS.

**Point to Remember:**
**Small quantum → More context switches**
**Large quantum → Fewer context switches**

---

## 8. Context Switching ⭐⭐⭐

A **context switch** occurs when the CPU switches from one process/thread to another.

The OS:

1. Saves the context of the currently running process.
2. Loads the saved context of the next process.
3. Resumes execution of the new process.

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

### Context Contains

* Program Counter
* CPU Registers
* Stack Pointer
* Process state
* Other execution information

### Context-Switch Overhead

Context switching consumes CPU time but **does not directly perform useful application work**.

**Point to Remember:**
**Context Switch = Save current context + Load next context.**

---

# 4. Scheduling Algorithms — MUST DO ⭐⭐⭐

## 9. FCFS — First-Come, First-Served

**FCFS** schedules processes according to their **arrival order**.

The process that arrives first gets the CPU first.

### Example

```text
Process    Arrival Time    Burst Time
P1              0              5
P2              1              3
P3              2              2
```

Gantt Chart:

```text
|    P1    |   P2   |  P3  |
0          5        8      10
```

### Characteristics

* Usually non-preemptive.
* Simple and easy to implement.
* Uses a FIFO queue.

### Advantages

* Simple.
* Fair according to arrival order.
* Low scheduling overhead.

### Disadvantages

* Can have high average waiting time.
* Convoy effect can occur.
* Poor response time for short processes behind long processes.

**Point to Remember:**
**FCFS = First arrival → First execution**

---

## 10. SJF — Shortest Job First

**SJF** selects the process with the **smallest CPU burst time**.

Usually, standard SJF is **non-preemptive**.

### Example

```text
Process    Burst Time
P1             6
P2             2
P3             4
```

Execution order:

```text
P2 → P3 → P1
```

Gantt Chart:

```text
| P2 |   P3   |      P1      |
0    2        6              12
```

### Advantages

* Gives minimum average waiting time when burst-time estimates are accurate.
* Short processes complete quickly.

### Disadvantages

* CPU burst time must be estimated.
* Long processes may suffer starvation.
* Not ideal when burst times are difficult to predict.

**Point to Remember:**
**SJF = Shortest burst time first.**

---

## 11. SRTF — Shortest Remaining Time First

**SRTF** is the **preemptive version of SJF**.

The process with the **shortest remaining CPU burst time** gets the CPU.

If a new process arrives with a shorter remaining time than the currently running process, the current process is preempted.

### Example

```text
P1: Burst = 8
P2: Burst = 3 arrives while P1 is running
```

If P1 has more than 3 units remaining:

```text
P1 Running
   ↓
P2 Arrives
   ↓
P2 has shorter remaining time
   ↓
P1 → Ready
P2 → Running
```

### Advantages

* Usually provides low average waiting time.
* Short processes receive CPU quickly.
* Better response than non-preemptive SJF in many cases.

### Disadvantages

* More context switches.
* Starvation of long processes is possible.
* Requires estimating remaining burst time.

**Point to Remember:**
**SJF → Shortest burst**
**SRTF → Shortest remaining burst + Preemptive**

---

## 12. Round Robin — RR

**Round Robin** is a **preemptive scheduling algorithm** designed mainly for time-sharing systems.

Each process gets a fixed **time quantum** in a circular order.

```text
        ┌──────────┐
        ↓          │
P1 → P2 → P3 → P4 ─┘
```

### Example

Suppose:

```text
Time Quantum = 2 ms
```

```text
P1 → 2 ms
P2 → 2 ms
P3 → 2 ms
P1 → 2 ms
P2 → ...
```

### Characteristics

* Preemptive.
* Uses a circular ready queue.
* Each process receives a time quantum.
* After quantum expires, the process goes to the end of the ready queue if it still has remaining work.

### Advantages

* Good response time.
* Fair CPU allocation.
* Suitable for interactive/time-sharing systems.
* Prevents a process from monopolizing the CPU.

### Disadvantages

* Too-small quantum causes many context switches.
* Too-large quantum makes it behave more like FCFS.
* Performance depends strongly on the chosen quantum.

**Point to Remember:**
**Round Robin = FCFS + Time Quantum + Preemption**

---

## 13. Priority Scheduling

In **Priority Scheduling**, the CPU is assigned to the process with the **highest priority** according to the system's priority convention.

```text
Higher Priority
      ↓
    CPU
      ↑
Lower Priority
```

> The exact numerical meaning of priority depends on the system. In some systems, a smaller number represents higher priority; in others, a larger number does.

### Types

#### Non-Preemptive Priority

Once a process gets the CPU, it keeps it until it terminates or waits.

#### Preemptive Priority

If a higher-priority process arrives, it can preempt the currently running process.

```text
P1 Running
    |
    | Higher-priority P2 arrives
    ↓
P1 → Ready
P2 → Running
```

### Advantages

* Important processes can be executed first.
* Useful when processes have different importance levels.
* Preemptive priority can provide fast response to high-priority tasks.

### Disadvantages

* Low-priority processes may suffer starvation.
* Priority inversion can occur in some systems.
* Requires a suitable priority assignment policy.

### Aging

**Aging** gradually increases the priority of a waiting process to reduce starvation.

```text
Waiting Time ↑
      ↓
Priority improves
      ↓
Process eventually gets CPU
```

**Point to Remember:**
**Priority Scheduling = Highest-priority eligible process gets CPU.**

---

# Quick Comparison of Scheduling Algorithms

| Algorithm   | Preemptive? | Selection Rule          | Main Problem                  |
| ----------- | ----------- | ----------------------- | ----------------------------- |
| FCFS        | No          | First arrival           | Convoy effect                 |
| SJF         | No          | Shortest burst          | Starvation                    |
| SRTF        | Yes         | Shortest remaining time | Starvation + context switches |
| Round Robin | Yes         | Time quantum            | Quantum selection             |
| Priority    | Yes/No      | Highest priority        | Starvation                    |

### One-Line Revision

```text
FCFS → First arrival first
SJF  → Shortest burst first
SRTF → Shortest remaining time first
RR   → Fixed time quantum in rotation
Priority → Highest-priority process first
```

### Important Formulas

```text
Turnaround Time = Completion Time − Arrival Time

Waiting Time = Turnaround Time − Burst Time

Response Time = First CPU Start Time − Arrival Time
```
