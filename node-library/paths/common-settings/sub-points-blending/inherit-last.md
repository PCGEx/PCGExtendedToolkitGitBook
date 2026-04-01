---
description: Copy attributes from the destination point to all sub-points.
icon: function
---

# Inherit Last

### Overview

This blending option makes all inserted sub-points inherit their attribute values from the destination point (the "To" point). Regardless of where along the path segment a sub-point is located, it receives a copy of the endpoint's attributes rather than any interpolated or blended values.

> This is a [sub-point-blending-operation.md](sub-point-blending-operation.md "mention")

### How It Works

1. **Identify Destination**: Determines the "To" point (the second point in the pair)
2. **Copy Attributes**: Transfers all eligible attributes from the destination point to each sub-point
3. **Apply to All**: Every sub-point in the segment receives the same values from the destination

**Usage Notes**

* **Uniform Values**: All sub-points between two points will have identical attribute values matching the destination
* **Direction-Dependent**: The result depends on path direction - reversing the path would cause sub-points to inherit from the opposite endpoint

### Behavior

```
Input Points:     A[value=10] ──────────── B[value=20]
                                │  │ │  │
Inserted Sub-points:           S1 S2 S3 S4

Result:         All sub-points inherit from B
                S1[value=20]
                S2[value=20]
                S3[value=20]
                S4[value=20]
```

### Settings

#### Inherited Settings

This factory inherits blending configuration from its base class.

→ See Sub-Point Blending Base for: Blending Filter, Filtered Attributes, Default Blending, Properties Overrides, Attributes Overrides

The Default Blending for this factory is set to `CopyOther` (copy from destination point) by default.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExBlending-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExBlending/Public/SubPoints/DataBlending/PCGExSubPointsBlendInheritEnd.h)
