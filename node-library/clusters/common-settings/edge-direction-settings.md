---
description: >-
  Configures how the directional orientation of edges is determined within a
  cluster.
icon: sliders-simple
---

# Edge Direction Settings

### Overview

This settings block controls how edges establish their "start" and "end" endpoints. Since edges are inherently bidirectional connections, many operations need a consistent way to define which endpoint comes first. These settings provide multiple methods for establishing edge direction based on vertex indices, attribute values, or custom sorting rules.

### How It Works

1. **Select Method**: Choose how direction is determined (index order, attribute comparison, or sorting)
2. **Apply Choice**: Optionally reverse the determined direction using Direction Choice
3. **Establish Flow**: Each edge now has a consistent start→end orientation for downstream operations

### Behavior

```
Edge between Vtx A and Vtx B:

EndpointsOrder:     Uses the order vertices appear in the edge definition
EndpointsIndices:   Compares point indices (lower → higher or vice versa)
EndpointsSort:      Uses custom sorting rules on endpoint attributes
EdgeDotAttribute:   Compares edge direction against a vector attribute

Direction Choice:
┌─────────────────────────────────┐
│ SmallestToGreatest: A → B       │
│ GreatestToSmallest: B → A       │
└─────────────────────────────────┘
```

### Settings

<details>

<summary><strong>Direction Method</strong> <code>EPCGExEdgeDirectionMethod</code></summary>

Determines how edge direction is calculated.

| Option                 | Description                                                                               |
| ---------------------- | ----------------------------------------------------------------------------------------- |
| **Endpoints Order**    | Uses the order in which endpoints appear in the edge data                                 |
| **Endpoints Indices**  | Compares the point indices of both endpoints                                              |
| **Endpoints Sort**     | Uses custom sorting rules to compare endpoint attributes                                  |
| **Edge Dot Attribute** | Compares edge direction against a vector attribute using dot product (measures alignment) |

Default: `EndpointsOrder`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Dir Source Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Selects the vector attribute to compare against when using Edge Dot Attribute method. The dot product between the edge direction and this attribute determines which endpoint is "first."

📋 _Visible when Direction Method = Edge Dot Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Direction Choice</strong> <code>EPCGExEdgeDirectionChoice</code></summary>

Refines the direction result by choosing whether smaller or greater values come first.

| Option                   | Description                                                      |
| ------------------------ | ---------------------------------------------------------------- |
| **Smallest to Greatest** | The endpoint with the smaller comparison value becomes the start |
| **Greatest to Smallest** | The endpoint with the greater comparison value becomes the start |

Default: `SmallestToGreatest`

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCore-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCore/Public/Clusters/PCGExEdgeDirectionDetails.h)
