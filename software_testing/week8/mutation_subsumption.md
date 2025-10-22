# Mutation Subsumption

## 1. Mutation vs. Other Criteria

Mutation testing is one of the strongest testing criteria. By carefully selecting operators, we can show that mutation testing **subsumes** (is equal to or stronger than) many other criteria.

**Local vs. Global Requirement**:

- **Graph/Logic Criteria** (like Edge Coverage) are **local**: they just require you to *reach* a program part.

- **Strong Mutation** is **global**: it requires you to *reach*, *infect*, *propagate*, and *reveal* a fault at the final output.

- To compare them fairly, we often use **Weak Mutation** (Reach + Infect) as the benchmark.

## 2. Mutation Subsumption of Graph Coverage

### I. Node Coverage: Subsumed

- **How**: Use the **Bomb operator** on every statement.

- **Why**: To "kill" all these mutants, a test suite must *reach* every statement to trigger the bomb. This is the definition of Node Coverage.

### II. Edge Coverage: Subsumed

- **How**: For a predicate $P$, use operators to create two mutants: `true` and `false`.

- **Why**: To kill the `false` mutant, you must run a test where $P$ would have been `true` (executing the 'true' path). To kill the `true` mutant, you must run a test where $P$ would have been `false` (executing the 'false' path). This satisfies Edge Coverage.

### III. Path Coverage (e.g., Prime Path): Not Subsumed

- **Why**: Mutation operates on individual statements and predicates, not on the complex *sequences* of execution required by path testing.

## 3. Mutation Subsumption of Logic Coverage

### I. Predicate Coverage: Subsumed

This is identical to Edge Coverage.

### II. Clause Coverage: Subsumed

- **How**: For a clause $C$ in a predicate, create two mutants: one replacing $C$ with `true` and one replacing $C$ with `false`.

- **Why**: To kill the `true` mutant, you must run a test where $C$ evaluates to `false`. To kill the `false` mutant, you must run a test where $C$ evaluates to `true`. This satisfies Clause Coverage.

### III. Generalized Active Clause Coverage (GACC): Subsumed

- **Why**: The act of *weakly killing* a clause mutant (as described above) requires both executing the clause (Reach) and having that clause's value change the predicate's outcome (Infect). This is the definition of GACC.

### IV. CACC and RACC: Not Subsumed

- **Why**: CACC and RACC are complex criteria that require specific *pairs* of tests with constraints on minor clauses. Mutation testing applies one operator at a time and cannot be designed to satisfy these paired requirements.

### V. Combinatorial Coverage: Not Subsumed

This is too exhaustive, and no set of operators can guarantee it.

## 4. Mutation Subsumption of Data Flow Coverage

### I. All-Defs Coverage (AD): Subsumed (using Strong Mutation)

- **How**: Use an operator to **delete the definition** (e.g., `x = 10;` becomes deleted).

- **Why**: To *strongly kill* this mutant, a test must:
  1. **Reach** the (missing) definition.
  2. **Infect** the state (because the variable never got its value).
  3. **Propagate** this incorrect value along a def-clear path to a *use* and then to the final output.
  
  This entire process proves a valid def-use path was tested.

### II. All-Uses (AU) Coverage: Not Subsumed

Or at least, not known to be.
