# Addressing Modes — First Principles Sheet

---

# 1. Problem

An instruction like:

```assembly
ADD 100
```

contains an **operand**.

The CPU understands the operation (**ADD**), but asks:

> **"How should I interpret the operand `100`?"**

Could it mean:

* The actual value **100**?
* A memory address?
* A register?
* A memory address stored inside a register?
* An address relative to the Program Counter?
* The top of the stack?

Without a standard, the CPU cannot determine where the operand is located.

---

# 2. Why Old Solutions Failed

If every instruction always interpreted operands in the same way:

```assembly
ADD 100
```

the CPU would never know whether **100** represents:

```text
Constant Value

OR

Memory Address

OR

Register Number
```

Different programming situations require operands to be obtained differently.

Therefore, a single interpretation is insufficient.

---

# 3. Questions Engineers Asked

* How can the CPU distinguish between constants and memory addresses?
* How can operands be accessed faster?
* How can data move without changing instructions?
* How can arrays be accessed efficiently?
* How can programs be relocated in memory?
* How can function calls be managed efficiently?

---

# 4. How the Solution Emerged

Engineers introduced **Addressing Modes**.

An addressing mode is a **rule** that tells the CPU **how to interpret the operand** specified in an instruction.

Different addressing modes solve different engineering problems.

---

# 5. Mental Model

```text
              Instruction

        ADD    Operand
               │
               ▼
      Addressing Mode decides

   ┌──────────────────────────┐
   │ Immediate  → Operand itself
   │ Direct     → Memory Address
   │ Indirect   → Address of Address
   │ Register   → CPU Register
   │ Reg Ind.   → Register contains Address
   │ Indexed    → Base + Offset
   │ PC Relative→ PC + Offset
   │ Stack      → Top of Stack (SP)
   └──────────────────────────┘
```

---

# 6. Official Exam Definitions

## A. Immediate Addressing Mode

### Definition

Immediate Addressing Mode is an addressing mode in which the **operand itself is stored inside the instruction**.

### Example

```assembly
ADD #100
```

### Used When

* Constants
* Fixed values
* Incrementing counters

### Advantage

No additional memory access.

---

## B. Direct Addressing Mode

### Definition

Direct Addressing Mode is an addressing mode in which the **operand field contains the memory address of the actual operand**.

### Example

```assembly
LOAD 100
```

Execution:

```text
Operand = Memory[100]
```

### Used When

* Variables stored in memory

### Advantage

Simple implementation.

---

## C. Indirect Addressing Mode

### Definition

Indirect Addressing Mode is an addressing mode in which the **operand field contains the address of a memory location, and that memory location stores the effective address of the actual operand**.

### Example

```text
Instruction

LOAD 500

Memory

500 → 100
100 → 75
```

Execution

```text
Memory[500]

↓

100

↓

Memory[100]

↓

75
```

### Used When

* Data location changes
* Pointer-like access
* Relocatable data

### Advantage

Instruction remains unchanged even if the data moves.

### Disadvantage

Requires two memory accesses.

---

## D. Register Addressing Mode

### Definition

Register Addressing Mode is an addressing mode in which the **operand field specifies a CPU register that contains the operand**.

### Example

```assembly
ADD R1
```

### Used When

* Arithmetic operations
* Frequently used variables
* Fast computation

### Advantage

No memory access.

Fast execution.

---

## E. Register Indirect Addressing Mode

### Definition

Register Indirect Addressing Mode is an addressing mode in which the **operand field specifies a register that contains the effective address of the operand**.

### Example

Registers

```text
R1 → 1000
```

Memory

```text
1000 → 75
```

Instruction

```assembly
ADD (R1)
```

Execution

```text
R1

↓

1000

↓

Memory[1000]

↓

75
```

### Used When

* Pointer access
* Dynamic data
* Linked data structures

### Advantage

Only one memory access is required.

---

## F. Indexed (Base + Offset / Displacement) Addressing Mode

### Definition

Indexed Addressing Mode is an addressing mode in which the **effective address is obtained by adding a base address to an index (or displacement)**.

### Formula

```text
EA = Base Address + Offset
```

For arrays

```text
EA = Base + (Index × SizeOfDataType)
```

### Example

```c
marks[3]
```

Base = 1000

Index = 3

Size = 4

```text
EA = 1000 + (3 × 4)

EA = 1012
```

### Used When

* Arrays
* Structures
* Tables
* Buffers

### Advantage

One instruction accesses every array element.

---

## G. PC Relative Addressing Mode

### Definition

PC Relative Addressing Mode is an addressing mode in which the **effective address is obtained by adding the signed offset contained in the instruction to the current value of the Program Counter (PC).**

### Formula

```text
EA = PC + Offset
```

### Example

Current PC

```text
500
```

Offset

```text
+8
```

Execution

```text
EA = 508
```

### Used When

* Branch instructions
* Loops
* Position Independent Code
* Program relocation

### Advantage

Program works correctly even if loaded at different memory locations.

---

## H. Stack Addressing Mode

### Definition

Stack Addressing Mode is an addressing mode in which the **operand is implicitly located at the top of the stack, and the CPU accesses it using the Stack Pointer (SP).**

### Example

```assembly
PUSH R1

POP R2
```

### Used When

* Function calls
* Function returns
* Local variables
* Recursive functions
* Interrupt handling

### Advantage

No need to specify memory addresses explicitly.

---

# 7. Addressing Modes Summary

| Addressing Mode   | Operand Obtained From | Effective Address | Used When                 |
| ----------------- | --------------------- | ----------------- | ------------------------- |
| Immediate         | Instruction           | Operand itself    | Constants                 |
| Direct            | Memory                | Address Field     | Variables                 |
| Indirect          | Memory                | Memory[Address]   | Data relocation, pointers |
| Register          | Register              | Register          | Fast arithmetic           |
| Register Indirect | Memory via Register   | Register Content  | Pointer access            |
| Indexed / Base    | Base + Offset         | Base + Offset     | Arrays, structures        |
| PC Relative       | PC + Offset           | PC + Offset       | Branches, loops           |
| Stack             | Top of Stack          | SP                | Function calls, recursion |

---

# 8. Engineering Evolution

```text
Need Constant Values

↓

Immediate Addressing

↓

Need Variables

↓

Direct Addressing

↓

Data May Move

↓

Indirect Addressing

↓

Pointer Access Is Slow

↓

Register Indirect Addressing

↓

Need Faster Computation

↓

Register Addressing

↓

Need Arrays

↓

Indexed Addressing

↓

Programs Move In Memory

↓

PC Relative Addressing

↓

Need Function Calls

↓

Stack Addressing
```

---

# 9. Exam Revision Bullets

* **Immediate** → Operand is inside the instruction.
* **Direct** → Operand field contains the memory address.
* **Indirect** → Memory contains another memory address.
* **Register** → Operand is inside a CPU register.
* **Register Indirect** → Register contains the memory address.
* **Indexed** → `EA = Base + Offset`; used for arrays.
* **PC Relative** → `EA = PC + Offset`; used for branches.
* **Stack** → Operand is at the top of the stack using the Stack Pointer.

---

# 10. Interview Questions

1. Why were Addressing Modes introduced?
2. Explain the difference between Immediate and Direct Addressing.
3. Why was Indirect Addressing introduced?
4. Compare Direct, Indirect, and Register Indirect Addressing.
5. Why is Register Addressing faster than Direct Addressing?
6. Why is Register Indirect Addressing faster than Indirect Addressing?
7. Derive the formula for Indexed Addressing.
8. Why is Indexed Addressing ideal for arrays?
9. Why does PC Relative Addressing make program relocation easier?
10. Why is Stack Addressing suitable for function calls and recursion?
11. Explain the advantages and disadvantages of each addressing mode.
12. Which addressing modes are most commonly used by modern processors, and why?

---

## Memory Hook

> **Addressing Modes answer one fundamental question:**
>
> **"The CPU knows the operation—how should it locate the operand?"**
>
> Every addressing mode is an engineering solution to a different data-access problem:
>
> * **Immediate** → Constant values
> * **Direct** → Variables
> * **Indirect** → Relocatable data
> * **Register** → Fast access
> * **Register Indirect** → Fast pointer access
> * **Indexed** → Arrays
> * **PC Relative** → Relocatable programs
> * **Stack** → Function calls and recursion