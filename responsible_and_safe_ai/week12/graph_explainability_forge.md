## jargon buster

- **gnn (graph neural network):** a neural network designed to work with data structured as a graph.
- **cell complex:** a higher-order generalization of a graph that can represent multi-node relationships (e.g., faces, volumes) in addition to nodes and edges.
- **p-cell:** an element of a cell complex with dimension "p". a 0-cell is a node, a 1-cell is an edge, a 2-cell is a face (like a cycle), etc.
- **graph lifting:** the process of converting a standard graph into a more expressive cell complex by creating "augmented nodes" for edges (1-cells) and cycles (2-cells).
- **info prop (information propagation):** the algorithm used in forge to transfer importance scores from the cell complex explanation back to the nodes and edges of the original graph.
- **perturbation-based methods:** a type of explainer that works by systematically changing, masking, or removing parts of the input data (i.e., "perturbing" it) and observing how the model's prediction changes. if a change to a certain feature significantly alters the output, that feature is considered important.

---

## the problem with graph explainability

- **pairwise limitation:** traditional graphs can only model relationships between two nodes at a time (pairwise).
- **gnn blindness:** because of this, gnns struggle to understand **higher-order interactions** where a group of nodes acts together (e.g., a **benzene ring** in a molecule is a group of six atoms, but a gnn just sees a simple cycle).
- **unreliable explanations:** this blindness can cause gnn explainers to produce incorrect or incomplete explanations, identifying spurious correlations instead of the true causal structure. this is a major issue in high-risk domains like drug discovery and fraud detection.

---

## forge: using cell complexes for better explanations

- **the solution:** use **cell complexes** to represent graphs. this allows the model to explicitly "see" and reason about higher-order structures like cycles by representing them as their own features (2-cells).
- **scalability issue:** full cell complexes are computationally expensive, scaling **quadratically** with the size of the graph.
    - **reduced complex:** the forge paper proposes a "reduced complex" that strategically removes certain connections to achieve **linear scaling**, making the approach practical for large graphs.
- **the forge framework:** a three-step process to improve any existing gnn explainer.
    1.  **lift** the input graph into a cell complex.
    2.  run a standard **gnn explainer** on the cell complex to get an explanation that includes the importance of higher-order cells (like faces).
    3.  use **info prop** to map the importance scores from the higher-order cells back to the original nodes and edges, creating a final, more accurate graph explanation.
- **key result:** using the **forge** framework significantly improves the accuracy and faithfulness of existing gnn explainers, especially **perturbation-based methods**.