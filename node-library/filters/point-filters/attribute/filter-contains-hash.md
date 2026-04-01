---
description: >-
  Checks whether a given value hash is contained within one or more sets of
  values.
icon: circle-dashed
---

# Filter : Contains (Hash)

### Overview

This filter tests whether the hash of an attribute value on each point exists within one or more reference sets provided as input. The reference sets are built by hashing a specified attribute from each input dataset. Multiple input sets can be merged into a single lookup table or tested individually with any/all logic. Because this operates on value hashes, comparisons are extremely type-sensitive -- a `float 0` and a `double 0` produce different hashes and will not match.

> This is a [filter-definition](../../common-settings/filter-definition/ "mention")

### How It Works

1. **Set Preparation**: Reads the specified attribute from each input set and builds hash tables of all values found.
2. **Hash Computation**: For each point, hashes the value of Operand A.
3. **Containment Check**: Tests whether the hash exists in the prepared set(s), using merged or individual mode.
4. **Result**: Returns pass if the hash is found (or not found, if inverted).

**Usage Notes**

* **Type Sensitivity**: This is a hash-based comparison, so types must match exactly. A `float` attribute and a `double` attribute with the same numeric value will produce different hashes and will **not** match.
* **Merged vs Individual**: In Merged mode, all input sets are combined into one lookup table. In Individual mode, each set is tested separately, and you can require the value to appear in Any or All sets.

### Behavior

**Merged Mode:**

```
Input Sets:
  Set A values: [10, 20, 30]
  Set B values: [30, 40, 50]
  → Merged: {10, 20, 30, 40, 50}

Point 0: Value=20 → hash found: Pass
Point 1: Value=35 → hash not found: Fail
Point 2: Value=50 → hash found: Pass
```

**Individual Mode (Any):**

```
Set A: {10, 20}    Set B: {30, 40}

Point: Value=20 → In Set A: Yes → Any: Pass
Point: Value=30 → In Set B: Yes → Any: Pass
Point: Value=99 → In neither   → Any: Fail
```

**Individual Mode (All):**

```
Set A: {10, 20}    Set B: {10, 30}

Point: Value=10 → In Set A: Yes, Set B: Yes → All: Pass
Point: Value=20 → In Set A: Yes, Set B: No  → All: Fail
```

### Inputs

| Pin      | Type   | Description                                                            |
| -------- | ------ | ---------------------------------------------------------------------- |
| **Sets** | Points | One or more point datasets providing reference values to match against |

### Settings

<details>

<summary><strong>Mode</strong> <code>EPCGExValueHashMode</code></summary>

How to process input sets.

| Option         | Description                                          |
| -------------- | ---------------------------------------------------- |
| **Merged**     | All input sets are merged into a single lookup table |
| **Individual** | Input sets are kept separate and tested individually |

Default: `Merged`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Inclusion</strong> <code>EPCGExValueHashSetInclusionMode</code></summary>

How to test against individual input sets.

| Option  | Description                                                      |
| ------- | ---------------------------------------------------------------- |
| **Any** | Value must be present in at least one set for the filter to pass |
| **All** | Value must be present in all input sets for the filter to pass   |

Default: `Any`

⚡ PCG Overridable

📋 _Visible when Mode = Individual_

</details>

<details>

<summary><strong>Operand A</strong> <code>FName</code></summary>

The attribute name to read and hash from each tested point.

Default: `Value`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Set Attribute Name</strong> <code>FName</code></summary>

The attribute name to read from the input sets to build the hash lookup tables. If `None`, uses the same name as Operand A.

Default: `None`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

Inverts the result of the filter, so it passes when the value is **not** found in the set(s).

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

[![Static Badge](https://img.shields.io/badge/Source-PCGExFilters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFilters/Public/Filters/Points/PCGExValueHashFilter.h)
