---
description: Removes leaf vertices (dead ends) and their connecting edges from the cluster.
icon: function
---

# Remove Leaves

### Overview

This edge refinement operation identifies and removes leaf nodes — vertices that have only one connection (dead ends). Both the leaf vertex and its single connecting edge are removed. This is useful for pruning terminal branches from a cluster while preserving the core connected structure.

> This is a [refine-operation.md](refine-operation.md "mention")

### How It Works

1. **Leaf Detection**: Identifies vertices with exactly one connected edge (leaves).
2. **Validation**: Confirms the vertex is a true leaf and not part of a more complex structure.
3. **Removal**: Marks both the leaf vertex and its connecting edge as invalid.

**Usage Notes**

* **Single Pass**: This operation removes only the outermost layer of leaves. Vertices that become leaves after removal are not affected.
* **Use Recursive**: For complete branch removal, use Remove Leaves Recursive instead.
* **Preserves Structure**: Vertices with two or more connections are never removed.

### Behavior

```
Before:                         After:

    A
    |
    B---C---D                   B---C---D
        |                           |
        E---F                       E
            |
            G (leaf)

Leaves removed: A, G (and their edges)
F becomes a new leaf but is NOT removed in this pass.
```

### Settings

This refinement operation has no additional settings. It simply removes all current leaf vertices.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClustersRefine/Public/Refinements/PCGExEdgeRefineRemoveLeaves.h)
