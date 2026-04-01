---
icon: circle
---

# Lloyd Relax 2D

### Overview

Lloyd Relax 2D iteratively moves points toward more uniform spacing using 2D Voronoi-based relaxation. Points are projected onto a plane, then each iteration computes a 2D Voronoi diagram and moves points toward their cell centroids. This creates evenly distributed points on a surface while preserving their position along the projection axis.

### How It Works

1. **Projection**: Projects points onto a 2D plane based on the projection settings.
2. **Voronoi Computation**: Builds a 2D Voronoi diagram from the projected positions.
3. **Centroid Calculation**: For each point, calculates the centroid of its Voronoi cell.
4. **Position Update**: Moves each point toward its cell centroid on the projection plane, scaled by influence.
5. **Iteration**: Repeats the process for the specified number of iterations.

**Usage Notes**

* **Height Preservation**: Points maintain their position along the projection axis (typically Z), only moving within the projected plane.
* **Use Case**: Ideal for distributing points on terrain or other surfaces where you want uniform 2D spacing while respecting elevation.

### Behavior

```
Top-down view (projected to XY):

Initial:                    After Lloyd Relax 2D:

  ●  ●●                        ●     ●
     ●                    →         ●
  ●     ●●                     ●     ●
                                  ●

Points spread uniformly in 2D while keeping original heights
```

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Iterations</strong> <code>int32</code></summary>

Number of relaxation iterations to perform. More iterations produce more uniform spacing, but with diminishing returns after a certain point.

Default: `5`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Influence Details</strong> <code>FPCGExInfluenceDetails</code></summary>

Controls how strongly points move toward their Voronoi cell centroids.

| Property                  | Type                   | Default    | Description                                                                        |
| ------------------------- | ---------------------- | ---------- | ---------------------------------------------------------------------------------- |
| **Influence Input**       | `EPCGExInputValueType` | `Constant` | Whether to use a constant value or read from an attribute                          |
| **Influence**             | `double`               | `1.0`      | How much each point moves toward its centroid (0 = no movement, 1 = full movement) |
| **Progressive Influence** | `bool`                 | `true`     | When enabled, influence increases each iteration for smoother convergence          |

⚡ PCG Overridable

</details>

<details>

<summary><strong>Projection Details</strong> <code>FPCGExGeo2DProjectionDetails</code></summary>

Settings controlling how points are projected to 2D for relaxation.

//→ See TODO FPCGExGeo2DProjectionDetails

</details>

#### Inherited Settings

This node inherits common settings from its base class.

→ See Points Processor Settings for shared point processing options.

### Outputs

| Pin     | Type       | Description                                          |
| ------- | ---------- | ---------------------------------------------------- |
| **Out** | Point Data | Points with more uniform 2D spacing after relaxation |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsSpatial-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsSpatial/Public/Elements/PCGExLloydRelax2D.h)
