---
description: >-
  Classic force-directed graph layout algorithm using spring attraction between
  connected vertices and electrostatic repulsion between all vertices.
icon: function
---

# Force Directed

### Overview

This relaxation operation implements the classic force-directed graph layout approach. Connected vertices attract each other like springs (Hooke's law), while all vertices repel each other like charged particles (Coulomb's law). The balance of these forces naturally produces aesthetically pleasing layouts where connected nodes cluster together while maintaining separation.

> This is a [relax-operation.md](relax-operation.md "mention")

### How It Works

1. **Attractive Forces**: For each vertex, calculates spring forces pulling it toward all connected neighbors. Force magnitude is proportional to distance (Hooke's law).
2. **Repulsive Forces**: For each vertex, calculates electrostatic repulsion from ALL other vertices. Force magnitude is inversely proportional to distance squared (Coulomb's law).
3. **Force Integration**: Combines all forces and updates vertex positions.
4. **Iteration**: Repeats until the system reaches equilibrium or iteration limit.

**Usage Notes**

* **Spring Constant**: Higher values make edges more rigid, keeping connected nodes closer together.
* **Electrostatic Constant**: Higher values create stronger repulsion, spreading vertices further apart.
* **Balance**: The ratio between these constants determines the final layout density.
* **Convergence**: May require many iterations for complex graphs to reach stable layouts.

### Behavior

```
Force-Directed Layout:

    Springs pull connected nodes together:
    A----B  (spring force →←)

    Electrostatic repulsion pushes all nodes apart:
    A  ←→  B  ←→  C  (repulsion)

    Final layout balances both forces.
```

### Settings

<details>

<summary><strong>Spring Constant</strong> <code>double</code></summary>

Controls the strength of attractive forces along edges. Higher values make connected vertices pull more strongly toward each other, resulting in tighter clusters around connected components.

Default: `0.1`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Electrostatic Constant</strong> <code>double</code></summary>

Controls the strength of repulsive forces between all vertex pairs. Higher values push vertices further apart, creating more spread-out layouts. The force follows an inverse-square law, so nearby vertices experience much stronger repulsion.

Default: `1000`

⚡ PCG Overridable

</details>

#### Inherited Settings

This operation inherits common relaxation settings from its base class, including iteration count and influence/strength.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Relaxations/PCGExForceDirectedRelax.h)
