---
description: Heuristics based on direction toward final goal (north star).
icon: circle-dashed
---

# Heuristics : Azimuth

### Overview

This heuristic guides pathfinding by favoring edges that point toward the final goal destination. It acts like a compass, scoring each potential path segment based on how well its direction aligns with the straight-line direction to the goal, encouraging paths that make consistent progress toward the target rather than wandering.

### How It Works

1. **Edge Direction Caching**: Pre-computes and caches the direction vector for every edge in the cluster for fast lookup during pathfinding
2. **Goal Direction Calculation**: For each node being evaluated, calculates the direction vector from that node toward the final goal
3. **Alignment Scoring**: Measures how well aligned each outgoing edge is with the goal direction using the dot product, which measures how parallel two directions are (1.0 = perfectly aligned, 0.0 = perpendicular, -1.0 = opposite)
4. **Curve Application**: The alignment value is passed through the inherited Score Curve to produce the final heuristic weight

### Behavior

```
Current Node → Goal Direction
      ↓
   Edge A: → (aligned with goal)      Score: High
   Edge B: ↗ (partially aligned)      Score: Medium
   Edge C: ← (opposite to goal)       Score: Low

Example:
Goal is East (1, 0, 0)
Edge pointing East (1, 0, 0):     Dot = 1.0  (perfect alignment)
Edge pointing Northeast (0.7, 0.7, 0): Dot = 0.7  (partial alignment)
Edge pointing West (-1, 0, 0):    Dot = -1.0 (opposite direction)
```

This heuristic is **Goal-Dependent**, meaning scores are recalculated for each unique goal position.

### Settings

#### Node-Specific Settings

This heuristic has no additional settings beyond those inherited from the base class.

#### Inherited Settings

This heuristic inherits common settings from its base class.

→ See Heuristics Overview for: Weight Factor, Score Curve, Invert, and other shared heuristic properties.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExHeuristics-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExHeuristics/Public/Heuristics/PCGExHeuristicAzimuth.h)
