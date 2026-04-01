---
description: Convert an asset collection to an attribute set.
icon: circle
---

# Asset Collection to Set

### Overview

This node converts an Asset Collection into an attribute set where each point represents one entry from the collection. The output includes configurable attributes such as asset paths, weights, categories, bounds, and nesting depth. This allows collections to be used as data sources for further PCG processing or analysis.

### How It Works

1. **Load Collection**: References and loads the specified Asset Collection
2. **Entry Expansion**: Processes entries based on sub-collection handling mode (expand, pick, or ignore)
3. **Attribute Writing**: Creates one point per entry with selected attributes written to point data
4. **Filtering**: Optionally removes duplicates and invalid/empty entries

**Usage Notes**

* **Attribute Set Output**: The output is a standard PCG attribute set
* **Sub-Collection Control**: Choose how nested collections are handled - expand all entries, pick one randomly, or ignore them
* **Duplicate Detection**: Duplicates are identified by matching asset path and category

### Behavior

```
Input Collection:
  Entry 1: MeshA (weight=10, category="Props")
  Entry 2: MeshB (weight=5, category="Props")
  Entry 3: SubCollection → [MeshC, MeshD]

SubCollectionHandling = Expand:
Output Points:
  Point 0: AssetPath="MeshA", Weight=10, Category="Props"
  Point 1: AssetPath="MeshB", Weight=5, Category="Props"
  Point 2: AssetPath="MeshC", Weight=..., NestingDepth=1
  Point 3: AssetPath="MeshD", Weight=..., NestingDepth=1

SubCollectionHandling = PickRandomWeighted:
  Point 0: AssetPath="MeshA", Weight=10, Category="Props"
  Point 1: AssetPath="MeshB", Weight=5, Category="Props"
  Point 2: AssetPath="MeshC or MeshD", Weight=..., NestingDepth=1
```

### Outputs

| Pin               | Type          | Description                                   |
| ----------------- | ------------- | --------------------------------------------- |
| **Attribute Set** | Attribute Set | Point set with one point per collection entry |

### Settings

#### Collection Configuration

<details>

<summary><strong>Asset Collection</strong> <code>TSoftObjectPtr</code></summary>

The Asset Collection to convert to an attribute set. Can be any collection type (Actor, Mesh, PCG Data Asset).

Default: None

⚡ PCG Overridable

</details>

<details>

<summary><strong>Sub Collection Handling</strong> <code>EPCGExSubCollectionToSet</code></summary>

Determines how sub-collections (nested collections) are processed.

| Option              | Description                                                            |
| ------------------- | ---------------------------------------------------------------------- |
| **Ignore**          | Skip sub-collection entries entirely                                   |
| **Expand**          | Recursively expand sub-collections, adding all nested entries          |
| **Random**          | Pick one entry from the sub-collection at random (uniform)             |
| **Random Weighted** | Pick one entry from the sub-collection using weighted random selection |
| **First Item**      | Pick the first entry from the sub-collection                           |
| **Last Item**       | Pick the last entry from the sub-collection                            |

Default: `Random Weighted`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Allow Duplicates</strong> <code>bool</code></summary>

Whether to allow duplicate entries in the output. Duplicates are identified by matching asset path and category.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Omit Invalid And Empty</strong> <code>bool</code></summary>

Filter out invalid entries (missing asset references, zero weight) and empty sub-collections from the output.

Default: `true`

⚡ PCG Overridable

</details>

#### Output Attributes

<details>

<summary><strong>Write Asset Path</strong> <code>bool</code></summary>

Write the asset's soft object path to an attribute.

**Asset Path** `FName`: Name of the attribute to write the path to.

Default: Enabled, attribute name `AssetPath`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Weight</strong> <code>bool</code></summary>

Write the entry's selection weight to an attribute.

**Weight** `FName`: Name of the attribute to write the weight to.

Default: Enabled, attribute name `Weight`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Category</strong> <code>bool</code></summary>

Write the entry's category name to an attribute.

**Category** `FName`: Name of the attribute to write the category to.

Default: Disabled, attribute name `Category`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Extents</strong> <code>bool</code></summary>

Write the asset's bounding box extents (half-size) to an attribute.

**Extents** `FName`: Name of the attribute to write the extents to.

Default: Disabled, attribute name `Extents`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Bounds Min</strong> <code>bool</code></summary>

Write the asset's bounding box minimum corner to an attribute.

**BoundsMin** `FName`: Name of the attribute to write the minimum bounds to.

Default: Disabled, attribute name `BoundsMin`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Bounds Max</strong> <code>bool</code></summary>

Write the asset's bounding box maximum corner to an attribute.

**BoundsMax** `FName`: Name of the attribute to write the maximum bounds to.

Default: Disabled, attribute name `BoundsMax`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Nesting Depth</strong> <code>bool</code></summary>

Write the entry's nesting depth (0 for top-level entries, 1+ for sub-collection entries) to an attribute.

**Nesting Depth** `FName`: Name of the attribute to write the depth to.

Default: Disabled, attribute name `NestingDepth`

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExCollections-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExCollections/Public/Elements/PCGExAssetCollectionToSet.h)
