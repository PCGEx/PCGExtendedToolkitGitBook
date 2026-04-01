---
description: Each seed reads its target goal index(es) from an attribute.
icon: function
---

# Goal Picker : Index Attribute

### Overview

This goal picker reads goal indices directly from attributes on seed points. Instead of using the seed's position in the collection, it uses attribute values to determine which goal(s) each seed should path toward. This enables data-driven goal assignment where each seed can target specific goals based on pre-computed or authored values.

### How It Works

1. **Read Attribute**: Gets the goal index value(s) from the seed point.
2. **Apply Safety**: Maps indices using the safety mode if out of range.
3. **Return Goals**: Returns the goal point(s) at the specified indices.

**Usage Notes**

* **Single Mode**: One attribute containing the goal index.
* **Multiple Mode**: Multiple attributes, each specifying a different goal index.
* **Index Values**: Attribute values are converted to int32 indices.
* **Data-Driven**: Pre-calculate indices with other nodes, then pathfind.

### Behavior

```
Attribute-Based Goal Selection:

Seeds with $TargetGoal attribute:
   S0 ($TargetGoal = 2)
   S1 ($TargetGoal = 0)
   S2 ($TargetGoal = 2)

Goals: [G0, G1, G2, G3]

Output Paths:
   S0 → G2 (from attribute)
   S1 → G0 (from attribute)
   S2 → G2 (from attribute)

Multiple Attributes Mode:
   S0 with $Goal1=0, $Goal2=3:
      S0 → G0
      S0 → G3
```

### Settings

<details>

<summary><strong>Goal Count</strong> <code>EPCGExGoalPickAttributeAmount</code></summary>

How many goal indices to read per seed.

| Option                  | Description                                         |
| ----------------------- | --------------------------------------------------- |
| **Single Attribute**    | Read one goal index from a single attribute         |
| **Multiple Attributes** | Read multiple goal indices from multiple attributes |

Default: `Single Attribute`

</details>

<details>

<summary><strong>Single Selector</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute containing the goal index (converted to int32).

📋 _Visible when Goal Count = Single Attribute_

</details>

<details>

<summary><strong>Comma Separated Names</strong> <code>FString</code></summary>

A list of attribute names separated by commas for easy overrides. These are added to the attribute selectors array.

📋 _Visible when Goal Count = Multiple Attributes_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Attribute Selectors</strong> <code>TArray</code></summary>

Array of attribute selectors, each specifying a goal index. Creates one path per selector per seed.

📋 _Visible when Goal Count = Multiple Attributes_

</details>

#### Inherited Settings

→ See Goal Picker : Default for Index Safety settings.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPathfinding-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPathfinding/Public/GoalPickers/PCGExGoalPickerAttribute.h)
