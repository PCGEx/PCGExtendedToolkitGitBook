---
description: Heuristics based on visited score feedback.
icon: circle-dashed
---

# Heuristics : Feedback

### Overview

This heuristic tracks which nodes and edges have been visited during pathfinding and adjusts their scores accordingly. Each time a path traverses a node or edge, it increments a visit counter that influences future scoring. This enables behaviors like avoiding path overlap, preferring established routes, or creating organic spreading patterns where paths naturally diverge.

{% hint style="warning" %}
\### Prevents Parallelism Because it rely on previously resolved queries, using this heuristics will prevent pathfinding task to run in parallel. Instead, they will each run one after another to ensure new paths can be affected by previous ones.
{% endhint %}

### How It Works

1. **Initialization**: Creates visit counters for all nodes and edges in the cluster, starting at zero
2. **Path Traversal Tracking**: As the pathfinding algorithm explores paths, it records each visited node and edge
3. **Feedback Scoring**: When evaluating a node or edge, retrieves its visit count and uses it to calculate a feedback score
4. **Score Contribution**: Depending on Binary mode:
   * **Binary Mode**: Returns either 0 (unvisited) or 1 (visited), creating sharp avoid/prefer behavior
   * **Accumulative Mode**: Scales the score by visit count × weight factors, allowing gradual preference changes
5. **Curve Application**: The feedback value is passed through the inherited Score Curve to produce the final heuristic weight

### Behavior

**Binary Mode (bBinary = true):**

```
Node/Edge Visit Status → Score
Unvisited              → 0.0
Visited (any count)    → 1.0
```

**Accumulative Mode (bBinary = false):**

```
Node visits: 0, 1, 2, 3...
Feedback: 0.0, 0.33, 0.66, 1.0... (scaled by weight factor)
```

**Global vs Local Feedback:**

```
┌─────────────────────────────────┐
│ Local (default, parallel-safe): │
│ Path A: ───●───●───●            │
│ Path B: ───●───●───●            │
│ Each path has independent       │
│ feedback tracking               │
└─────────────────────────────────┘
* Local only affect plots

┌─────────────────────────────────┐
│ Global (breaks parallelism):    │
│ Path A: ───●───●───●            │
│            └──●───●             │
│ Path B:       ↑                 │
│ Path B sees Path A's feedback   │
└─────────────────────────────────┘
```

This heuristic is in the **Feedback** category, meaning it dynamically updates during pathfinding execution.

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Binary</strong> <code>bool</code></summary>

When enabled, feedback scores are binary (0 or 1) rather than scaling with visit count. An element is either visited or not, with no middle ground.

* **False**: Feedback scales with the number of visits (0.0 → 1.0 based on visit count)
* **True**: Feedback is either 0.0 (never visited) or 1.0 (visited at least once)

Use binary mode for sharp behavioral boundaries (e.g., strict path avoidance). Use accumulative mode for gradual preference changes.

Default: `false`

Range: `0.0` to `1.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Visited Points Weight Factor</strong> <code>double</code></summary>

Controls how much each node visit contributes to the feedback score. Higher values increase the feedback strength for visited nodes, making them more strongly avoided or preferred (depending on curve and invert settings).

A value of 1.0 means each visit adds full weight. Lower values (e.g., 0.5) reduce the impact of node visits on the final score.

Default: `1`

Range: `0.0` to `1.0`

📋 _Visible when Binary = false_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Visited Edges Weight Factor</strong> <code>double</code></summary>

Controls how much each edge traversal contributes to the feedback score. Higher values increase the feedback strength for visited edges, making them more strongly avoided or preferred (depending on curve and invert settings).

A value of 1.0 means each edge use adds full weight. Lower values (e.g., 0.5) reduce the impact of edge traversals on the final score.

Default: `1`

Range: `0.0` to `1.0`

📋 _Visible when Binary = false_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Global Feedback</strong> <code>bool</code></summary>

When enabled, feedback weights persist and accumulate across all path queries within a single pathfinding operation. This means Path B can see and react to nodes/edges visited by Path A.

**IMPORTANT**: Enabling this breaks parallelism and may significantly reduce performance, as paths cannot be computed independently.

* **False** (default): Each path tracks feedback independently (parallel-safe, faster)
* **True**: All paths share the same feedback state (sequential, slower, but creates cross-path awareness)

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Affect All Connected Edges</strong> <code>bool</code></summary>

When enabled, visiting a node applies feedback to all edges connected to that node, not just the specific edge that was traversed. This creates a broader "influence zone" around visited areas.

* **True** (default): Visiting node A marks all edges connected to A
* **False**: Only the specific traversed edge receives feedback

Default: `true`

⚡ PCG Overridable

</details>

#### Inherited Settings

This heuristic inherits common settings from its base class.

→ See Heuristics Overview for: Weight Factor, Score Curve, Invert, and other shared heuristic properties.

**Tip**: Combine with an inverted Score Curve to create path avoidance behavior, or use a standard curve with high weight to encourage path reuse and consolidation.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExHeuristics-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExHeuristics/Public/Heuristics/PCGExHeuristicFeedback.h)
