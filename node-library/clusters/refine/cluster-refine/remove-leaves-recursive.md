---
description: >-
  Recursively removes leaf vertices and their edges until no leaves remain or
  the iteration limit is reached.
icon: function
---

# Remove Leaves (Recursive)

### Overview

This edge refinement operation repeatedly removes leaf nodes — vertices with only one connection — until the cluster contains no more dead ends. When a leaf is removed, its parent may become a new leaf, which is then removed in the next iteration. This continues until only vertices with two or more connections remain, effectively stripping away all terminal branches.

> This is a [refine-operation.md](refine-operation.md "mention")

### How It Works

1. **Initial Scan**: Identifies all current leaf vertices (vertices with exactly one connection).
2. **Parallel Removal**: Removes all identified leaves and their connecting edges simultaneously.
3. **Cascade Detection**: Identifies vertices that became leaves after the removal.
4. **Iteration**: Repeats the process until no leaves remain or max iterations is reached.

**Usage Notes**

* **Complete Branch Removal**: Unlike the non-recursive version, this removes entire branches down to their junction points.
* **Max Iterations**: Use this to limit how deep the pruning goes — useful for preserving some branch structure.
* **Junction Preservation**: Vertices with 3+ connections are never removed, as they always have multiple paths.

### Behavior

```
Before:                         After (unlimited iterations):

    A
    |
    B---C---D                       C
        |                           |
        E---F                       E
            |
            G

Iteration 1: Remove A, D, G (leaves)
Iteration 2: Remove B, F (became leaves)
Result: Only C-E remain (both have 1+ connections to each other)
```

### Settings

<details>

<summary><strong>Max Iterations</strong> <code>int32</code></summary>

Maximum number of removal iterations to perform. Set to 0 or less for unlimited iterations, which continues until no leaves remain in the cluster.

Default: `0` (unlimited)

⚡ PCG Overridable

</details>

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClustersRefine/Public/Refinements/PCGExEdgeRefineRemoveLeavesRecursive.h)
