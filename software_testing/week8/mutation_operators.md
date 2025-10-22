# Mutation Operators

## 1. Designing Mutation Operators

Mutation operators are the rules for creating mutants. They are not random; they are designed to:

- **Mimic Typical Programmer Mistakes**: Such as using the wrong variable or the wrong relational operator (e.g., `>` instead of `>=`).

- **Enforce Testing Heuristics**: Encourage testers to check for common weak points, like division by zero.

## 2. Effective Mutation Operators

The list of all possible operators is huge. We don't use all of them. We want an **effective** set.

- **Definition**: A set of operators is "effective" if the test cases you write to kill *its* mutants have a very high probability of *also* killing mutants from all the *other* operators you didn't use.

- **Empirical Studies**: Studies show that operators targeting **unary and binary operators** (like arithmetic and relational) tend to be the most effective.

## 3. A Catalog of Common Mutation Operators

Here are common operators that apply to most programming languages. These are generally considered **program-level** operators applied during unit testing.



- **Absolute Value Insertion (ABS)**: Replaces an expression $e$ with `abs(e)`, `negAbs(e)`, or `failOnZero(e)`.
  - `x = a;` → `x = abs(a);`

- **Arithmetic Operator Replacement (AOR)**: Replaces one arithmetic operator (`+`, `-`, `*`, `/`, `%`, `**`) with another.
  - `x = a + b;` → `x = a - b;`
  - Also includes special operators:
    - `leftOp`: `x = a + b;` → `x = a;`
    - `rightOp`: `x = a + b;` → `x = b;`
    - `mod`: Modulo operation

- **Relational Operator Replacement (ROR)**: Replaces one relational operator (`<`, `>`, `<=`, `>=`, `==`, `!=`) with another.
  - `if (m > n)` → `if (m <= n)`
  - Can also replace the entire expression with `true` or `false`.

- **Conditional/Logical Operator Replacement (COR)**: Replaces one logical operator (`&&`, `||`, `^`, `&`, `|`) with another.
  - `if (a && b)` → `if (a || b)`
  - Also includes special operators:
    - `falseOp`: Replaces the entire expression with `false`
    - `trueOp`: Replaces the entire expression with `true`
    - `leftOp`: Returns only the left operand
    - `rightOp`: Returns only the right operand

- **Shift Operator Replacement (SOR)**: Replaces one bitwise shift (`<<`, `>>`, `>>>`) with another.
  - `x = m << a;` → `x = m >> a;`
  - Also includes special operator:
    - `leftOp`: Returns the unshifted left operand

- **Bitwise Logical Operator Replacement (LOR)**: Replaces one bitwise operator (`&`, `|`, `^`) with another.
  - `x = m & n;` → `x = m | n;`
  - Also includes special operators:
    - `leftOp`: Returns only the left operand
    - `rightOp`: Returns only the right operand

- **Assignment Operator Replacement (ASOR)**: Replaces one compound assignment (`+=`, `-=`, `*=`, etc.) with another.
  - `x += 3;` → `x -= 3;`

- **Unary Operator Insertion (UOI)**: Inserts a unary operator (`+`, `-`, `~`, `!`) before an expression.
  - `x = a;` → `x = -a;`

- **Unary Operator Deletion (UOD)**: Deletes a unary operator (`+`, `-`, `~`, `!`).
  - `x = -a;` → `x = a;`

- **Scalar Variable Replacement (VR)**: Replaces a variable with another variable of the same type that is in the same scope.
  - `x = a * b;` → `x = a * a;`
  - `x = a * b;` → `x = x * b;`

- **Bomb Statement**: Replaces an entire statement with a `Bomb()` function that forces a failure. This is used to test reachability.
