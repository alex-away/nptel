# Symbolic Testing

## 1. Testing vs. Proving: Finding the Middle Ground

When we want to be sure software works, there are two extremes:

- **Program Testing** (What we've done so far):
  - Run the code with **sample concrete inputs** (like `5`, `"hello"`).
  - **Goal**: Find bugs for those specific inputs.
  - **Limitation**: Can only show *presence* of bugs, **never** proves correctness for *all* inputs.

- **Program Proving** (Formal Methods):
  - Use **math** (logic, proofs) to show the code is correct for *all possible* valid inputs.
  - **Goal**: Prove correctness.
  - **Limitation**: Very hard, often manual, not always practical.

**Symbolic Execution** is the practical middle ground.

- **The Idea**: Execute the program, but use **symbolic values** (like `α1`, `x0`) instead of concrete ones.
- **What it does**: Builds up *mathematical expressions* for variables as it runs.
- **The Benefit**: One symbolic run can represent *thousands* or even *infinite* concrete runs.

- **Think of it like this**: Instead of testing your function with `f(5)`, `f(10)`, `f(100)` separately, you test it once with `f(x0)` and get a formula that describes what happens for *any* input.

## 2. How Symbolic Execution Works

### I. Simple Example (No Branches)

Let's trace a simple program symbolically.

```c
// Assume a, b, c are inputs
x = a + b;
y = b + c;
z = x + y - b;
// return z;
```

- **Concrete Run**: `a=1, b=3, c=5` → `x=4, y=8, z=9`.

- **Symbolic Run**:
  1. Start with symbolic inputs: `a = α1`, `b = α2`, `c = α3`.
  2. Build expressions:
     - `x` becomes `α1 + α2`.
     - `y` becomes `α2 + α3`.
     - `z` becomes `(α1 + α2) + (α2 + α3) - α2`.
  3. **Path Constraint (PC)**: This is the condition needed to follow this execution path. Since there are no `if`s, the PC is just `true`.

- **Think of it like this**: The symbolic run creates a "recipe" showing how the output is calculated from the inputs, rather than just computing one specific answer.

### II. Example with Branches (`test_me`)

This is where symbolic execution shines.

```c
int twice(int v) {
    return 2 * v;
}

void test_me(int x, int y) {
    int z = twice(y);
    if (z == x) {
        if (x > y + 10) {
            // ERROR state we want to reach
            abort();
        }
    }
    // Program terminates normally here
    printf("Ok");
}
```

- **Goal**: Find inputs `x` and `y` that reach the `error` line.

- **Symbolic Run (Execution Tree)**:
  1. Start: `x = x0`, `y = y0`, `PC = true`.
  2. `z = twice(y);` → State now includes `z = 2 * y0`.
  3. `if (z == x)`: The path **forks**. We explore both options:

     - **Path 1 (Else Branch)**:
       - Condition: `z != x`
       - New PC: `true AND not(2 * y0 == x0)`
       - *Path Ends*.
     
     - **Path 2 (Then Branch)**:
       - Condition: `z == x`
       - New PC: `true AND (2 * y0 == x0)`.
       - *Continue Execution...*
  
  4. `if (x > y + 10)` (inside Path 2): This path forks again:
     - **Path 2a (Else Branch)**:
       - Condition: `x <= y + 10`
       - New PC: `(2 * y0 == x0) AND (x0 <= y0 + 10)`.
       - *Path Ends*.
     
     - **Path 2b (Then Branch - ERROR Path)**:
       - Condition: `x > y + 10`
       - New PC: `(2 * y0 == x0) AND (x0 > y0 + 10)`.
       - *Path Ends at `error`*.

- **Think of it like this**: Symbolic execution explores your code like a maze, keeping track of every turn (condition) it takes. At each fork, it remembers the math formula needed to go down that path.

### III. Generating Test Cases (Constraint Solving)

- At the end, we have a list of **Path Constraints (PCs)**, one for each path through the code.
- Each PC is a logical formula (math expression) about the inputs.
- We feed each PC to a **Constraint Solver** (like Z3, SMT solver).
- The solver finds **concrete input values** that make the formula `true`.

- **Example**: For the error path `PC = (2 * y0 == x0) AND (x0 > y0 + 10)`, the solver might give us `x0 = 30, y0 = 15`.
- **Result**: This `(x=30, y=15)` is a **concrete test case** guaranteed to hit the error.

- **Think of it like this**: The constraint solver is like a reverse calculator. Instead of "What's 2 + 3?", you ask it "What numbers make 2*y = x AND x > y+10 true?" and it gives you the answer.

---

**Key Insight**: Symbolic execution automatically generates test cases aimed at covering all paths/branches by treating inputs as mathematical variables and collecting the conditions needed to reach each program location.
