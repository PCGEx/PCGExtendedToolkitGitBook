---
description: Creates hierarchical hub-and-spoke network topology.
icon: circle-dashed
---

# G-Probe : Hub & Spoke

### Overview

This global probe creates a two-tier network structure where selected "hub" points serve as central connectors, and remaining "spoke" points connect to their nearest hub(s). Hubs can be selected using various criteria including local density, attribute values, geometric centrality, or K-means clustering. This creates efficient star-topology networks commonly used for distribution systems, transportation hubs, or hierarchical organization.

<figure><img src="../../../../.gitbook/assets/image (290).png" alt=""><figcaption></figcaption></figure>

### How It Works

1. **Hub Selection**: Identifies hub points using the chosen selection mode (density, attribute, centrality, or K-means)
2. **Hub Interconnection**: Optionally connects hubs to each other to form a backbone network
3. **Spoke Assignment**: Connects each non-hub point to its nearest hub (or all hubs within range)
4. **Edge Creation**: Builds the final hub-and-spoke connectivity pattern

**Usage Notes**

* **Hub Count**: The NumHubs parameter controls how many hubs are created. More hubs create shorter spoke distances but more complex hub interconnections
* **K-Means Mode**: Creates virtual hub positions at cluster centroids, then assigns the nearest actual point as each hub
* **Hub Backbone**: Enable "Connect Hubs" to create a secondary network linking hubs together, forming a hierarchical structure

### Behavior

```
Hub & Spoke Topology:

    Input Points:                 Hub Selection:              Final Network:
                                  (By Density)

      •   •   •   •                 •   •   •   •               •───┬───•───┬───•
        •   •   •                     •   ●   •                     │       │
      •   •   •   •                 •   •   •   •               •───●───────●───•
        •   •   •                     •   ●   •                     │       │
      •   •   •   •                 •   •   •   •               •───┴───•───┴───•

    Scattered points            ● = Selected hubs           Spokes connect to
                                (dense regions)             nearest hub, hubs
                                                            interconnect


    bNearestHubOnly=true:        bNearestHubOnly=false:

         •                            •
         │                           ╱│╲
         ●───────●                  ● │ ●
         │       │                 ╱│ │ │╲
         •       •                • │ • │ •
                                   ╲╲ │ ╱╱
                                      •

    Each spoke → 1 hub          Each spoke → all nearby hubs
```

### Settings

#### Hub Selection

<details>

<summary><strong>Hub Selection Mode</strong> <code>EPCGExHubSelectionMode</code></summary>

Determines how hub points are selected from the input.

| Option                | Description                                                                       |
| --------------------- | --------------------------------------------------------------------------------- |
| **By Local Density**  | Points in dense regions (many nearby neighbors) become hubs                       |
| **By Attribute**      | Points with the highest values of the specified attribute become hubs             |
| **By Centrality**     | Points closest to the geometric center of their local region become hubs          |
| **K-Means Centroids** | Runs K-means clustering and selects points nearest to each cluster center as hubs |

Default: `By Local Density`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Num Hubs</strong> <code>int32</code></summary>

The number of hub points to select. For K-Means mode, this is the K value (number of clusters).

Default: `10`

Min: `1`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Hub Attribute</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute used to rank points for hub selection when using "By Attribute" mode. Points with the highest values become hubs.

Default: `$Density`

📋 _Visible when Hub Selection Mode = By Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>K-Means Iterations</strong> <code>int32</code></summary>

Number of iterations for the K-means algorithm when using K-Means Centroids mode. More iterations improve cluster quality but increase computation time.

Default: `10`

Range: `1` - `100`

📋 _Visible when Hub Selection Mode = K-Means Centroids_

⚡ PCG Overridable

</details>

#### Connection Options

<details>

<summary><strong>Connect Hubs</strong> <code>bool</code></summary>

When enabled, creates connections between hub points in addition to hub-spoke connections. This forms a backbone network linking all hubs together.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Nearest Hub Only</strong> <code>bool</code></summary>

When enabled, each spoke point connects only to its single nearest hub. When disabled, spoke points connect to all hubs within the search radius.

Default: `true`

⚡ PCG Overridable

</details>

#### Search Radius (Inherited)

<details>

<summary><strong>Search Radius Input</strong> <code>EPCGExInputValueType</code></summary>

Determines whether the search radius is a constant value or read from an attribute.

| Option        | Description                            |
| ------------- | -------------------------------------- |
| **Constant**  | Use the constant value specified below |
| **Attribute** | Read the radius from a point attribute |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Search Radius</strong> <code>double</code></summary>

The maximum distance for hub-spoke connections (and optionally hub-hub connections).

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

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsProbing-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsProbing/Public/Probes/PCGExGlobalProbeHubSpoke.h)
