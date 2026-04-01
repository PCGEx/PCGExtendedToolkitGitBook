---
description: >-
  Filters vertices by comparing an attribute value against values from adjacent
  vertices or connected edges.
icon: circle-dashed
---

# Vtx Filter : Adjacency

### Overview

This filter answers the question: **"How does this vertex relate to its neighbors?"**

It compares a value (Operand A) against values from neighboring vertices or connecting edges (Operand B), then determines if the vertex passes based on how many neighbors satisfy the comparison. This enables powerful neighborhood-aware filtering:

* **Find local maxima/minima**: Vertices whose value is greater/smaller than all neighbors
* **Detect uniformity**: Vertices where all neighbors share the same attribute value
* **Threshold-based selection**: Vertices where at least N neighbors (or N% of neighbors) match a condition
* **Aggregate comparisons**: Compare against the average, min, max, or sum of neighbor values

#### The Two Modes

The filter operates in one of two fundamental modes:

| Mode     | Question It Answers                           |
| -------- | --------------------------------------------- |
| **All**  | "Do ALL neighbors satisfy the comparison?"    |
| **Some** | "Do ENOUGH neighbors satisfy the comparison?" |

### How It Works

#### Mode: All Neighbors

When set to **All**, every neighbor must satisfy the comparison for the vertex to pass.

The **Consolidation** setting controls how neighbor values are evaluated:

| Consolidation  | Behavior                                                                    |
| -------------- | --------------------------------------------------------------------------- |
| **Individual** | Each neighbor is tested separately. ALL must pass. Fails on first mismatch. |
| **Average**    | Compute the average of all neighbor values, then do a single comparison.    |
| **Min**        | Find the minimum neighbor value, then do a single comparison.               |
| **Max**        | Find the maximum neighbor value, then do a single comparison.               |
| **Sum**        | Sum all neighbor values, then do a single comparison.                       |

#### Mode: Some Neighbors

When set to **Some**, the filter counts how many neighbors pass the comparison, then checks if that count meets a threshold.

**Step 1**: Test each neighbor individually, count successes.

**Step 2**: Compare the success count against the threshold:

* **Discrete threshold**: A fixed number (e.g., "at least 3 neighbors")
* **Relative threshold**: A percentage of total neighbors (e.g., "at least 50%")

**Step 3**: The threshold comparison determines pass/fail using operators like `>=`, `<=`, `==`, etc.

### Behavior

```
═══════════════════════════════════════════════════════════════════
MODE: ALL + INDIVIDUAL
"Is my value greater than ALL my neighbors?"
═══════════════════════════════════════════════════════════════════

Operand A = Vertex "Height"    Operand B = Neighbor "Height"
Comparison = StrictlyGreater (A > B)

    [5]──[3]                    [5]──[7]
     │                           │
    [2]                         [2]

Vertex [5] neighbors: [3, 2]    Vertex [5] neighbors: [7, 2]
  5 > 3? YES                      5 > 7? NO ← fails immediately
  5 > 2? YES
  Result: PASS ✓                  Result: FAIL ✗

═══════════════════════════════════════════════════════════════════
MODE: ALL + AVERAGE
"Is my value greater than the AVERAGE of my neighbors?"
═══════════════════════════════════════════════════════════════════

Operand A = Vertex "Height"    Operand B = Neighbor "Height"
Comparison = StrictlyGreater

    [10]                        [10]
   / │ \                       / │ \
 [4][6][8]                   [8][12][16]

Neighbors: [4, 6, 8]           Neighbors: [8, 12, 16]
Average: 6                     Average: 12
10 > 6? YES                    10 > 12? NO
Result: PASS ✓                 Result: FAIL ✗

═══════════════════════════════════════════════════════════════════
MODE: SOME + DISCRETE THRESHOLD
"Do AT LEAST 2 neighbors match my value?"
═══════════════════════════════════════════════════════════════════

Operand A = Vertex "Type"      Operand B = Neighbor "Type"
Comparison = NearlyEqual       Threshold = 2 (EqualOrGreater)

     [A]                         [A]
    / │ \                       / │ \
  [A][B][A]                   [A][B][C]

Matches: 2 (the two [A]s)      Matches: 1 (only one [A])
2 >= 2? YES                    1 >= 2? NO
Result: PASS ✓                 Result: FAIL ✗

═══════════════════════════════════════════════════════════════════
MODE: SOME + RELATIVE THRESHOLD
"Do MORE THAN HALF my neighbors have higher value?"
═══════════════════════════════════════════════════════════════════

Operand A = Vertex "Priority"  Operand B = Neighbor "Priority"
Comparison = StrictlySmaller   Threshold = 50% (StrictlyGreater)

     [5]                         [5]
    / │ \                       / │ \
  [8][9][3]                   [8][4][3]

Neighbors higher: 2 of 3       Neighbors higher: 1 of 3
50% of 3 = 1.5 → 2 (rounded)   50% of 3 = 1.5 → 2 (rounded)
2 > 2? NO                      1 > 2? NO
Result: FAIL ✗                 Result: FAIL ✗
```

### Common Use Cases

#### Find Local Maxima

Vertices whose value is higher than all neighbors:

* Mode: `All`, Consolidation: `Individual`
* Comparison: `StrictlyGreater` (Operand A > Operand B)
* Operand A & B: Same attribute (e.g., both read "Height")

#### Find Vertices in Uniform Regions

Vertices where all neighbors share the same attribute value:

* Mode: `All`, Consolidation: `Individual`
* Comparison: `NearlyEqual`
* Operand A & B: Same attribute

#### Find Well-Connected Hubs

Vertices where most neighbors have similar properties:

* Mode: `Some`, Threshold Type: `Relative`, Threshold: `0.75` (75%)
* Threshold Comparison: `EqualOrGreater`

#### Filter by Edge Properties

Vertices connected only by edges with specific attributes:

* Operand B Source: `Edge` (instead of Vtx)
* Mode: `All`, Consolidation: `Individual`
* Compare vertex attribute against edge attributes

> This is a [filter-definition](../common-settings/filter-definition/ "mention")

### Settings

#### Adjacency Settings

<details>

<summary><strong>Mode</strong> <code>EPCGExAdjacencyTestMode</code></summary>

How many adjacent items should be tested.

| Option   | Description                                                           |
| -------- | --------------------------------------------------------------------- |
| **All**  | All neighbors must satisfy the comparison (or use consolidated value) |
| **Some** | Count neighbors that pass, compare count against threshold            |

Default: `Some`

</details>

<details>

<summary><strong>Consolidation</strong> <code>EPCGExAdjacencyGatherMode</code></summary>

How to consolidate neighbor values before comparison.

| Option         | Description                                        |
| -------------- | -------------------------------------------------- |
| **Individual** | Test each neighbor separately; all must pass       |
| **Average**    | Compare against the average of all neighbor values |
| **Min**        | Compare against the minimum neighbor value         |
| **Max**        | Compare against the maximum neighbor value         |
| **Sum**        | Compare against the sum of all neighbor values     |

Default: `Average`

📋 _Visible when Mode = All_

</details>

<details>

<summary><strong>Threshold Comparison</strong> <code>EPCGExComparison</code></summary>

How to compare the success count against the threshold.

| Option              | Description                      |
| ------------------- | -------------------------------- |
| **EqualOrGreater**  | At least N neighbors must pass   |
| **EqualOrSmaller**  | At most N neighbors must pass    |
| **StrictlyEqual**   | Exactly N neighbors must pass    |
| **StrictlyGreater** | More than N neighbors must pass  |
| **StrictlySmaller** | Fewer than N neighbors must pass |

Default: `NearlyEqual`

📋 _Visible when Mode = Some_

</details>

<details>

<summary><strong>Threshold Type</strong> <code>EPCGExMeanMeasure</code></summary>

Whether the threshold is an absolute count or a percentage.

| Option       | Description                                     |
| ------------ | ----------------------------------------------- |
| **Discrete** | Fixed integer count (e.g., 3 neighbors)         |
| **Relative** | Percentage of total neighbors (e.g., 0.5 = 50%) |

Default: `Discrete`

📋 _Visible when Mode = Some_

</details>

<details>

<summary><strong>Threshold (Discrete)</strong> <code>int32</code></summary>

The exact number of neighbors that must pass the comparison.

Default: `1`

📋 _Visible when Mode = Some, Threshold Type = Discrete, Threshold Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Threshold (Relative)</strong> <code>double</code></summary>

The percentage of neighbors that must pass (0.0 to 1.0).

Default: `0.5`

📋 _Visible when Mode = Some, Threshold Type = Relative, Threshold Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Rounding</strong> <code>EPCGExRelativeThresholdRoundingMode</code></summary>

How to round relative thresholds to integer counts.

| Option    | Description                          |
| --------- | ------------------------------------ |
| **Round** | Standard rounding (0.5 → 1, 0.4 → 0) |
| **Floor** | Always round down (0.9 → 0)          |
| **Ceil**  | Always round up (0.1 → 1)            |

Default: `Round`

📋 _Visible when Threshold Type = Relative_

</details>

#### Comparison Settings

<details>

<summary><strong>Compare Against</strong> <code>EPCGExInputValueType</code></summary>

Whether to use a constant value or read Operand A from a vertex attribute.

| Option        | Description                            |
| ------------- | -------------------------------------- |
| **Constant**  | Use a constant value for Operand A     |
| **Attribute** | Read Operand A from the current vertex |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Operand A (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Attribute to read Operand A from on the current vertex.

📋 _Visible when Compare Against = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Operand A</strong> <code>double</code></summary>

Constant value for Operand A.

Default: `0`

📋 _Visible when Compare Against = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Comparison</strong> <code>EPCGExComparison</code></summary>

The comparison operator for testing Operand A against Operand B.

| Option               | Description                |
| -------------------- | -------------------------- |
| **StrictlyGreater**  | A > B                      |
| **StrictlySmaller**  | A < B                      |
| **EqualOrGreater**   | A >= B                     |
| **EqualOrSmaller**   | A <= B                     |
| **StrictlyEqual**    | A == B                     |
| **StrictlyNotEqual** | A != B                     |
| **NearlyEqual**      | A ≈ B (within tolerance)   |
| **NearlyNotEqual**   | A !≈ B (outside tolerance) |

Default: `NearlyEqual`

</details>

<details>

<summary><strong>Operand B Source</strong> <code>EPCGExClusterElement</code></summary>

Where to read Operand B from — the neighboring vertex or the connecting edge.

| Option   | Description                                    |
| -------- | ---------------------------------------------- |
| **Vtx**  | Read from the neighboring vertex               |
| **Edge** | Read from the edge connecting to that neighbor |

Default: `Vtx`

</details>

<details>

<summary><strong>Operand B (Neighbor)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Attribute to read Operand B from on the neighbor vertex or connecting edge.

</details>

<details>

<summary><strong>Tolerance</strong> <code>double</code></summary>

Tolerance value for approximate comparison modes.

Default: `DBL_COMPARE_TOLERANCE`

📋 _Visible when Comparison = NearlyEqual or NearlyNotEqual_

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Filters/Nodes/PCGExNodeAdjacencyFilter.h)
