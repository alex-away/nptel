# Regression Testing

## 1. What is Regression Testing?

Regression testing happens **after** software is released and is being maintained. When code is modified (to fix bugs or add features), we need to re-test.

- **Goals**:
  1. Verify the **modification works** correctly.
  2. Check if the change **broke anything else** (introduced *regressions*) in previously tested parts of the code.
  3. Build confidence the change is safe.

- **Approach**: Usually treated as a **black-box** technique.

- **Cost**: Can be very expensive, especially selecting *which* tests to re-run.

- **Think of it like this**: Regression testing is like checking that fixing a squeaky door didn't somehow break the lock, the hinges, or the doorbell. You need to verify both the fix and everything around it.

## 2. The Regression Testing Process

Given Original Program `P`, its Test Suite `T`, and Modified Program `P'`:

1. **(Optional) Identify Obsolete Tests**: If the *requirements* changed, remove tests from `T` that are no longer valid for `P'`.

2. **Select `T'` ⊆ `T`**: Choose a subset of the *original* tests (`T'`) to run on `P'`. This is the **Regression Test Selection Problem**.

3. **Execute `T'`**: Run the selected old tests on `P'` to check for regressions.

4. **(Optional) Identify Need for New Tests**: Determine if parts of `P'` (especially the changed/new code) require *new* tests. This is the **Coverage Identification Problem**.

5. **(Optional) Create `T''`**: Design new test cases (`T''`) for the modifications.

6. **(Optional) Execute `T''`**: Run the new tests on `P'`.

7. **Maintain Test Suite**: Create the final test suite for `P'` (often combining `T'`, `T''`, and maybe parts of `T`) for future use. This is the **Test Suite Maintenance Problem**.

*Steps 3, 6, 7 involve the **Test Suite Execution Problem** (running tests efficiently)*.

## 3. Regression Test Selection Techniques (Step 2)

How do we choose which old tests (`T'`) to re-run?

- **Retest All**: Simplest approach. Re-run the *entire* original test suite `T`.
  - **Pro**: Guaranteed "safe" (won't miss regressions caught by `T`).
  - **Con**: Often too slow and expensive.

- **Random Selection**: Choose a random subset of `T`.
  - **Pro**: Cheap.
  - **Con**: Ineffective; offers no guarantee of finding regressions. Often done just to "check the box".


- **Selection Based on Heuristics**: Choose tests based on experience.
  - Focus on areas with **frequent defects**.
  - Focus on areas **changed recently/frequently**.
  - Focus on **critical features**.

- **Minimization Techniques**: Select the *smallest* subset `T'` that achieves a specific coverage goal (e.g., covers all modified code blocks).
  - **Pro**: Reduces test execution time.
  - **Con**: Computationally hard (often undecidable); finding the true minimum is difficult.

- **Data Flow Techniques**: Select tests from `T` that exercise **def-use pairs** affected by the code change (added, deleted, or modified def-use pairs).

- **Safe Techniques**: These aim to *guarantee* that the selected `T'` includes *all* tests from `T` that *could possibly* detect a fault in `P'` related to the changes.
  - **How**: Often use CFG analysis. Select tests from `T` that executed code which was **deleted** or **modified** in `P'`, or tests that *will execute* code that is **new** in `P'`.
  - **Pro**: High confidence in catching regressions.
  - **Con**: Can be complex to implement; might select more tests than minimization.

**Trade-offs**: The choice depends on balancing the cost of selection/execution vs. the risk of missing regressions. Empirical studies show the technique matters.

## 4. Automation & Tools

Regression testing benefits hugely from automation, especially for executing large test suites.

- **Common Approach**: Record-and-playback tools capture test execution for later re-run.

- **Tools**: Examples include Regression Tester, TimeShiftX, Micro Focus tools, and many general functional automation tools (like Selenium for web apps) can be used.

- **Think of it like this**: Automating regression tests is like having a robot that can repeat the same quality checks thousands of times without getting tired or making mistakes—essential when you're making frequent changes to your software.
