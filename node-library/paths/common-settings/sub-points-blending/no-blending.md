---
description: Skip attribute blending for inserted sub-points.
icon: function
---

# No Blending

### Overview

This blending option preserves original point data without any interpolation or mixing. When sub-points are inserted between two existing points, they retain their initial attribute values rather than receiving blended values from their neighbors.

> This is a [sub-point-blending-operation.md](sub-point-blending-operation.md "mention")

### How It Works

1. **Bypass Blending**: The operation skips the blending process entirely
2. **Preserve Source Data**: Sub-points maintain their original attribute values as they were first created
3. **No Interpolation**: No calculations are performed between neighboring points

**Usage Notes**

* **Default Behavior**: When you need sub-points to have specific pre-set values rather than interpolated ones from their neighbors
* **Performance**: This is the fastest blending option since it performs no computations

### Behavior

```
Input Points:     A[value=10] ──────────── B[value=20]
                                │
Inserted Sub-point:        S[value=X]

Result:                    S keeps its original value X
                           (not interpolated between 10 and 20)
```

### Settings

This factory has no configurable settings.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExBlending-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExBlending/Public/SubPoints/DataBlending/PCGExSubPointsBlendNone.h)
