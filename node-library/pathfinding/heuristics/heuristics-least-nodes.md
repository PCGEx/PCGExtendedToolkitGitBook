---
description: Heuristics based on node count.
icon: circle-dashed
---

# Heuristics : Least Nodes

### Overview

This heuristic guides pathfinding by favoring paths that pass through fewer nodes (hops), regardless of the actual distance traveled. Unlike the Shortest Distance heuristic which minimizes spatial distance, this heuristic minimizes the number of stops along the way. Each edge is treated equally, making it ideal for scenarios where traversal cost is per-node rather than per-distance.

### How It Works

1. **Bounds Calculation**: During preparation, calculates the size of the cluster's bounding box (inherited from distance heuristic)
2. **Node Count Estimation**: For each node being evaluated, estimates how many hops would be needed to reach the goal
3. **Equal Edge Treatment**: Unlike distance-based scoring, all edges contribute equally to the score regardless of their length
4. **Score Generation**: Produces scores that favor paths requiring fewer intermediate nodes
5. **Curve Application**: The normalized node count estimation is passed through the inherited Score Curve to produce the final heuristic weight

### Behavior

```
Scenario: Path from A to Goal

Path 1: A → B → C → Goal (3 hops, 300 units)
Path 2: A → D → Goal (2 hops, 400 units)
```

```
Least Nodes Heuristic:

Path 2 scores better (fewer hops) even though it's longer in distance.
```

```
Shortest Distance Heuristic:

Path 1 scores better (shorter distance) even though it has more hops.

Use Case Examples:
- Minimizing waypoints in a navigation system
- Reducing the number of transfers in a route
- Scenarios where each node has a fixed cost (time, resources, etc.)
```

This heuristic is **Fully Static**, meaning scores are pre-computed once per cluster.

### Settings

#### Node-Specific Settings

This heuristic has no additional settings beyond those inherited from the base class.

#### Inherited Settings

This heuristic inherits common settings from its base class.

→ See Heuristics Overview for: Weight Factor, Score Curve, Invert, and other shared heuristic properties.

**Tip**: Combine with the Shortest Distance heuristic using different weights to balance between minimizing hops and minimizing travel distance.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExHeuristics-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExHeuristics/Public/Heuristics/PCGExHeuristicNodeCount.h)
