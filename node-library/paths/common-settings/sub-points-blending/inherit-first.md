---
description: Copy attributes from the source point to all sub-points.
icon: function
---

# Inherit First

### Overview

This blending option makes all inserted sub-points inherit their attribute values from the source point (the "From" point). Regardless of where along the path segment a sub-point is located, it receives a copy of the starting point's attributes rather than any interpolated or blended values.

> This is a [sub-point-blending-operation.md](sub-point-blending-operation.md "mention")

### How It Works

1. **Identify Source**: Determines the "From" point (the first point in the pair)
2. **Copy Attributes**: Transfers all eligible attributes from the source point to each sub-point
3. **Apply to All**: Every sub-point in the segment receives the same values from the source

**Usage Notes**

* **Uniform Values**: All sub-points between two points will have identical attribute values matching the source
* **Direction-Dependent**: The result depends on path direction - reversing the path would cause sub-points to inherit from the opposite endpoint

### Behavior

```
Input Points:     A[value=10] ──────────── B[value=20]
                                │  │ │  │
Inserted Sub-points:           S1 S2 S3 S4

Result:         All sub-points inherit from A
                S1[value=10]
                S2[value=10]
                S3[value=10]
                S4[value=10]
```

### Settings

#### Inherited Settings

This factory inherits blending configuration from its base class.

→ See Sub-Point Blending Base for: Blending Filter, Filtered Attributes, Default Blending, Properties Overrides, Attributes Overrides

The Default Blending for this factory is set to `Copy` (copy from source point) by default.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExBlending-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExBlending/Public/SubPoints/DataBlending/PCGExSubPointsBlendInheritStart.h)
