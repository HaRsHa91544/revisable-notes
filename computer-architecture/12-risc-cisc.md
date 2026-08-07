Perfect. We won't go deeper into flip-flops. What you've understood about the clock is sufficient for Computer Organization. We can always revisit digital electronics later if needed.

Now let's study **RISC & CISC** from first principles.

---

# RISC & CISC — First Principles Sheet

---

# 1. Problem

Early computers had only a few basic instructions such as:

```assembly
LOAD
STORE
ADD
SUB
JUMP
```

Complex operations like multiplication had to be implemented using many simple instructions.

Example:

```text
5 × 4

↓

5 + 5 + 5 + 5
```

This increased the number of instructions executed.

Engineers wondered whether the CPU itself should support more powerful instructions.

---

# 2. Why Old Solutions Failed

### Approach 1: Only Simple Instructions

Advantages:

* Simple CPU hardware
* Easier Control Unit
* Lower manufacturing cost

Disadvantages:

* Programs contain many instructions.
* Complex operations require many clock cycles.

---

### Approach 2: Many Complex Instructions

Advantages:

* Programs contain fewer instructions.
* Complex tasks execute using a single instruction.

Disadvantages:

* CPU hardware becomes more complex.
* Control Unit becomes larger and harder to design.
* Some complex instructions are rarely used.

Neither approach was perfect.

---

# 3. Questions Engineers Asked

* Should the CPU implement every complex operation in hardware?
* Is it better to execute fewer complex instructions or more simple instructions?
* Which design gives better overall performance?
* Should complexity be placed in the hardware or in the compiler?

---

# 4. How the Solution Emerged

Two different CPU design philosophies emerged.

## CISC (Complex Instruction Set Computer)

Idea:

> Make hardware powerful.

Provide many complex instructions so programmers and compilers execute fewer instructions.

---

## RISC (Reduced Instruction Set Computer)

Idea:

> Keep hardware simple.

Provide only a small set of simple instructions. Let the compiler combine them to perform complex tasks.

---

# 5. Mental Model

```text
                     Need Complex Operations
                              │
          ┌───────────────────┴───────────────────┐
          │                                       │
          ▼                                       ▼
      CISC Philosophy                      RISC Philosophy

Add more hardware                    Keep hardware simple

Many complex instructions            Few simple instructions

Less program code                    More program code

Complex Control Unit                 Simple Control Unit
```

---

# 6. Official Exam Definitions

## CISC

**Complex Instruction Set Computer (CISC)** is a processor design philosophy that provides a **large number of complex instructions**, where a single instruction can perform multiple operations.

---

## RISC

**Reduced Instruction Set Computer (RISC)** is a processor design philosophy that provides a **small set of simple instructions**, each designed to execute efficiently, often in a single clock cycle.

---

# 7. Comparison

| Feature                      | RISC                | CISC                  |
| ---------------------------- | ------------------- | --------------------- |
| Instruction Set              | Small               | Large                 |
| Instruction Complexity       | Simple              | Complex               |
| Instruction Length           | Usually fixed       | Usually variable      |
| Clock Cycles per Instruction | Usually 1           | May require multiple  |
| Hardware                     | Simpler             | More complex          |
| Control Unit                 | Hardwired           | Often Microprogrammed |
| Compiler                     | More responsibility | Less responsibility   |
| Program Size                 | Larger              | Smaller               |
| Pipeline Efficiency          | Excellent           | More difficult        |
| Examples                     | ARM, RISC-V, MIPS   | x86 (Intel, AMD)      |

---

# 8. Engineering Insight

Engineers initially believed:

> **Fewer instructions = Faster computer**

But they discovered this was not always true.

Performance depends on:

```text
CPU Execution Time

=

Instruction Count

×

Cycles Per Instruction (CPI)

×

Clock Cycle Time
```

### CISC

* Lower Instruction Count
* Higher CPI
* More complex hardware

### RISC

* Higher Instruction Count
* Lower CPI
* Simpler hardware
* Higher possible clock frequencies
* Better pipelining

---

# 9. When Each Is Used

## RISC

Used when:

* High performance is required
* Low power consumption is important
* Efficient pipelining is desired
* Mobile and embedded devices

Examples:

* ARM
* RISC-V
* MIPS

---

## CISC

Used when:

* Backward compatibility is important
* Rich instruction set is preferred
* Existing software ecosystem is large

Examples:

* Intel x86
* AMD x86-64

---

# 10. Modern Reality

Modern processors are **not purely RISC or purely CISC**.

### Modern CISC CPUs (Intel, AMD)

Externally:

```text
CISC Instructions
```

Internally:

```text
↓

Decoded into simple micro-operations (μops)

↓

Executed by a RISC-like execution engine
```

### Modern RISC CPUs

They may include a few specialized instructions for performance, but the overall philosophy remains simple.

---

# 11. Exam Revision Bullets

* **RISC = Reduced Instruction Set Computer**
* **CISC = Complex Instruction Set Computer**
* RISC uses **few simple instructions**.
* CISC uses **many complex instructions**.
* RISC emphasizes **simple hardware**.
* CISC emphasizes **powerful instructions**.
* RISC generally has **fixed-length instructions**.
* CISC often has **variable-length instructions**.
* RISC is highly suitable for **pipelining**.
* Modern Intel and AMD processors translate CISC instructions into simpler internal operations.

---

# 12. Interview Questions

1. Why were RISC and CISC introduced?
2. Explain the engineering trade-off between RISC and CISC.
3. Why does RISC often achieve high performance despite executing more instructions?
4. Why is RISC well suited for pipelining?
5. Explain the CPU performance equation:

   * Execution Time = Instruction Count × CPI × Clock Cycle Time
6. Why do modern Intel processors internally execute RISC-like micro-operations?
7. Compare hardware complexity in RISC and CISC.
8. Give examples of RISC and CISC processors.

---

# 13. Memory Hook

> **The real difference is not "few instructions vs many instructions."**
>
> It is **where the complexity is placed**:
>
> * **RISC:** Simpler CPU hardware, smarter compiler.
> * **CISC:** Smarter CPU hardware, simpler compiler.
>
> Both aim to execute programs efficiently, but they distribute complexity differently.

---

### One correction to a common exam myth

Many books state:

> **"RISC instructions execute in one clock cycle."**

Treat this as a **general characteristic**, not an absolute rule. Modern RISC processors have instructions (such as some memory operations or floating-point operations) that can take multiple cycles. The key philosophy is **keeping instructions simple and regular**, not guaranteeing every instruction completes in exactly one cycle.