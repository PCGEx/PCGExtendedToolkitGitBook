---
description: Find intersection with target input points.
icon: circle
---

# Path × Bounds Intersection

### Overview

This node detects where paths intersect with the oriented bounding boxes (OBB) of target points and inserts new points at those intersection locations. The bounds respect each target point's rotation, making this useful for collision detection with arbitrarily oriented volumes.

Intersection points receive metadata about the cut (entry vs exit, surface normal, which bound was hit) and can inherit attributes from both the path's neighboring points and the intersected bound itself.

### How It Works

1. **Build OBBs**: Oriented bounding boxes are constructed from target points using their bounds, scale, and rotation.
2. **Test Intersections**: Each path segment is tested against relevant bounds for intersection.
3. **Insert Points**: New points are created at exact intersection locations along the path.
4. **Classify Cuts**: Each intersection is classified by type (entry, exit, or edge cases).
5. **Blend & Forward**: Intersection points receive blended attributes from path neighbors and optionally forwarded attributes from the bound.
6. **Tag Paths**: Paths are tagged based on whether they were cut.

### Cut Types

The `CutType` attribute classifies each intersection point:

| Value           | Description                                                    |
| --------------- | -------------------------------------------------------------- |
| **Entry**       | Path enters a bound (normal case - will have a matching Exit). |
| **Exit**        | Path exits a bound (normal case - has a matching Entry).       |
| **EntryNoExit** | Path enters a bound but ends inside (no exit intersection).    |
| **ExitNoEntry** | Path starts inside a bound and exits (no entry intersection).  |

These edge cases are important for handling paths that begin or end within bounds.

**Usage Notes**

* **Oriented Bounds**: Unlike axis-aligned tests, the bounds respect point rotation. A rotated box point creates a rotated intersection volume.
* **Data Matching**: Use data matching to control which target bounds are tested against which paths.
* **Attribute Forwarding**: Attributes from the intersected bound can be forwarded to intersection points, useful for transferring material IDs or zone identifiers.

### Behavior

```
Normal intersection (Entry + Exit):

    ●───────────●───────────●  Path
                 \         /
              ┌───●───────●───┐
              │  Entry   Exit │  Bound
              └───────────────┘

Path ends inside bound (EntryNoExit):

    ●───────────●───────●
                 \
              ┌───●──────┼──┐
              │  Entry   │  │  Path ends inside
              └──────────┼──┘
                         ●

Path starts inside bound (ExitNoEntry):

                ●───────●───────────●
                       /
              ┌──────●───┐
              │     Exit │  Path started inside
              └──────────┘
```

### Inputs

| Pin        | Type   | Description                                            |
| ---------- | ------ | ------------------------------------------------------ |
| **In**     | Points | Path points to test for intersections                  |
| **Bounds** | Points | Target points whose bounds define intersection volumes |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Data Matching</strong> <code>FPCGExMatchingDetails</code></summary>

Controls which target bounds are tested against which paths.

| Property            | Description                                   |
| ------------------- | --------------------------------------------- |
| **Mode**            | Matching mode (Disabled, One-to-One, etc.).   |
| **Split Unmatched** | Whether to output unmatched paths separately. |
| **Limit Matches**   | Limit the number of bounds matched per path.  |

</details>

<details>

<summary><strong>Blending</strong> <code>UPCGExSubPointsBlendInstancedFactory</code></summary>

How intersection point attributes are blended from neighboring path points.

| Option            | Description                                        |
| ----------------- | -------------------------------------------------- |
| **Inherit Start** | Inherit attributes from the segment's start point. |
| **Inherit End**   | Inherit attributes from the segment's end point.   |
| **None**          | No blending, use default values.                   |

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output</strong> <code>FPCGExBoxIntersectionDetails</code></summary>

Settings for intersection point output and attributes.

**Bounds Source** - How target point bounds are computed:

| Option             | Description                                                       |
| ------------------ | ----------------------------------------------------------------- |
| **Scaled Bounds**  | Point bounds with scale applied (default).                        |
| **Density Bounds** | Bounds derived from point density.                                |
| **Bounds**         | Raw point bounds without scale.                                   |
| **Center**         | Point center only (zero volume - useful for point-on-line tests). |

**Output Attributes:**

| Property                    | Description                                                              | Default                   |
| --------------------------- | ------------------------------------------------------------------------ | ------------------------- |
| **Write Is Intersection**   | Boolean marking intersection points.                                     | `true` → `IsIntersection` |
| **Write Cut Type**          | Integer cut type (see Cut Types above).                                  | `true` → `CutType`        |
| **Write Normal**            | Surface normal at intersection.                                          | `false` → `Normal`        |
| **Write Bound Index**       | Which bound was intersected (index into Bounds input).                   | `false` → `BoundIndex`    |
| **Intersection Forwarding** | Forward attributes from the intersected bound to the intersection point. | -                         |

⚡ PCG Overridable

</details>

***

#### Tagging Settings

<details>

<summary><strong>Tag If Has Cuts</strong> <code>bool</code></summary>

Add a tag to paths that have intersection cuts.

Default: `true`

Tag: `HasCuts`

</details>

<details>

<summary><strong>Tag If Uncut</strong> <code>bool</code></summary>

Add a tag to paths that have no intersection cuts.

Default: `false`

Tag: `Uncut`

</details>

#### Inherited Settings

This node inherits path processing settings from its base class.

→ See Path Processor Settings for: Path handling options.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/PCGExBoundsPathIntersection.h)
