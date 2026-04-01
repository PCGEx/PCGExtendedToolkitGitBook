---
description: >-
  A Picker that accepts lists of ranges in the form of FVector2, read from one
  or more attributes.
icon: circle-dashed
---

# Picker : Ranges from Set

### Overview

This picker reads range values from one or more attributes and generates index selections for all values within those ranges. Each attribute should contain FVector2 values where X is the range start and Y is the range end. The picker aggregates all ranges from all specified attributes and expands them into a complete set of discrete indices, making it easy to select contiguous blocks of points.

### How It Works

1. **Attribute Reading**: Reads FVector2 values from all specified attributes in the Attributes list
2. **Range Aggregation**: Collects all range definitions from all attributes
3. **Range Expansion**: For each range (min, max), generates all indices between min and max (inclusive)
4. **Index Interpretation**: Depending on the Treat As Normalized setting:
   * **Discrete Mode**: Range values are used directly as indices (FVector2(2, 5) = indices 2, 3, 4, 5)
   * **Normalized Mode**: Range values are treated as 0-1 positions and converted to indices
5. **Negative Index Support**: Negative values select from the end (-1 = last item, -5 to -1 = last 5 items)
6. **Deduplication**: Overlapping ranges are merged, producing a unique set of indices
7. **Safety Application**: Applies the configured Safety mode to handle out-of-bounds indices
8. **Output**: Returns a set of unique indices ready for point selection

### Behavior

**Example with 1 attribute on a collection of 20 points:**

```
Attribute values (FVector2):

[(2, 5), (10, 12), (15, 17)]
```

**Discrete Mode:**

```
Range (2, 5):   {2, 3, 4, 5}
Range (10, 12): {10, 11, 12}
Range (15, 17): {15, 16, 17}
Combined picks: {2, 3, 4, 5, 10, 11, 12, 15, 16, 17}
```

**Normalized Mode (0-1 values):**

```
Range (0.1, 0.25): 10% to 25% of 20 points → {2, 3, 4, 5}
Range (0.5, 0.6):  50% to 60% → {10, 11, 12}
Range (0.75, 0.85): 75% to 85% → {15, 16, 17}
Combined: {2, 3, 4, 5, 10, 11, 12, 15, 16, 17}
```

**Overlapping Ranges:**

```
[(2, 5), (4, 8)]
Expanded: {2, 3, 4, 5} ∪ {4, 5, 6, 7, 8}
Result: {2, 3, 4, 5, 6, 7, 8} (deduplicated)
```

**Negative Indices (20 points):**

```
Range (-5, -1) = Range (15, 19)
Picks: {15, 16, 17, 18, 19} (last 5 items)
```

### Inputs

| Pin        | Type       | Description                                                     |
| ---------- | ---------- | --------------------------------------------------------------- |
| **Ranges** | Point Data | Point data containing the attributes with FVector2 range values |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Attributes</strong> <code>TArray&#x3C;FPCGAttributePropertyInputSelector></code></summary>

List of attributes to read range values from. Each attribute should contain FVector2 values where:

* **X component**: Start of the range (inclusive)
* **Y component**: End of the range (inclusive)

The picker will aggregate and expand all ranges from all specified attributes. If no attributes are specified, the picker will attempt to use the first available FVector2 attribute.

**Note**: Use negative values to select from the end of the collection (e.g., FVector2(-10, -1) = last 10 items).

⚡ PCG Overridable

</details>

#### Inherited Settings

This picker inherits common settings from the base picker configuration.

→ See Picker Factory Provider for: Treat As Normalized, Truncate Mode, Safety, and other shared picker properties.

### Outputs

| Pin        | Type   | Description                                                        |
| ---------- | ------ | ------------------------------------------------------------------ |
| **Picker** | Picker | Picker factory that can be used by nodes requiring index selection |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExPickers-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExPickers/Public/Pickers/PCGExPickerAttributeSetRanges.h)
