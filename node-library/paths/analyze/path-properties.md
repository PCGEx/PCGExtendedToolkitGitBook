---
description: One-stop node to compute useful path infos.
icon: circle
---

# Path : Properties

### Overview

This node computes a comprehensive set of path metrics and writes them to attributes. It can output path-level data (length, area, centroid, winding, oriented bounding box) and per-point data (angles, distances, normals, directions). It also analyzes path inclusion relationships to identify outer, inner, and nested paths.

### How It Works

1. **Project to 2D**: Some metrics require 2D analysis, so paths are projected using the configured projection settings.
2. **Compute Path Metrics**: Calculate length, area, perimeter, centroid, winding direction, and bounding box.
3. **Analyze Inclusion**: Determine which paths contain other paths to compute inclusion depth.
4. **Compute Point Data**: For each point, calculate angles, distances, normals, and directions.
5. **Write Attributes**: Output computed values to the configured attribute names.
6. **Tag and Route**: Apply tags and route paths to inclusion-based output pins.

**Usage Notes**

* **@Data Attributes**: Path-level attributes use the `@Data.` prefix by default, storing them in the data domain rather than per-point.
* **Inclusion Analysis**: Outer paths contain no other paths. Inner paths are contained by at least one other. Inclusion depth indicates nesting level.
* **2D Projection**: Area, perimeter, and winding are computed on a 2D projection. Configure the projection plane to match your use case.
* **Compactness**: Measures how close the shape is to a circle (1.0 = perfect circle, lower = more irregular).

### Outputs

| Pin                | Type       | Description                                 |
| ------------------ | ---------- | ------------------------------------------- |
| **Out**            | Points     | All processed paths                         |
| **Outer**          | Points     | Paths not contained by any other path       |
| **Inner**          | Points     | Paths contained by at least one other path  |
| **Odd**            | Points     | Paths at odd inclusion depth                |
| **PathProperties** | Param Data | Attribute set containing path-level metrics |

### Settings

#### Projection

<details>

<summary><strong>Projection Details</strong> <code>FPCGExGeo2DProjectionDetails</code></summary>

Settings for projecting 3D paths to 2D for area, perimeter, and winding calculations.

//→ See TODO FPCGExGeo2DProjectionDetails

</details>

***

#### Path Attributes

These attributes describe the path as a whole.

| Attribute           | Type      | Description                                | Default Name           |
| ------------------- | --------- | ------------------------------------------ | ---------------------- |
| **Path Length**     | `double`  | Total length of the path.                  | `@Data.PathLength`     |
| **Path Direction**  | `FVector` | Averaged direction of all segments.        | `@Data.PathDirection`  |
| **Path Centroid**   | `FVector` | Center of mass of the path points.         | `@Data.PathCentroid`   |
| **Clockwise**       | `bool`    | Winding direction (true = clockwise).      | `@Data.Clockwise`      |
| **Area**            | `double`  | 2D projected area enclosed by the path.    | `@Data.Area`           |
| **Perimeter**       | `double`  | 2D projected perimeter length.             | `@Data.Perimeter`      |
| **Compactness**     | `double`  | Shape regularity (1.0 = circle).           | `@Data.Compactness`    |
| **OBB Center**      | `FVector` | Oriented bounding box center.              | `@Data.OBBCenter`      |
| **OBB Extent**      | `FVector` | Oriented bounding box half-extents.        | `@Data.OBBExtent`      |
| **OBB Orientation** | `FQuat`   | Oriented bounding box rotation.            | `@Data.OBBOrientation` |
| **Inclusion Depth** | `int32`   | Nesting level (0 = outermost).             | `@Data.InclusionDepth` |
| **Num Inside**      | `int32`   | Number of paths contained inside this one. | `@Data.NumInside`      |

<details>

<summary><strong>Write Path Data To Points</strong> <code>bool</code></summary>

Also write path-level attributes to the point domain (legacy behavior). Can have significant memory cost for large datasets.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Packing</strong> <code>EPCGExAttributeSetPackingMode</code></summary>

How the PathProperties attribute set is organized.

| Option        | Description                                |
| ------------- | ------------------------------------------ |
| **Per Input** | Separate attribute set per input path.     |
| **Merged**    | Single merged attribute set for all paths. |

Default: `Merged`

⚡ PCG Overridable

</details>

***

#### Point Attributes

These attributes are computed per point along the path.

| Attribute             | Type      | Description                                             | Default Name      |
| --------------------- | --------- | ------------------------------------------------------- | ----------------- |
| **Dot**               | `double`  | Dot product of prev/next directions (corner sharpness). | `Dot`             |
| **Angle**             | `double`  | Angle at this point (configurable range).               | `Angle`           |
| **Distance To Next**  | `double`  | Distance to the next point.                             | `DistanceToNext`  |
| **Distance To Prev**  | `double`  | Distance to the previous point.                         | `DistanceToPrev`  |
| **Distance To Start** | `double`  | Cumulative distance from path start.                    | `DistanceToStart` |
| **Distance To End**   | `double`  | Remaining distance to path end.                         | `DistanceToEnd`   |
| **Point Time**        | `double`  | Normalized position along path (0-1).                   | `PointTime`       |
| **Point Normal**      | `FVector` | Normal vector at this point.                            | `PointNormal`     |
| **Point Avg Normal**  | `FVector` | Averaged normal from neighbors.                         | `PointAvgNormal`  |
| **Point Binormal**    | `FVector` | Binormal vector at this point.                          | `PointBinormal`   |
| **Direction To Next** | `FVector` | Direction toward next point.                            | `DirectionToNext` |
| **Direction To Prev** | `FVector` | Direction toward previous point.                        | `DirectionToPrev` |

<details>

<summary><strong>Up Vector</strong> <code>FVector</code></summary>

Reference up vector for computing normals and binormals.

Default: `(0, 0, 1)` (World Up)

⚡ PCG Overridable

</details>

***

#### Tagging

<details>

<summary><strong>Tag Concave</strong> / <strong>Tag Convex</strong></summary>

Tag paths based on their convexity. A convex path has no inward-pointing corners.

Default Tags: `Concave`, `Convex`

</details>

<details>

<summary><strong>Tag Outer</strong> / <strong>Tag Inner</strong></summary>

Tag paths based on inclusion. Outer paths are not enclosed by any other; inner paths are enclosed by at least one.

Default Tags: `Outer`, `Inner`

</details>

<details>

<summary><strong>Tag Odd Inclusion Depth</strong></summary>

Tag paths at odd nesting levels (1, 3, 5...). Useful for fill/hole relationships where odd-depth paths represent holes.

Default Tag: `OddDepth`

</details>

<details>

<summary><strong>Use Inclusion Pins</strong> <code>bool</code></summary>

Enable the Outer, Inner, and Odd output pins for routing paths based on inclusion.

Default: `false`

</details>

#### Inherited Settings

This node inherits path processing settings from its base class.

→ See Path Processor Settings for: Path handling options.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/PCGExWritePathProperties.h)
