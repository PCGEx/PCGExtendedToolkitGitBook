---
description: Heuristics based on turn angle between consecutive edges.
icon: circle-dashed
---

# Heuristics : Turn Penalty

### Overview

This heuristic penalizes sharp directional changes by measuring the angle between consecutive edges in a path. It looks at the previous edge's direction and compares it to each potential next edge, scoring them based on how much turning is required. This creates smoother, more natural-looking paths that avoid zigzagging and prefer gentle curves over sharp corners.

### How It Works

1. **Travel History Retrieval**: Accesses the TravelStack to examine the previous edge in the current path
2. **Direction Comparison**: Calculates the angle between the incoming edge direction and each candidate outgoing edge direction
3. **Threshold Application**: Maps the angle against Min/Max thresholds:
   * Angles below MinAngleThreshold receive no penalty (considered straight)
   * Angles above MaxAngleThreshold receive maximum penalty (sharp turns)
   * Angles in between are linearly interpolated
4. **Absolute vs Signed**: Can treat all turns equally (absolute) or distinguish left from right turns (signed)
5. **Fallback Handling**: Uses configured fallback score when there's no previous edge (path start)
6. **Curve Application**: The normalized angle value is passed through the inherited Score Curve to produce the final heuristic weight

### Behavior

```
Previous Edge direction: → (East)

Evaluating outgoing edges from current node:
Edge A: → (0°, straight)      Low penalty
Edge B: ↗ (45°, gentle turn)  Medium penalty
Edge C: ↑ (90°, right angle)  High penalty
Edge D: ← (180°, U-turn)      Maximum penalty
```

```
Example with Thresholds:

MinAngleThreshold: 15°
MaxAngleThreshold: 90°

0° turn:   0.00 (no penalty - below min)
10° turn:  0.00 (no penalty - below min)
30° turn:  0.20 (15° to 90° range, 20% through)
60° turn:  0.60 (60% through the range)
90° turn:  1.00 (maximum penalty)
120° turn: 1.00 (clamped to maximum)
```

With **Absolute Angle** (default):

```
Left 45° turn:  Same penalty as Right 45° turn
All turns judged by magnitude only
```

Without **Absolute Angle**:

```
Left 45° turn:  -0.5 (remapped to 0.25)
Right 45° turn: +0.5 (remapped to 0.75)
Can favor one direction over the other
```

This heuristic is **Travel-Dependent**, meaning it depends on the path taken to reach each node.

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Min Angle Threshold</strong> <code>double</code></summary>

Angle in degrees below which no penalty is applied. Turns smaller than this are considered "straight enough" and receive a score of 0.

Use this to define a tolerance zone for nearly-straight paths. For example, setting this to 15° means paths can deviate up to 15° without penalty.

Default: `0`

Range: `0` to `180` degrees

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Angle Threshold</strong> <code>double</code></summary>

Angle in degrees at which maximum penalty is applied. Turns at or above this angle receive the full penalty score of 1.0, and sharper turns are clamped to this maximum.

For example, setting this to 90° means right-angle turns receive maximum penalty, and U-turns (180°) are treated the same as 90° turns.

Default: `180`

Range: `0` to `180` degrees

⚡ PCG Overridable

</details>

<details>

<summary><strong>Absolute Angle</strong> <code>bool</code></summary>

Determines whether turn direction matters.

* **True** (default): Uses the absolute angle, treating left and right turns equally. A 45° left turn scores the same as a 45° right turn.
* **False**: Distinguishes turn direction. Can be used with an asymmetric Score Curve to favor turns in one direction over the other.

Most use cases benefit from absolute angle (simpler, symmetric behavior).

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Global Score</strong> <code>double</code></summary>

Fallback value used for the global score calculation. Primarily used by pathfinding algorithms (like A\*) for initial node sorting before any edges have been traversed.

Since there's no turn angle at the global level (no previous edge), this provides a default value.

Default: `0`

Range: `0.0` to `1.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Fallback Score</strong> <code>double</code></summary>

Fallback score used when no previous direction exists (i.e., the first edge from the seed node). Since there's no incoming direction to compare against, this value is used instead.

A value of 0.0 means no penalty for the initial direction choice.

Default: `0`

Range: `0.0` to `1.0`

⚡ PCG Overridable

</details>

#### Inherited Settings

This heuristic inherits common settings from its base class.

→ See Heuristics Overview for: Weight Factor, Score Curve, Invert, and other shared heuristic properties.

**Tip**: Combine with Inertia for even smoother paths, or pair with Distance to create paths that both reach goals and avoid sharp corners. Adjust Min/Max thresholds to tune the "smoothness" of resulting paths.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExHeuristics-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExHeuristics/Public/Heuristics/PCGExHeuristicTurnPenalty.h)
