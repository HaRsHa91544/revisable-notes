# Instruction Set Architecture (ISA)

> **Mental Model**
>
> **ISA is the contract between software and hardware.**
>
> If two CPUs implement the same ISA, they can execute the same machine program even if their internal hardware organization is completely different.

---

# 1. Why?

### Problem

Different CPU manufacturers designed processors with different:

- Instructions
- Registers
- Data representation
- Memory access methods

As a result,

- Programs written for one CPU could not run on another.
- Compilers had to generate different machine code for different processors.

The question engineers asked was:

> **How can software know exactly what a CPU understands?**

---

# 2. Failed Approach

Allow every CPU manufacturer to define its own instruction format.

Problems:

- Programs become non-portable.
- Compilers become processor-specific.
- Software development becomes difficult.
- No standard communication between software and hardware.

---

# 3. First-Principles Discovery

Think from the compiler's perspective.

The compiler asks:

### What instructions can I generate?

↓

Need a standard **Instruction Set**.

---

### Which registers are available?

↓

Need standard **registers**.

---

### How large are the registers?

↓

Need standard **register size**.

---

### How should I access memory?

↓

Need standard **addressing methods**.

---

### How are numbers and data represented?

↓

Need standard **data representation**.

---

Every question is part of one common agreement between software and hardware.

That agreement is called the **Instruction Set Architecture (ISA).**

---

# 4. Mental Model

```text
        Software

     (Program/Compiler)

             │

     Generates Machine Code

             │

      Instruction Set
      Architecture
          (ISA)

             │

     CPU implements ISA

             │

          Hardware
```

The compiler and the CPU communicate through the ISA.

---

# 5. Core Idea

ISA defines everything a programmer or compiler needs to know to write programs that a CPU can execute.

It specifies **what the CPU supports**, not **how the CPU is internally built**.

Different processors can have different internal organizations while implementing the same ISA.

---

# 6. Official Definition (Exam)

**Instruction Set Architecture (ISA):**

> Instruction Set Architecture (ISA) is the interface between software and hardware. It defines the programmer-visible features of a processor, such as the instruction set, registers, data types, addressing modes, and memory model.

---

# 7. Components of ISA

- Instruction set
- Registers
- Data types
- Register size
- Addressing modes
- Memory model
- Data representation
- Input/Output instructions

---

# 8. Advantages

- Software portability.
- Standard interface between hardware and software.
- Enables different processors to run the same programs.
- Allows hardware improvements without changing software.

---

# 9. Examples

- x86 / x86-64
- ARM
- RISC-V
- MIPS

Processors implementing the same ISA can execute the same machine code.

---

# 10. Revision (30 Seconds)

- ISA = Contract.
- Interface between software and hardware.
- Defines what the CPU understands.
- Includes:
  - Instructions
  - Registers
  - Data types
  - Addressing modes
  - Memory model
- Same ISA → Same machine code.
- Different Organization → Still possible.

---

# 11. Interview Questions

### Why is ISA important?

It provides a standard interface between software and hardware, allowing programs to run on different processors implementing the same ISA.

---

### Does ISA describe the internal design of a processor?

No.

ISA specifies the programmer-visible features.

The internal implementation belongs to **Computer Organization (Microarchitecture)**.

---

### Can Intel and AMD processors execute the same programs?

Yes.

Both implement the **x86 ISA**, although their internal hardware designs are different.

---

### Is ISA the same as Computer Architecture?

ISA is a major part of **Computer Architecture**. It defines the programmer-visible interface of the processor.

---

# 12. Transition

Computer Organization explained **how hardware is built**.

Von Neumann and Harvard Architectures explained **how a computer is organized**.

The next question was:

> **How should software communicate with that hardware?**

The answer is the **Instruction Set Architecture (ISA).**

---

# 13. One-Line Memory Hook

> **ISA is the language and contract that allows software and the CPU to understand each other.**