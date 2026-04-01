---
icon: filter
description: 'Filter : Inclusion (Volume) - Tests points against volume data for spatial inclusion checks.'
---

# Filter : Inclusion (Volume)

Creates a filter definition that tests points against volume data.

## Overview

The Volume Inclusion filter checks whether each point is inside, intersects, or is outside a set of volumes. It derives a sphere from each point's bounds and tests it against the volumes' brush geometry. Multiple check modes let you distinguish between full containment, boundary intersection, and combinations of both. An optional penetration threshold adds a minimum overlap requirement before the test passes.

## How It Works

1. **Volume Collection**: On preparation, all input volume actors are cached along with their world-space bounds, and an octree is built for fast spatial lookup.
2. **Sphere Derivation**: For each point, a sphere radius is computed from the point's bounds using the selected radius source (min extent, max extent, average, or diagonal half), plus any extra radius.
3. **Penetration Adjustment**: If a penetration threshold is enabled, the effective radius is reduced — either by a fixed world-space amount (Discrete) or by a ratio of the radius (Relative) — so the sphere must penetrate the volume boundary by that much to count as overlapping.
4. **Volume Test**: The point's position and effective radius are tested against nearby volumes (via octree culling), using `EncompassesPoint` to determine overlap and distance to surface. The selected check type determines what constitutes a pass.

#### Usage Notes

- **Check types explained**:
  - **Is Inside**: The point center must be within a volume (sphere radius is ignored for the "inside" check, but the sphere is still used for octree culling).
  - **Intersects**: The sphere must overlap a volume boundary, but the point center must be *outside* — a "touching the edge" test.
  - **Is Inside or Intersects**: Passes if the sphere overlaps any volume at all.
  - **Is Outside or Intersects**: Passes if the point is outside all volumes, or if it intersects a boundary. Only fails for points fully inside a volume.
- **Penetration threshold**: Useful for avoiding false positives from points barely grazing a volume surface. Discrete mode subtracts world units from the radius; Relative mode reduces it by a 0–1 ratio.
- **Per-point values**: Both Extra Radius and Penetration Threshold support attribute-driven values, so they can vary per point.

## Inputs

| Pin | Type | Description |
|-----|------|-------------|
| **Volumes** | Volume Data | Volume actors to test points against |

## Settings

### Node-Specific Settings

<details>
<summary><strong>Check Type</strong> <code>EPCGExVolumeCheckType</code></summary>

Type of volume check to perform.

| Option | Description |
|--------|-------------|
| **Is Inside** | Point overlaps any volume |
| **Intersects** | Point sphere overlaps the volume boundary but center is outside |
| **Is Inside or Intersects** | Point is inside OR sphere overlaps any volume |
| **Is Outside or Intersects** | Point is outside all volumes OR sphere overlaps any volume boundary |

Default: `Is Inside`

⚡ PCG Overridable

</details>

<details>
<summary><strong>Bounds Source</strong> <code>EPCGExPointBoundsSource</code></summary>

Which bounds to use on input points for deriving the sphere radius.

Default: `ScaledBounds`

⚡ PCG Overridable

</details>

<details>
<summary><strong>Radius Source</strong> <code>EPCGExVolumeRadiusSource</code></summary>

How to derive the sphere radius from the point's bounds box.

| Option | Description |
|--------|-------------|
| **Min Extent** | Smallest axis of the local bounds half-extent |
| **Max Extent** | Largest axis of the local bounds half-extent |
| **Average Extent** | Average of the three half-extent axes |
| **Diagonal Half** | Half the diagonal of the local bounds box |

Default: `Max Extent`

⚡ PCG Overridable

</details>

<details>
<summary><strong>Extra Radius</strong> <code>FPCGExInputShorthandSelectorDouble</code></summary>

Extra radius added to the bounds-derived radius. Can be a constant or driven by an attribute.

Default: `0.0` (attribute name: `ExtraRadius`)

⚡ PCG Overridable

</details>

<details>
<summary><strong>Use Penetration Threshold</strong> <code>bool</code></summary>

If enabled, intersection tests require a minimum penetration depth to pass.

Default: `false`

⚡ PCG Overridable

</details>

<details>
<summary><strong>Penetration Mode</strong> <code>EPCGExVolumePenetrationMode</code></summary>

How penetration is measured.

| Option | Description |
|--------|-------------|
| **Discrete** | Penetration depth in world units must meet threshold |
| **Relative** | Penetration ratio (depth / radius, 0–1) must meet threshold |

Default: `Discrete`

📋 *Visible when Use Penetration Threshold is enabled*

⚡ PCG Overridable

</details>

<details>
<summary><strong>Penetration Threshold</strong> <code>FPCGExInputShorthandSelectorDouble</code></summary>

Minimum penetration to pass. In Discrete mode, this is world units subtracted from the radius. In Relative mode, this is a 0–1 ratio of the radius. Can be a constant or driven by an attribute.

Default: `10.0` (attribute name: `PenetrationThreshold`)

📋 *Visible when Use Penetration Threshold is enabled*

⚡ PCG Overridable

</details>

<details>
<summary><strong>Invert</strong> <code>bool</code></summary>

If enabled, inverts the result of the test — points that fail the volume check will pass instead.

Default: `false`

⚡ PCG Overridable

</details>

### Inherited Settings

This node inherits common settings from its base class.

→ See [Filter Provider Settings](../../_staging-concepts/04-filters.md) for shared filter options including missing data policy.

## Outputs

| Pin | Type | Description |
|-----|------|-------------|
| **Filter** | Filter Factory | Filter definition for use with filter-consuming nodes |

---

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Points/PCGExVolumeFilter.h)

<!-- VERIFICATION REPORT
Node-Specific Properties: 8 documented (from FPCGExVolumeFilterConfig)
Inherited Properties: Referenced to UPCGExFilterProviderSettings
Inputs: Volumes (custom pin)
Outputs: Filter (inherited from provider)
Nested Types: EPCGExVolumeCheckType (documented inline), EPCGExVolumeRadiusSource (documented inline), EPCGExVolumePenetrationMode (documented inline)
-->
