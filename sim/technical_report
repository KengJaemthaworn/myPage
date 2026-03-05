*Acknowledge this is AI generated content*

# 🛠️ Technical Report: In Silico Construction of the Geodesic Cellular Landscape

## 1. Rationale and Objective

As a network biologist, looking at static PCA or UMAP plots of Gene Expression Space (GXS) often feels inadequate. These Euclidean projections fail to capture the underlying "gravity" of gene co-expression networks. When cells transition between states, they do not move in straight lines; they navigate a curved manifold dictated by Network Similarity Space (NSS).

This simulator was built to visually and mathematically prove why the discordance between GXS and NSS occurs during transition states. By constructing a 3D physics engine where **Network Interaction = Gravity**, we can observe single cells following geodesic paths (the path of least resistance on a curved surface) rather than Euclidean straight lines.

## 2. Mathematical Architecture of the Manifold

The landscape (manifold) is not a static 3D model. It is a dynamically generated mathematical surface.

### 2.1 The Topography (Gaussian States)

Each biological state (Progenitor, Transition, Differentiated) is modeled as a 2D Gaussian function.

* **X, Y (mapped to X, Z in 3D):** The spatial coordinates of the network state.

* **Potency (Z-axis/Height):** The strength of the interaction network. Positive values create peaks (unstable high-potency states); negative values create valleys (stable differentiated attractors).

* **Spread (Width):** The variance of the network state, determining how wide the "gravity well" reaches.

The height $y$ at any point $(x, z)$ on the manifold is the sum of all $n$ biological states:

$$
y(x, z) = \sum_{i=1}^{n} P_i \cdot e^{-\frac{(x - X_i)^2 + (z - Z_i)^2}{W_i}}
$$


*(Where* $P$ *= Potency,* $X,Z$ *= Coordinates, and* $W$ *= Width/Spread)*

### 2.2 The Developmental Boundary (Edge Mask)

To prevent cells from escaping the network space entirely and drifting into a mathematical void, the manifold is multiplied by a high-degree polynomial edge mask. This mathematically crushes the topology to $0$ at the absolute boundaries, creating a flat "floor" while preserving the steepness of the inner valleys.

$$
Mask = \max\left(0, 1 - \left(\frac{x}{max\_bounds}\right)^{12}\right)
$$

## 3. The Cellular Physics Engine

Cells are treated as point masses subject to a custom kinematics engine. The engine runs at 60 frames per second, calculating the next position of the cell based on three biological vectors:

1. **Network Gravity (Gradient Descent):** The slope of the manifold at the cell's exact position. Cells are pulled downward toward the lowest local Ricci flow (stable state).

2. **Noise Response (Stochasticity):** Biological noise (e.g., transcriptional bursting). Added as a randomized vector to simulate cellular heterogeneity.

3. **Signal Inertia (Friction):** Resistance to state change. This scalar dampens the cell's velocity, preventing infinite acceleration and allowing cells to eventually settle into stable attractor valleys.

## 4. Engine Pseudo-Code (R-Flavored)

While the real-time simulation is rendered in WebGL/JavaScript, the mathematical core translates cleanly to R. Here is the conceptual pseudo-code detailing the physics loop.

```R
# ---------------------------------------------------------
# 1. Define the Environment (The Landscape)
# ---------------------------------------------------------

# A dataframe holding our biological states
landscape_states <- data.frame(
  id = c(1, 2, 3),
  x_pos = c(-10, 10, 0),
  z_pos = c(-10, 10, 0),
  potency = c(8, -8, -8), # + is peak, - is valley
  width = c(20, 30, 30)
)

# Function to calculate manifold height at any given coordinate
get_landscape_height <- function(x, z) {
  y <- 0
  
  # Sum the Gaussian influence of all biological states
  for (i in 1:nrow(landscape_states)) {
    state <- landscape_states[i, ]
    y <- y + (state$potency * exp(-((x - state$x_pos)^2 + (z - state$z_pos)^2) / state$width))
  }
  
  # Apply developmental boundary mask (forces edges to flat 0)
  edge_mask_x <- max(0, 1 - (x / 14.5)^12)
  edge_mask_z <- max(0, 1 - (z / 14.5)^12)
  
  return(y * edge_mask_x * edge_mask_z)
}

# ---------------------------------------------------------
# 2. Define Network Gravity (The Gradient)
# ---------------------------------------------------------

# Calculate the partial derivatives (slope) to determine which way is "down"
get_gradient <- function(x, z) {
  h <- 0.01 # Small step for derivative
  
  dy_dx <- (get_landscape_height(x + h, z) - get_landscape_height(x - h, z)) / (2 * h)
  dy_dz <- (get_landscape_height(x, z + h) - get_landscape_height(x, z - h)) / (2 * h)
  
  return(list(dx = dy_dx, dz = dy_dz))
}

# ---------------------------------------------------------
# 3. The Main Physics Loop (Time Integration)
# ---------------------------------------------------------

simulate_cell_trajectories <- function(cells_df, signal_inertia = 0.96, noise_response = 0.05, steps = 1000) {
  
  # cells_df contains columns: id, x, z, velocity_x, velocity_z
  gravity_strength <- 0.4
  dt <- 0.016 # Time step (~60fps)
  
  for (step in 1:steps) {
    for (i in 1:nrow(cells_df)) {
      
      # 1. Calculate gravity pull based on local slope
      grad <- get_gradient(cells_df$x[i], cells_df$z[i])
      
      cells_df$velocity_x[i] <- cells_df$velocity_x[i] - (grad$dx * gravity_strength)
      cells_df$velocity_z[i] <- cells_df$velocity_z[i] - (grad$dz * gravity_strength)
      
      # 2. Apply Biological Noise (Gaussian/Brownian kick)
      cells_df$velocity_x[i] <- cells_df$velocity_x[i] + rnorm(1, mean=0, sd=noise_response)
      cells_df$velocity_z[i] <- cells_df$velocity_z[i] + rnorm(1, mean=0, sd=noise_response)
      
      # 3. Apply Signal Inertia (Damping factor)
      cells_df$velocity_x[i] <- cells_df$velocity_x[i] * signal_inertia
      cells_df$velocity_z[i] <- cells_df$velocity_z[i] * signal_inertia
      
      # 4. Update Position
      cells_df$x[i] <- cells_df$x[i] + (cells_df$velocity_x[i] * dt * 10)
      cells_df$z[i] <- cells_df$z[i] + (cells_df$velocity_z[i] * dt * 10)
      
      # Record trajectory point here...
    }
  }
  
  return(cells_df) # Returns final positions and trajectories
}
```

## 5. Conclusion

By separating the cellular trajectory from Euclidean space and forcing it onto a Geodesic manifold, we can clearly visualize the "transition state discordance."

When a cell approaches a saddle point (a bifurcation ridge) between two stable networks, the Gradient ($dy/dx$) approaches $0$. At this exact moment, **Network Gravity becomes extremely weak, and Noise Response (stochasticity) takes over**. This mathematical vulnerability is exactly what allows a homogeneous progenitor population to break symmetry and diverge into multiple distinct phenotypic lineages.
