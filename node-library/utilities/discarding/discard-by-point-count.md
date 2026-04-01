---
description: Filters out point collections based on their point count.
icon: circle
---

# Discard By Point Count

### Overview

This node removes point collections that fall outside a specified point count range. It can filter out collections with too few points (below minimum), too many points (above maximum), or both. Collections that pass the point count criteria are forwarded to the output, while those that fail are optionally removed or sent to a separate discard output.

### How It Works

1. **Threshold Configuration**: Defines minimum and/or maximum point count thresholds
2. **Collection Evaluation**: For each input collection, counts its points
3. **Range Testing**: Compares point count against active thresholds
4. **Filtering Decision**: Keeps collections within range, discards those outside
5. **Output Routing**: Routes kept collections to output, optionally handles discards

### Behavior

**Remove Below Only (bRemoveBelow = true, MinPointCount = 10):**

```
Collection A: 5 points → Discarded (below 10)
Collection B: 10 points → Kept (>= 10)
Collection C: 15 points → Kept (>= 10)
Collection D: 100 points → Kept (>= 10)
```

**Remove Above Only (bRemoveAbove = true, MaxPointCount = 50):**

```
Collection A: 5 points → Kept (<= 50)
Collection B: 50 points → Kept (<= 50)
Collection C: 75 points → Discarded (> 50)
Collection D: 100 points → Discarded (> 50)
```

**Remove Both (bRemoveBelow = true, bRemoveAbove = true):**

```
MinPointCount = 10, MaxPointCount = 50

Collection A: 5 points → Discarded (< 10)
Collection B: 10 points → Kept (10-50 range)
Collection C: 25 points → Kept (10-50 range)
Collection D: 50 points → Kept (10-50 range)
Collection E: 75 points → Discarded (> 50)
Collection F: 100 points → Discarded (> 50)

Only collections with 10-50 points are kept
```

**Neither Filter Enabled:**

```
bRemoveBelow = false
bRemoveAbove = false

All collections pass through (no filtering)
```

**Empty Outputs Handling:**

```
Input: 10 collections
After filtering: 7 kept, 3 discarded

bAllowEmptyOutputs = false:
  If kept = 0, output is completely removed
  If discarded = 0, discard pin is completely removed

bAllowEmptyOutputs = true:
  Outputs may have zero collections
```

**Threshold Edge Cases:**

```
MinPointCount = 1:
  Removes only empty collections (0 points)

MaxPointCount = 0:
  Removes all collections (none can have 0 or fewer)

MinPointCount = MaxPointCount = 10:
  Only collections with exactly 10 points are kept
```

Good for: removing degenerate data, enforcing size constraints, cleaning up small fragments, limiting large collections

### Settings

<details>

<summary><strong>Remove Below</strong> <code>bool</code></summary>

Enable filtering out collections with too few points.

* **true** (default): Remove collections below MinPointCount
* **false**: No minimum point count filtering

When enabled, collections with fewer points than MinPointCount are discarded.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Min Point Count</strong> <code>int32</code></summary>

Minimum number of points required to keep a collection.

Collections with fewer points are discarded.

Examples:

* `1` (default): Remove only empty collections
* `3`: Require at least 3 points
* `100`: Remove small collections

📋 _Visible when Remove Below = true_

Default: `1`

Range: `0` to max int

⚡ PCG Overridable

</details>

<details>

<summary><strong>Remove Above</strong> <code>bool</code></summary>

Enable filtering out collections with too many points.

* **false** (default): No maximum point count filtering
* **true**: Remove collections above MaxPointCount

When enabled, collections with more points than MaxPointCount are discarded.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Point Count</strong> <code>int32</code></summary>

Maximum number of points allowed to keep a collection.

Collections with more points are discarded.

Examples:

* `500` (default): Remove very large collections
* `1000`: Allow up to 1000 points
* `10`: Keep only very small collections

📋 _Visible when Remove Above = true_

Default: `500`

Range: `0` to max int

⚡ PCG Overridable

</details>

<details>

<summary><strong>Allow Empty Outputs</strong> <code>bool</code></summary>

Whether to allow output pins with zero collections.

* **false** (default): Remove output pins that would be empty
* **true**: Keep output pins even when they have no collections

Affects both the kept and discarded output pins.

Default: `false`

⚡ PCG Overridable

</details>

### Practical Examples

**Remove Empty Collections:**

```
bRemoveBelow = true
MinPointCount = 1
Result: Only collections with 1+ points kept
```

**Keep Only Medium-Sized Collections:**

```
bRemoveBelow = true, MinPointCount = 50
bRemoveAbove = true, MaxPointCount = 500
Result: Only collections with 50-500 points kept
```

**Remove Oversized Collections:**

```
bRemoveAbove = true
MaxPointCount = 1000
Result: Collections with >1000 points discarded
```

### Inputs

| Pin    | Type       | Description                 |
| ------ | ---------- | --------------------------- |
| **In** | Point Data | Point collections to filter |

### Outputs

| Pin                      | Type       | Description                           |
| ------------------------ | ---------- | ------------------------------------- |
| **Out**                  | Point Data | Collections within point count range  |
| **Discarded** (optional) | Point Data | Collections outside point count range |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Elements/Filtering/PCGExDiscardByPointCount.h)
