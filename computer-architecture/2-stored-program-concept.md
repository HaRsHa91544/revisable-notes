# Stored Program Concept

> **Mental Model**
>
> **A computer becomes general-purpose when both instructions and data are stored in memory.**

---

# 1. Why?

### Problem

Early computers were **hardwired**.

- To perform a new task, the hardware had to be rewired or manually reconfigured.
- A single machine could not easily switch from one program to another.

The computer needed a way to change its behavior **without changing the hardware**.

---

# 2. Failed Approach

**Hardwired Programming**

```
Hardware
    ↓
One Fixed Task
```

Problems:

- Rewiring required for every new program.
- Time-consuming.
- Not flexible.
- Difficult to maintain.

---

# 3. First-Principles Discovery

Ask the question:

> **Instead of changing the hardware, can we change only the instructions?**

If instructions are stored in memory like data,

```
Memory

↓

Instructions
+
Data
```

Then,

- To run a different program,
- simply load different instructions into memory.

The hardware remains the same.

The program changes.

---

# 4. Mental Model

```
            Hardware

               │

        Executes Instructions

               ▲

        Instructions Stored

               │

            Memory

      (Instructions + Data)
```

---

# 5. Core Idea

Instead of wiring a computer for every task,

store the program in memory.

Changing the program changes the computer's behavior without changing the hardware.

---

# 6. Official Definition (Exam)

**Stored Program Concept:**

> The Stored Program Concept, proposed by **John von Neumann**, states that **both program instructions and data are stored together in the same main memory**. The CPU fetches instructions from memory, decodes them, and executes them sequentially.

---

# 7. Revision (30 Seconds)

- Early computers were hardwired.
- Hardware had to be changed for every program.
- Store instructions in memory.
- Instructions and data share the same memory.
- CPU fetches and executes instructions.
- Same hardware → Different programs.

---

# 8. Interview Questions

### Why was the Stored Program Concept introduced?

To eliminate the need for rewiring hardware whenever a new program had to be executed.

---

### What is the biggest advantage of the Stored Program Concept?

A single computer can execute many different programs by simply loading different instructions into memory.

---

### Who proposed the Stored Program Concept?

**John von Neumann**

---

# 9. One-Line Memory Hook

> **Don't change the hardware. Change the program stored in memory.**