---
description: Outputs separate buckets of points based on attribute values.
icon: circle-plus
---

# Partition by Values

### Overview

This node partitions point collections into separate output buckets based on computed partition keys derived from attribute values. Each unique key combination produces a distinct output collection. The node accepts Partition Rule sub-nodes to define how attributes are transformed into keys, enabling flexible multi-attribute partitioning schemes.

### How It Works

1. **Gather Rules**: Collects partition rules from connected Partition Rule sub-nodes.
2. **Compute Keys**: For each point, evaluates all rules to produce a combined partition key.
3. **Group Points**: Points with identical combined keys are grouped together.

**Usage Notes**

* **Merge Before**: It is recommended to use a Merge node before partitioning to combine data from multiple sources.
* **Multi-Attribute Keys**: Connect multiple Partition Rule sub-nodes to partition by combinations of attributes.
* **Key Naming**: Output collections are named using the combined partition key values.

### Behavior

```
Multi-Attribute Partitioning:

Input Points with Biome and Height attributes:
   ┌─────────┬────────┬────────┐
   │ Point   │ Biome  │ Height │
   ├─────────┼────────┼────────┤
   │ A       │ Forest │ 100    │
   │ B       │ Forest │ 200    │
   │ C       │ Desert │ 100    │
   │ D       │ Forest │ 100    │
   │ E       │ Desert │ 200    │
   └─────────┴────────┴────────┘

With Rules: Biome (no filter) + Height (filter size=100):

   Bucket "Forest_1" → [A, D]  (Biome=Forest, Height key=1)
   Bucket "Forest_2" → [B]     (Biome=Forest, Height key=2)
   Bucket "Desert_1" → [C]     (Biome=Desert, Height key=1)
   Bucket "Desert_2" → [E]     (Biome=Desert, Height key=2)
```

### Inputs

| Pin                | Type   | Description                                                     |
| ------------------ | ------ | --------------------------------------------------------------- |
| **In**             | Points | Input point collection to partition                             |
| **PartitionRules** | Params | Partition Rule sub-nodes defining how to compute partition keys |

### Settings

<details>

<summary><strong>Split Output</strong> <code>bool</code></summary>

When enabled, outputs separate point collections for each partition. When disabled, only writes partition identifier values to attributes without splitting.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Key Sum</strong> <code>bool</code></summary>

When enabled, writes the sum of all partition rule keys to an attribute.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Key Sum Attribute Name</strong> <code>FName</code></summary>

The attribute name to write the key sum to. Note that this value is not guaranteed to be unique across different key combinations.

Default: `KeySum`

📋 _Visible when Write Key Sum is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Partition Rules</strong> <code>TArray&#x3C;FPCGExPartitonRuleConfig></code></summary>

Array of partition rule configurations. Each rule transforms an attribute value into a partition key component.

→ See Partition Rule for rule configuration details.

⚡ PCG Overridable

</details>

#### Inherited Settings

→ See Points Processor Settings for common point processing settings.

### Outputs

| Pin           | Type   | Description                                                                          |
| ------------- | ------ | ------------------------------------------------------------------------------------ |
| **(Dynamic)** | Points | Separate point collections for each unique partition key (when Split Output enabled) |
| **Out**       | Points | Original points with partition attributes (when Split Output disabled)               |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsMeta-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsMeta/Public/Elements/Partition/PCGExModularPartitionByValues.h)
