---
description: Combines point collections into merged datasets with optional grouping.
icon: circle
---

# Merge Points

### Overview

This node merges multiple point collections into fewer consolidated collections, either combining all inputs into one output or grouping them based on matching rules. When matching is enabled, collections with matching properties (tags, attributes, indices) are merged together while others remain separate. The node supports exclusive partitioning (each collection belongs to one group) or non-exclusive (collections can appear in multiple groups), and provides attribute and tag carry-over control.

### How It Works

1. **Matching Configuration**: Determines if and how to group collections before merging
2. **Group Formation**: Creates merge groups based on matching criteria
3. **Partitioning**: Assigns collections to groups (exclusive or non-exclusive)
4. **Point Consolidation**: Combines all points from each group into a single collection
5. **Attribute Carry-Over**: Transfers specified attributes/tags to merged result
6. **Output Generation**: Produces merged point collections

### Behavior

**Simple Merge (No Matching):**

```
Input: 5 collections (100, 50, 200, 75, 125 points)
MatchingDetails.Mode: Disabled

Output: 1 merged collection (550 points total)
All inputs combined into single output
```

**Matched Grouping:**

```
Input collections:
- Collection A (tag: "Trees")
- Collection B (tag: "Trees")
- Collection C (tag: "Rocks")
- Collection D (tag: "Rocks")
- Collection E (tag: "Props")

MatchingDetails.Mode: Tag

Output:
- Merged 1: Collections A + B (Trees group)
- Merged 2: Collections C + D (Rocks group)
- Merged 3: Collection E (Props group)

3 outputs from 5 inputs
```

**Exclusive vs Non-Exclusive Partitions:**

```
Collections with tags:
- Collection A: ["Group1", "Group2"]
- Collection B: ["Group1"]
- Collection C: ["Group2"]

MatchingDetails.Mode: Tag
bExclusivePartitions = true:
  Collection A → Group1 (first match only)
  Collection B → Group1
  Collection C → Group2
  Result: 2 merged collections

bExclusivePartitions = false:
  Collection A → Group1 AND Group2 (both)
  Collection B → Group1
  Collection C → Group2
  Result: 2 merged collections
    Group1: A + B
    Group2: A + C (A appears twice)
```

**Carry-Over Settings:**

```
Input collections have attributes: Type, Size, Color
CarryOverDetails.Attributes:
  FilterMode: Whitelist
  Names: "Type", "Size"

Merged output:
  Has attributes: Type, Size
  Missing: Color (not carried over)
```

**Sort Direction:**

```
Collections ordered by some criteria
SortDirection = Ascending:
  Merge order: A, B, C, D
  Result points: [A points, B points, C points, D points]

SortDirection = Descending:
  Merge order: D, C, B, A
  Result points: [D points, C points, B points, A points]
```

**Unmatched Handling:**

```
MatchingDetails.bSplitUnmatched = true:
  Unmatched collections → Separate "Unmatched" output

MatchingDetails.bSplitUnmatched = false:
  Unmatched collections → Merged into main output or discarded
```

Good for: consolidation, grouping by category, combining similar data, batch merging

### Settings

<details>

<summary><strong>Matching Details</strong> <code>FPCGExMatchingDetails</code></summary>

Matching settings to determine which collections should be grouped together.

When disabled, all inputs merge into one output.

**Mode**: How to match collections

* **Disabled** (default): Merge all into one
* **Tag**: Group by matching tags
* **Attribute**: Group by attribute values
* **Index**: Group by index ranges

**Split Unmatched**: Whether to separate unmatched collections **Limit Matches**: Restrict how many collections per group

⚡ PCG Overridable

</details>

<details>

<summary><strong>Exclusive Partitions</strong> <code>bool</code></summary>

If enabled, each collection can only belong to one group (first match).

* **true** (default): Each collection in one group only
* **false**: Collections can appear in multiple groups

When false, a collection matching multiple criteria will be duplicated and merged into each matching group.

📋 _Visible when Matching Details Mode != Disabled_

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Sort Direction</strong> <code>EPCGExSortDirection</code></summary>

Controls the order in which collections are merged when sorting rules are used.

| Direction               | Description                    |
| ----------------------- | ------------------------------ |
| **Ascending** (default) | Merge in ascending sort order  |
| **Descending**          | Merge in descending sort order |

Affects the final point order in merged collections.

Default: `Ascending`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Carry Over Settings</strong> <code>FPCGExCarryOverDetails</code></summary>

Configuration for which attributes and tags to transfer to merged collections.

//→ See TODO FPCGExCarryOverDetails

</details>

#### Carry-Over Details

<details>

<summary><strong>Preserve Attributes Default Value</strong> <code>bool</code></summary>

Whether to preserve attribute default values during merge.

Default: `false`

</details>

<details>

<summary><strong>Attributes</strong> <code>FPCGExNameFiltersDetails</code></summary>

Attribute name filtering configuration.

//→ See TODO FPCGExNameFiltersDetails

</details>

<details>

<summary><strong>Tags</strong> <code>FPCGExNameFiltersDetails</code></summary>

Tag name filtering configuration.

//→ See TODO FPCGExNameFiltersDetails

</details>

### Matching Details

When Matching Details is enabled, collections are grouped before merging:

**Match Modes:**

* **Tag**: Collections with matching tags merge together
* **Attribute**: Collections with matching attribute values merge together
* **Index**: Collections within index ranges merge together

**Exclusive vs Non-Exclusive:**

* Exclusive: Each collection appears in one group only
* Non-Exclusive: Collections matching multiple criteria appear in all matching groups

**Split Unmatched**: Separate unmatched collections to a different output pin.

### Practical Examples

**Combine All Inputs:**

```
[Multiple scattered point sets]
  ↓
[Merge Points] Mode: Disabled
  ↓
[Single consolidated point set]
```

**Group by Category:**

```
[Collections tagged Trees, Rocks, Props]
  ↓
[Merge Points] Mode: Tag
  ↓
[3 outputs: Trees merged, Rocks merged, Props merged]
```

**Consolidate Similar:**

```
[Collections with Type attribute]
  ↓
[Merge Points] Mode: Attribute, Match: "Type"
  ↓
[Outputs grouped by Type value]
```

### Inputs

| Pin    | Type       | Description                |
| ------ | ---------- | -------------------------- |
| **In** | Point Data | Point collections to merge |

### Outputs

| Pin                      | Type       | Description                                          |
| ------------------------ | ---------- | ---------------------------------------------------- |
| **Out**                  | Point Data | Merged point collections (count depends on matching) |
| **Unmatched** (optional) | Point Data | Unmatched collections (when Split Unmatched = true)  |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Elements/Utils/PCGExMergePoints.h)
