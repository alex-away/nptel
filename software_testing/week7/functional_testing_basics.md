# Functional (Black-Box) Testing Basics

## 1. What is Functional Testing?

Functional testing (also called black-box testing) is a testing approach that evaluates software based only on its **inputs and outputs**, without any knowledge of its internal code or structure.

- **The Core Idea**: The software is treated as a "black box" or a function $P$ that transforms an input $X$ into an output $Y$.

- **Think of it like this**: You're testing a calculator. You don't open it up to see the circuits. You just press buttons (inputs) and check if the display shows the right answer (output).

- **Broad Application**: This approach can be used to test any software artifact:
  - Requirements documents
  - Design specifications
  - Final code

- **The Problem**: The total "input domain" (all possible inputs) is almost always too large or infinite to test exhaustively. The goal is to select a small, finite set of test inputs that are effective at finding faults.

## 2. Key Functional Testing Techniques

Here are the main techniques used to intelligently select test inputs from a large domain.

### I. Input Space Partitioning (ISP) / Equivalence Class Partitioning (ECP)

- **The Idea**: Divide the input domain into "partitions" (or "equivalence classes").

- **Assumption**: All inputs within a single partition are processed in the same way, so testing just one value from the partition is as good as testing all of them.

- **Example: Tax System**
  - If tax rates change at $100k, the partitions for income would be:
    - $b_1$: AGI < $1 (Invalid)
    - $b_2$: $1 ≤ AGI ≤ $100k (Bracket 1)
    - $b_3$: AGI > $100k (Bracket 2)
  - A test set would pick one value from each partition (e.g., `-50`, `50000`, `150000`).

- **Why this works**: If the code handles `50000` correctly, it should handle `75000` the same way (they're in the same bracket).

### II. Boundary Value Analysis (BVA)

- **The Idea**: Errors (like off-by-one errors) most often occur at the **boundaries** of partitions.

- **The Process**: For each boundary, select test values:
  1. Exactly **on** the boundary
  2. Just **below** the boundary
  3. Just **above** the boundary

- **Example: Tax System**
  - For the $100k boundary, the test values would be:
    - $99,999.99 (just below)
    - $100,000.00 (on boundary)
    - $100,000.01 (just above)
  - This is done for all identified boundaries.

- **Why this matters**: Most bugs hide at the edges. A programmer might write `if (income > 100000)` when they meant `if (income >= 100000)`.

- **Note on "Ordered Sets"**:
  - The "on, below, above" rule applies to **ranges** (e.g., `1...100`).
  - If the partition is an **ordered set** (e.g., `{Spring, Summer, Fall, Winter}`), there is no "below" or "above". The boundaries are simply the **first** and **last** elements of the set.

### III. Decision Tables

- **The Idea**: A systematic way to test **combinations** of multiple input conditions, especially when their interactions determine the outcome.

- **The Structure**: A table that lists:
  - **Conditions**: The input factors (e.g., "Repayment Amount Mentioned?")
  - **Actions**: The possible outcomes or effects (e.g., "Process Loan Amount")
  - **Rules**: Columns that represent combinations of conditions (Y/N/Don't Care) that trigger specific actions

- **How it works**: Each "Rule" (column) becomes a single test case.

- **Example: Loan Processing**

  | Condition | Rule 1 | Rule 2 | Rule 3 |
  |-----------|--------|--------|--------|
  | Amount Mentioned? | Y | N | Y |
  | Valid Customer? | Y | Y | N |
  | Process Loan | X | - | - |
  | Reject | - | X | X |

  - Each column is a test case.

### IV. Random Testing

- **The Idea**: Select test inputs randomly from the input domain.

- **The Process**: Requires:
  - A random number generator
  - An "oracle" (a way to know the correct output) to verify the results

- **Usefulness**: Can be good for finding unexpected crashes but is often less effective than targeted methods like BVA for finding boundary errors.

- **When to use it**: Good for stress testing or when you don't understand the requirements well enough to partition intelligently.
