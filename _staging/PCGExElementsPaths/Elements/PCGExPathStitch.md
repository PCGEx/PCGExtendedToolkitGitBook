---
icon: route
description: 'Path : Stitch - Stitch paths together by their endpoints.'
---

# Path : Stitch

Stitch paths together by their endpoints.

## Overview

Path : Stitch finds paths whose endpoints are close enough and joins them into longer paths. Endpoints within the tolerance distance are connected or fused, chaining separate path segments into continuous sequences. When multiple stitching candidates exist, sorting rules control which paths are joined first.

## How It Works

1. **Compute Endpoints**: For each path, identifies the start segment (first two points) and end segment (last two points), building bounding volumes around each endpoint
2. **Sort & Prioritize**: Applies optional sorting rules to control the order in which stitching candidates are evaluated
3. **Match Endpoints**: Uses an octree to find endpoint pairs within the tolerance distance. Optionally enforces start-to-end matching and angular alignment.
4. **Build Chains**: Links matched paths into chains. Detects closed loops when a chain connects back to its starting path.
5. **Merge**: Combines chained paths into single output paths, respecting the chosen method (Connect preserves all points, Fuse removes the duplicate endpoint)

#### Usage Notes

- **Closed Loop Detection**: If stitching creates a circular chain, the output path is automatically marked as a closed loop
- **Sorting Impact**: Sorting rules determine which paths get stitched first when multiple candidates are within tolerance. Without sorting, order is arbitrary.
- **Alignment Filter**: When enabled, paths must point in compatible directions at their endpoints to be stitched — prevents stitching paths that meet at sharp angles

## Behavior

```
Connect:  ---A---x x---B---  →  ---A---x---x---B---  (all points preserved)
Fuse:     ---A---x x---B---  →  ---A---x---B---      (one endpoint removed)
```

## Inputs

| Pin | Type | Description |
|-----|------|-------------|
| **In** | Points | Paths to stitch |
| **Sorting Rules** | Factories | Optional sorting rules to prioritize stitching order |

## Settings

### Node-Specific Settings

<details>
<summary><strong>Method</strong> <code>EPCGExStitchMethod</code></summary>

How matching endpoints are joined.

| Option | Description |
|--------|-------------|
| **Connect** | Adds a segment between the two endpoints, preserving all input points |
| **Fuse** | Merges the two endpoints into one, removing the duplicate |

Default: `Connect`

</details>

<details>
<summary><strong>Method (Fuse)</strong> <code>EPCGExStitchFuseMethod</code></summary>

When using Fuse, which endpoint is kept.

| Option | Description |
|--------|-------------|
| **Keep Start** | Keep the start point of the connection |
| **Keep End** | Keep the end point of the connection |

Default: `Keep Start`

📋 *Visible when Method = Fuse*

</details>

<details>
<summary><strong>Operation</strong> <code>EPCGExStitchFuseOperation</code></summary>

When using Fuse, how the kept point's position is determined.

| Option | Description |
|--------|-------------|
| **None** | Keep the chosen point's position as-is |
| **Average** | Position the kept point at the average of both endpoints |
| **Line Intersection** | Position the kept point at the intersection of the two endpoint segments |

Default: `None`

📋 *Visible when Method = Fuse*

</details>

<details>
<summary><strong>Only Match Start and Ends</strong> <code>bool</code></summary>

If enabled, stitching only occurs between one path's end point and another path's start point. Otherwise, any endpoint pair within tolerance can be stitched.

Default: `false`

⚡ PCG Overridable

</details>

<details>
<summary><strong>Requires Alignment</strong> <code>FPCGExStaticDotComparisonDetails</code></summary>

When enabled, paths must be aligned within an angular threshold at their endpoints before stitching. This prevents stitching paths that meet at incompatible angles.

Contains:
- **Domain**: Scalar or Degrees for the comparison value
- **Comparison**: The comparison operator (e.g., Equal or Greater)
- **Unsigned Comparison**: If enabled, ignores direction — measures alignment regardless of which way paths point
- **Scalar / Degrees**: The threshold value

Default: Disabled

⚡ PCG Overridable

📋 *Visible when Requires Alignment is enabled*

</details>

<details>
<summary><strong>Tolerance</strong> <code>double</code></summary>

Maximum distance between endpoints for stitching to occur. Endpoint pairs farther apart than this are not considered.

Default: `10`

⚡ PCG Overridable

</details>

<details>
<summary><strong>Sort Direction</strong> <code>EPCGExSortDirection</code></summary>

Controls the order in which paths are evaluated for stitching when sorting rules are provided.

| Option | Description |
|--------|-------------|
| **Ascending** | Lower sort values are processed first |
| **Descending** | Higher sort values are processed first |

Default: `Ascending`

⚡ PCG Overridable

</details>

<details>
<summary><strong>Carry Over Settings</strong> <code>FPCGExCarryOverDetails</code></summary>

Controls which attributes and tags are carried over when paths are merged during stitching.

Default: Default carry-over behavior

⚡ PCG Overridable

→ See [Carry Over Details](../../node-library/common-settings/data-utils/carry-over-details.md) for details.

</details>

### Inherited Settings

This node inherits common settings from its base class.

→ See [Path Processor Settings](../../node-library/paths/common-settings/path-processor-settings.md) for shared processing options.

## Outputs

| Pin | Type | Description |
|-----|------|-------------|
| **Out** | Points | Stitched paths |

---

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/PCGExPathStitch.h)



<!-- VERIFICATION REPORT
Node-Specific Properties: 8 documented (Method, FuseMethod, MergeOperation, bOnlyMatchStartAndEnds, bDoRequireAlignment, DotComparisonDetails, Tolerance, SortDirection, CarryOverDetails)
Inherited Properties: Referenced to UPCGExPathProcessorSettings
Inputs: In (Points), Sorting Rules (Factories)
Outputs: Out (Points)
Nested Types: EPCGExStitchMethod, EPCGExStitchFuseMethod, EPCGExStitchFuseOperation, FPCGExStaticDotComparisonDetails, EPCGExSortDirection documented inline
-->
