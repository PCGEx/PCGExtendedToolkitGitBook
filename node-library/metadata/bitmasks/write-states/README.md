---
description: Writes point states as a int64 flag mask.
icon: circle
---

# Write States

### Overview

This node evaluates connected State sub-nodes against each point and writes the results as a 64-bit flag attribute. Each state is a filter-driven condition that can set, toggle, or clear specific bits in the flag mask based on whether its filters pass or fail. This enables complex conditional logic stored as compact bitmask values for downstream processing.

### How It Works

1. **Initialize Flags**: Sets each point's flags to the specified initial value.
2. **Evaluate States**: For each connected state, tests all its filters against each point.
3. **Apply Operations**: Based on pass/fail results, applies the configured bitmask operations.
4. **Write Output**: Stores the final flag value in the specified attribute.

**Usage Notes**

* **Bitmask Operations**: States can SET, OR, AND, NOT, or XOR bits based on filter results.
* **State Priority**: States are evaluated in priority order, with later states modifying flags from earlier ones.
* **Filter Composition**: Each state can have multiple filters; all must pass for the "pass" operation to apply.
* **Bitmask Output**: States can optionally output their bitmasks as separate data for debugging.

### Behavior

**State Evaluation:**

```
Initial Flags: 0b00000000

State "IsLarge" (filters: Bounds > 100):
   On Pass: OR with 0b00000001 (set bit 0)
   On Fail: No operation

State "IsRed" (filters: Color.R > 0.5):
   On Pass: OR with 0b00000010 (set bit 1)
   On Fail: No operation

Results:
   Point A (Large, Red):   Flags = 0b00000011 (bits 0 and 1 set)
   Point B (Small, Red):   Flags = 0b00000010 (only bit 1 set)
   Point C (Large, Blue):  Flags = 0b00000001 (only bit 0 set)
   Point D (Small, Blue):  Flags = 0b00000000 (no bits set)
```

### Inputs

| Pin        | Type   | Description                                           |
| ---------- | ------ | ----------------------------------------------------- |
| **In**     | Points | Input point collections                               |
| **Labels** | Params | State : Point sub-nodes defining filter-driven states |

### Settings

<details>

<summary><strong>Flag Attribute</strong> <code>FName</code></summary>

The attribute name to write the resulting flag bitmask to.

Default: `Flags`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Initial Flags</strong> <code>int64</code></summary>

The starting flag value before any state operations are applied. States will modify this base value.

Default: `0`

⚡ PCG Overridable

</details>

#### Inherited Settings

→ See Points Processor Settings for common point processing settings.

### Outputs

| Pin     | Type   | Description                                                   |
| ------- | ------ | ------------------------------------------------------------- |
| **Out** | Points | Points with flag attribute containing evaluated state results |

***

## State : Point

A single, filter-driven point state.

### Overview

This sub-node defines a named state that evaluates filters against each point. When connected to the Write States node, it contributes to the flag bitmask by applying specified operations when its filters pass or fail. Multiple states can be chained to build complex flag combinations.

### How It Works

1. **Receive Filters**: Accepts filter sub-nodes that define the pass/fail condition.
2. **Evaluate Per Point**: Tests all filters against each point in the collection.
3. **Apply Pass/Fail Operations**: Modifies the point's flags based on filter results.

### Inputs

| Pin         | Type   | Description                                              |
| ----------- | ------ | -------------------------------------------------------- |
| **Filters** | Params | Filter sub-nodes that determine pass/fail for this state |

### Settings

#### State Identity

<details>

<summary><strong>Name</strong> <code>FName</code></summary>

Unique name for this state definition. Used for identification and debugging.

Default: `Flag`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Priority</strong> <code>int32</code></summary>

Evaluation priority. Higher priority states are evaluated later and can override earlier state results.

Default: `0`

⚡ PCG Overridable

_Advanced setting_

</details>

<details>

<summary><strong>Output Bitmasks</strong> <code>bool</code></summary>

When enabled, outputs the pass and fail bitmasks as separate data for debugging or reuse.

Default: `true`

_Advanced setting_

</details>

#### Pass Condition

<details>

<summary><strong>On Test Pass</strong> <code>bool</code></summary>

When enabled, applies the pass state flags operation when all filters pass.

Default: `true`

</details>

<details>

<summary><strong>Pass State Flags</strong> <code>FPCGExBitmaskWithOperation</code></summary>

Operations executed on the flag if all filters pass. Defines how to modify the bitmask.

| Property         | Description                                                   |
| ---------------- | ------------------------------------------------------------- |
| **Mode**         | Direct (use raw value) or Mutations (per-bit operations)      |
| **Bitmask**      | The 64-bit mask value to apply                                |
| **Op**           | How to combine: SET (=), AND (&), OR (\|), NOT (&\~), XOR (^) |
| **Mutations**    | Per-bit operations when Mode = Mutations                      |
| **Compositions** | Additional bitmask references to combine                      |

📋 _Visible when On Test Pass is enabled_

⚡ PCG Overridable (Bitmask only)

</details>

#### Fail Condition

<details>

<summary><strong>On Test Fail</strong> <code>bool</code></summary>

When enabled, applies the fail state flags operation when any filter fails.

Default: `true`

</details>

<details>

<summary><strong>Fail State Flags</strong> <code>FPCGExBitmaskWithOperation</code></summary>

Operations executed on the flag if any filters fail. Same structure as Pass State Flags.

📋 _Visible when On Test Fail is enabled_

⚡ PCG Overridable (Bitmask only)

</details>

### Outputs

| Pin             | Type   | Description                                       |
| --------------- | ------ | ------------------------------------------------- |
| **State**       | Params | State factory for connection to Write States node |
| **BitmaskPass** | Params | Pass bitmask data (when Output Bitmasks enabled)  |
| **BitmaskFail** | Params | Fail bitmask data (when Output Bitmasks enabled)  |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsMeta-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsMeta/Public/Elements/PCGExWriteStates.h)
