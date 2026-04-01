---
description: Base factory for path orientation methods.
icon: arrow-down-from-arc
---

# Orient Operation

### Overview

This abstract base class defines the common interface and settings for all path orientation operations. Derived classes implement specific orientation algorithms while sharing common axis configuration.

### How It Works

1. **Prepare**: The operation is prepared with path data, allowing it to access neighboring points and path topology.
2. **Compute Orientation**: For each point, the derived operation computes a new transform based on its specific algorithm.
3. **Apply Axes**: The computed direction is mapped to the configured orient axis, with the up axis defining the secondary rotation constraint.

### Common Settings

All orient operations share these base settings:

#### Orient Axis

The local axis that will be aligned with the computed forward direction.

| Option       | Description                      |
| ------------ | -------------------------------- |
| **Forward**  | +X axis (Unreal default forward) |
| **Backward** | -X axis                          |
| **Right**    | +Y axis                          |
| **Left**     | -Y axis                          |
| **Up**       | +Z axis                          |
| **Down**     | -Z axis                          |

#### Up Axis

The local axis that will be constrained toward world up (or a reference direction).

| Option       | Description       |
| ------------ | ----------------- |
| **Forward**  | +X axis           |
| **Backward** | -X axis           |
| **Right**    | +Y axis           |
| **Left**     | -Y axis           |
| **Up**       | +Z axis (default) |
| **Down**     | -Z axis           |

### Available Orient Operations

* **Average** - Averages directions to neighboring points for smooth orientation
* **Look At** - Orients toward a target point, direction, or position

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/Orient/PCGExOrientOperation.h)
