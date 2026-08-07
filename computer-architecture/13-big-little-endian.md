# Big Endian & Little Endian — First Principles Sheet

---

# 1. Problem

Modern processors work with **multi-byte data**.

Example:

* 16-bit integer = 2 bytes
* 32-bit integer = 4 bytes
* 64-bit integer = 8 bytes

Suppose the CPU wants to store the 32-bit number:

```text
0x12345678
```

Memory is **byte-addressable**, meaning each memory location stores only **1 byte (8 bits)**.

The CPU asks:

> **"In what order should these four bytes be stored in memory?"**

---

# 2. Why Old Solutions Failed

If every computer stored multi-byte data differently without any standard:

* Data exchanged between different computers would be interpreted incorrectly.
* Files written by one computer might be read incorrectly by another.
* Network communication would become unreliable.

A standard byte-order convention became necessary.

---

# 3. Questions Engineers Asked

* Which byte should occupy the lowest memory address?
* Should memory preserve the natural order in which humans write numbers?
* Should the least significant byte be placed first for easier processing?
* Can different processors use different conventions?

---

# 4. How the Solution Emerged

Engineers proposed two conventions:

### Big Endian

Store the **Most Significant Byte (MSB)** at the **lowest memory address**.

### Little Endian

Store the **Least Significant Byte (LSB)** at the **lowest memory address**.

Both store the **same value**.

Only the **order of bytes in memory** differs.

---

# 5. Mental Model

Suppose:

```text
32-bit Number

0xABCDEF12
```

Split into bytes:

```text
MSB                     LSB

AB | CD | EF | 12
```

---

## Big Endian

```text
Address        Data

1000           AB
1001           CD
1002           EF
1003           12
```

Memory preserves the same order in which humans write the number.

---

## Little Endian

```text
Address        Data

1000           12
1001           EF
1002           CD
1003           AB
```

Memory starts with the least significant byte.

---

# 6. Official Exam Definitions

## Big Endian

**Big Endian** is a byte-ordering scheme in which the **Most Significant Byte (MSB)** of a multi-byte data item is stored at the **lowest memory address**.

---

## Little Endian

**Little Endian** is a byte-ordering scheme in which the **Least Significant Byte (LSB)** of a multi-byte data item is stored at the **lowest memory address**.

---

# 7. Why They Were Introduced

## Big Endian

### Engineering Idea

Store bytes exactly in the order humans naturally write numbers.

### Advantages

* Easier for humans to read memory dumps.
* Natural byte ordering.
* Used in many communication protocols.

---

## Little Endian

### Engineering Idea

Store the least significant byte first.

### Advantages

* Convenient for accessing the least significant byte.
* Certain arithmetic, type-casting, and pointer operations become simpler.
* Widely used in Intel and AMD processors.

---

# 8. Examples

Suppose:

```text
0x12345678
```

---

### Big Endian

```text
Address      Data

1000         12
1001         34
1002         56
1003         78
```

---

### Little Endian

```text
Address      Data

1000         78
1001         56
1002         34
1003         12
```

---

# 9. Important Observation

The value:

```text
0x12345678
```

does **not** change.

Only the arrangement of bytes in memory changes.

When the CPU reads the bytes according to its endianness rules, it reconstructs the same value.

---

# 10. Uses

### Big Endian

Used in:

* Some processors (historically Motorola, SPARC in some modes)
* Many network protocols ("network byte order")
* Situations where human readability of byte order is preferred

---

### Little Endian

Used in:

* Intel x86 processors
* AMD x86-64 processors
* Most modern desktop and laptop computers

---

# 11. Comparison

| Feature                      | Big Endian                             | Little Endian        |
| ---------------------------- | -------------------------------------- | -------------------- |
| Lowest Memory Address Stores | MSB                                    | LSB                  |
| Human Readability            | Easier                                 | Less intuitive       |
| Byte Order                   | Natural                                | Reversed in memory   |
| Used By                      | Some architectures, network byte order | Intel, AMD, most PCs |

---

# 12. Exam Revision Bullets

* Endianness defines the **byte order** of multi-byte data in memory.
* Memory is **byte-addressable**, so a 32-bit value occupies four consecutive memory locations.
* **Big Endian:** MSB at the lowest address.
* **Little Endian:** LSB at the lowest address.
* Endianness changes **only the storage order**, **not the value**.
* Intel and AMD processors use **Little Endian**.
* Network byte order uses **Big Endian**.

---

# 13. Interview Questions

1. What is Endianness?
2. Why is Endianness required?
3. Differentiate between Big Endian and Little Endian.
4. Store `0x12345678` in both Big Endian and Little Endian memory layouts.
5. Does Endianness change the value of the data? Explain.
6. Why do Intel processors use Little Endian?
7. What is Network Byte Order?
8. Can two computers with different endianness communicate? If yes, how?

---

# 14. Memory Hook

> **Endianness answers one question:**
>
> **"When storing a multi-byte value in byte-addressable memory, which byte should occupy the lowest memory address?"**
>
> * **Big Endian** → Store the **Most Significant Byte first**.
> * **Little Endian** → Store the **Least Significant Byte first**.
>
> The **number remains the same**; only the **byte arrangement in memory** changes.