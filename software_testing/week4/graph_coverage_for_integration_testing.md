# Applying Graph Coverage to Integration Testing

For integration testing, the main graph model we use is the **Call Graph**, where nodes are modules and edges are calls between them.

## 1. Structural Coverage on Call Graphs

- **Node Coverage**: Means every **module is called** at least once.
- **Edge Coverage**: Means every **call interface is executed** at least once.

## 2. Data Flow on Call Graphs (Coupling Data Flow)

This is crucial for integration testing and focuses on how data is passed between modules.

- **Key Terminology**:
  - **Caller**: The module making the call.
  - **Callee**: The module being called.
  - **Coupling Variables**: Variables that are defined in one module and used in another. This "coupling" can happen through parameters, return values, or shared memory.
  - **Shared Data Coupling**: Occurs when two modules communicate by reading and writing to the same global or shared variable.
  - **Last-Def**: The last statement in a module that **defines** a variable before its value is passed to another module.
  - **First-Use**: The first statement in a module where that **passed-in variable** is actually used.
  - **Coupling DU-Path**: A simple, def-clear path that connects a **Last-Def** in one module to a **First-Use** in another. This is the fundamental requirement for integration data flow testing.
