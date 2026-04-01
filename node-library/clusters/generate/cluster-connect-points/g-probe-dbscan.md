---
description: Density-based connectivity/reachability (DBSCAN-style).
icon: circle-dashed
---

# G-Probe : DBSCAN

### Overview

This global probe creates connections based on the DBSCAN (Density-Based Spatial Clustering of Applications with Noise) algorithm. Points are classified as either "core" points (having sufficient neighbors within the search radius) or "border" points (within range of a core point but lacking enough neighbors themselves). Connections are then established based on these classifications, creating clusters that follow local point density.

<figure><img src="../../../../.gitbook/assets/image (289).png" alt=""><figcaption></figcaption></figure>

### How It Works

1. **Core Point Detection**: Identifies points with at least MinPoints neighbors within the search radius (Epsilon)
2. **Border Point Association**: Points near core points but below the neighbor threshold are classified as border points
3. **Connection Rules**: Creates edges based on configuration - core-to-core, and optionally border-to-core connections
4. **Noise Exclusion**: Points that are neither core nor border remain unconnected (noise)

**Usage Notes**

* **Density-Adaptive**: Unlike fixed-neighbor probes (KNN), DBSCAN naturally adapts to varying point densities - sparse regions form fewer connections
* **Core Points**: A point needs MinPoints neighbors (including itself) within the search radius to be considered "core"
* **Border Points**: Can connect to core points but don't propagate connections further
* **Noise Isolation**: Points in very sparse regions remain disconnected, which can be useful for filtering outliers

### Behavior

```
DBSCAN Classification (MinPoints=3, Epsilon=SearchRadius):

   Points within radius:

   ╭─────────╮
   │ •  •  • │  ← Core point (3+ neighbors)
   │    ●    │
   │ •     • │
   ╰─────────╯

   ╭─────────╮
   │    •    │  ← Border point (near core, <3 neighbors)
   │    ○    │
   ╰─────────╯

        ×       ← Noise point (isolated)


Core-to-Core connections:        With Border connections:

    ●───────●                    ●───────●
    │╲     ╱│                    │╲  ○  ╱│
    │ ╲   ╱ │                    │ ╲ │ ╱ │
    │  ╲ ╱  │                    ○──●───●──○
    ●───●───●                    │ ╱ │ ╲ │
                                 │╱  ○  ╲│
                                 ●───────●
```

### Settings

#### DBSCAN Parameters

<details>

<summary><strong>Min Points</strong> <code>int32</code></summary>

The minimum number of points (including the point itself) that must be within the search radius for a point to be classified as a "core" point. Higher values create stricter density requirements.

Default: `3`

Min: `1`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Core to Core Only</strong> <code>bool</code></summary>

When enabled, only creates connections between core points. Border points remain connected to the cluster through their nearest core point but don't add additional edges. This creates a sparser, backbone-like structure.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Border to Nearest Core Only</strong> <code>bool</code></summary>

When enabled, each border point connects only to its single nearest core point. When disabled, border points connect to all reachable core points within the search radius.

Default: `true`

⚡ PCG Overridable

</details>

#### Search Radius (Inherited)

<details>

<summary><strong>Search Radius Input</strong> <code>EPCGExInputValueType</code></summary>

Determines whether the search radius (Epsilon in DBSCAN terminology) is a constant value or read from an attribute.

| Option        | Description                            |
| ------------- | -------------------------------------- |
| **Constant**  | Use the constant value specified below |
| **Attribute** | Read the radius from a point attribute |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Search Radius</strong> <code>double</code></summary>

The Epsilon parameter - the maximum distance to consider points as neighbors. This defines the "reachability" distance for density calculations.

Default: `100`

📋 _Visible when Search Radius Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Offset</strong> <code>double</code></summary>

Additional offset added to the search radius value.

Default: `0`

⚡ PCG Overridable

</details>

### Outputs

| Pin       | Type           | Description                                    |
| --------- | -------------- | ---------------------------------------------- |
| **Probe** | PCGEx \| Probe | The probe factory to connect to Connect Points |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsProbing-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsProbing/Public/Probes/PCGExGlobalProbeDBSCAN.h)
