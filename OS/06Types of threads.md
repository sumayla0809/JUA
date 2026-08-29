# 9. Types of Threads — VERY IMPORTANT

## 1. User-Level Threads (ULT) ⭐⭐⭐

**User-Level Threads (ULTs)** are threads managed entirely by a **user-level thread library** without direct kernel management of each thread.

* The kernel is generally unaware of individual ULTs.
* Thread management is performed in **user space**.
* The thread library handles:

  * Thread creation
  * Thread scheduling
  * Thread termination
  * Thread synchronization
* Thread switching can be very fast because it does not require kernel intervention.
* A process can contain multiple ULTs.

```text
Process
   |
   ├── ULT 1
   ├── ULT 2
   └── ULT 3
        |
        v
 User-Level Thread Library
```

**Point to Remember:** ULT = **Managed by user-level library, kernel does not directly manage individual threads.**

---

## 2. Kernel-Level Threads (KLT) ⭐⭐⭐

**Kernel-Level Threads (KLTs)** are threads managed directly by the **operating system kernel**.

* The kernel knows about each thread.
* The kernel performs:

  * Thread creation
  * Thread scheduling
  * Thread termination
  * Thread synchronization
* Kernel can schedule individual threads.
* If one thread blocks, another thread of the same process can continue.
* KLTs can take advantage of multiple CPU cores.

```text
Process
   |
   ├── KLT 1
   ├── KLT 2
   └── KLT 3
        |
        v
      Kernel
```

**Point to Remember:** KLT = **Managed directly by the operating system kernel.**

---

## 3. ULT Implementation

ULTs are implemented using a **thread library in user space**.

### Working

1. The application creates threads using the thread library.
2. The library maintains information about each thread.
3. The user-level scheduler selects which thread should run.
4. The library performs context switching between threads.
5. The kernel schedules the process rather than individual ULTs.

```text
Application
     |
     v
Thread Library
     |
     ├── ULT 1
     ├── ULT 2
     └── ULT 3
     |
     v
   Kernel
     |
     v
   Process
```

**Point to Remember:** ULT implementation is handled mainly by **user-space thread libraries**.

---

## 4. KLT Implementation

KLTs are implemented and managed by the **operating system kernel**.

### Working

1. The application requests thread creation.
2. The kernel creates and maintains the thread.
3. The kernel keeps a **Thread Control Block (TCB)** for each thread.
4. The kernel scheduler selects a thread to execute.
5. The kernel performs thread context switching.

```text
Application
     |
     v
 System Call
     |
     v
   Kernel
     |
     ├── KLT 1
     ├── KLT 2
     └── KLT 3
     |
     v
 CPU
```

**Point to Remember:** KLT implementation is handled by the **operating system kernel**.

---

## 5. Merits of ULT

* **Fast thread management** because kernel intervention is not required for every thread operation.
* **Fast context switching** between threads.
* **Flexible scheduling** because the application can use its own scheduling policy.
* **Portable** because the thread library can work without requiring specific kernel thread support.
* **Lower overhead** compared with kernel-managed thread operations.

**Point to Remember:** Main advantage of ULT = **Fast and low-overhead thread management.**

---

## 6. Demerits of ULT

* If one ULT performs a **blocking system call**, the entire process may become blocked in many-to-one implementations.
* The kernel cannot schedule individual ULTs.
* True parallel execution on multiple CPU cores is limited in many-to-one implementations.
* Thread libraries must handle thread management and synchronization.
* A thread library may not take full advantage of kernel-level parallelism.

**Point to Remember:** Main disadvantage of ULT = **A blocking operation can block the whole process in many-to-one implementations.**

---

## 7. Merits of KLT

* The kernel can schedule **individual threads**.
* If one thread blocks, other threads can continue execution.
* Supports **parallel execution** on multiple CPU cores.
* Better integration with operating-system resources.
* Kernel can manage thread priorities and scheduling.

**Point to Remember:** Main advantage of KLT = **Better kernel scheduling and true parallelism.**

---

## 8. Demerits of KLT

* Thread management requires **kernel involvement**.
* Thread creation and termination are generally more expensive than ULT operations.
* Context switching can have higher overhead.
* Requires more operating-system resources.
* More overhead is involved because the kernel maintains information about threads.

**Point to Remember:** Main disadvantage of KLT = **Higher overhead due to kernel involvement.**

---

## 9. ULT vs KLT — Most Important ⭐⭐⭐

| Feature            | User-Level Thread (ULT)                                      | Kernel-Level Thread (KLT)                                |
| ------------------ | ------------------------------------------------------------ | -------------------------------------------------------- |
| Managed By         | User-level thread library                                    | Operating system kernel                                  |
| Kernel Awareness   | Kernel generally does not know individual ULTs               | Kernel knows individual KLTs                             |
| Scheduling         | User-level scheduler                                         | Kernel scheduler                                         |
| Thread Creation    | Faster                                                       | Relatively slower                                        |
| Context Switching  | Faster                                                       | Relatively slower                                        |
| Kernel Involvement | Not required for most thread operations                      | Required                                                 |
| Blocking           | One blocking ULT may block the entire process in many-to-one | One blocked KLT does not necessarily block other threads |
| Parallelism        | Limited in many-to-one model                                 | Supports parallel execution on multiple cores            |
| Overhead           | Low                                                          | Higher                                                   |
| Flexibility        | More flexible user-level scheduling                          | Controlled by kernel scheduling                          |
| Portability        | Generally more portable                                      | Depends more on OS support                               |
| Resource Usage     | Lower                                                        | Higher                                                   |
| Management         | Thread library manages threads                               | Kernel manages threads                                   |

### Easy Way to Remember

```text
ULT → User manages → Fast → Low overhead
KLT → Kernel manages → More overhead → Better parallelism
```

**Final Point to Remember:**

> **ULT is managed by the user-level thread library, whereas KLT is managed directly by the operating system kernel.**
# 10. Multithreading Models — MUST DO

Multithreading models define the **relationship between user-level threads (ULTs) and kernel-level threads (KLTs)**.

There are three main models:

1. Many-to-One
2. One-to-One
3. Many-to-Many

---

## 1. Many-to-One Model ⭐⭐⭐

In the **Many-to-One model**, multiple user-level threads are mapped to **one kernel-level thread**.

```text
User-Level Threads (ULTs)          Kernel-Level Threads (KLTs)

       ULT 1  ────────┐
       ULT 2  ────────┼──────────>   KLT 1
       ULT 3  ────────┤
       ULT 4  ────────┘
```

### Working

* Multiple ULTs are managed by a user-level thread library.
* All ULTs are mapped to a single KLT.
* The kernel sees only **one thread**.
* Thread management is performed mainly in user space.

### Advantages

* Simple to implement.
* Thread creation and management are fast.
* Context switching between ULTs is fast.
* Low overhead because kernel intervention is not required for individual ULTs.

### Disadvantages

* If one thread performs a blocking system call, the **entire process may block**.
* Cannot achieve true parallelism on multiple CPU cores because only one KLT is available.
* The kernel cannot schedule individual ULTs.

**Point to Remember:**
**Many ULTs → One KLT**

---

## 2. One-to-One Model ⭐⭐⭐

In the **One-to-One model**, each user-level thread is mapped to **one kernel-level thread**.

```text
User-Level Threads (ULTs)          Kernel-Level Threads (KLTs)

       ULT 1  ───────────────────>   KLT 1
       ULT 2  ───────────────────>   KLT 2
       ULT 3  ───────────────────>   KLT 3
       ULT 4  ───────────────────>   KLT 4
```

### Working

* Each ULT has a corresponding KLT.
* The kernel manages and schedules each KLT independently.
* If one thread blocks, other threads can continue.
* Multiple threads can execute simultaneously on multiple CPU cores.

### Advantages

* Provides true parallelism on multiprocessor systems.
* If one thread blocks, other threads can continue execution.
* Kernel can schedule individual threads.
* Better responsiveness.

### Disadvantages

* Creating a thread requires kernel involvement.
* Thread creation is more expensive than the many-to-one model.
* More kernel resources are required.
* Too many threads can increase system overhead.

**Point to Remember:**
**One ULT → One KLT**

---

## 3. Many-to-Many Model ⭐⭐⭐

In the **Many-to-Many model**, many user-level threads are mapped to **multiple kernel-level threads**.

```text
User-Level Threads (ULTs)          Kernel-Level Threads (KLTs)

       ULT 1  ────────────────┐
       ULT 2  ────────────────┼────>   KLT 1
       ULT 3  ────────────────┼────>   KLT 2
       ULT 4  ────────────────┼────>   KLT 3
       ULT 5  ────────────────┘
```

The number of ULTs can be **greater than or equal to** the number of KLTs.

### Working

* Multiple ULTs are mapped to a pool of KLTs.
* The user-level thread library manages ULTs.
* The kernel manages KLTs.
* The system can schedule multiple KLTs on multiple CPU cores.
* Blocking of one thread does not necessarily block the entire process.

### Advantages

* Supports concurrency and parallelism.
* More flexible than many-to-one.
* Does not require one KLT for every ULT.
* Can provide better resource utilization.
* Blocking of one thread does not necessarily stop all other threads.

### Disadvantages

* More complex to implement.
* Requires coordination between user-level and kernel-level thread management.
* Scheduling and mapping are more complicated.

**Point to Remember:**
**Many ULTs → Multiple KLTs**

---

## 4. Comparison of Multithreading Models ⭐⭐⭐

| Feature            | Many-to-One                                  | One-to-One                 | Many-to-Many               |
| ------------------ | -------------------------------------------- | -------------------------- | -------------------------- |
| Mapping            | Many ULT → One KLT                           | One ULT → One KLT          | Many ULT → Many KLT        |
| Kernel Threads     | One                                          | One for each ULT           | Multiple                   |
| Parallelism        | ❌ No true parallelism                        | ✅ Yes                      | ✅ Yes                      |
| Blocking           | One blocking thread may block entire process | Other threads can continue | Other threads can continue |
| Thread Creation    | Fast                                         | Relatively expensive       | Moderate                   |
| Overhead           | Low                                          | High                       | Moderate                   |
| Implementation     | Simple                                       | Simple                     | Complex                    |
| Kernel Involvement | Low                                          | High                       | Moderate                   |
| Resource Usage     | Low                                          | High                       | Moderate                   |

---

## 5. Easy Diagram to Remember

```text
Many-to-One
ULT  ULT  ULT  ULT
 \    |    |   /
      KLT
       1


One-to-One
ULT 1 ─── KLT 1
ULT 2 ─── KLT 2
ULT 3 ─── KLT 3
ULT 4 ─── KLT 4


Many-to-Many
ULT 1 ───┐
ULT 2 ───┤
ULT 3 ───┼──> KLT 1
ULT 4 ───┤    KLT 2
ULT 5 ───┘    KLT 3
```

### One-Line Revision

```text
Many-to-One  → Many ULTs → 1 KLT → No true parallelism
One-to-One   → 1 ULT → 1 KLT → True parallelism
Many-to-Many → Many ULTs → Many KLTs → Flexible + parallel
```

**Most Important:**

> **Many-to-One:** Many ULTs are mapped to one KLT.
> **One-to-One:** Each ULT is mapped to one KLT.
> **Many-to-Many:** Many ULTs are mapped to multiple KLTs.
