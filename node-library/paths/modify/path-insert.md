---
description: Insert target points into paths at their nearest location.
icon: circle
---

# Path : Insert

### Overview

This node inserts points from a target collection into existing paths, positioning each target at its closest location on the path. The inserted points become part of the path, potentially splitting edges where insertions occur. Various controls limit insertions by range, count, or spacing, and multiple output attributes can track insertion metadata.

### How It Works

1. **Match Data**: Pair path collections with target collections using matching criteria
2. **Find Insertion Points**: For each target, compute its nearest location on the path
3. **Filter Candidates**: Apply range and limit constraints to determine valid insertions
4. **Insert Points**: Add valid targets into the path at their computed positions
5. **Blend Attributes**: Interpolate point attributes at insertion locations
6. **Write Metadata**: Output insertion flags, alpha values, distances, and directions

**Usage Notes**

* **Exclusive Targets**: When enabled, each target point can only be inserted once across all edges. Without this, a target might be inserted into multiple edges if equally close.
* **Edge Interior**: Restricting to edge interior prevents insertions at path endpoints, keeping the original path bounds intact.
* **Collocation Prevention**: Prevents multiple insertions at nearly the same location, useful when targets cluster together.

### Behavior

```
Path Insert Example:

Original Path: A ───────────── B ───────────── C

Target Points:  x (near AB)    y (near BC)     z (far from path)

With bWithinRange = true, Range = 50:
  z is outside range → not inserted

Result: A ────── x ────── B ────── y ────── C

Insertion Alpha (x): 0.4  (40% along edge AB)
Insertion Alpha (y): 0.3  (30% along edge BC)

Limit Mode Examples:
  Count = 2 per edge:   Max 2 insertions per edge
  Spacing = 100:        Insertions must be at least 100 units apart on edge
```

### Inputs

| Pin         | Type   | Description                           |
| ----------- | ------ | ------------------------------------- |
| **In**      | Points | Path point collections to insert into |
| **Targets** | Points | Target points to insert into paths    |

### Settings

#### Data Matching

<details>

<summary><strong>Data Matching</strong> <code>FPCGExMatchingDetails</code></summary>

Controls how path collections are paired with target collections for insertion operations.

→ See Matching Details

</details>

#### Insertion Behavior

<details>

<summary><strong>Exclusive Targets</strong> <code>bool</code></summary>

When enabled, each target point can only be inserted into one edge. The closest edge claims the target, preventing duplicate insertions when a target is equidistant from multiple edges.

Default: `false`

</details>

<details>

<summary><strong>Snap To Path</strong> <code>bool</code></summary>

When enabled, snaps inserted points to the exact path position. When disabled, inserted points retain their original position while being logically part of the path.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Edge Interior Only</strong> <code>bool</code></summary>

When enabled, only allows insertions along edge interiors. Targets nearest to path endpoints are not inserted.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Allow Path Extension</strong> <code>bool</code></summary>

When enabled, allows insertions that extend the path beyond its current endpoints if targets are nearest to the start or end.

Default: `true`

📋 _Visible when Edge Interior Only = false_

⚡ PCG Overridable

</details>

#### Range Filter

<details>

<summary><strong>Within Range</strong> <code>bool</code></summary>

When enabled, only inserts targets within a specified distance from the path.

Default: `false`

</details>

<details>

<summary><strong>Range</strong> <code>double</code></summary>

Maximum distance from the path for a target to be considered for insertion.

Default: `100`

📋 _Visible when Within Range = true_

⚡ PCG Overridable

</details>

#### Insertion Limits

<details>

<summary><strong>Limit Inserts Per Edge</strong> <code>bool</code></summary>

When enabled, restricts how many targets can be inserted into each edge.

Default: `false`

</details>

<details>

<summary><strong>Limit Mode</strong> <code>EPCGExInsertLimitMode</code></summary>

How to measure insertion limits per edge.

| Option      | Description                                  |
| ----------- | -------------------------------------------- |
| **Count**   | Limit by discrete number of insertions       |
| **Spacing** | Limit by minimum distance between insertions |

Default: `Count`

📋 _Visible when Limit Inserts Per Edge = true_

</details>

<details>

<summary><strong>Insert Limit</strong> <code>double</code></summary>

The limit value. When using Count mode, this is the maximum number of insertions. When using Spacing mode, this is the minimum distance between insertions on the edge.

Default: `5`

📋 _Visible when Limit Inserts Per Edge = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Limit Truncate</strong> <code>EPCGExTruncateMode</code></summary>

How to truncate fractional limit values when using Spacing mode.

Default: `Round`

📋 _Visible when Limit Inserts Per Edge = true and Limit Mode = Spacing_

</details>

<details>

<summary><strong>Prevent Collocation</strong> <code>bool</code></summary>

When enabled, prevents multiple insertions at nearly the same location on an edge.

Default: `false`

</details>

<details>

<summary><strong>Collocation Tolerance</strong> <code>double</code></summary>

Minimum distance between insertion points to avoid collocation.

Default: `1.0`

📋 _Visible when Prevent Collocation = true_

⚡ PCG Overridable

</details>

#### Attribute Blending

<details>

<summary><strong>Blending</strong> <code>UPCGExSubPointsBlendInstancedFactory</code></summary>

Controls how point attributes are interpolated at insertion locations. The blending method determines how attribute values from neighboring path points are combined for the inserted point.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Target Forwarding</strong> <code>FPCGExForwardDetails</code></summary>

Controls which attributes from target points are forwarded to the inserted path points.

→ See Forward Details

⚡ PCG Overridable

</details>

#### Output Attributes

<details>

<summary><strong>Flag Inserted Points</strong> <code>bool</code></summary>

When enabled, writes a boolean attribute to identify which points were inserted.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Inserted Flag Name</strong> <code>FName</code></summary>

Name of the boolean attribute marking inserted points.

Default: `IsInserted`

📋 _Visible when Flag Inserted Points = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Alpha</strong> <code>bool</code></summary>

When enabled, writes the insertion alpha (normalized position along the edge, 0-1) to an attribute.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Alpha Attribute Name</strong> <code>FName</code></summary>

Name of the attribute storing insertion alpha values.

Default: `InsertAlpha`

📋 _Visible when Write Alpha = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Default Alpha</strong> <code>double</code></summary>

Alpha value written to non-inserted points.

Default: `-1`

📋 _Visible when Write Alpha = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Distance</strong> <code>bool</code></summary>

When enabled, writes the distance from the target to its insertion point on the path.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Distance Attribute Name</strong> <code>FName</code></summary>

Name of the attribute storing insertion distances.

Default: `InsertDistance`

📋 _Visible when Write Distance = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Default Distance</strong> <code>double</code></summary>

Distance value written to non-inserted points.

Default: `-1`

📋 _Visible when Write Distance = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Target Index</strong> <code>bool</code></summary>

When enabled, writes the original index of the target point that was inserted.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Target Index Attribute Name</strong> <code>FName</code></summary>

Name of the attribute storing target indices.

Default: `TargetIndex`

📋 _Visible when Write Target Index = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Default Target Index</strong> <code>int32</code></summary>

Target index value written to non-inserted points.

Default: `-1`

📋 _Visible when Write Target Index = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Direction</strong> <code>bool</code></summary>

When enabled, writes the direction vector from the target to its insertion point on the path.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Direction Attribute Name</strong> <code>FName</code></summary>

Name of the attribute storing insertion directions.

Default: `InsertDirection`

📋 _Visible when Write Direction = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert Direction</strong> <code>bool</code></summary>

When enabled, inverts the direction vector to point from the path toward the target instead of target toward path.

Default: `false`

📋 _Visible when Write Direction = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Default Direction</strong> <code>FVector</code></summary>

Direction value written to non-inserted points.

Default: `(0, 0, 0)`

📋 _Visible when Write Direction = true_

⚡ PCG Overridable

</details>

#### Tagging

<details>

<summary><strong>Tag If Has Inserts</strong> <code>bool</code></summary>

When enabled, adds a tag to path collections that received at least one insertion.

Default: `false`

</details>

<details>

<summary><strong>Has Inserts Tag</strong> <code>FString</code></summary>

Tag to apply to paths that have insertions.

Default: `HasInserts`

📋 _Visible when Tag If Has Inserts = true_

</details>

<details>

<summary><strong>Tag If No Inserts</strong> <code>bool</code></summary>

When enabled, adds a tag to path collections that received no insertions.

Default: `false`

</details>

<details>

<summary><strong>No Inserts Tag</strong> <code>FString</code></summary>

Tag to apply to paths with no insertions.

Default: `NoInserts`

📋 _Visible when Tag If No Inserts = true_

</details>

### Outputs

| Pin     | Type   | Description                         |
| ------- | ------ | ----------------------------------- |
| **Out** | Points | Modified paths with inserted points |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/PCGExPathInsert.h)
