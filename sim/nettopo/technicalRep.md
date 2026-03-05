# Technical Report: Graph Topology Simulator

**Bridging Visual Hypotheses and Rigorous Network Mathematics for Complex Systems**

## 1. The Motivation

In the study of complex, interconnected systems, raw adjacency matrices are often dense and cognitively overwhelming. Staring at a massive grid of correlation values ranging from -1 to 1 makes it nearly impossible to intuitively grasp the underlying topological structure or identify critical bottlenecks within the network.

This tool was built to solve that exact problem. It serves as an interactive "mathematical sandbox" where researchers can manually draw their network hypotheses on a blank canvas, group nodes by spatial intuition, and have the system instantly translate that visual topology back into strict mathematical matrices and runnable R (`igraph`) code. It is designed to bridge the gap between human visual intuition and algorithmic rigor.

## 2. System Architecture (The "No-Build" Approach)

To ensure this tool can be hosted effortlessly on platforms like GitHub Pages or run locally without forcing the user to install Node.js, manage dependencies, or maintain a backend server, the architecture is condensed into a **Single HTML File**:

-   **Core UI:** React.js (loaded via CDN).

-   **Styling:** Tailwind CSS (loaded via CDN for highly flexible, responsive layouts).

-   **Icons:** FontAwesome (bypassing the need for npm-installed SVG components).

-   **Compiler:** Babel (embedded directly in the browser to compile JSX on the fly).

**Architectural Advantage:** There is zero server-side latency. The drag-and-drop interactions and coordinate mapping happen 100% on the client side, allowing for a completely fluid experience when manually positioning nodes to explore structural hypotheses.

## 3. UI/UX Mapping to Network Data

The interface is designed to reduce cognitive load, utilizing visual aesthetics as direct representations of mathematical metadata:

-   **Nodes (Entities):** Shapes and colors are used to categorize distinct sub-groups, clusters, or typologies within the network.

-   **Edges (Relationships):** \* **Styles:** Solid lines (stable connections), dashed lines (transient or weak connections).

    -   **Directionality:** Arrows dictate a Directed Graph (asymmetrical relationships), while lines without arrows dictate an Undirected Graph (symmetrical relationships).

    -   **Weights:** Edge colors are strictly mapped to correlation coefficients (\$w\_{ij} \\in [-1, 1]\$):

        -   `Dark Blue (+1.0)`, `Blue (+0.7)`, `Light Blue (+0.3)`

        -   `White (0.0)`

        -   `Light Red (-0.3)`, `Red (-0.7)`, `Dark Red (-1.0)`

## 4. The Mathematical Engine

The system employs a dual-layer calculation approach to balance real-time UI rendering with publication-grade mathematical rigor.

### Layer 1: JavaScript Topological Approximation (Live UI Engine)

To ensure the Centrality metrics table updates instantaneously without freezing the browser, the JS engine calculates a **Topological Approximation**. It treats the graph as an unweighted topology (where distance \$d = 1\$ for all existing edges) to circumvent the infinite loops and errors that standard shortest-path algorithms encounter when faced with negative weights.

-   **Degree Centrality:** Counts the number of incident edges.

    \$\$C_D(v) = \\text{deg}(v)\$\$

-   **Closeness Centrality:** Calculated using the Floyd-Warshall algorithm for unweighted shortest paths.

    \$\$C(u) = \\frac{n-1}{\\sum\_{v=1}\^{n} d(u,v)}\$\$

-   **Betweenness Centrality:** Measures the number of shortest paths passing through a node.

    \$\$C_B(v) = \\sum\_{s \\neq v \\neq t \\in V} \\frac{\\sigma\_{st}(v)}{\\sigma\_{st}}\$\$

    *(Where* \$\\sigma\_{st}\$ *is the total number of shortest paths from node* \$s\$ *to node* \$t\$*, and* \$\\sigma\_{st}(v)\$ *is the number of those paths that pass through* \$v\$*.)*

-   **Eigenvector Centrality:** Approximated using the Power Iteration Method (20 iterations) to find the principal eigenvector.

    \$\$Ax = \\lambda x\$\$

-   **Clustering Coefficient:** Local interconnectivity of a node's neighborhood.

    \$\$C_i = \\frac{2e_i}{k_i(k_i-1)}\$\$

    *(Where* \$k_i\$ *is the degree of node* \$i\$*, and* \$e_i\$ *is the number of edges between the neighbors of node* \$i\$*.)*

### Layer 2: The R `igraph` Rigorous Backend (Exported Pipeline)

The ultimate source of truth is the auto-generated R script.

**The Negative Weight Mathematical Fix:** Standard algorithms in `igraph` (like `betweenness` and `closeness`) will fail if edge weights are negative. To calculate rigorous path-based metrics using the actual correlation values, the generated R code includes a fundamental transformation. It converts the correlation matrix into a distance matrix:

\$\$d\_{ij} = 1 - w\_{ij}\$\$

```         
# Transform correlation (-1 to 1) into distance (> 0)
dist_weights <- 1 - E(g)$weight
```

**Logic:**

-   If Correlation is \$+1\$ (Perfectly positive) \$\\rightarrow\$ Distance becomes \$0\$ (Identical/Immediate).

-   If Correlation is \$-1\$ (Perfectly negative) \$\\rightarrow\$ Distance becomes \$2\$ (Maximum separation).

This allows the strict mathematical models in R to traverse the graph flawlessly, reflecting the true weighted topology of the network.

## 5. Export Pipeline & Overcoming Browser Security

Exporting visual canvas elements to local files often triggers severe browser security blocks (CORS policies, "tainted canvas" errors). To ensure seamless extraction of the visual models, the export pipeline utilizes:

1.  **High-Resolution Vector Scaling:** The SVG elements are injected with explicit `viewBox` parameters to scale the internal vectors mathematically (e.g., 3x scale) before they are rasterized. This guarantees publication-quality clarity without pixelation.

2.  **Forced Transparency:** Background color properties are explicitly stripped from the cloned DOM nodes prior to canvas rendering, ensuring the resulting PNG has a true transparent alpha channel.

3.  **Data URI & Blob Modals:** Instead of forcing a direct, silent download (which browsers often block), the system converts the canvas to a Data URI and presents it in a secure Modal Pop-up. This allows researchers to visually verify the export and safely save the asset using native browser commands.
