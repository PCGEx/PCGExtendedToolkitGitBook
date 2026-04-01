---
description: Filter that returns a constant value.
icon: circle-dashed
---

# Filter : Constant

### Overview

This filter returns the same result for all points regardless of their attributes or position. It always passes or always fails based on the configured constant value. Useful for testing filter chains, creating placeholder filters, or forcing specific filter outcomes during development.

### How It Works

1. **Constant Value**: Uses the configured boolean value for all evaluations.
2. **Inversion**: Optionally inverts the constant value.
3. **Result**: Returns the same pass/fail result for every point.

**Usage Notes**

* **Testing**: Useful for testing downstream behavior with guaranteed filter results.
* **Placeholder**: Can serve as a placeholder while developing filter logic.
* **Override**: When combined in filter groups, can force specific outcomes.

### Behavior

```
Config: Value = true, Invert = false
All points → Pass

Config: Value = false, Invert = false
All points → Fail

Config: Value = true, Invert = true
All points → Fail

Config: Value = false, Invert = true
All points → Pass
```

### Settings

<details>

<summary><strong>Value</strong> <code>bool</code></summary>

The constant value returned by this filter for all points.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

Invert the filter result.

Default: `false`

⚡ PCG Overridable

</details>

#### Inherited Settings

> See Filter Definition for: Priority, Initialization Failure Policy

### Outputs

| Pin        | Type                    | Description                   |
| ---------- | ----------------------- | ----------------------------- |
| **Filter** | PCGEx \| Filter (Point) | The configured filter factory |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Points/PCGExConstantFilter.h)
