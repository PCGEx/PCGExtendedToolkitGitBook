---
description: >-
  Routes point collections based on their spatial relationship to reference
  bounds.
icon: circle
---

# Spatial Triage

### Overview

This node classifies point collections by testing their bounding boxes against a reference bounds volume. Collections are routed to three outputs based on spatial relationship: fully inside the reference bounds, partially overlapping (touching), or completely outside. The node performs fast axis-aligned bounding box (AABB) intersection tests, making it efficient for partition-based workflows and spatial culling.

### How It Works

1. **Bounds Loading**: Reads reference bounds from the Bounds input pin
2. **Collection Testing**: For each input collection, retrieves its bounding box
3. **Spatial Comparison**: Tests collection bounds against reference bounds using AABB intersection
4. **Classification**: Determines if collection is Inside, Touching, or Outside
5. **Output Routing**: Routes collection to the appropriate output pin

**Usage Notes**

* **Primary Use Case**: Partition-based workflows where you need to determine which data is uniquely owned by a partition (Inside), which crosses partition boundaries (Touching), and which is irrelevant (Outside).
* **Performance**: Very fast box-box intersection tests make this suitable for large-scale spatial filtering.
* **Bounds Input**: The Bounds input expects bounds data (typically from a partition or region definition).
* **No Configuration**: This node has no settings - behavior is purely determined by spatial relationships.

### Behavior

```
Reference Bounds: Min (0, 0, 0), Max (100, 100, 100)

Collection A Bounds: Min (10, 10, 10), Max (50, 50, 50)
  → Fully inside reference bounds
  → Routed to "Inside" output

Collection B Bounds: Min (80, 80, 80), Max (120, 120, 120)
  → Partially overlaps reference bounds
  → Routed to "Touching" output

Collection C Bounds: Min (150, 150, 150), Max (200, 200, 200)
  → Completely outside reference bounds
  → Routed to "Outside" output

Collection D Bounds: Min (-10, -10, -10), Max (10, 10, 10)
  → Partially overlaps reference bounds (corner touching)
  → Routed to "Touching" output
```

**Spatial Relationships:**

```
Inside:
  Collection bounds entirely within reference bounds
  No part extends outside

Touching:
  Collection bounds partially overlap reference bounds
  Some points inside, some outside
  OR boundaries touch exactly

Outside:
  Collection bounds completely outside reference bounds
  No overlap at all
```

**Partition Workflow Example:**

```
Partition Bounds: 1km × 1km tile at (0, 0)

Input Collections:
- Road segment fully in tile → Inside
- Road crossing tile boundary → Touching
- Distant road segment → Outside

Processing:
- Inside: Process uniquely (no neighbors needed)
- Touching: Process with edge handling
- Outside: Skip processing for this partition
```

**AABB Intersection Tests:**

```
Fast box-box comparisons:
- 6 axis tests (min/max X, Y, Z)
- Early exit on first non-overlap
- No per-point testing required
```

Good for: spatial partitioning, bounds culling, partition filtering, tile-based processing, region classification

### Inputs

| Pin        | Type        | Description                             |
| ---------- | ----------- | --------------------------------------- |
| **In**     | Point Data  | Point collections to classify spatially |
| **Bounds** | Bounds Data | Reference bounds to test against        |

### Outputs

| Pin          | Type       | Description                                         |
| ------------ | ---------- | --------------------------------------------------- |
| **Inside**   | Point Data | Collections fully contained within reference bounds |
| **Touching** | Point Data | Collections partially overlapping reference bounds  |
| **Outside**  | Point Data | Collections completely outside reference bounds     |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Elements/Filtering/PCGExSpatialTriage.h)
