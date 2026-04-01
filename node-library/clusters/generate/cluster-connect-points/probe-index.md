---
description: Connects to a specific index, ignoring search radius.
icon: circle-dashed
---

# Probe : Index

### Overview

This per-point probe creates connections based on point indices rather than spatial proximity. Each point connects to a target index that can be constant, read from an attribute, or calculated as an offset from the point's own index. This is useful for creating deterministic connections in ordered point sets, such as linking sequential points or creating specific topological patterns.

<figure><img src="../../../../.gitbook/assets/image (284).png" alt=""><figcaption></figcaption></figure>

### How It Works

1. **Index Resolution**: Gets the target index (constant, attribute, or calculated offset)
2. **Safety Check**: Validates the index against the point count using the safety mode
3. **Edge Creation**: Creates a direct edge to the target point if valid
4. **Offset Processing**: For two-way offset mode, creates edges in both directions

**Usage Notes**

* **No Search Radius**: This probe ignores spatial distance entirely - connections are based purely on index values
* **Index Safety**: Choose how to handle out-of-range indices (ignore, clamp, wrap, etc.)
* **Offset Modes**: Use offsets to create chains (offset=1 connects each point to the next) or bidirectional links

### Behavior

```
Index Probe Modes:

    Target Mode (Index=3):
    Points: [0] [1] [2] [3] [4]
                        ↑
            All points connect to index 3

    One-Way Offset (Index=1):
    Points: [0]→[1]→[2]→[3]→[4]
            Each point connects to the next (index + 1)

    Two-Way Offset (Index=1):
    Points: [0]↔[1]↔[2]↔[3]↔[4]
            Each point connects to both neighbors (index ± 1)


    Index Safety:

    Points: [0] [1] [2] [3] [4]   Index=10

    Ignore:  No connection (index out of range)
    Clamp:   Connect to [4] (clamped to max)
    Wrap:    Connect to [0] (10 % 5 = 0)
```

### Settings

#### Index Configuration

<details>

<summary><strong>Mode</strong> <code>EPCGExProbeTargetMode</code></summary>

How to interpret and use the resolved index value.

| Option             | Description                                                                     |
| ------------------ | ------------------------------------------------------------------------------- |
| **Target**         | Index is used directly as the target point to connect to                        |
| **One-Way Offset** | Index is added to the current point's index (connects to Index + Offset)        |
| **Two-Way Offset** | Index is used as both positive and negative offset (connects to Index ± Offset) |

Default: `Target`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Index Safety</strong> <code>EPCGExIndexSafety</code></summary>

How to handle index values that are out of the valid range (0 to point count - 1).

| Option     | Description                                  |
| ---------- | -------------------------------------------- |
| **Ignore** | Skip the connection if index is out of range |
| **Clamp**  | Clamp to valid range (0 or max index)        |
| **Wrap**   | Wrap around using modulo                     |

Default: `Ignore`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Index Input</strong> <code>EPCGExInputValueType</code></summary>

Determines whether the index is a constant value or read from an attribute.

| Option        | Description                            |
| ------------- | -------------------------------------- |
| **Constant**  | Use the constant index specified below |
| **Attribute** | Read the index from a point attribute  |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Index</strong> <code>int32</code></summary>

The constant index value (target index or offset depending on Mode).

Default: `1`

Min: `0`

📋 _Visible when Index Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Index (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute containing the index value per point.

📋 _Visible when Index Input = Attribute_

⚡ PCG Overridable

</details>

### Outputs

| Pin       | Type           | Description                                    |
| --------- | -------------- | ---------------------------------------------- |
| **Probe** | PCGEx \| Probe | The probe factory to connect to Connect Points |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsProbing-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsProbing/Public/Probes/PCGExProbeIndex.h)
