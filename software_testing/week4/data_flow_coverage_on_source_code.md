# Data Flow Coverage on Source Code

While structural coverage tests the "map" of the code (the CFG), **data flow coverage** focuses on the life of the **variables**. The goal is to track a variable from where it gets a value (**Definition** or **Def**) to where that value is read (**Use**).

## 1. Core Concepts

- **DU-Path (Def-Use Path)**: A simple path through the CFG from a variable's **Def** to one of its **Uses**.
- **Def-Clear Path**: This is the most important rule for a valid DU-Path. The path must be "def-clear," meaning the variable is **not redefined** anywhere between the start (Def) and the end (Use).

## 2. The Step-by-Step Process

Applying data flow coverage is a systematic process, best understood through an example like the "Statistics Program" from the lecture.

1. **Augment the CFG with Defs and Uses**: For every node and edge in the Control Flow Graph, identify which variables are defined (`x = 10;`) and which are used (`y = x + 5;`).
2. **List All DU Pairs**: For each variable, create a list of all its `(Def, Use)` pairs.
3. **Find All Unique DU-Paths**: Trace all the simple, def-clear paths that connect each DU-Pair. This collection of unique paths becomes your set of **Test Requirements (TRs)**.
4. **Design Test Cases**: Create inputs to force the program to execute these required paths. This is where you find bugs. For instance, testing the path that skips a loop often requires an empty input (like an array of length 0), which can uncover faults like the `IndexOutOfBoundsException` found in the lecture.

## 3. Data Flow Coverage Criteria

- **All-Defs Coverage (ADC)**: The weakest. Every **Def** must reach at least **one** Use.
- **All-Uses Coverage (AUC)**: Stronger. Every **Def** must reach **all** of its possible Uses.
- **All-DU-Paths Coverage (ADPC)**: The strongest. Every simple, def-clear path from a Def to every one of its Uses must be tested.
