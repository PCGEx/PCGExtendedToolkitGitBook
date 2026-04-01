---
description: Matches based on spatial bounding box overlap.
icon: circle-dashed
---

# Match : Overlap

### Overview

This match rule establishes correspondences between data elements based on whether their axis-aligned bounding boxes (AABBs) overlap in 3D space. It evaluates spatial intersection between candidate and target datasets, optionally expanding bounds, requiring minimum overlap ratios, and supporting transitive matching through overlapping chains. An octree spatial index accelerates overlap queries for efficient large-scale matching.

### How It Works

1. **Bounds Preparation**: Computes or retrieves bounding boxes for all matchable sources
2. **Bounds Expansion**: Optionally expands bounds based on ExpansionMode:
   * **None**: Uses original bounds
   * **Add**: Adds Expansion value to extents
   * **Scale**: Multiplies bounds by Expansion factor
3. **Octree Construction**: Builds spatial octree index for efficient overlap queries
4. **Overlap Test**: For each target-candidate pair, tests if their AABBs intersect
5. **Overlap Ratio**: If enabled, computes overlap volume / smallest box volume
6. **Ratio Threshold**: Checks if overlap ratio meets MinOverlapRatio requirement
7. **Recursive Matching**: If enabled, expands matches transitively (A overlaps B, B overlaps C → A matches C)
8. **Strictness Application**: Applies inherited Strictness setting
9. **Inversion**: Applies inherited Invert flag if enabled

### Behavior

**Basic Overlap Example:**

```
Box A: (0,0,0) to (5,5,5)
Box B: (3,3,3) to (8,8,8)
Overlap: (3,3,3) to (5,5,5) = 2×2×2 = 8 cubic units
→ Match ✓
```

**No Overlap:**

```
Box A: (0,0,0) to (5,5,5)
Box C: (10,10,10) to (15,15,15)
→ No Match ✗
```

**Expansion Modes:**

```
Original Box: (0,0,0) to (10,10,10)

None:  (0,0,0) to (10,10,10)  (unchanged)
Add(2): (-2,-2,-2) to (12,12,12)  (±2 on all sides)
Scale(1.5): (-2.5,-2.5,-2.5) to (12.5,12.5,12.5)  (50% larger)

Overlap Ratio Example:
Box A: 1000 cubic units
Box B: 100 cubic units
Overlap: 50 cubic units
Ratio = 50 / 100 = 0.5 (50% of smaller box)

If MinOverlapRatio = 0.3 → Match ✓
If MinOverlapRatio = 0.8 → No Match ✗
```

**Recursive/Transitive Matching:**

```
A overlaps B (direct match)
B overlaps C (direct match)
→ With bRecursive=true: A, B, C all match each other
→ With bRecursive=false: Only direct overlaps match
```

Good for: spatial queries, proximity matching, region-based association, containment tests, spatial filtering

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Expansion Mode</strong> <code>EPCGExMatchOverlapExpansionMode</code></summary>

Determines how bounding boxes are expanded before overlap testing.

| Option             | Description                                                    |
| ------------------ | -------------------------------------------------------------- |
| **None** (default) | Use original bounds without modification                       |
| **Add**            | Add the Expansion value to the extents (absolute distance)     |
| **Scale**          | Multiply bounds by the Expansion factor (proportional scaling) |

**Add Example**: Box of size 10 with Expansion(2) becomes size 14 (±2 on each side) **Scale Example**: Box of size 10 with Expansion(1.5) becomes size 15 (50% larger)

Default: `None`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Expansion</strong> <code>FPCGExInputShorthandNameVector</code></summary>

The expansion value used when ExpansionMode is Add or Scale. Can be a constant vector or read from an attribute.

* **Add Mode**: Absolute distance to add to each extent (e.g., `(2,2,2)` adds 2 units on all sides)
* **Scale Mode**: Scale factor to multiply bounds (e.g., `(1.5,1.5,1.5)` makes boxes 50% larger)

Supports per-axis expansion for non-uniform scaling.

Default Attribute: `"@Data.Expansion"` Default Value: `(1, 1, 1)`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Use Min Overlap Ratio</strong> <code>bool</code></summary>

When enabled, requires a minimum overlap ratio for a match to succeed. Without this, any amount of overlap (even a single point touching) counts as a match.

* **false** (default): Any overlap counts as a match
* **true**: Overlap must meet the MinOverlapRatio threshold

Useful for filtering out barely-touching matches.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Min Overlap Ratio</strong> <code>FPCGExInputShorthandNameDouble01</code></summary>

Minimum overlap ratio (0-1) required for a match. The ratio is computed as:

**Overlap Ratio = (Overlap Volume) / (Smallest Box Volume)**

Examples:

* **0.1**: At least 10% of the smaller box must overlap
* **0.5** (default): At least 50% of the smaller box must overlap
* **1.0**: The smaller box must be entirely contained in the larger box

Only used when bUseMinOverlapRatio is enabled. Can be a constant or read from an attribute.

Default Attribute: `"@Data.MinOverlapRatio"` Default Value: `0.5`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Recursive</strong> <code>bool</code></summary>

Enable recursive/transitive matching through overlapping chains.

* **false** (default): Only direct overlaps create matches
* **true**: Matches propagate transitively (if A overlaps B and B overlaps C, then A, B, and C all match each other)

Useful for grouping connected regions or finding proximity clusters.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Recursion Depth</strong> <code>int32</code></summary>

Maximum number of hops for recursive matching. Only used when bRecursive is enabled.

* **-1** (default): Unlimited depth (follow all transitive connections)
* **1**: Direct overlaps only (equivalent to bRecursive=false)
* **2**: A→B→C (2 hops)
* **N**: Up to N hops away

Limits how far transitive matching can propagate. Lower values improve performance but may miss distant connections.

Default: `-1` (unlimited)

Range: `-1` to max int

⚡ PCG Overridable

</details>

#### Inherited Settings

This match rule inherits common settings from the base match rule configuration.

→ See Match Rule Factory Provider for: Strictness, Invert

**Performance Note**: This rule uses an octree spatial index for efficient overlap queries. It performs well even with large datasets, as overlap tests are accelerated by spatial partitioning.

**Tip**: Use Expansion Mode = Add with a small constant value to create a "tolerance buffer" for near-miss matches. Use Min Overlap Ratio to filter out barely-touching false positives.

### Outputs

| Pin            | Type       | Description                                     |
| -------------- | ---------- | ----------------------------------------------- |
| **Match Rule** | Match Rule | Match rule factory for spatial overlap matching |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExMatching-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExMatching/Public/Matching/PCGExMatchOverlap.h)
