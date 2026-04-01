---
description: >-
  Filters entire point collections based on how many points pass filter
  conditions.
icon: circle-plus
---

# Uber Filter (Data)

### Overview

This node evaluates filter conditions against points within each collection and keeps or discards entire collections based on how many points pass. Unlike point-level filtering, this operates at the collection level with three modes: All (all points must pass), Any (at least one point must pass), or Partial (a threshold number/percentage must pass). Collections that meet the criteria are kept, while those that don't are optionally discarded.

### How It Works

1. **Filter Loading**: Loads filter factory configurations from the Filters input pin
2. **Per-Collection Evaluation**: For each collection, tests all points against filters
3. **Pass Count**: Counts how many points passed the filter conditions
4. **Threshold Testing**: Compares pass count against mode requirements
5. **Collection Classification**: Determines if collection passes or fails as a whole
6. **Output Routing**: Routes passing collections to Inside, failing to Outside

### Behavior

**All Mode (All points must pass):**

```
Collection A (10 points):
  8 points pass filter → FAIL (not all 10)
  → Routed to Outside

Collection B (5 points):
  5 points pass filter → PASS (all 5)
  → Routed to Inside
```

**Any Mode (At least one point must pass):**

```
Collection A (10 points):
  0 points pass filter → FAIL (none passed)
  → Routed to Outside

Collection B (10 points):
  1 point passes filter → PASS (at least one)
  → Routed to Inside

Collection C (10 points):
  7 points pass filter → PASS (at least one)
  → Routed to Inside
```

**Partial Mode - Relative (Percentage):**

```
Measure: Relative
Comparison: >= (Equal Or Greater)
DblThreshold: 0.5 (50%)

Collection A (10 points):
  4 points pass → 40% → FAIL (< 50%)
  → Routed to Outside

Collection B (10 points):
  5 points pass → 50% → PASS (>= 50%)
  → Routed to Inside

Collection C (10 points):
  7 points pass → 70% → PASS (>= 50%)
  → Routed to Inside
```

**Partial Mode - Discrete (Count):**

```
Measure: Discrete
Comparison: >= (Equal Or Greater)
IntThreshold: 5

Collection A (10 points):
  3 points pass → FAIL (3 < 5)
  → Routed to Outside

Collection B (10 points):
  5 points pass → PASS (5 >= 5)
  → Routed to Inside

Collection C (20 points):
  8 points pass → PASS (8 >= 5)
  → Routed to Inside
```

**Comparison Operators (Partial Mode):**

```
Threshold: 50% (0.5)

Collection with 60% pass rate:

Comparison = >= : PASS (60% >= 50%)
Comparison = > : PASS (60% > 50%)
Comparison = == : FAIL (60% != 50%)
Comparison = < : FAIL (60% not < 50%)
Comparison = <= : FAIL (60% not <= 50%)
```

**Swap (Invert Results):**

```
Mode: Any
bSwap: true

Collection A: 5 points pass
  Normal: PASS → Inside
  Swapped: FAIL → Outside

Collection B: 0 points pass
  Normal: FAIL → Outside
  Swapped: PASS → Inside
```

**Nearly Equal Comparison:**

```
Measure: Relative
Comparison: ~= (Nearly Equal)
DblThreshold: 0.5
Tolerance: 0.05

Collection A: 48% pass → Within tolerance (|0.48-0.5|=0.02 < 0.05) → PASS
Collection B: 45% pass → Outside tolerance (|0.45-0.5|=0.05 = 0.05) → PASS
Collection C: 44% pass → Outside tolerance (|0.44-0.5|=0.06 > 0.05) → FAIL
```

Good for: collection-level filtering, quality thresholds, batch validation, data completeness checks

### Settings

<details>

<summary><strong>Mode</strong> <code>EPCGExUberFilterCollectionsMode</code></summary>

Determines how many points must pass for a collection to be kept.

| Mode              | Description                                       |
| ----------------- | ------------------------------------------------- |
| **All** (default) | All points in the collection must pass filters    |
| **Any**           | At least one point must pass filters              |
| **Partial**       | A threshold number/percentage of points must pass |

**All**: Strictest - requires 100% pass rate **Any**: Most lenient - requires at least 1 passing point **Partial**: Configurable threshold for pass rate

Default: `All`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Measure</strong> <code>EPCGExMeanMeasure</code></summary>

How to measure the threshold in Partial mode.

| Type                   | Description                          |
| ---------------------- | ------------------------------------ |
| **Relative** (default) | Threshold as percentage (0.0 to 1.0) |
| **Discrete**           | Threshold as absolute point count    |

**Relative**: Threshold is a fraction (0.5 = 50%) **Discrete**: Threshold is a number of points (10 = ten points)

📋 _Visible when Mode = Partial_

Default: `Relative`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Comparison</strong> <code>EPCGExComparison</code></summary>

How to compare pass count against threshold in Partial mode.

| Operator                       | Symbol | Description                               |
| ------------------------------ | ------ | ----------------------------------------- |
| **Equal Or Greater** (default) | >=     | Pass count >= threshold                   |
| **Strictly Greater**           | >      | Pass count > threshold                    |
| **Strictly Equal**             | ==     | Pass count == threshold                   |
| **Equal Or Smaller**           | <=     | Pass count <= threshold                   |
| **Strictly Smaller**           | <      | Pass count < threshold                    |
| **Strictly Not Equal**         | !=     | Pass count != threshold                   |
| **Nearly Equal**               | \~=    | Pass count ≈ threshold (within tolerance) |
| **Nearly Not Equal**           | !\~=   | Pass count not ≈ threshold                |

📋 _Visible when Mode = Partial_

Default: `Equal Or Greater`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Dbl Threshold</strong> <code>double</code></summary>

The relative threshold for Partial mode (percentage of points that must pass).

Examples:

* `0.5` (default): 50% of points must pass
* `0.75`: 75% of points must pass
* `0.1`: 10% of points must pass
* `1.0`: 100% (same as All mode)

📋 _Visible when Mode = Partial and Measure = Relative_

Default: `0.5`

Range: `0.0` to `1.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Int Threshold</strong> <code>int32</code></summary>

The discrete threshold for Partial mode (absolute number of points that must pass).

Examples:

* `10` (default): 10 points must pass
* `1`: At least 1 point (similar to Any mode)
* `100`: 100 points must pass regardless of collection size

📋 _Visible when Mode = Partial and Measure = Discrete_

Default: `10`

Range: `0` to max int

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tolerance</strong> <code>double</code></summary>

Tolerance for nearly-equal comparisons.

Pass counts within this range of the threshold are considered equal.

📋 _Visible when Comparison = Nearly Equal or Nearly Not Equal_

Default: `DBL_COMPARE_TOLERANCE` (very small value, typically 1e-8)

⚡ PCG Overridable

</details>

<details>

<summary><strong>Swap</strong> <code>bool</code></summary>

Invert the filter result.

* **false** (default): Pass = Inside, Fail = Outside
* **true**: Pass = Outside, Fail = Inside

Inverts the collection classification logic.

Default: `false`

⚡ PCG Overridable

</details>

### Practical Examples

**Quality Control:**

```
Mode: All
Purpose: Keep only collections where all points are valid
Use: Ensure 100% data quality before processing
```

**Presence Detection:**

```
Mode: Any
Purpose: Keep collections with at least one matching point
Use: Find collections containing specific features
```

**Majority Vote:**

```
Mode: Partial, Measure: Relative, Threshold: 0.5
Purpose: Keep collections where majority pass
Use: Democratic selection based on point properties
```

**Minimum Count:**

```
Mode: Partial, Measure: Discrete, Threshold: 50
Purpose: Keep collections with at least 50 passing points
Use: Ensure sufficient data density
```

### Filter Factories

The node accepts filter factory inputs that define point-level filtering logic:

**Available Filter Types:**

* **Attribute Filters**: Test point attributes
* **Bounds Filters**: Test spatial properties
* **Tag Filters**: Match tags
* **Custom Filters**: User-defined conditions

Multiple filters can be connected and are combined based on their factory settings.

### Inputs

| Pin         | Type             | Description                          |
| ----------- | ---------------- | ------------------------------------ |
| **In**      | Point Data       | Point collections to filter          |
| **Filters** | Filter Factories | Filter configurations to test points |

### Outputs

| Pin         | Type       | Description                               |
| ----------- | ---------- | ----------------------------------------- |
| **Inside**  | Point Data | Collections that pass the filter criteria |
| **Outside** | Point Data | Collections that fail the filter criteria |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Elements/Filtering/PCGExUberFilterCollections.h)
