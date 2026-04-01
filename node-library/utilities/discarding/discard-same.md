---
description: Removes duplicate point collections based on configurable similarity tests.
icon: circle
---

# Discard Same

### Overview

This node detects and removes duplicate point collections by comparing multiple properties: bounds, point count, point positions, and attribute value hashes. When duplicates are found, it keeps only one copy based on the selected mode (first, last, or discard all). Tests can be combined with AND/OR logic and support tolerance values for near-equality comparisons.

### How It Works

1. **Test Configuration**: Defines which properties to compare (bounds, count, positions, attributes)
2. **Hash Generation**: For each collection, computes hashes for enabled test properties
3. **Duplicate Detection**: Compares hashes across collections to find matches
4. **Filter Logic**: Combines test results using AND/OR mode
5. **Duplicate Handling**: Keeps one duplicate or discards all based on Mode setting
6. **Output Generation**: Outputs unique collections and optionally discarded duplicates

### Behavior

**FIFO Mode (First In, First Out - Keep First):**

```
Input order: A, B, C, D
Duplicates: A=C, B=D

Output: A, B (kept first occurrence)
Discarded: C, D (removed later duplicates)
```

**LIFO Mode (Last In, First Out - Keep Last):**

```
Input order: A, B, C, D
Duplicates: A=C, B=D

Output: C, D (kept last occurrence)
Discarded: A, B (removed earlier duplicates)
```

**All Mode (Discard All Duplicates):**

```
Input order: A, B, C, D, E
Duplicates: A=C, B=D
Unique: E

Output: E (only truly unique collections)
Discarded: A, B, C, D (all duplicates removed)
```

**Test Mode AND (All tests must match):**

```
bTestBounds = true
bTestPointCount = true
TestMode = AND

Collection A: Bounds=(10,10,10), Count=100
Collection B: Bounds=(10,10,10), Count=50

Bounds match: ✓
Point count match: ✗
Result: NOT duplicates (both tests required)
```

**Test Mode OR (Any test can match):**

```
bTestBounds = true
bTestPointCount = true
TestMode = OR

Collection A: Bounds=(10,10,10), Count=100
Collection B: Bounds=(10,10,10), Count=50

Bounds match: ✓
Point count match: ✗
Result: ARE duplicates (only one test needed)
```

**Point Count Test:**

```
Collection A: 100 points
Collection B: 100 points
Collection C: 102 points

TestPointCountTolerance = 0: A=B (exact match)
TestPointCountTolerance = 5: A=B=C (all within tolerance)
```

**Bounds Test:**

```
Collection A: Bounds min=(0,0,0), max=(10,10,10)
Collection B: Bounds min=(0,0,0), max=(10.05,10,10)

TestBoundsTolerance = 0.01: NOT duplicates (>0.01 diff)
TestBoundsTolerance = 0.1: ARE duplicates (within tolerance)
```

**Position Test:**

```
Collection A: Points at (0,0,0), (10,0,0), (20,0,0)
Collection B: Points at (0,0.05,0), (10,0.05,0), (20,0.05,0)

TestPositionTolerance = 0.01: NOT duplicates
TestPositionTolerance = 0.1: ARE duplicates

Note: Computes spatial occupation, not per-point matching
```

**Attribute Hash Test:**

```
AttributeHashConfig: Attribute = "Type"
Collection A: Type values = {"Rock", "Rock", "Tree"}
Collection B: Type values = {"Rock", "Rock", "Tree"}
Collection C: Type values = {"Tree", "Rock", "Rock"}

Scope = Uniques:
  A hash: {"Rock", "Tree"}
  B hash: {"Rock", "Tree"}
  C hash: {"Rock", "Tree"}
  Result: A=B=C (same unique values)

Scope = Count:
  A hash: {Rock:2, Tree:1}
  B hash: {Rock:2, Tree:1}
  C hash: {Rock:2, Tree:1}
  Result: A=B=C (same counts)

bSortInputValues = false:
  A hash: ["Rock", "Rock", "Tree"]
  C hash: ["Tree", "Rock", "Rock"]
  Result: A≠C (different order)
```

Good for: removing duplicate collections, deduplication, cleaning redundant data, unique filtering

### Settings

<details>

<summary><strong>Mode</strong> <code>EPCGExDiscardSameMode</code></summary>

Which duplicate to keep when matches are found.

| Mode               | Description                                                     |
| ------------------ | --------------------------------------------------------------- |
| **FIFO** (default) | First in, first out - keep first occurrence, discard later ones |
| **LIFO**           | Last in, first out - keep last occurrence, discard earlier ones |
| **All**            | Discard all duplicates - remove all matching collections        |

**FIFO**: Preserves the first encountered collection **LIFO**: Preserves the most recent collection **All**: Removes all duplicates, keeping only truly unique collections

Default: `FIFO`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Test Mode</strong> <code>EPCGExFilterGroupMode</code></summary>

How to combine multiple test conditions.

| Mode              | Description                                                   |
| ----------------- | ------------------------------------------------------------- |
| **AND** (default) | All enabled tests must match for collections to be duplicates |
| **OR**            | Any enabled test matching means collections are duplicates    |

**AND**: Strict - requires all tests to agree **OR**: Lenient - any matching test is sufficient

Default: `AND`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Test Bounds</strong> <code>bool</code></summary>

Test collection bounds for equality.

When enabled, compares the bounding boxes of collections.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Test Bounds Tolerance</strong> <code>double</code></summary>

Tolerance for bounds equality comparison.

Bounds are considered equal if their min/max corners differ by less than this value.

📋 _Visible when Test Bounds = true_

Default: `0.1`

Range: `0.000001` to max

⚡ PCG Overridable

</details>

<details>

<summary><strong>Test Point Count</strong> <code>bool</code></summary>

Test collection point count for equality.

When enabled, compares the number of points in each collection.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Test Point Count Tolerance</strong> <code>int32</code></summary>

Tolerance for point count equality.

Point counts are considered equal if they differ by at most this many points.

Examples:

* `0` (default): Exact point count match required
* `5`: Point counts within 5 are considered equal

📋 _Visible when Test Point Count = true_

Default: `0`

Range: `0` to max int

⚡ PCG Overridable

</details>

<details>

<summary><strong>Test Positions</strong> <code>bool</code></summary>

Test point positions for equality.

When enabled, compares the spatial distribution of points. Note that this computes space occupation (spatial hash), not per-point position matching, and does not account for point count.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Test Position Tolerance</strong> <code>double</code></summary>

Tolerance for position equality.

Positions are considered equal if they occupy similar spatial regions within this tolerance.

📋 _Visible when Test Positions = true_

Default: `0.1`

Range: `0.000001` to max

⚡ PCG Overridable

</details>

<details>

<summary><strong>Test Attributes Hash</strong> <code>EPCGExDiscardAttributeHashMode</code></summary>

Whether and how to use attribute value hashes for duplicate detection.

| Mode               | Description                                              |
| ------------------ | -------------------------------------------------------- |
| **None** (default) | Don't test attribute values                              |
| **Single**         | Use a single, overridable attribute configuration        |
| **List**           | Use a list of attribute configurations (not overridable) |

**None**: No attribute testing **Single**: Simple, one attribute comparison **List**: Multiple attributes tested simultaneously

Default: `None`

</details>

<details>

<summary><strong>Attribute Hash Configs</strong> <code>TArray</code></summary>

List of attribute configurations to hash and compare.

Each config specifies an attribute to compare and how to hash its values.

📋 _Visible when Test Attributes Hash = List_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Append Overridable</strong> <code>bool</code></summary>

Also include the single overridable attribute config in the hash.

Allows combining the list of attributes with one additional overridable attribute.

📋 _Visible when Test Attributes Hash = List_

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Attribute Hash Config</strong> <code>FPCGExAttributeHashConfig</code></summary>

Single attribute configuration to hash and compare.

📋 _Visible when Test Attributes Hash = Single, or when Test Attributes Hash = List and Append Overridable = true_

⚡ PCG Overridable

</details>

#### Attribute Hash Configuration (FPCGExAttributeHashConfig)

<details>

<summary><strong>Source Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute to hash and compare.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Scope</strong> <code>EPCGExDataHashScope</code></summary>

How to compute the hash from attribute values.

**Uniques**: Hash only unique values (ignores duplicates and count) **Count**: Hash unique values with their occurrence counts

⚡ PCG Overridable

</details>

<details>

<summary><strong>Sort Input Values</strong> <code>bool</code></summary>

Sort attribute values before hashing.

* **true** (default): Order-independent hash ({"A","B"} = {"B","A"})
* **false**: Order-dependent hash ({"A","B"} ≠ {"B","A"})

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Sorting</strong> <code>EPCGExSortDirection</code></summary>

Sort direction when Sort Input Values = true.

**Ascending** or **Descending**

⚡ PCG Overridable

</details>

### Inputs

| Pin    | Type       | Description                      |
| ------ | ---------- | -------------------------------- |
| **In** | Point Data | Point collections to deduplicate |

### Outputs

| Pin                      | Type       | Description                     |
| ------------------------ | ---------- | ------------------------------- |
| **Out**                  | Point Data | Unique collections (kept)       |
| **Discarded** (optional) | Point Data | Duplicate collections (removed) |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Elements/Filtering/PCGExDiscardSame.h)
