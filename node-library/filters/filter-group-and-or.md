---
description: Combines multiple filters into a single composite filter.
icon: circle-dashed
---

# Filter Group (And/Or)

### Overview

This node creates a filter group that combines multiple input filters using boolean AND or OR logic. In AND mode, all child filters must pass for the group to pass. In OR mode, at least one child filter must pass for the group to pass. Filter groups can be nested to create complex logical expressions and can inherit the highest priority from their child filters.

### How It Works

1. **Filter Collection**: Gathers all filter factories from the input pin.
2. **Priority Resolution**: Uses the maximum priority between the group's setting and all child filters.
3. **Evaluation**: For each point, evaluates all child filters in the group.
4. **Logic Application**: Combines results using the configured mode (AND/OR).
5. **Inversion**: Optionally inverts the final group result.

**Usage Notes**

* **Nesting**: Filter groups can contain other filter groups for complex boolean expressions.
* **Short-Circuit Evaluation**: AND mode stops on first failure, OR mode stops on first pass.
* **Priority Inheritance**: The group uses the highest priority from itself or any child filter.

### Behavior

**AND Mode:**

```
Filter A: Pass
Filter B: Pass
Filter C: Fail

Group Result: Fail (all must pass)
```

**OR Mode:**

```
Filter A: Fail
Filter B: Pass
Filter C: Fail

Group Result: Pass (at least one passed)
```

**Nested Groups:**

```
Group 1 (AND):
  ├─ Filter A: Pass
  └─ Group 2 (OR):
       ├─ Filter B: Fail
       └─ Filter C: Pass
       └─ Group 2 Result: Pass

Group 1 Result: Pass (A passed AND Group 2 passed)
```

**With Inversion:**

```
Mode: AND, Invert: true
Filter A: Pass
Filter B: Pass

AND Result: Pass → Inverted: Fail
```

### Inputs

| Pin         | Type            | Description                            |
| ----------- | --------------- | -------------------------------------- |
| **Filters** | PCGEx \| Filter | Child filters to combine in this group |

### Settings

<details>

<summary><strong>Priority</strong> <code>int32</code></summary>

Filter Priority. The group will use the highest value between this setting and the priorities of connected filters.

Default: `0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Mode</strong> <code>EPCGExFilterGroupMode</code></summary>

The logical operation used to combine filter results.

| Option  | Description                                               |
| ------- | --------------------------------------------------------- |
| **And** | All child filters must pass for the group to pass         |
| **Or**  | At least one child filter must pass for the group to pass |

Default: `And`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

Inverts the group's output value after applying the logical operation.

Default: `false`

⚡ PCG Overridable

</details>

### Outputs

| Pin        | Type            | Description                         |
| ---------- | --------------- | ----------------------------------- |
| **Filter** | PCGEx \| Filter | The configured filter group factory |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Points/PCGExFilterGroup.h)
