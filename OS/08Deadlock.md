# 🔴 Deadlock and Starvation — MUST DO ⭐⭐⭐

## 1. Deadlock

A **deadlock** is a situation where two or more processes/threads are permanently waiting for resources held by each other.

### Example

```text
Process P1 holds Resource R1
        ↓
      waits for
        ↓
Resource R2

Process P2 holds Resource R2
        ↓
      waits for
        ↓
Resource R1
```

Neither process can continue because:

```text
P1 → waits for R2 → held by P2
P2 → waits for R1 → held by P1
```

### Necessary Conditions for Deadlock

Deadlock can occur only when all four conditions exist:

1. **Mutual Exclusion** — A resource can be used by only one process at a time.
2. **Hold and Wait** — A process holds at least one resource while waiting for another.
3. **No Preemption** — A resource cannot be forcibly taken from a process.
4. **Circular Wait** — A circular chain of processes exists where each process waits for a resource held by the next process.

**Point to Remember:**
`Mutual Exclusion + Hold and Wait + No Preemption + Circular Wait = Deadlock`

---

## 2. Starvation

**Starvation** is a situation where a process waits for a very long time because other processes continuously receive the required resources or CPU.

### Example

```text
High-Priority Process → CPU
High-Priority Process → CPU
High-Priority Process → CPU
        ↓
Low-Priority Process → Keeps Waiting
```

The low-priority process may remain waiting because higher-priority processes are continuously selected.

### Causes of Starvation

* Priority scheduling
* Unfair resource allocation
* Unfair scheduling policies
* Continuous preference given to other processes

### Prevention of Starvation

**Aging** can be used.

In aging, the priority of a waiting process is gradually increased so that it eventually gets CPU time.

**Point to Remember:**
Starvation = **Indefinite waiting due to unfair allocation/scheduling.**

---

## 3. Deadlock vs Starvation ⭐⭐⭐

| Feature             | Deadlock                                                     | Starvation                                                               |
| ------------------- | ------------------------------------------------------------ | ------------------------------------------------------------------------ |
| Meaning             | Processes wait for each other permanently                    | A process waits indefinitely because others keep getting resources       |
| Main Cause          | Circular resource dependency                                 | Unfair scheduling/resource allocation                                    |
| Progress            | Involved processes cannot progress                           | Other processes may continue                                             |
| Number of Processes | Usually involves two or more processes                       | Can affect one process                                                   |
| Resource Dependency | Usually present                                              | Not necessarily circular                                                 |
| Solution            | Prevention, avoidance, detection and recovery                | Fair scheduling, aging                                                   |
| Example             | P1 waits for R2 held by P2, while P2 waits for R1 held by P1 | Low-priority process keeps waiting while high-priority processes execute |

**Easy Difference:**

> **Deadlock:** Everyone involved is stuck waiting for each other.
> **Starvation:** One process keeps waiting while others continue getting resources.

---

# 4. Deadlock Handling

The operating system can handle deadlocks using four major approaches:

```text
                 Deadlock Handling
                        |
       ┌────────────────┼────────────────┐
       ↓                ↓                ↓
  Prevention        Avoidance       Detection
                                         |
                                         ↓
                                      Recovery
```

### 1. Deadlock Prevention

Prevent deadlock by ensuring that **at least one necessary condition for deadlock never occurs**.

### 2. Deadlock Avoidance

The OS examines resource allocation and grants resources only if the system remains in a **safe state**.

### 3. Deadlock Detection

The OS allows deadlocks to occur and periodically checks whether a deadlock exists.

### 4. Deadlock Recovery

After detecting a deadlock, the OS takes action to remove it.

**Point to Remember:**

> **Prevention → Stop deadlock conditions.**
> **Avoidance → Avoid unsafe states.**
> **Detection → Find deadlock.**
> **Recovery → Remove deadlock.**

---

# 5. Deadlock Prevention ⭐⭐⭐

Deadlock prevention ensures that at least one of the four necessary conditions for deadlock is eliminated.

### 1. Eliminate Mutual Exclusion

Make resources sharable whenever possible.

**Example:** Read-only files can often be shared.

However, some resources cannot be shared, such as printers.

### 2. Eliminate Hold and Wait

Require a process to request all required resources before execution.

```text
Request all resources
        ↓
Acquire resources
        ↓
Execute
        ↓
Release resources
```

### 3. Eliminate No Preemption

If a process requests a resource that is unavailable, resources held by that process may be released/preempted where possible.

### 4. Eliminate Circular Wait

Assign an ordering to resource types and require processes to request resources in that order.

```text
R1 → R2 → R3 → R4
```

A process cannot request `R1` after already acquiring `R3`.

**Point to Remember:** Prevention works by **breaking at least one necessary condition**.

---

# 6. Deadlock Avoidance ⭐⭐⭐

**Deadlock avoidance** dynamically examines each resource request and decides whether granting it will keep the system in a **safe state**.

The OS needs information about the maximum resource requirements of processes.

### Safe State

A system is in a **safe state** if there exists at least one sequence in which all processes can complete without causing deadlock.

```text
Safe State
    ↓
Resource Request
    ↓
Check Safety
    ↓
Safe? ── Yes ──> Grant Resource
  |
  No
  ↓
Delay Request
```

### Banker's Algorithm

The **Banker's Algorithm** is a common deadlock-avoidance algorithm for systems with multiple instances of resources.

**Point to Remember:**

> **Avoidance does not prevent the conditions directly; it avoids entering an unsafe state.**

---

# 7. Deadlock Detection ⭐⭐⭐

In **deadlock detection**, the operating system allows resource allocation and checks whether a deadlock has occurred.

### Working

```text
Resource Allocation
        ↓
Allow Processes to Run
        ↓
Check for Deadlock
        ↓
Deadlock Found?
    ↙          ↘
  Yes           No
   ↓             ↓
Recovery      Continue
```

### Detection Methods

* **Single instance of each resource:** Use a **Wait-For Graph**.
* **Multiple instances of resources:** Use a detection algorithm based on available resources and allocation information.

### Wait-For Graph

A directed graph represents which process is waiting for another process.

```text
P1 → P2
↑     ↓
└─────┘
```

A cycle indicates a deadlock when each resource type has a single instance.

**Point to Remember:** Detection means **allow first, detect later**.

---

# 8. Deadlock Recovery ⭐⭐⭐

After detecting a deadlock, the operating system must recover the system.

### 1. Process Termination

The OS terminates one or more processes involved in the deadlock.

Two approaches:

* Abort all deadlocked processes.
* Abort one process at a time until the deadlock is removed.

### 2. Resource Preemption

The OS temporarily takes resources from some processes and gives them to others.

Important considerations:

* Select a suitable victim process.
* Roll back the process if necessary.
* Avoid repeatedly selecting the same process.

### 3. Process Rollback

A process is returned to a previous safe state and restarted later.

**Point to Remember:** Recovery methods include **process termination, resource preemption, and rollback**.

---

# 9. Prevention vs Avoidance vs Detection ⭐⭐⭐

| Feature            | Prevention                   | Avoidance                        | Detection                       |
| ------------------ | ---------------------------- | -------------------------------- | ------------------------------- |
| Basic Idea         | Prevent deadlock conditions  | Avoid unsafe states              | Detect deadlock after it occurs |
| Deadlock Allowed?  | No                           | No, if algorithm works correctly | Yes                             |
| Main Requirement   | Restrict resource allocation | Maximum resource requirements    | Detection mechanism             |
| Approach           | Static restrictions          | Dynamic decision-making          | Periodic/triggered checking     |
| Main Concept       | Break a necessary condition  | Maintain safe state              | Find deadlock                   |
| Example            | Resource ordering            | Banker's Algorithm               | Wait-For Graph                  |
| Recovery Required? | Usually no                   | Usually no                       | Yes, if deadlock is detected    |

### Easy Memory Trick

```text
Prevention → Don't let deadlock conditions form.
Avoidance  → Don't enter an unsafe state.
Detection  → Check whether deadlock happened.
Recovery   → Remove the deadlock.
```

---

# 10. Complete Deadlock Handling Summary

```text
                    DEADLOCK
                       |
                       v
              Deadlock Handling
                       |
        ┌──────────────┼──────────────┐
        ↓              ↓              ↓
   Prevention       Avoidance      Detection
        |              |              |
 Break conditions   Safe state     Find deadlock
                                      |
                                      v
                                  Recovery
                                      |
                         ┌────────────┼────────────┐
                         ↓            ↓            ↓
                    Termination  Preemption    Rollback
```

**Final Points to Remember:**

* **Deadlock:** Processes are permanently blocked waiting for resources.
* **Starvation:** A process waits indefinitely because resources/CPU are repeatedly given to others.
* **Prevention:** Break at least one necessary condition.
* **Avoidance:** Keep the system in a safe state.
* **Detection:** Detect deadlock after it occurs.
* **Recovery:** Remove the detected deadlock.
* **Banker's Algorithm:** Used for deadlock avoidance.
* **Wait-For Graph:** Used for detecting deadlock with single instances of each resource.
