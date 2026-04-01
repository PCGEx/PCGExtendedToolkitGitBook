---
description: Configures how cell boundaries grow or shrink during cell-finding operations.
icon: sliders-simple
---

# Cell Growth Details

### Overview

This settings block controls the growth (or shrinkage) of cell boundaries after discovery. Growth expands cells outward by a specified number of edges, while negative values shrink cells inward. The growth value can be a constant or read from an attribute, enabling per-seed or per-hole variation in cell sizing.

### How It Works

1. **Find Cell Boundary**: Initial cell path is discovered
2. **Apply Growth**: Expand or contract the boundary by the specified edge count
3. **Output Result**: Modified cell boundary is used for output

### Behavior

```
Cell Growth Example:

Original Cell (5 edges):
    ●───●
   /     \
  ●       ●
   \     /
    ●───●

Growth = 1 (expand by 1 edge outward):
      ●───●
     /     \
    ●       ●
   /         \
  ●           ●
   \         /
    ●───────●

Growth = -1 (shrink by 1 edge inward):
      ●─●
      │ │
      ●─●
```

### Settings

<details>

<summary><strong>Growth</strong> <code>FPCGExInputShorthandSelectorInteger32Abs</code></summary>

The number of edges to grow (positive) or shrink (negative) the cell boundary. Can be specified as a constant value or read from an integer attribute.

* **Constant**: Use a fixed growth value for all cells
* **Attribute**: Read growth value per-seed/per-hole from an attribute

Default: `0` (no growth)

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExGraphs-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExGraphs/Public/Clusters/Artifacts/PCGExCellDetails.h)
