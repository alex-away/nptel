# Concolic Testing & DART

## 1. Problems with Pure Symbolic Testing

Symbolic execution is powerful but has major practical problems:

- **Loops & Recursion**: Can lead to **infinite execution paths**.
  - *Practical Fix*: Limit the exploration depth (e.g., max loop iterations).
  - *Critical Issue*: Symbolic execution cannot generally prove if a loop will terminate or not for all inputs satisfying a path constraint (related to the Halting Problem). It only explores a bounded number of iterations.

- **Path Constraint (PC) Solving**: This is the biggest hurdle. Solvers often fail when the PC involves:
  - **Unknown Functions**: Calls to external libraries or system code where we don't have the source.
  - **Complex Math**: Non-linear arithmetic (like `x*x`, `log(x)`) that the solver doesn't support.
  - *Theoretical Issue*: Satisfying predicate logic is undecidable in general.

- **Path Explosion**: Real programs have way too many paths to explore them all symbolically.

- **Think of it like this**: Pure symbolic execution is like trying to predict every possible chess game from the starting position. It's theoretically possible but practically impossible.

## 2. The Solution: Concolic Testing

**Concolic Testing** (CONCrete + symbOLIC) is a hybrid approach that fixes these problems.

- **The Idea**: Run the program with **both** concrete values *and* symbolic values **at the same time**.

- **How it works**:
  1. Use a **concrete run** (with actual numbers) to explore just *one* path.
  2. Use a **symbolic run** to "shadow" that path and collect its Path Constraint (PC).
  3. **Negate** part of the PC to find the *next* adjacent path to explore.
  4. Use a **solver** to get *new concrete inputs* for that next path.
  5. Repeat.

- **Think of it like this**: You're exploring a maze. Instead of trying to map out every possible turn from the start (symbolic), you walk down one path (concrete). At each junction you passed, you note down the turn you *didn't* take (negated PC). Then you backtrack and explore one of those noted paths.

## 3. DART: Directed Automated Random Testing

DART is a specific, popular algorithm implementing concolic testing. Its goal is to automate unit testing by exploring all feasible paths.

### I. The DART 3-Step Process

1. **Automatic Interface Extraction**: DART reads the source code to find all inputs (function args, global vars, file reads, etc.) and their types.

2. **Random Test Execution**: DART generates a test driver, picks a *random* starting input, and runs the program concretely.

3. **Directed Search (The Concolic Loop using Random Start, Symbolic Shadowing, Constraint Solving, and Path Negation)**:
   - **Random Start**: Begin with randomly generated concrete inputs.
   - **Symbolic Shadowing**: Simultaneously run symbolically, collecting the PC for the path taken.
   - **Path Negation**: Negate one condition in the PC to create a goal for a *new path*.
   - **Constraint Solving**: Use a constraint solver to find *new concrete inputs* for that new path.
   - **Repeat** with the new inputs, forcing exploration down the new path.

### II. DART Example Walkthrough

Let's trace DART on a function with a bug.

```c
int f(int x) { return 2*x; }

void h(int x, int y) {
    // Condition 1
    if (x != y) {
        // Condition 2 (uses complex function f)
        if (f(x) == x + 10) {
            // Buggy "error" state
            abort();
        }
    }
    // Normal termination
    printf("Ok");
}
```

- **Run 1 (Random Start)**:
  - Input: `(x=354, y=34567)` (random).
  - Path Taken: `(x != y)` is True, `(f(x) == x + 10)` is False. Bug missed.
  - Symbolic PC Collected: `(x0 != y0) AND (2*x0 != x0 + 10)`.

- **Run 2 (Directed Search)**:
  - DART **negates the second clause** of the PC from Run 1.
  - New Goal PC: `(x0 != y0) AND (2*x0 == x0 + 10)`.
  - Solver is called: It solves `2*x0 = x0 + 10` → `x0 = 10`. It might return `(x=10, y=20)` (since `y0` just needs to be different).

- **Run 3 (Directed Search - Bug Found!)**:
  - Input: `(x=10, y=20)` (from solver).
  - Path Taken: `(10 != 20)` is True, `(f(10) == 10 + 10)` i.e., `(20 == 20)` is True.
  - Result: `abort()` is reached! Bug found.

- **Think of it like this**: DART is like a detective. It starts with a random guess, then uses clues from each attempt to systematically explore different scenarios until it finds the bug.

### III. How DART Handles Symbolic Failures

DART uses the concrete run to bypass problems that kill pure symbolic execution:

- **Non-Linear Constraints / Complex Math**:
  - When DART hits a condition it can't solve symbolically (e.g., `x*x > 0`).
  - It **substitutes the concrete value** from the parallel run (e.g., if concrete `x` was 5, it uses `25 > 0`).
  - It sets a flag `all_linear = false` but **continues execution**.

- **Pointers / Unknown Memory**:
  - Handled similarly: substitute the concrete address/value, set a flag `all_locations_definite = false`, and continue.

- **Think of it like this**: When DART encounters math it can't solve, it "cheats" by looking at the concrete answer and moving on, rather than giving up entirely like pure symbolic execution would.

### IV. DART Implementation Details

- **Instrumentation**: DART modifies the program (instruments it) to track execution.

- **Symbolic State**: Tracks variables as expressions involving initial symbolic inputs (e.g., `z` holds `2 * y0`).

- **Path Constraint Collection**:
  - When an `if(e)` is taken (e.g., the `then` branch), DART adds `e` to the current path's PC.
  - Crucially, it also calculates `not(e)` and **pushes it onto a stack `S`** of unexplored paths.

- **Test Driver Loop**: The main loop pops constraints from `S`, solves them to get new inputs, and runs again, ensuring all paths on the stack eventually get explored.

- **Auto-Driver Generation**: DART automatically creates a `main()` function (test driver) that initializes random inputs (using type info from Interface Extraction) and calls the function under test in a loop.

---

**Key Insight**: Concolic testing combines the best of both worlds—concrete execution provides a practical path through the program, while symbolic execution provides the mathematical insight to systematically explore alternative paths. This makes automated test generation feasible for real-world programs.
