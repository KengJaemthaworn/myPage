📖 The In Silico Laboratory: Simulating Cell States

Welcome to the sandbox.

If you've been following my research log, you know I've been wrestling with a massive question: Why does the way we usually look at cells (Gene Expression Space, or GXS) completely disagree with how cells actually interact (Network Similarity Space, or NSS) right when they are transitioning between states?

When cells differentiate, they don't move in a straight Euclidean line. They follow the invisible curves of gene regulatory networks.

To really wrap my head around this discordance, I needed to see it. I needed to build it. So, I built a physics simulation.

🌐 The 3D Geodesic Landscape Simulator

(You can access this in the left menu under Simulators -> Landscape Sim (3D))

This isn't just a pretty animation of balls rolling down a hill. It is a mathematical translation of biology into topology. We are taking the classic Waddington Epigenetic Landscape and applying principles of General Relativity (specifically Ricci flow) to it.

Here is the core concept: In this space, Gene Interaction = Gravity.

When you open the simulator, you aren't just dropping marbles; you are observing single cells navigating a curved interaction manifold.

The Biological Physics Dictionary

To play with the simulator, you need to know how the physics controls map to actual biology. Here is what you are actually tweaking when you move the sliders:

The Topology (Peaks and Valleys): * High Potency (Peaks/Blue): These are unstable Progenitor or Stem Cell states. Cells want to roll away from here.

Low Potency (Valleys/Red): These are stable, differentiated attractor states. The "gravity" of the gene co-expression network pulls cells into these deep pockets.

The Discordance: Watch the grey trajectory lines. Notice how cells have to curve around ridges (saddle points) to get to the valleys? Euclidean space (GXS) draws a straight line through the mountain. Network space (NSS) forces the cell to walk the path.

Signal Inertia (Friction):

Cells don't just teleport to a new state. It takes time and ATP to dismantle old networks and transcribe new ones.

In the sim, this is friction. If Inertia is low, cells react violently and quickly. If Inertia is high, they are sluggish, resisting the pull of the differentiation signal.

Noise Response (Stochastic Initiation):

Biology is messy. Transcriptional bursting means no two cells are exactly alike, even if they start in the exact same spot.

In the sim, this is physical noise (Brownian motion). It's the random "kicks" a cell gets while rolling. This noise is crucial—it is what forces cells standing on a ridge to break symmetry and commit to Lineage A or Lineage B.

The Gaussian Spread (Cellular Heterogeneity):

When you click "Spawn Batch", the cells don't drop from a single pixel. They drop in a Gaussian distribution (a bell curve). Most fall near the center, but a few "rebel" cells fall further away. This simulates the natural heterogeneity of a progenitor pool.

🧪 How to use the Sandbox

Drop some cells: Open the parameter menu and hit Spawn Batch. Watch the trajectories.

Try the Classic Waddington Preset: I built a preset that perfectly mimics a bifurcating lineage. A wide progenitor peak, a dividing ridge in the middle, and two deep differentiated valleys. Watch how the Noise Response forces the population to split.

Break the Universe: While the cells are rolling, try grabbing a "Stable State" slider and changing its Potency (Z-axis) from -10 to +10. You will literally invert the gravity well into a mountain, forcing the cells to scatter. This simulates a massive genetic perturbation (like knocking out a master regulator gene).

Go play with it. Watch the trajectories. It makes the abstract math of Network Biology incredibly, rawly visible.

Note: The "Other Sim" is currently under construction. Stay tuned.
