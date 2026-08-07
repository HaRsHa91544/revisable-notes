# Instruction Set — First Principles Sheet

---

# 1. Problem

A CPU can execute instructions only if it understands their meaning.

Suppose different programmers invent different instructions:

```assembly
ADD R1, R2
```

```assembly
SUM R1, R2
```

```assembly
PLUS R1, R2
```

```assembly
INCREASE R1, R2
```

The CPU cannot understand every programmer's own instruction names.

There must be a **standard set of instructions** that every compiler, assembler, and CPU understands.

---

# 2. Why Old Solutions Failed

Without a standard instruction set:

* Every CPU would understand different instructions.
* Compilers would have to generate different instructions for every processor.
* Programs could not run on different computers using the same ISA.
* Software portability would become impossible.

---

# 3. Questions Engineers Asked

* What is the minimum set of operations every computer must support?
* Should every complex operation be implemented in hardware?
* Can complex operations be built from simpler ones?
* How should instructions be organized?

---

# 4. How the Solution Emerged

Engineers designed a **standard Instruction Set**.

An Instruction Set specifies all the operations that a CPU can execute.

Each instruction performs a specific task such as:

* Moving data
* Performing arithmetic
* Performing logical operations
* Controlling program execution
* Communicating with input/output devices

Programs are built by combining these basic instructions.

---

# 5. Mental Model

```text
                Program
                    │
                    ▼
        Sequence of Instructions
                    │
                    ▼
        ┌──────────────────────┐
        │ LOAD                 │
        │ STORE                │
        │ ADD                  │
        │ SUB                  │
        │ AND                  │
        │ OR                   │
        │ CMP                  │
        │ JUMP                 │
        │ CALL                 │
        │ RETURN               │
        └──────────────────────┘
                    │
                    ▼
             CPU Executes
```

---

# 6. Official Exam Definition

**Instruction Set** is the complete collection of machine language instructions that a processor can understand and execute. It defines all the operations supported by the CPU.

---

# 7. Categories of Instructions

---

## A. Data Transfer Instructions

### Purpose

Move data between registers, memory and I/O devices.

### Common Instructions

```assembly
LOAD
STORE
MOVE
PUSH
POP
```

### Used When

* Reading variables from memory
* Saving results
* Passing function parameters
* Stack operations

---

## B. Arithmetic Instructions

### Purpose

Perform mathematical calculations.

### Common Instructions

```assembly
ADD
SUB
MUL
DIV
INC
DEC
NEG
```

### Used When

* Calculations
* Loop counters
* Index calculations
* Address calculations

---

## C. Logical Instructions

### Purpose

Perform bitwise operations.

### Common Instructions

```assembly
AND
OR
XOR
NOT
```

### Used When

* Masking bits
* Setting flags
* Permission checking
* Bit manipulation

---

## D. Shift and Rotate Instructions

### Purpose

Move bits left or right.

### Common Instructions

```assembly
SHL
SHR
ROL
ROR
```

### Used When

* Multiplication and division by powers of 2
* Bit-field extraction
* Cryptography
* Efficient arithmetic

---

## E. Compare Instructions

### Purpose

Compare two operands without modifying them.

Internally, the ALU performs subtraction but only updates the condition flags.

### Common Instruction

```assembly
CMP
```

### Used When

* if statements
* while loops
* for loops
* switch statements

---

## F. Program Control Instructions

### Purpose

Change the normal sequence of program execution.

### Common Instructions

```assembly
JMP
BEQ
BNE
CALL
RET
```

### Used When

* Branching
* Loops
* Function calls
* Function returns

---

## G. Input / Output Instructions

### Purpose

Transfer data between the CPU and external devices.

### Common Instructions

```assembly
IN
OUT
```

### Used When

* Keyboard input
* Display output
* Printer communication
* Peripheral communication

---

# 8. Why Some Instructions Exist

### LOAD

Moves data from memory to registers.

Without LOAD, the ALU cannot access memory operands.

---

### STORE

Moves data from registers back to memory.

Without STORE, computation results cannot be saved.

---

### ADD

Performs arithmetic addition.

Used in:

* Arithmetic
* Counters
* Address calculations

---

### SUB

Performs subtraction.

Also used internally for comparisons.

---

### CMP

Uses the subtraction hardware to compare operands **without modifying them**.

Updates only:

```text
Z
N
C
V
```

---

### JUMP

Changes the Program Counter.

Without it:

* No loops
* No conditions
* No functions

Programs would execute only sequentially.

---

### CALL and RETURN

Support function execution.

They automatically manage the return address using the stack.

---

# 9. Instruction Set Summary

| Category        | Examples                     | Used For                |
| --------------- | ---------------------------- | ----------------------- |
| Data Transfer   | LOAD, STORE, MOVE, PUSH, POP | Move data               |
| Arithmetic      | ADD, SUB, MUL, DIV, INC, DEC | Mathematical operations |
| Logical         | AND, OR, XOR, NOT            | Bit operations          |
| Shift / Rotate  | SHL, SHR, ROL, ROR           | Bit shifting            |
| Compare         | CMP                          | Decision making         |
| Program Control | JMP, BEQ, CALL, RET          | Control program flow    |
| Input / Output  | IN, OUT                      | Device communication    |

---

# 10. Engineering Insight

Programs are not built from hundreds of unique operations.

Almost every program is a combination of only a few categories:

```text
Move Data

↓

Perform Computation

↓

Compare Result

↓

Change Program Flow

↓

Repeat
```

---

# 11. Exam Revision Bullets

* Instruction Set = Complete set of machine instructions supported by a CPU.
* Every instruction performs a specific operation.
* Instructions are grouped by functionality.
* `CMP` compares operands without modifying them.
* `LOAD` transfers memory → register.
* `STORE` transfers register → memory.
* `JUMP` changes the Program Counter.
* `CALL` and `RET` support function execution.
* Arithmetic instructions perform calculations.
* Logical instructions manipulate bits.

---

# 12. Interview Questions

1. What is an Instruction Set?
2. Why is an Instruction Set necessary?
3. Explain the major categories of instructions.
4. Why is `LOAD` required if memory already stores data?
5. Differentiate between `LOAD` and `STORE`.
6. Why does the CPU have a separate `CMP` instruction instead of using `SUB`?
7. Explain the role of Program Control instructions.
8. What is the purpose of Shift and Rotate instructions?
9. Why are Logical instructions important?
10. How do `CALL` and `RETURN` support function execution?

---

# 13. Memory Hook

> **An Instruction Set is the CPU's vocabulary.**
>
> Just as humans communicate using a fixed vocabulary, a CPU executes programs using a fixed set of instructions.
>
> Every program is fundamentally built by combining five basic activities:
>
> 1. **Move Data**
> 2. **Compute**
> 3. **Compare**
> 4. **Control Execution**
> 5. **Communicate with I/O**