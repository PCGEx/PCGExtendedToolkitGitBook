---
description: Find the edge that matches the closest provided direction.
icon: circle-dashed
---

# Vtx : Edge Match

### Overview

This sub-node identifies which connected edge best aligns with a specified direction for each vertex. Using dot product comparison, it finds the edge whose direction most closely matches the input direction and outputs edge information as attributes. This enables directional queries on graph structures, finding edges pointing in specific directions relative to each vertex.

> This is a [vtx-property-provider.md](vtx-property-provider.md "mention")

### How It Works

1. **Get Direction**: Read comparison direction from constant or attribute
2. **Transform Direction**: Optionally apply vertex transform to the direction
3. **Compare Edges**: Compute dot product between direction and each edge
4. **Find Best Match**: Select the edge with highest alignment score
5. **Write Output**: Store matching edge data to vertex attributes

**Usage Notes**

* **Direction Transform**: When enabled, the direction is transformed by the vertex's rotation, making "forward" relative to each point's orientation.
* **Dot Product Matching**: The edge with the highest dot product (most aligned) is selected. Use the comparison details to control tolerance and matching criteria.

### Behavior

```
Edge Match Example:

Vertex A with 3 edges:
  → Edge 1: direction (1, 0, 0)   "pointing right"
  → Edge 2: direction (0, 1, 0)   "pointing forward"
  → Edge 3: direction (0, 0, 1)   "pointing up"

Query Direction: (0.9, 0.1, 0)  "mostly right"

Dot Products:
  Edge 1: 0.9 × 1 + 0.1 × 0 = 0.9  ← Best match
  Edge 2: 0.9 × 0 + 0.1 × 1 = 0.1
  Edge 3: 0.0

Result: Edge 1 written as "MatchingEdge"
```

### Settings

#### Direction Configuration

<details>

<summary><strong>Origin</strong> <code>EPCGExAdjacencyDirectionOrigin</code></summary>

How edge directions are computed for comparison.

| Option            | Description                                          |
| ----------------- | ---------------------------------------------------- |
| **From Node**     | Direction points from the vertex toward the neighbor |
| **From Neighbor** | Direction points from the neighbor toward the vertex |

Default: `From Node`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Direction Input</strong> <code>EPCGExInputValueType</code></summary>

Whether the comparison direction comes from a constant or attribute.

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Direction (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Attribute selector for the comparison direction vector.

📋 _Visible when Direction Input ≠ Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

When enabled, flips the direction vector before comparison.

Default: `false`

📋 _Visible when Direction Input ≠ Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Direction</strong> <code>FVector</code></summary>

Constant direction vector for edge matching.

Default: `(1, 0, 0)` (Forward)

📋 _Visible when Direction Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Transform Direction</strong> <code>bool</code></summary>

When enabled, transforms the direction by the vertex's transform, making the direction relative to each point's orientation.

Default: `true`

⚡ PCG Overridable

</details>

#### Comparison Settings

<details>

<summary><strong>Dot Comparison Details</strong> <code>FPCGExDotComparisonDetails</code></summary>

Settings controlling how dot products are compared to determine the best match.

→ See Dot Comparison Details

⚡ PCG Overridable

</details>

#### Output

<details>

<summary><strong>Output</strong> <code>FPCGExEdgeOutputWithIndexSettings</code></summary>

Configuration for the matching edge output attributes. Controls what edge data is written and the attribute name prefix.

Default prefix: `Matching`

⚡ PCG Overridable

</details>

### Outputs

| Pin     | Type         | Description                                              |
| ------- | ------------ | -------------------------------------------------------- |
| **Out** | Vtx Property | Vertex property factory for use with Vtx Properties node |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/Meta/VtxProperties/PCGExVtxPropertyEdgeMatch.h)
