---
description: One-stop node to combine multiple blends.
icon: circle
---

# Uber Blend

### Overview

This node applies multiple blend operations to point attributes in a single pass. It accepts blend operation sub-nodes that define how attributes should be combined or modified. The node processes all connected blend operations together, making it efficient for complex blending scenarios that would otherwise require chaining multiple nodes.

### How It Works

1. **Gather Blend Ops**: Collects all connected blend operation factories.
2. **Filter Points**: Optionally filters which points participate in blending.
3. **Execute Blends**: Applies all blend operations to each point in the collection.
4. **Output**: Returns points with blended attribute values.

**Usage Notes**

* **Blend Operations**: Connect Blend Op sub-nodes to define what blending to perform.
* **Point Filtering**: Use filters to limit which points are affected by blending.

### Behavior

```
Multiple Blend Operations:

Input Points:
   Point 0: Color = Red,   Scale = 1.0
   Point 1: Color = Blue,  Scale = 2.0
   Point 2: Color = Green, Scale = 3.0

Connected Blend Ops:
   1. Average Color from neighbors
   2. Lerp Scale toward mean

Output Points:
   All blend operations applied in single pass
```

### Inputs

| Pin          | Type   | Description                                              |
| ------------ | ------ | -------------------------------------------------------- |
| **In**       | Points | Input point collections to blend                         |
| **Filters**  | Params | Optional point filters to limit which points are blended |
| **Blending** | Params | Blend operation sub-nodes defining the blending behavior |

### Settings

This node has no node-specific settings beyond the inherited base settings.

#### Inherited Settings

→ See Points Processor Settings for common point processing settings.

### Outputs

| Pin     | Type   | Description                              |
| ------- | ------ | ---------------------------------------- |
| **Out** | Points | Points with all blend operations applied |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsMeta-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsMeta/Public/Elements/PCGExBlendAttributes.h)
