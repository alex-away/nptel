## modeling software with graphs

- this module focuses on using **graphs** to model software artifacts, which is the first of four major structures we will use for test case design (the others being logic, inputs, and syntax).
- nearly any software artifact can be represented as a graph:
    - **control flow graphs**: model the flow of execution through the code.
    - **data flow graphs**: model how data is created and used.
    - **call graphs**: model the relationships between different functions or methods.
    - **design models**: uml diagrams like state machines or activity diagrams can be treated as graphs.

## key graph concepts for testing

- for testing purposes, we primarily deal with **directed** and **finite** graphs.
- **degree**: the number of edges connected to a node.
    - **in-degree**: number of edges coming *into* a node.
    - **out-degree**: number of edges going *out of* a node.
- **initial and final nodes**:
    - **initial node**: the starting point of execution. a good model has only one to avoid ambiguity.
    - **final node**: a point where execution can end. a model can have multiple final nodes.

## paths and reachability

- a **path** is a sequence of nodes connected by edges. the **length** of a path is the number of edges.
- **test path**: a specific type of path that starts at an **initial node** and ends at a **final node**. each test path represents a potential execution of the software.
- **reachability**: a node or edge is **reachable** if there is a path from an initial node to it.
    - test case design algorithms often use **dfs (depth first search)** and **bfs (breadth first search)** to determine reachability.
- **visiting vs. touring**:
    - a test path **visits** a node or edge if it includes that element.
    - a test path **tours** another path `q` if `q` is a sub-path of the test path.
- **touring with a sidetrip**: a test path tours a requirement `q` with a sidetrip if the sequence of nodes in `q` appears in the test path, but is not contiguous. for example, if the requirement is `[a, b, c]` and the test path is `[start, a, b, x, y, b, c, end]`, the path `[x, y]` is a sidetrip.

## feasible vs. infeasible paths

- this is a critical concept. just because a path exists in the graph doesn't mean it can actually be executed in the software.
- **feasible path**: a test path that corresponds to an actual execution that can be triggered by one or more test cases.
- **infeasible path**: a path in the graph that cannot be executed by any possible input. this often happens due to contradictory conditions in the code (e.g., dead code).
- an **infeasible test requirement** is a testing goal (like covering a specific path) that is impossible to achieve.

## control flow graph examples

- the lecture showed how basic programming structures can be modeled as control flow graphs.

- **example 1: if-else statement**
    - this graph has a decision node with two branches that later merge.

    ```mermaid
    graph TD
    A[start] --> B{if condition}
    B -->|true| C[true block]
    B -->|false| D[else block]
    C --> E[end]
    D --> E
    ```

- **example 2: if statement (no else)**
    - this graph has a branch that skips the "true" block entirely.

    ```mermaid
    graph TD
    A[start] --> B{if condition}
    B -->|true| C[true block]
    B -->|false| D[end]
    C --> D
    ```


- **example 3: switch-case statement**
    - this graph shows a single entry point that splits into multiple, mutually exclusive paths.

    ```mermaid
    graph TD
    A[start] --> B{switch variable}
    B -->|case 'a'| C[case 'a' block]
    B -->|case 'b'| D[case 'b' block]
    B -->|default| E[default block]
    C --> F[end]
    D --> F
    E --> F
    ```

## structural (control flow) coverage criteria

- the goal is to design tests that cover the structure of the control flow graph.
- we will use the following example graph to illustrate the criteria:

    ```mermaid
    graph TD
        A[0] --> B[1]
        B --> C[2]
        C --> D[3]
        C --> E[4]
        D --> F[5]
        E --> F
        F --> B
        B --> G[6]
    ```

- **node coverage (nc)**
    - **definition**: the weakest criterion. requires that every node in the graph is executed at least once. this is equivalent to **statement coverage** in code.
    - **test requirements (tr)**: the set of all nodes in the graph. `tr = {0, 1, 2, 3, 4, 5, 6}`.
    - **example path to satisfy nc**: one test path like `[0, 1, 2, 3, 5, 1, 6]` covers all nodes.

- **edge coverage (ec)**
    - **definition**: requires that every edge in the graph is traversed at least once. this is equivalent to **branch coverage** in code.
    - **test requirements (tr)**: the set of all edges. `tr = {(0,1), (1,2), (2,3), (2,4), (3,5), (4,5), (5,1), (1,6)}`.
    - **example paths to satisfy ec**: we need at least two paths to cover the split at node 2.
        - path 1: `[0, 1, 2, 3, 5, 1, 6]` (covers most edges).
        - path 2: `[0, 1, 2, 4, 5, 1, 6]` (needed to cover edge (2,4)).

- **edge-pair coverage (epc)**
    - **definition**: requires that every pair of adjacent edges (paths of length 2) is traversed.
    - **test requirements (tr)**: the set of all paths of length 2. `tr = {[0,1,2], [1,2,3], [1,2,4], [2,3,5], [2,4,5], [3,5,1], [4,5,1], [5,1,2], [5,1,6]}`.
    - **reasoning**: this is stronger because it tests the interaction between consecutive branches.

- **simple path coverage & prime path coverage**
    - **simple path**: a path that does not contain any repeated nodes, except possibly the first and last.
    - **prime path**: a simple path that is not a sub-path of any other simple path. these represent the fundamental "tours" through a loop.
    - **prime path coverage (ppc)**: requires that every prime path is toured by some test path. this is a practical way to test loops thoroughly without aiming for infinite path coverage.

- **subsumption hierarchy**
    - a criterion `c1` **subsumes** `c2` if satisfying `c1` guarantees that `c2` is also satisfied.
    - the hierarchy for structural criteria is:
        - `edge-pair coverage` → `edge coverage` → `node coverage`.
    - **prime path coverage** also subsumes edge coverage.

## algorithms for generating test paths

- this section covers the process for finding test requirements (tr) and then creating a set of test paths to satisfy them.

- **for node and edge coverage**:
    - **test requirements**: the tr set is simply the set of all nodes or all edges, respectively.
    - **test paths**: a single traversal of the graph using **dfs** or **bfs** starting from the initial node is usually sufficient to generate a test path that covers all reachable nodes and most edges. a few additional paths may be needed to cover any remaining edges.

- **for prime path coverage (ppc)**:
    - this is a two-step process: first find all prime paths (the trs), then find test paths to cover them.

    - **step 1: find all prime paths (test requirements)**
        - this is an iterative algorithm that finds all simple paths in increasing order of length.
        - **initialization (paths of length 0)**: the initial set of simple paths, $p_0$, is the set of all nodes in the graph.
        - **iteration**: to create $p_{k+1}$ from $p_k$:
            - for each path $p$ in $p_k$, try to extend it by one node to every adjacent neighbor.
            - if the new, extended path is still a **simple path** (no repeated nodes), add it to $p_{k+1}$.
            - a path is considered a **prime path** if it cannot be extended further. this happens when:
                1.  the path ends at a node with no outgoing edges.
                2.  extending the path would cause a node to be repeated (i.e., it would no longer be a simple path).
        - this process continues until no new simple paths can be found.

    - **step 2: create test paths to cover prime paths**
        - once the set of all prime paths (tr) is known, we need to create test paths that tour each of them.
        - a **test path** must start at an initial node and end at a final node, but it does *not* have to be a simple path (it can have loops).
        - a common heuristic is:
            1. sort the prime paths in the tr list by length, longest to shortest.
            2. pick the longest uncovered prime path `p` from the list.
            3. create a test path that tours `p` by extending it forward and backward until it starts at an initial node and ends at a final node.
            4. add this new test path to your final set of tests.
            5. mark all prime paths that are covered (toured) by this new test path.
            6. repeat from step 2 until all prime paths are marked as covered.