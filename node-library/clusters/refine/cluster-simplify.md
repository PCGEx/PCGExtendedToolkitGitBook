---
description: >-
  Simplify connections by operating on isolated chains of nodes (only two
  neighbors).
icon: share-nodes
---

# Cluster : Simplify

### Overview

This node reduces cluster complexity by identifying **chains** — sequences of vertices where each intermediate vertex has exactly two neighbors. It can collapse these chains, remove dead-end branches, or selectively prune vertices based on angular thresholds.

#### What Is a Chain?

A chain is a path through the cluster where every vertex along the way has exactly two connections:

```
Junction (3+)    Chain (all have 2 neighbors)    Leaf (1)
     ●━━━━━━━━━━━●━━━━━●━━━━━●━━━━━●━━━━━━━━━━━━●
     A           B     C     D     E             F

Chain B-C-D-E connects junction A to leaf F.
B, C, D, E each have exactly 2 neighbors.
```

Chains always terminate at:

* **Junctions**: Vertices with 3+ neighbors (never removed)
* **Leaves**: Vertices with 1 neighbor (dead ends)
* **Breakpoints**: Vertices protected by filters

#### Common Configurations

| Goal                    | Settings                                                          |
| ----------------------- | ----------------------------------------------------------------- |
| **Quick Cleanup**       | `Operate On Leaves Only` = ✓, `Prune Leaves` = ✓                  |
| **Remove Dead Ends**    | `Prune Leaves` = ✓                                                |
| **Collapse All Chains** | `Operate On Leaves Only` = ✗, `Merge Above Angular Threshold` = ✗ |
| **Smooth Corners Only** | `Merge Above Angular Threshold` = ✓, set angle                    |

### How It Works

#### Step 1: Chain Detection

The node identifies all chains in the cluster by finding vertices with exactly 2 neighbors.

#### Step 2: Processing Decision

For each chain, the node decides what to do based on settings:

```
                                    ┌─────────────────────────┐
                                    │  Is chain a dead end?   │
                                    │     (ends in leaf)      │
                                    └───────────┬─────────────┘
                                                │
                              ┌─────────────────┴─────────────────┐
                              │                                   │
                            YES                                  NO
                              │                                   │
                              ▼                                   ▼
                   ┌──────────────────┐              ┌────────────────────┐
                   │  Prune Leaves?   │              │ Operate On Leaves  │
                   └────────┬─────────┘              │      Only?         │
                            │                        └──────────┬─────────┘
              ┌─────────────┴─────────────┐                     │
              │                           │           ┌─────────┴─────────┐
            YES                          NO           │                   │
              │                           │         YES                  NO
              ▼                           ▼           │                   │
        ┌──────────┐              ┌────────────┐      ▼                   ▼
        │ REMOVED  │              │ Simplified │  ┌────────┐      ┌────────────┐
        │ entirely │              │ or kept    │  │ KEPT   │      │ Simplified │
        └──────────┘              └────────────┘  │ as-is  │      │            │
                                                  └────────┘      └────────────┘
```

#### Step 3: Simplification (if applicable)

When a chain is simplified, the node either:

* **Collapses entirely**: Removes all intermediate vertices, creates single edge between endpoints
* **Angle-based**: Only removes vertices where the bend angle is below the threshold

### Behavior

```
═══════════════════════════════════════════════════════════════════
QUICK CLEANUP MODE
Operate On Leaves Only = ✓    Prune Leaves = ✓
═══════════════════════════════════════════════════════════════════

Before:                         After:
      ●                               ●
      │                               │
●━━━━━●━━━━━●━━━━━●━━━━━●       ●━━━━━●━━━━━●━━━━━●━━━━━●
      │                               │
      ●━━━●━━━●  ← dead end           (removed)
          │
          ●

Only dead-end branches are removed. Main structure untouched.

═══════════════════════════════════════════════════════════════════
FULL CHAIN COLLAPSE
Operate On Leaves Only = ✗    Merge Above Angular Threshold = ✗
═══════════════════════════════════════════════════════════════════

Before:                         After:
●━━━●━━━●━━━●━━━●━━━●           ●━━━━━━━━━━━━━━━━━●
A   B   C   D   E   F           A                 F

Entire chain collapsed to single edge. B, C, D, E removed.

═══════════════════════════════════════════════════════════════════
ANGLE-BASED SIMPLIFICATION
Merge Above Angular Threshold = ✓    Angular Threshold = 15°
═══════════════════════════════════════════════════════════════════

Before:                         After:
    C                               C
   ╱ ╲  ← sharp angle              ╱ ╲
  ╱   ╲  (preserved)              ╱   ╲
●╱━━━━━╲●━━━━━●━━━━●            ●╱━━━━━╲●━━━━━━━━━━●
A       D     E    F            A       D          F
        ↑
   nearly straight                 E removed (angle < 15°)
   (removed)

Only vertices on nearly-straight segments are removed.

═══════════════════════════════════════════════════════════════════
WITH KEEP CONDITIONS (Vertex Filters)
═══════════════════════════════════════════════════════════════════

Before (● = protected by filter):

●━━━○━━━●━━━○━━━○━━━●
A   B   C   D   E   F

After (full collapse):

●━━━━━━━●━━━━━━━━━━━●
A       C           F

Protected vertices become breakpoints, splitting the chain.
```

### Use Cases

#### Quick Dead-End Cleanup

Remove stray branches without affecting the main network structure:

```
Settings:
  ✓ Operate On Leaves Only
  ✓ Prune Leaves
```

This is the fastest way to clean up a cluster. Non-leaf chains pass through completely unchanged — no simplification occurs, no vertices are removed from the main structure. Only dead-end branches are eliminated.

#### Reduce Vertex Count

Collapse all chain segments to minimize vertex count while preserving topology:

```
Settings:
  ✗ Operate On Leaves Only
  ✗ Merge Above Angular Threshold
```

Every chain becomes a single edge. Use when you need a minimal representation of the cluster's connectivity.

#### Preserve Sharp Corners

Simplify while keeping meaningful bends:

```
Settings:
  ✗ Operate On Leaves Only
  ✓ Merge Above Angular Threshold
  Angular Threshold: 10-20°
```

Removes vertices on nearly-straight segments while preserving corners and turns.

#### Protect Specific Vertices

Use **Keep Conditions** filters to mark vertices that should never be removed:

```
Connect a filter to Keep Conditions that matches vertices you want to preserve.
These become "breakpoints" that split chains and are never collapsed.
```

### Inputs

| Pin                 | Type             | Description                                                   |
| ------------------- | ---------------- | ------------------------------------------------------------- |
| **Vtx**             | Points           | Cluster vertices                                              |
| **Edges**           | Points           | Cluster edges                                                 |
| **Keep Conditions** | Filter Factories | Vertices passing these filters become breakpoints (preserved) |
| **Edge Filters**    | Filter Factories | Control which edges trigger endpoint preservation or collapse |

### Settings

#### Simplification Settings

<details>

<summary><strong>Operate On Leaves Only</strong> <code>bool</code></summary>

**When enabled**: Only dead-end chains (ending in leaves) are considered for removal. All other chains pass through unchanged — no simplification occurs on the main structure.

**When disabled**: All chains are processed for simplification.

This setting is key for "quick cleanup" mode. Combined with `Prune Leaves`, it removes dead ends without touching anything else.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Prune Leaves</strong> <code>bool</code></summary>

**When enabled**: Chains that end in a leaf (dead end) are removed entirely.

**When disabled**: Leaf chains are simplified or preserved like any other chain.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Edge Filter Role</strong> <code>EPCGExSimplifyClusterEdgeFilterRole</code></summary>

When edge filters are connected, this determines how matched edges affect simplification.

| Option       | Description                                               |
| ------------ | --------------------------------------------------------- |
| **Preserve** | Endpoints of matched edges become breakpoints (protected) |
| **Collapse** | Endpoints of matched edges are marked for collapse        |

Default: `Preserve`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Merge Above Angular Threshold</strong> <code>bool</code></summary>

Enable angle-based simplification. When enabled, vertices are only removed if the angle at that vertex is below the threshold (nearly straight).

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Angular Threshold</strong> <code>double</code></summary>

The angle (in degrees) below which chain vertices are considered nearly collinear and can be removed.

* **Lower values** (5-10°): Conservative, only removes vertices on very straight segments
* **Higher values** (30-45°): Aggressive, removes vertices even at moderate bends

Default: `10`

📋 _Visible when Merge Above Angular Threshold is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

When enabled, removes vertices at sharp angles instead of collinear ones — vertices are removed when the angle is _above_ the threshold rather than below.

Default: `false`

📋 _Visible when Merge Above Angular Threshold is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Fuse Collocated</strong> <code>bool</code></summary>

If enabled, vertices that are very close together (within tolerance) are fused as part of simplification.

Default: `true`

📋 _Visible when Merge Above Angular Threshold is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Tolerance</strong> <code>double</code></summary>

Distance threshold for considering vertices as overlapping for fusion.

Default: `0.001`

📋 _Visible when Merge Above Angular Threshold and Fuse Collocated are enabled_

⚡ PCG Overridable

</details>

#### Data Blending

<details>

<summary><strong>Edge Blending Details</strong> <code>FPCGExBlendingDetails</code></summary>

When multiple edges are collapsed into one, this controls how their attributes are merged.

//→ See TODO FPCGExBlendingDetails

</details>

<details>

<summary><strong>Carry Over Settings</strong> <code>FPCGExCarryOverDetails</code></summary>

Meta filter settings for edge data carry-over during simplification.

//→ See TODO FPCGExCarryOverDetails

</details>

<details>

<summary><strong>Edge Union Data</strong> <code>FPCGExEdgeUnionMetadataDetails</code></summary>

Settings for writing metadata about edge unions created during simplification.

| Setting               | Type    | Default     | Description                                                          |
| --------------------- | ------- | ----------- | -------------------------------------------------------------------- |
| **Write Is Union**    | `bool`  | `false`     | Write a boolean indicating if this edge is a union of multiple edges |
| **Is Union**          | `FName` | `bIsUnion`  | Attribute name for the union flag                                    |
| **Write Union Size**  | `bool`  | `false`     | Write the number of original edges combined into this edge           |
| **Union Size**        | `FName` | `UnionSize` | Attribute name for the union size                                    |
| **Write Is Sub Edge** | `bool`  | `false`     | Write a boolean indicating if this edge was part of a larger chain   |
| **Is Sub Edge**       | `FName` | `SubEdge`   | Attribute name for the sub-edge flag                                 |

⚡ PCG Overridable

</details>

#### Output Settings

<details>

<summary><strong>Cluster Output Settings</strong> <code>FPCGExGraphBuilderDetails</code></summary>

Graph and edge output properties for the simplified cluster.

⚡ PCG Overridable

</details>

### Outputs

| Pin       | Type   | Description                 |
| --------- | ------ | --------------------------- |
| **Vtx**   | Points | Simplified cluster vertices |
| **Edges** | Points | Simplified cluster edges    |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/PCGExSimplifyClusters.h)
