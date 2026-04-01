---
description: >-
  Copies source points to target point locations with transform fitting and
  attribute forwarding.
icon: circle
---

# Copy to Points

### Overview

This node copies source point collections to target point locations, optionally inheriting transforms, scaling to fit target bounds, and applying justification alignment. It supports data matching to control which sources copy to which targets, attribute forwarding from targets to copies, and conversion of target attributes to tags. The node creates one copy of each source collection at each target point location.

### How It Works

1. **Input Loading**: Loads source collections (In pin) and target points (Targets pin)
2. **Data Matching** (optional): Pairs specific sources with specific targets based on matching rules
3. **Copy Generation**: For each target point, creates a copy of the matched source collection
4. **Transform Application**: Applies target transforms to copied points
5. **Scale-to-Fit** (optional): Scales copies to fit target bounds
6. **Justification** (optional): Aligns copies within target bounds
7. **Attribute Forwarding**: Copies specified target attributes to output points
8. **Output Generation**: Produces copied point collections

### Behavior

**Basic Copy:**

```
Source: 5 points (shapes/objects)
Targets: 3 points (locations)

Output:
  Copy 1: 5 points at Target[0] location
  Copy 2: 5 points at Target[1] location
  Copy 3: 5 points at Target[2] location
Total: 15 points (3 copies × 5 points each)
```

**Transform Inheritance:**

```
Target Point:
  Position: (100, 200, 0)
  Rotation: 45° around Z
  Scale: (2, 2, 1)

Source Point at (0, 0, 0):

bInheritScale = true, bInheritRotation = true:
  → Copied to (100, 200, 0), rotated 45°, scaled 2×
```

**Scale to Fit:**

```
Source bounds: 100×100×100 cube
Target bounds: 50×50×50 cube

ScaleToFit enabled, Uniform scaling:
  Source scaled down by 0.5 to fit in target
  Result: 50×50×50 cube at target location
```

**Justification:**

```
Source (small object)
Target bounds (large box)

Justification = Center:
  Source centered in target box

Justification = Bottom:
  Source aligned to bottom of target box

Justification = TopLeft:
  Source aligned to top-left of target box
```

**Data Matching:**

```
Sources: CollectionA, CollectionB, CollectionC
Targets: Point1 (Tag="A"), Point2 (Tag="B")

Match Mode: Tag
  CollectionA → Point1 (matching tag "A")
  CollectionB → Point2 (matching tag "B")
  CollectionC → Unmatched (no target with tag "C")

bSplitUnmatched = true:
  Unmatched sources output separately
```

**Attribute Forwarding:**

```
Target Point has attributes:
  - Type = "Rock"
  - Size = 5.0
  - Color = (255, 0, 0)

TargetsForwarding enabled for "Type" and "Size":
  All copied points get Type="Rock" and Size=5.0
  (Color not forwarded)
```

**Attributes to Tags:**

```
Target Point:
  - Category = "Tree"
  - BiomeID = 3

TargetsAttributesToCopyTags enabled:
  Copied points get tags: "Category:Tree", "BiomeID:3"
```

Good for: instancing, placement, scattering, transform inheritance, distribution

### Settings

<details>

<summary><strong>Data Matching</strong> <code>FPCGExMatchingDetails</code></summary>

Controls which source collections are copied to which target points.

Allows matching sources to targets based on tags, attributes, or indices.

**Mode**: How to match sources to targets

* **Disabled** (default): All sources copied to all targets
* **Tag**: Match by tag presence
* **Attribute**: Match by attribute values
* **Index**: Match by index (source\[i] → target\[i])

**Limit Matches**: Restrict how many targets each source copies to

</details>

<details>

<summary><strong>Transform Details</strong> <code>FPCGExTransformDetails</code></summary>

Configuration for how source transforms are applied at target locations.

Controls scale-to-fit, justification, and transform inheritance behavior.

//→ See TODO FPCGExTransformDetails

</details>

#### Transform Details

<details>

<summary><strong>Scale to Fit</strong> <code>FPCGExScaleToFitDetails</code></summary>

Scales source points to fit within target bounds.

//→ See TODO FPCGExScaleToFitDetails

</details>

<details>

<summary><strong>Justification</strong> <code>FPCGExJustificationDetails</code></summary>

Aligns source points within target bounds.

//→ See TODO FPCGExJustificationDetails

</details>

<details>

<summary><strong>Inherit Scale</strong> <code>bool</code></summary>

Whether copied points inherit target point scale.

* **false** (default): Use source point scales
* **true**: Multiply source scales by target scales

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Inherit Rotation</strong> <code>bool</code></summary>

Whether copied points inherit target point rotation.

* **false** (default): Use source point rotations
* **true**: Apply target rotation to source points

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Ignore Bounds</strong> <code>bool</code></summary>

Whether to ignore bounds when computing transforms.

* **false** (default): Use bounds for scale-to-fit and justification
* **true**: Ignore bounds calculations

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Targets Attributes to Copy Tags</strong> <code>FPCGExAttributeToTagDetails</code></summary>

Converts target point attributes to tags on copied points.

Allows tagging copies with information from their target point.

//→ See TODO FPCGExAttributeToTagDetails

</details>

<details>

<summary><strong>Targets Forwarding</strong> <code>FPCGExForwardDetails</code></summary>

Specifies which target point attributes to forward to copied points.

Copied points receive these attributes from their target point.

**Enabled**: Whether to forward attributes **Filter Mode**: All, Whitelist, or Blacklist **Attribute Names**: Specific attributes to forward/exclude **Preserve PCGEx Data**: Keep PCGEx-specific metadata

</details>

### Data Matching Details

When Data Matching is enabled, you can control which sources copy to which targets:

**Match Modes:**

* **Tag**: Match sources/targets with same tags
* **Attribute**: Match by attribute value comparison
* **Index**: Pair by position (source\[0]→target\[0], source\[1]→target\[1])

**Limit Matches**: Restrict each source to copy to a maximum number of targets.

**Split Unmatched**: Separate unmatched sources to a different output pin.

### Practical Examples

**Scatter Objects:**

```
Sources: Tree models (3 variations)
Targets: 100 scatter points
Result: Trees copied to all 100 locations
```

**Matched Placement:**

```
Sources: RockA (tag:Rock), TreeB (tag:Tree)
Targets: Points with tags Rock/Tree
Data Matching: Tag mode
Result: Rocks only to Rock points, Trees only to Tree points
```

**Fit to Bounds:**

```
Sources: Furniture pieces (various sizes)
Targets: Room volumes with bounds
ScaleToFit: Uniform
Result: Each piece scaled to fit in its target volume
```

### Inputs

| Pin         | Type       | Description                      |
| ----------- | ---------- | -------------------------------- |
| **In**      | Point Data | Source point collections to copy |
| **Targets** | Point Data | Target points to copy sources to |

### Outputs

| Pin                      | Type       | Description                                                       |
| ------------------------ | ---------- | ----------------------------------------------------------------- |
| **Out**                  | Point Data | Copied point collections at target locations                      |
| **Unmatched** (optional) | Point Data | Unmatched sources when Data Matching enabled with Split Unmatched |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Elements/PCGExCopyToPoints.h)
