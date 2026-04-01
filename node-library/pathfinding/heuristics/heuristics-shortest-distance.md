---
description: Heuristics based on distance.
icon: circle-dashed
---

# Heuristics : Shortest Distance

### Overview

This heuristic guides pathfinding by favoring nodes and edges that are closer to the final goal. It calculates the straight-line distance from each potential position to the goal and scores them accordingly, creating a "pull" toward the destination. This is the classic distance-based heuristic commonly used in A\* pathfinding algorithms.

### How It Works

1. **Bounds Calculation**: During preparation, calculates the size of the cluster's bounding box to use for normalizing distance values
2. **Distance Measurement**: For each node or edge being evaluated, calculates the straight-line (Euclidean) distance to the goal position
3. **Normalization**: Normalizes the distance using the bounds size, converting absolute distances into relative values (0-1 range where 0 is far and 1 is close)
4. **Curve Application**: The normalized distance is passed through the inherited Score Curve to produce the final heuristic weight

### Behavior

```
Goal Position
     ●
     ↑
     |  Distance: 100 units → Normalized: 0.2 → Score: Low
     ↑
     |  Distance: 50 units  → Normalized: 0.5 → Score: Medium
     ↑
     |  Distance: 10 units  → Normalized: 0.9 → Score: High
     ●
Current Node

Closer nodes receive higher scores (after curve application),
encouraging the pathfinder to move toward the goal.
```

This heuristic is **Goal-Dependent**, meaning scores are recalculated for each unique goal position.

### Settings

#### Node-Specific Settings

This heuristic has no additional settings beyond those inherited from the base class.

#### Inherited Settings

This heuristic inherits common settings from its base class.

→ See Heuristics Overview for: Weight Factor, Score Curve, Invert, and other shared heuristic properties.

**Tip**: Use the Invert setting to favor longer paths or to create "avoidance" behavior where the pathfinder prefers to stay away from the goal initially.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExHeuristics-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExHeuristics/Public/Heuristics/PCGExHeuristicDistance.h)
