---
description: >-
  Configures filtering constraints for cells discovered during pathfinding and
  topology operations.
icon: sliders-simple
---

# Cell Constraints Details

### Overview

This settings block controls which discovered cells are kept or discarded based on various geometric criteria. Cells can be filtered by their shape (convex/concave), size, point count, area, perimeter, segment length, and compactness. This allows fine-tuning of cell discovery to exclude unwanted results like degenerate cells, the outer boundary, or cells that don't meet size requirements.

### How It Works

1. **Discover Cells**: Cell-finding operations identify closed regions in the cluster
2. **Measure Properties**: Calculate each cell's area, perimeter, point count, etc.
3. **Apply Filters**: Discard cells that don't meet the constraint criteria
4. **Output Valid Cells**: Only cells passing all constraints are included in output

### Behavior

```
Cell Filtering Pipeline:

Discovered Cells → Shape Filter → Size Filters → Output
                       ↓              ↓
                  Convex/Concave   Area, Perimeter,
                  classification   Point Count, etc.

Example:
  Cell A: 50 points, Area=1000 ✓
  Cell B: 3 points, Area=10    ✗ (below min area)
  Cell C: Wrapping boundary    ✗ (omit wrapping bounds)
```

### Settings

#### Rotation & Winding

<details>

<summary><strong>Rotation Method</strong> <code>EPCGExCellRotationMethod</code></summary>

Method used to determine cell orientation.

| Option                | Description                                      |
| --------------------- | ------------------------------------------------ |
| **Projection 2D**     | Uses 2D projection plane for winding calculation |
| **Topological Hints** | Uses cluster topology to infer orientation       |
| **Local Tangent 3D**  | Uses 3D tangent-based calculation                |

Default: `Projection2D`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output Winding</strong> <code>EPCGExWinding</code></summary>

The winding order for output cell paths.

| Option                | Description                      |
| --------------------- | -------------------------------- |
| **Clockwise**         | Points ordered clockwise         |
| **Counter Clockwise** | Points ordered counter-clockwise |

Default: `CounterClockwise`

📋 _Visible when used for path output_

⚡ PCG Overridable

</details>

#### Shape Filters

<details>

<summary><strong>Aspect Filter</strong> <code>EPCGExCellShapeTypeOutput</code></summary>

Filter cells by their convexity.

| Option           | Description                   |
| ---------------- | ----------------------------- |
| **Both**         | Keep convex and concave cells |
| **Convex Only**  | Only keep convex cells        |
| **Concave Only** | Only keep concave cells       |

Default: `Both`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Keep Cells With Leaves</strong> <code>bool</code></summary>

When enabled, keeps cells that contain leaf nodes (dead-end vertices). Leaf nodes create spurs within cells.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Omit Wrapping Bounds</strong> <code>bool</code></summary>

When enabled, discards the outer boundary cell that wraps the entire cluster. The classification tolerance controls how this boundary is detected.

Default: `true`

⚡ PCG Overridable

</details>

#### Size Constraints

<details>

<summary><strong>Omit Below/Above Bounds Size</strong> <code>bool</code></summary>

Filter cells by their bounding box size.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Omit Below/Above Point Count</strong> <code>bool</code></summary>

Filter cells by the number of vertices they contain.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Omit Below/Above Area</strong> <code>bool</code></summary>

Filter cells by their calculated 2D area.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Omit Below/Above Perimeter</strong> <code>bool</code></summary>

Filter cells by their total perimeter length.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Omit Below/Above Segment Length</strong> <code>bool</code></summary>

Filter cells based on their edge segment lengths.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Omit Below/Above Compactness</strong> <code>bool</code></summary>

Filter cells by compactness ratio (0-1, where 1 is a perfect circle). Compactness measures how efficiently the area is enclosed relative to the perimeter.

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExGraphs-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExGraphs/Public/Clusters/Artifacts/PCGExCellDetails.h)
