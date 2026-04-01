---
description: Generates a hash from the input data, based on an attribute or property.
icon: circle
---

# Attribute Hash

### Overview

This node computes a deterministic hash value from attribute data on each point collection. The hash can be output as an attribute, a tag, or both. Hash computation can be configured to consider all values, unique values only, and can optionally sort values before hashing for order-independent results.

### How It Works

1. **Read Attribute**: Reads the specified attribute values from all points in the collection.
2. **Process Values**: Depending on scope, extracts all values or only unique values.
3. **Sort (Optional)**: Optionally sorts values to ensure consistent hash regardless of point order.
4. **Compute Hash**: Generates a deterministic integer hash from the processed values.
5. **Output**: Writes the hash to an attribute and/or tag on the data.

**Usage Notes**

* **Deterministic**: The same input data with the same settings will always produce the same hash.
* **Data-Level Output**: The hash is written at the @Data domain level (on the collection, not individual points).
* **Unique Scope**: Using "Uniques" scope ignores duplicate values, useful for set-like comparisons.
* **Sorting**: Enable sorting for order-independent hashing where point order shouldn't affect the result.

### Behavior

```
Hash Computation:

Input Points with "Type" attribute:
   Point 0: Type = "Tree"
   Point 1: Type = "Bush"
   Point 2: Type = "Tree"
   Point 3: Type = "Rock"

Scope = All, Sort = true:
   Hash = hash(["Bush", "Rock", "Tree", "Tree"])

Scope = Uniques, Sort = true:
   Hash = hash(["Bush", "Rock", "Tree"])

Output written to @Data.Hash attribute
```

### Inputs

| Pin    | Type   | Description                                   |
| ------ | ------ | --------------------------------------------- |
| **In** | Points | Input point collections to compute hashes for |

### Settings

#### Hash Configuration

<details>

<summary><strong>Source Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute or property to read values from for hash computation.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Scope</strong> <code>EPCGExDataHashScope</code></summary>

Determines which values to include in the hash computation.

| Option      | Description                                        |
| ----------- | -------------------------------------------------- |
| **All**     | Include all attribute values, including duplicates |
| **Uniques** | Include only unique attribute values               |

Default: `Uniques`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Sort Input Values</strong> <code>bool</code></summary>

When enabled, sorts values before hashing. This ensures the hash is independent of point order.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Sorting</strong> <code>EPCGExSortDirection</code></summary>

The sort direction when sorting is enabled.

| Option         | Description                     |
| -------------- | ------------------------------- |
| **Ascending**  | Sort values in ascending order  |
| **Descending** | Sort values in descending order |

Default: `Ascending`

⚡ PCG Overridable

</details>

#### Output Settings

<details>

<summary><strong>Output Name</strong> <code>FName</code></summary>

The name used for the output hash (applies to both attribute and tag output).

Default: `Hash`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output To Tags</strong> <code>bool</code></summary>

When enabled, adds the hash as a tag to the output data.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output To Attribute</strong> <code>bool</code></summary>

When enabled, writes the hash to an attribute on the @Data domain.

Default: `true`

⚡ PCG Overridable

</details>

#### Inherited Settings

→ See Points Processor Settings for common point processing settings.

### Outputs

| Pin     | Type   | Description                                 |
| ------- | ------ | ------------------------------------------- |
| **Out** | Points | Point collections with computed hash values |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsMeta-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsMeta/Public/Elements/PCGExAttributeHash.h)
