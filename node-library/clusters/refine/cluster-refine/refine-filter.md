---
description: Removes or keeps edges based on connected filter evaluation.
icon: function
---

# Refine : Filter

### Overview

This edge refinement operation uses edge filters to determine which edges to remove from a cluster. Edges that pass the connected filters are removed by default, or kept if the Invert option is enabled. This provides flexible, condition-based edge pruning using any compatible edge filter.

> This is a [refine-operation.md](refine-operation.md "mention")

### How It Works

1. **Filter Evaluation**: Each edge is evaluated against the connected edge filters.
2. **Edge Selection**: Edges that pass the filter criteria are marked based on the Invert setting.
3. **Removal**: Marked edges are removed from the cluster.

**Usage Notes**

* **Requires Filters**: This refinement operation requires edge filters to be connected to function.
* **Default Behavior**: By default, edges that pass the filter are _removed_. Enable Invert to _keep_ passing edges instead.
* **Filter Combination**: Multiple filters can be combined — an edge must pass all filters to be affected.

### Behavior

```
Default (bInvert = false):      Inverted (bInvert = true):

Edges passing filter → REMOVED  Edges passing filter → KEPT
Edges failing filter → KEPT     Edges failing filter → REMOVED
```

### Settings

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

If enabled, filtered out edges are kept while edges that pass the filter are removed. When disabled (default), edges passing the filter are removed.

Default: `false`

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClustersRefine/Public/Refinements/PCGExEdgeRefineByFilter.h)
