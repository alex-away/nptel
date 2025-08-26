## graph representations

- before applying algorithms, a graph must be represented in a data structure.
- **adjacency list**: an array of lists. for each node `v`, there is a list of all nodes adjacent to `v`.
    - this is the most common representation for testing as control flow graphs are usually **sparse** (have relatively few edges).
    - memory size is $o(|v| + |e|)$, where `v` is the set of vertices and `e` is the set of edges.
- **adjacency matrix**: a $|v| \times |v|$ matrix where an entry `(i, j)` is 1 if there is an edge from node `i` to node `j`, and 0 otherwise.
    - this is better for **dense graphs** (many edges).
    - memory size is $o(|v|^2)$.

***

## breadth-first search (bfs)

- **purpose**: a graph search algorithm that explores nodes "layer by layer." it is used to find the **shortest path** (in terms of number of edges) from a source node `s` to all other reachable nodes.
- **how it works**:
    - it uses a **queue** (first-in, first-out) to manage discovered nodes.
    - it uses a three-color system to track node states:
        - **white**: undiscovered.
        - **grey**: discovered and in the queue, but its neighbors have not been explored.
        - **black**: discovered and all of its neighbors have also been discovered.
- **attributes for each node `u`**:
    - `u.color`: stores the color (white, grey, or black).
    - `u.d`: stores the distance (shortest path) from the source node.
    - `u.π`: stores the predecessor (parent) of `u` in the search tree.
- **output**: the algorithm produces a **bfs-tree**, where the `π` attributes of each node point back to its parent, forming the shortest paths from the source.
- **running time**: the algorithm is very efficient, with a running time of $o(|v| + |e|)$.

- **example of bfs progression**:
    - this diagram shows a bfs search starting from node `s`. the numbers inside the nodes represent their distance `d` from `s`. the arrows show the parent `π` pointers that form the bfs-tree.

    ```mermaid
    graph TD
        subgraph BFS Tree
            s((s - 0)) --> r(r - 1)
            s --> w(w - 1)
            r --> v(v - 2)
            w --> t(t - 2)
            w --> x(x - 2)
            t --> u(u - 3)
            t --> y(y - 3)
        end
    ```

## depth-first search (dfs)

- **purpose**: a graph search algorithm that explores as far as possible along each branch before backtracking.
- **how it works**:
    - unlike bfs's queue, dfs uses a **stack** (last-in, first-out), which is typically managed implicitly through **recursion**.
    - it also uses the white, grey, black color scheme to track visited nodes.
- **timestamps**: dfs associates two timestamps with each node `u`:
    - `u.d`: the **discovery time**, when `u` is first found (colored grey).
    - `u.f`: the **finishing time**, when `u`'s entire adjacency list has been explored (colored black).
- **parenthesis theorem**: for any two nodes `u` and `v`, their discovery/finishing time intervals `[u.d, u.f]` and `[v.d, v.f]` are either completely disjoint, or one is nested entirely within the other. this reflects the recursive structure of the search.
- **edge classification**: dfs is powerful because it can classify every edge of a graph into a type, which helps in analyzing the graph's structure.
    - **tree edge**: an edge in the dfs-forest, discovered when exploring a white node.
    - **back edge**: an edge `(u, v)` connecting a node `u` to an ancestor `v`. the presence of a **back edge** indicates that the graph contains a **cycle**.
    - **forward edge**: a non-tree edge connecting a node `u` to a descendant `v`.
    - **cross edge**: an edge that goes between different subtrees in the dfs-forest.
- **example of edge classification**:
    - in this diagram, solid lines are **tree edges**. the dashed lines show the other types based on a dfs traversal starting at `u`, then `w`.

    ```mermaid
    graph TD
    subgraph DFS Forest
        u --> v
        v --> x
        u --> y
        y --> z

        x -.->|Back Edge| v
        u -.->|Forward Edge| z
        w -.->|Cross Edge| z
    end

    linkStyle 0,1,2,3 stroke-width:2px,stroke:black;
    linkStyle 4 stroke:red,stroke-dasharray: 5 5;
    linkStyle 5 stroke:blue,stroke-dasharray: 5 5;
    linkStyle 6 stroke:green,stroke-dasharray: 5 5;
    ```

## application: topological sort

- **purpose**: for a **directed acyclic graph (dag)**, a topological sort is a linear ordering of all its nodes such that for every directed edge `(u, v)`, node `u` comes before node `v` in the ordering.
- **algorithm**:
    1. call `dfs` on the graph to compute the finishing times `u.f` for all nodes `u`.
    2. as each node is finished, insert it onto the front of a linked list.
    3. return the linked list of nodes.
- this is equivalent to sorting the nodes in descending order of their finishing times.