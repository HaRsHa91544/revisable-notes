# Harvard Architecture

> **Mental Model**
>
> **Separate instructions from data so the CPU can access both simultaneously.**

---

# 1. Why?

### Problem

Von Neumann Architecture stores **instructions and data in the same memory** and transfers them through the **same communication path**.

As a result,

- Instruction fetch
- Data read
- Data write

cannot happen simultaneously.

The CPU spends time waiting for memory, creating the **Von Neumann Bottleneck**.

The question engineers asked was:

> **Can we separate instruction access from data access?**

---

# 2. Failed Approach

Using a single memory for both instructions and data.

```
            CPU
             │
             │
      Shared Memory
     ┌──────────────┐
     │ Instructions │
     │      +       │
     │    Data      │
     └──────────────┘
```

Problems:

- Shared memory.
- Shared communication path.
- CPU waits while memory serves one request at a time.
- Reduced performance.

---

# 3. First-Principles Discovery

Instead of using one memory,

ask:

> **Why not keep instructions and data completely separate?**

Separate them into:

- Instruction Memory
- Data Memory

Now the CPU can do two operations simultaneously.

Example:

```
Instruction Memory

↓

Next Instruction

------------------------

Data Memory

↓

Operand Data
```

Both memories work independently.

The CPU no longer waits for instruction fetch before accessing data.

---

# 4. Mental Model

```text
                 CPU

           ┌───────────┐
           │           │
           ▼           ▼

Instruction Memory   Data Memory

     Instructions         Data
```

Instruction Memory

↓

Instruction Bus

CPU

Data Bus

↓

Data Memory

Both transfers occur simultaneously.

---

# 5. Core Idea

Harvard Architecture improves performance by using **separate memories and separate communication paths** for instructions and data.

This allows the CPU to fetch the next instruction while simultaneously reading or writing data.

---

# 6. Official Definition (Exam)

**Harvard Architecture:**

> Harvard Architecture is a computer architecture in which **instructions and data are stored in separate memories and accessed through separate communication paths**, allowing simultaneous instruction fetch and data transfer.

---

# 7. Characteristics

- Separate instruction memory.
- Separate data memory.
- Separate buses for instruction and data.
- Simultaneous instruction fetch and data access.
- Reduces the Von Neumann Bottleneck.

---

# 8. Advantages

- Faster execution.
- Parallel instruction and data access.
- Higher throughput.
- Reduced CPU waiting time.

---

# 9. Disadvantages

- More hardware is required.
- Increased design complexity.
- Higher implementation cost.
- Less flexible because instruction and data memories are separate.

---

# 10. Revision (30 Seconds)

- Two memories.
- Two buses.
- Instructions and data are separate.
- Simultaneous access.
- Higher performance.
- Reduces Von Neumann Bottleneck.

---

# 11. Interview Questions

### Why was Harvard Architecture introduced?

To overcome the performance limitation caused by the Von Neumann Bottleneck.

---

### What is the main difference between Harvard and Von Neumann Architecture?

| Von Neumann | Harvard |
|-------------|----------|
| One memory for instructions and data | Separate memories for instructions and data |
| Shared communication path | Separate communication paths |
| Sequential memory access | Simultaneous instruction and data access |

---

### Does Harvard Architecture completely eliminate the Von Neumann Bottleneck?

No.

It **significantly reduces** the bottleneck by separating instruction and data memory access. Other factors, such as main memory speed and CPU design, can still limit performance.

---

### Where is Harvard Architecture used?

- Digital Signal Processors (DSPs)
- Microcontrollers
- Embedded systems

Modern processors often use a **Modified Harvard Architecture**, with separate instruction and data caches but a unified main memory.

---

# 12. One-Line Memory Hook

> **One memory causes waiting. Two memories allow parallel execution.**