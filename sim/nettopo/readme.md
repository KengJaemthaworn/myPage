# Graph Topology Simulator - Quick Start Guide

This web application is an interactive sandbox for visually constructing network hypotheses. You can manually draw nodes and edges, and the app will instantly translate your drawing into an adjacency matrix and a runnable R script for rigorous mathematical analysis.

## How to Use the Sandbox

1.  **Stamp Nodes:** Click anywhere on the empty gray canvas to drop a new node.

2.  **Draw Edges:** Click one node to select it, then click a second node to draw an edge between them.

3.  **Move Nodes:** Click and drag any node to reposition it and visually cluster your network.

4.  **Customize Properties:** Use the left control panel to change the style of your next stamp:

    -   **Node Shapes & Colors:** Assign different visual identities to distinct groups.

    -   **Edge Colors (Weights):** Colors automatically map to mathematical correlation weights ranging from `-1`(Dark Red) to `+1` (Dark Blue).

    -   **Arrows (Directionality):** Use arrows to create directed, asymmetrical connections.

5.  **Edit Existing Elements:** Click on any existing node or edge on the canvas to select it, then use the left panel to change its specific properties or delete it.

## Exporting Your Data

Scroll down to the bottom panel to access your mathematical data:

-   **Adjacency Matrix:** View the live, symmetrical or asymmetrical grid of your network weights. You can download this directly as a CSV.

-   **Centrality Metrics:** View a live topological approximation of degree, closeness, betweenness, and eigenvector centrality. *(Note: This uses unweighted distances for browser speed).*

-   **R Code (Rigorous Export):** Copy the auto-generated `igraph` script. Run this in RStudio to apply strict, weighted path calculations and automatically save your matrices, dataframes, and high-resolution transparent PNG figures locally.
