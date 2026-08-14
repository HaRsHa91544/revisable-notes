# Why Postfix?
1. Infix expressions like (A + B) cannot be executable by the computer because first we need to read the A, B values then we need to perform addition. 
2. Finally, it should be converted as (AB+).

## Building the Algorithm

### Core Idea

1. **Postfix shows the actual execution order of operations.**

   * An operator can execute only after all its operands are available.

2. **Operands are already ready to use.**

   * Send operands directly to the output.

3. **Operators may not be ready yet.**

   * A future operator may have higher priority.
   * So operators wait in a temporary place.

4. **The stack is a waiting room for unfinished operators.**

   * The most recently waiting operator is checked first because it is the closest unfinished operation.

5. **When a new operator arrives, compare it with the top waiting operator.**

   * If the waiting operator should execute first (higher or same precedence), execute it.
   * Otherwise let it continue waiting.

6. **Parentheses create a private world.**

   * Operations inside parentheses must finish before outside operations interfere.

7. **`(` is stored as a boundary marker.**

   * It tells the algorithm: "Do not compare beyond this point."

8. **`)` means the private world is ending.**

   * Finish all operators inside that boundary.
   * Remove the boundary marker `(`.

9. **End of expression means nobody needs to wait anymore.**

   * Execute all remaining waiting operators.

---

# Compressed Algorithm

### When reading left → right

1. **Operand**

   * Send to output.

2. **`(`**

   * Push to stack (start boundary).

3. **Operator (`+ - * / ^`)**

   * While top of stack contains an operator that should execute first:

     * Send that operator to output.
   * Store current operator in stack.

4. **`)`**

   * Send operators to output until `(` is found.
   * Remove `(`.

5. **End of expression**

   * Send all remaining operators from stack to output.

---

# One-Line Memory Trick

> **Output ready operands immediately. Let unfinished operators wait in a stack. Parentheses create boundaries. When waiting is no longer needed, move operators to output.**