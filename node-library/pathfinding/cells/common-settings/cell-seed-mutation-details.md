---
description: >-
  Configures how seed points are transformed and annotated based on the cells
  they produced.
icon: sliders-simple
---

# Cell Seed Mutation Details

### Overview

This settings block controls mutations applied to seed points when outputting filtered seeds from cell-finding operations. Each seed can be repositioned to match its cell's geometry, have its transform reset, and receive cell metrics (area, perimeter, compactness) written to point properties. This enables using seeds as representatives of their discovered cells.

### How It Works

1. **Find Cells**: Cell discovery identifies which seed produced which cell
2. **Filter Seeds**: Seeds are filtered based on cell shape (convex/concave)
3. **Relocate Seeds**: Optionally move seeds to cell centroid or other location
4. **Apply Metrics**: Write cell measurements to seed point properties
5. **Output Seeds**: Modified seeds represent their respective cells

### Behavior

```
Seed Mutation Example:

Original Seed → Cell Found → Mutated Seed

Position: (10, 20, 0)     Cell Area: 500     Position: (50, 60, 0) ← Centroid
Bounds: original          Cell Perimeter: 80  Bounds: matches cell
Scale: (2, 2, 2)                              Scale: (1, 1, 1) ← Reset
                                              Density: 500 ← Area written
```

### Settings

<details>

<summary><strong>Aspect Filter</strong> <code>EPCGExCellShapeTypeOutput</code></summary>

Filter which seeds to output based on their cell's convexity.

| Option           | Description                                   |
| ---------------- | --------------------------------------------- |
| **Both**         | Output seeds for convex and concave cells     |
| **Convex Only**  | Only output seeds that produced convex cells  |
| **Concave Only** | Only output seeds that produced concave cells |

Default: `Both`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Location</strong> <code>EPCGExCellSeedLocation</code></summary>

Where to position the seed point relative to its cell.

| Option                 | Description                            |
| ---------------------- | -------------------------------------- |
| **Original**           | Keep seed at its original position     |
| **Centroid**           | Move to cell centroid (center of mass) |
| **Path Bounds Center** | Move to center of cell's bounding box  |
| **First Node**         | Move to first vertex of cell path      |
| **Last Node**          | Move to last vertex of cell path       |

Default: `Centroid`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Match Cell Bounds</strong> <code>bool</code></summary>

When enabled, the seed point's bounds are set to match the cell's bounding box.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Reset Scale</strong> <code>bool</code></summary>

When enabled, resets the seed point's scale to (1, 1, 1).

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Reset Rotation</strong> <code>bool</code></summary>

When enabled, resets the seed point's rotation to identity.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Area To</strong> <code>EPCGExPointPropertyOutput</code></summary>

Write the cell's area to a point property (Density, Steepness, or Color channel).

Default: `None`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Perimeter To</strong> <code>EPCGExPointPropertyOutput</code></summary>

Write the cell's perimeter length to a point property.

Default: `None`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Compactness To</strong> <code>EPCGExPointPropertyOutput</code></summary>

Write the cell's compactness ratio (0-1) to a point property.

Default: `None`

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExGraphs-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExGraphs/Public/Clusters/Artifacts/PCGExCellDetails.h)
