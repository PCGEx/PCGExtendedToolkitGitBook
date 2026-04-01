---
icon: diagram-project
description: 'Cluster to Zone Graph - Create Zone Graph from clusters.'
---

# Cluster to Zone Graph

Create Zone Graph from clusters.

## Overview

Cluster to Zone Graph converts PCGEx cluster data into Unreal's ZoneGraph representation. It decomposes the cluster into intersection polygons (at Vtx with 2+ edges) and road spline shapes (along edge chains), then spawns configured `UZoneShapeComponent` actors. Each shape carries lane profiles, routing types, and tangent settings derived from the cluster topology and per-point attributes.

## How It Works

1. **Decompose Cluster**: Breaks the cluster into node chains (sequences of edges between branching points) and intersection nodes (Vtx with multiple connections)
2. **Orient Roads**: Determines road direction using the selected orientation mode — depth-first traversal, sorting rules, or a global direction vector
3. **Build Polygons**: Creates polygon shapes at intersection nodes. Polygon radius can be fixed, per-point, or auto-computed from connected road lane profiles.
4. **Build Roads**: Creates road spline shapes along each chain. Endpoints are optionally trimmed to the polygon boundary to prevent overlap.
5. **Resolve Overrides**: Reads per-point/per-edge attribute overrides for lane profiles, routing types, point types, intersection tags, and tangent lengths
6. **Spawn Components**: Creates `UZoneShapeComponent` actors for each polygon and road, configured with the resolved settings

#### Usage Notes

- **Main Thread Only**: ZoneGraph component creation must happen on the main thread. This node cannot be parallelized.
- **Break Conditions**: Optional Vtx filters mark additional points as "break" points, forcing polygon creation at those locations even if they have only 2 connections
- **Lane Profile Lookup**: Per-point lane profile overrides use `FName` attribute values that must match registered lane profile names in ZoneGraph project settings
- **Tangent Control**: Road bezier curves are controlled via the tangent length mode — Default lets ZoneGraph handle it, Auto computes from neighbor distances, Catmull-Rom uses standard spline math, Manual reads a constant or attribute value
- **Output Paths**: Optionally outputs the polygon boundaries and road splines as PCG paths for visualization or downstream processing

## Inputs

| Pin | Type | Description |
|-----|------|-------------|
| **Vtx** | Points | Cluster vertices |
| **Edges** | Points | Cluster edge data |
| **Break Conditions** | Factories | Optional Vtx filters to force polygon creation at specific vertices |

## Settings

### Direction

<details>
<summary><strong>Direction Settings</strong> <code>FPCGExEdgeDirectionSettings</code></summary>

Defines the direction in which points are ordered to form final paths. Used when Orientation Mode is set to Sort Direction.

⚡ PCG Overridable

→ See [Edge Direction Settings](../../node-library/clusters/common-settings/edge-direction-settings.md) for details.

</details>

<details>
<summary><strong>Orientation Mode</strong> <code>EPCGExZGOrientationMode</code></summary>

How road orientation is determined. Affects lane profile alignment at intersections.

| Option | Description |
|--------|-------------|
| **Sort Direction** | Uses the Direction Settings sorting rules to determine road orientation |
| **Depth-First** | Uses BFS depth ordering to orient roads from lower to higher depth. Consistent for tree-like topologies |
| **Global Direction** | Orients all roads to flow along a global direction vector |

Default: `Depth-First`

</details>

<details>
<summary><strong>Invert Orientation</strong> <code>bool</code></summary>

Flips all road orientations.

Default: `false`

📋 *Visible when Orientation Mode is not Sort Direction*

</details>

<details>
<summary><strong>Orientation Direction</strong> <code>FVector</code></summary>

Global direction vector used to orient roads. Each road is oriented so its travel direction aligns with this vector.

Default: `(1, 0, 0)` (Forward)

📋 *Visible when Orientation Mode = Global Direction*

</details>

### General

<details>
<summary><strong>Component Tags</strong> <code>FString</code></summary>

Comma-separated tags applied to all spawned ZoneShapeComponent actors.

Default: `"PCGExZoneGraph"`

⚡ PCG Overridable

</details>

### ZoneGraph

<details>
<summary><strong>Polygon Radius</strong> <code>double</code></summary>

Base radius for intersection polygon shapes.

Default: `100`

</details>

<details>
<summary><strong>Polygon Radius (Attr)</strong> <code>FName</code></summary>

Per-point polygon radius override attribute.

Default: `"ZG.PolygonRadius"`

⚡ PCG Overridable

📋 *Visible when Override Polygon Radius is enabled*

</details>

<details>
<summary><strong>Auto Radius Mode</strong> <code>EPCGExZGAutoRadiusMode</code></summary>

Auto-computes polygon radius from connected road lane profiles.

| Option | Description |
|--------|-------------|
| **Disabled** | Use user radius only |
| **Widest Lane** | Radius = widest single lane across connected roads |
| **Half Profile** | Radius = max(total profile width) / 2 across connected roads |
| **Widest Lane (Min)** | Use the larger of user radius and widest lane |
| **Half Profile (Min)** | Use the larger of user radius and half profile width |

Default: `Disabled`

</details>

<details>
<summary><strong>Trim Road Endpoints</strong> <code>bool</code></summary>

Trims road shape points inside the polygon boundary so roads start and end precisely at the polygon edge. When disabled, road endpoints are offset by the polygon radius along the road direction.

Default: `true`

</details>

<details>
<summary><strong>Endpoint Trim Buffer</strong> <code>double</code></summary>

After trimming, removes road points closer than this distance to the polygon boundary. Prevents auto-bezier artifacts from near-coincident points at the trim boundary.

Default: `0`

⚡ PCG Overridable

📋 *Visible when Trim Road Endpoints is enabled*

</details>

<details>
<summary><strong>Polygon Routing Type</strong> <code>EZoneShapePolygonRoutingType</code></summary>

Default routing type for polygon shapes.

Default: `Arcs`

</details>

<details>
<summary><strong>Polygon Point Type</strong> <code>FZoneShapePointType</code></summary>

Default shape point type for polygon shapes.

Default: `LaneProfile`

</details>

<details>
<summary><strong>Road Point Type</strong> <code>FZoneShapePointType</code></summary>

Default shape point type for road shapes.

Default: `AutoBezier`

</details>

<details>
<summary><strong>Tangent Length Mode</strong> <code>EPCGExZGTangentLengthMode</code></summary>

How road tangent lengths are determined. Controls bezier curve tightness.

| Option | Description |
|--------|-------------|
| **Default** | ZoneGraph handles tangent length automatically |
| **Manual** | Uses a constant or per-point attribute value |
| **Auto** | Average distance to chain neighbors, divided by 3 |
| **Catmull-Rom** | \|P_next - P_prev\| * 0.5 — standard Catmull-Rom tangent length |

Default: `Default`

📋 *Visible when Road Point Type supports custom tangent lengths (Bezier/AutoBezier)*

</details>

<details>
<summary><strong>Lane Profile</strong> <code>FZoneLaneProfileRef</code></summary>

Default lane profile applied to all shapes. Initialized from the first profile in ZoneGraph project settings.

</details>

<details>
<summary><strong>Additional Intersection Tags</strong> <code>FZoneGraphTagMask</code></summary>

Additional ZoneGraph tags applied to intersection polygon shapes.

Default: `None`

</details>

### Output

<details>
<summary><strong>Output Polygon Paths</strong> <code>bool</code></summary>

Output polygon shapes as closed PCG paths.

Default: `false`

</details>

<details>
<summary><strong>Output Road Paths</strong> <code>bool</code></summary>

Output road splines as PCG paths with tangent attributes.

Default: `false`

</details>

<details>
<summary><strong>Arrive Name / Leave Name</strong> <code>FName</code></summary>

Attribute names for arrive and leave tangent vectors written to road path output.

Defaults: `"ArriveTangent"` / `"LeaveTangent"`

⚡ PCG Overridable

📋 *Visible when Output Road Paths is enabled*

</details>

### Advanced

<details>
<summary><strong>Post Process Function Names</strong> <code>TArray&lt;FName&gt;</code></summary>

List of functions to call on the target actor after zone shape creation. Functions must be parameter-less with the `CallInEditor` flag enabled.

Default: Empty

</details>

<details>
<summary><strong>Attachment Rules</strong> <code>FPCGExAttachmentRules</code></summary>

Controls how spawned zone shape actors are attached to the target actor.

→ See [Attachment Rules](../../node-library/common-settings/transform-details/attachment-rules.md) for details.

</details>

### Inherited Settings

This node inherits common settings from its base class.

→ See [Cluster Processor Settings](../../node-library/clusters/common-settings/cluster-processor-settings.md) for shared cluster processing options.

## Outputs

| Pin | Type | Description |
|-----|------|-------------|
| **Vtx** | Points | Pass-through cluster vertices |
| **Edges** | Points | Pass-through cluster edges |
| **Polygon Paths** | Points | Polygon boundaries as closed paths (when enabled) |
| **Road Paths** | Points | Road splines as paths with tangent attributes (when enabled) |

---

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsZoneGraph-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsZoneGraph/Public/Graph/PCGExClusterToZoneGraph.h)



<!-- VERIFICATION REPORT
Node-Specific Properties: 20+ documented (DirectionSettings, OrientationMode, bInvertOrientation, OrientationDirection, CommaSeparatedComponentTags, PolygonRadius, AutoRadiusMode, bTrimRoadEndpoints, EndpointTrimBuffer, PolygonRoutingType, PolygonPointType, RoadPointType, RoadTangentLengthMode, TangentLength, TangentLengthScale, LaneProfile, AdditionalIntersectionTags, bOutputPolygonPaths, bOutputRoadPaths, ArriveName, LeaveName, PostProcessFunctionNames, AttachmentRules, plus override toggles and attribute names)
Inherited Properties: Referenced to UPCGExClustersProcessorSettings
Inputs: Vtx (Points), Edges (Points), Break Conditions (Factories)
Outputs: Vtx (Points), Edges (Points), Polygon Paths (Points), Road Paths (Points)
Nested Types: EPCGExZGOrientationMode, EPCGExZGAutoRadiusMode, EPCGExZGTangentLengthMode documented inline
-->
