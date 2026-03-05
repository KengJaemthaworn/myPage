Technical Architecture Report: Discrete Small World Wavefront Simulator
Context: Engineering a browser-based, interactive simulation of the Watts-Strogatz Small World Network model with real-time signal diffusion tracking.
1. Introduction & Core Philosophy
This document outlines the technical implementation of a standalone, dependency-free Small World Network simulator. The objective of this software is to physically visualize the relationship between network topology and information diffusion speed (Broadcast Time).
The system is built entirely in front-end web technologies (HTML5 Canvas, JavaScript) to ensure immediate execution in any modern browser without backend processing.
2. Graph Generation Engine
The simulator utilizes a variant of the classic 1998 Watts-Strogatz algorithm to generate the network structure.
2.1 The Newman-Watts Approach
To avoid the mathematical problem of disconnected components (which causes path length calculations to return infinity), the engine uses the Newman-Watts model.
The Base Lattice (P=0): The system generates N nodes in a circular coordinate layout. Every node is deterministically connected to its K nearest neighbors (a regular ring lattice).
The Stochastic Phase (P > 0): Rather than breaking existing edges, the algorithm iterates through the base lattice and adds random shortcuts between non-adjacent nodes with probability P.
Complexity Check: Edge generation executes in O(N \times K) time. To maintain a smooth 60 FPS UI during slider interactions, the maximum node count is capped at N=200.
2.2 Logarithmic UI Scaling
The phase transition that defines a "Small World" occurs at very low rewiring probabilities (0.001 \le P \le 0.1). A linear slider would hide this behavior. To solve this, the UI slider controls a base-10 exponent (x \in [-4, 0]). The engine maps this to the probability value as P = 10^x. This allows the user to scrub precisely through the critical threshold.
3. Real-Time Topological Metrics
The engine calculates two primary metrics every time the network topology changes.
3.1 Characteristic Path Length (L)
L is the average shortest path between all reachable pairs of vertices in the network.
Algorithm: The engine runs an unweighted Breadth-First Search (BFS) originating from every single node.
Implementation: A queue-based state machine traverses the adjacency list, recording distance depth until all reachable nodes are discovered.
Algorithmic Cost: O(N \times (V + E)).
3.2 Global Clustering Coefficient (C)
C measures network transitivity (the probability that two neighbors of a node are also neighbors of each other).
Algorithm: For every node, the engine examines its adjacency list, checks all possible pairs of its neighbors, and verifies if an edge exists between them in the global adjacency map.
4. Phase Space Normalization (The Min-Max Chart)
To visually prove the Small World phenomenon, the raw values of L and C must be plotted together on a scale of 0 to 1. Because the graph size (N) in this simulation is relatively small, a simple ratio L(P)/L(0) does not visually approach 0.
4.1 The "Ghost Graph" Baseline
Every time the user modifies the structural parameters (N or K), the engine silently calculates two theoretical boundaries:
Maximums (L_{max}, C_{max}): Computed by generating a pure, rigid ring lattice (P=0).
Minimums (L_{min}, C_{min}): Computed by generating a highly rewired graph (P=1).
4.2 The Mathematical Transformation
As the user generates new networks by adjusting P, the live metrics are stretched using Min-Max scaling:
This transformation guarantees that both the Path Length curve and the Clustering curve span the exact same visual height on the UI canvas, perfectly illustrating the gap where L crashes but C remains high.
5. The Physics Engine: Discrete BFS Wavefront
To observe how topology dictates diffusion speed, the simulation injects a "Signal" and tracks the steps required to reach 100% network coverage.
5.1 Decoupling Render from Logic (The Tick Engine)
If signal diffusion is tied directly to the browser's requestAnimationFrame loop (running at ~60fps), the visual is too fast to observe. Furthermore, updating an array concurrently in a standard for loop causes "directional bias" (nodes processed earlier infect later nodes instantly).
Solution: The physics engine is locked to a strict Discrete Time Step (100ms per tick).
5.2 The 3-State Cellular Automaton
To guarantee a pure, expanding wavefront without infinite feedback loops, nodes follow a strict state machine:
State 0 (Susceptible): Node has never received the signal.
State 1 (Wavefront / Firing): Node has just received the signal. It flashes bright red. It identifies all its State 0 neighbors and flags them to become State 1 in the next tick.
State 2 (Recovered / Trail): Immediately after firing, the node drops to a muted color. It will no longer receive or transmit signals.
By forcing nodes into State 2, the signal cannot flow backward. This strict BFS approach guarantees that the "Step Counter" on the UI is an exact, mathematically accurate representation of the network's Maximum Shortest Path (Diameter) from the injection point.
