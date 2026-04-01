---
description: Write the number of times a point shares its location with another.
icon: circle
---

# Collocation Count

### Overview

Collocation Count identifies points that occupy the same position (within a tolerance) and writes count metadata to each point. For each point, it records how many other points share its location, and optionally assigns a linear occurrence index to distinguish between collocated points.

### How It Works

1. **Spatial Query**: Builds an octree from the input points for efficient spatial lookups.
2. **Collocation Detection**: For each point, searches for other points within the specified tolerance distance.
3. **Count Assignment**: Writes the number of collocated points (including itself) to the specified attribute.
4. **Optional Indexing**: If enabled, assigns a linear occurrence index (0, 1, 2...) to each point within a collocated group.

### Behavior

```
Input Points:           Output Attributes:

A ● ● B (same pos)      A: NumCollocations=2, LinearOccurrences=0
                        B: NumCollocations=2, LinearOccurrences=1
C ●                     C: NumCollocations=1

D ● ● ● E F (same pos)  D: NumCollocations=3, LinearOccurrences=0
                        E: NumCollocations=3, LinearOccurrences=1
                        F: NumCollocations=3, LinearOccurrences=2
```

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Collocation Num Attribute Name</strong> <code>FName</code></summary>

The name of the attribute to write collocation count to.

Default: `NumCollocations`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Linear Occurrences</strong> <code>bool</code></summary>

When enabled, writes the linear index of occurrence for collocated points. This assigns each point in a collocated group a unique index (0, 1, 2...) based on processing order.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Linear Occurrences Attribute Name</strong> <code>FName</code></summary>

The name of the attribute to write linear occurrence indices to.

Default: `NumLinearOccurences`

📋 _Visible when Write Linear Occurrences is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tolerance</strong> <code>double</code></summary>

Distance tolerance for considering two points as collocated. Points within this distance are treated as sharing the same location.

Default: `DBL_COLLOCATION_TOLERANCE` (engine default)

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits common settings from its base class.

→ See Points Processor Settings for shared point processing options.

### Outputs

| Pin     | Type       | Description                                          |
| ------- | ---------- | ---------------------------------------------------- |
| **Out** | Point Data | Input points with collocation count attributes added |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsSpatial-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsSpatial/Public/Elements/PCGExCollocationCount.h)
