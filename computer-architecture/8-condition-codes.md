# Condition Codes (Flags) — First Principles Notes

---

# 1. The Engineering Problem

Suppose the CPU executes:

```assembly
CMP R1, R2
```

Example:

```text
R1 = 25
R2 = 25
```

The ALU performs:

```text
25 - 25 = 0
```

Immediately after this, the next instruction is:

```assembly
BEQ LABEL
```

The Control Unit now asks:

> "Were the two numbers equal?"

But the ALU has already finished its work.

**How does the CPU remember useful information about the previous operation?**

---

# 2. Why Previous Solutions Failed

One solution could be:

Store the entire arithmetic result.

```text
Result Register = 0
```

Then every branch instruction would have to examine the entire register.

For a 32-bit CPU:

```text
00000000000000000000000000000000
```

This is inefficient because branch instructions usually need only simple information like:

* Is the result zero?
* Is it negative?
* Did an extra bit occur?
* Was the signed result invalid?

---

# 3. The Question Engineers Asked

Instead of storing the entire result,

Can we store only the important **conditions** produced by the ALU?

Since every answer is only:

```text
Yes
or
No
```

Each condition requires only **1 bit**.

---

# 4. The Solution

Engineers introduced a **special status register** containing several 1-bit flags.

Example:

```text
+----------------------+
| Z | N | C | V |
+----------------------+
```

After every arithmetic or logical operation, the ALU automatically updates these bits.

The next instruction simply checks these flags.

---

# 5. Zero Flag (Z)

## Problem

```text
15 - 15 = 0
```

Question:

Were the operands equal?

Instead of checking the whole result,

the ALU stores:

```text
Z = 1
```

If the result is not zero:

```text
Z = 0
```

### Example

```text
15 - 15 = 0

Z = 1
```

```text
15 - 20 = -5

Z = 0
```

---

# 6. Negative Flag (N)

Question:

Did the result become negative?

Example:

```text
8 - 10 = -2
```

The ALU stores:

```text
N = 1
```

Example:

```text
8 - 5 = 3

N = 0
```

---

# 7. Overflow Flag (V)

## Engineering Problem

Suppose the CPU has only **4-bit signed numbers**.

Range:

```text
-8 to +7
```

Now compute:

```text
+7 + +1
```

Binary:

```text
0111
0001
-----
1000
```

The stored result becomes:

```text
1000 = -8
```

But mathematically:

```text
7 + 1 = 8
```

The correct answer **cannot be represented**.

Therefore:

```text
Overflow = 1
```

### Important Idea

Overflow does **NOT** mean:

> An extra bit came out.

Overflow means:

> **The signed mathematical result cannot be represented using the available bits.**

---

# 8. Carry Flag (C)

## Engineering Problem

4-bit ALU:

```text
1111
0001
-----
10000
```

The ALU produced **5 bits**.

But the register can store only:

```text
0000
```

The extra leftmost bit would be lost.

Engineers asked:

Can we at least remember that an extra bit was produced?

So they added:

```text
Carry = 1
```

### Important Idea

Carry means:

> **An extra bit was generated beyond the register width.**

---

# 9. Carry vs Overflow

| Carry                                   | Overflow                            |
| --------------------------------------- | ----------------------------------- |
| Used mainly for **unsigned arithmetic** | Used for **signed arithmetic**      |
| Extra bit came out                      | Signed result doesn't fit           |
| Hardware width problem                  | Mathematical representation problem |

---

## Example 1

```text
1111
0001
-----
10000
```

Unsigned:

```text
15 + 1 = 16
```

Result:

```text
Carry = 1
Overflow = 0
```

---

## Example 2

```text
0111
0001
-----
1000
```

Signed:

```text
+7 + +1 = +8
```

Stored result:

```text
1000 = -8
```

Result:

```text
Carry = 0
Overflow = 1
```

---

## Example 3

```text
1000
1000
-----
10000
```

Signed:

```text
-8 + -8 = -16
```

Cannot be represented.

Result:

```text
Carry = 1
Overflow = 1
```

---

## Example 4

```text
0011
0010
-----
0101
```

```text
3 + 2 = 5
```

Result:

```text
Carry = 0
Overflow = 0
```

---

# 10. Mental Model

Think of the ALU as producing **two outputs** every time.

```text
          ALU
      +-------------+
A --->|             |
B --->|             |
      +-------------+
            |
      Arithmetic Result
            |
            +------------------------+
            |                        |
        Store Result          Update Flags
                              |
                    +------------------+
                    | Z | N | C | V |
                    +------------------+
```

The result is stored in the destination register.

The flags summarize important properties of that result.

---

# 11. Official Definition

**Condition Codes (Flags)** are **1-bit status indicators** stored in a special status register and automatically updated by the ALU after arithmetic or logical operations. They indicate properties of the result such as whether it is zero, negative, produced a carry, or caused signed overflow. The Control Unit uses these flags to make branching and decision-making instructions possible.

---

# 12. Quick Revision

### Zero (Z)

```text
Result == 0
```

---

### Negative (N)

```text
Result < 0
(Sign bit = 1)
```

---

### Carry (C)

```text
Extra bit generated beyond register width
(Mainly unsigned arithmetic)
```

---

### Overflow (V)

```text
Signed result cannot be represented
(Mainly signed arithmetic)
```

---

# 13. Interview Questions

1. Why can't the CPU simply check the result register instead of using flags?
2. Explain the difference between Carry and Overflow with examples.
3. Can Carry be 1 while Overflow is 0? Give an example.
4. Can Overflow be 1 while Carry is 0? Give an example.
5. Can both Carry and Overflow be 1 simultaneously? Explain.
6. Why is the Carry flag mainly associated with unsigned arithmetic?
7. Why is the Overflow flag mainly associated with signed arithmetic?

---

# 14. Memory Hook

> **Condition codes are the ALU's one-bit report card.**
>
> * **Z** → "Is the result zero?"
> * **N** → "Is the result negative?"
> * **C** → "Did an extra bit come out?"
> * **V** → "Did the signed result become impossible to represent?"