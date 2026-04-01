---
description: Connect to the N closest neighbors within search radius.
icon: circle-dashed
---

# Probe : Closest

### Overview

This per-point probe finds and connects to the closest neighbors within the search radius. Unlike the global KNN probe, this operates on a per-point basis during candidate processing, making it suitable for combining with other probes. The maximum number of connections can be constant or driven by a per-point attribute.

<figure><img src="../../../../.gitbook/assets/image (285).png" alt=""><figcaption></figcaption></figure>

### How It Works

1. **Candidate Gathering**: Collects all points within the search radius as candidates
2. **Distance Sorting**: Sorts candidates by distance from the source point
3. **Connection Selection**: Selects up to MaxConnections closest candidates
4. **Coincidence Filtering**: Optionally skips candidates too close to already-selected connections
5. **Edge Creation**: Creates edges to the selected neighbors

**Usage Notes**

* **Per-Point Probe**: Unlike global KNN, this probe processes each point individually and can be combined with other probes in the same Connect Points node
* **Variable Connections**: Use attribute mode for MaxConnections to allow different points to have different connection limits
* **Coincidence Prevention**: Helps avoid selecting multiple neighbors in nearly the same direction

### Behavior

```
Closest Probe (MaxConnections=3):

    Candidates within radius:        Selected connections:

         •(4)                             •
          ╲
       •(2)╲   •(5)                      •(2)  •
            ╲ ╱                           ╲   ╱
             ●                             ╲ ╱
            ╱ ╲                             ●
       •(1)╱   •(6)                        ╱
          ╱                               ╱
         •(3)                            •(1)
                                         │
    Numbers = distance rank              •(3)

    Connects to 3 closest: (1), (2), (3)
```

### Settings

#### Connection Configuration

<details>

<summary><strong>Max Connections Input</strong> <code>EPCGExInputValueType</code></summary>

Determines whether the maximum connections is a constant value or read from an attribute.

| Option        | Description                            |
| ------------- | -------------------------------------- |
| **Constant**  | Use the constant value specified below |
| **Attribute** | Read the limit from a point attribute  |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Connections</strong> <code>int32</code></summary>

The maximum number of closest neighbors to connect to per point.

Default: `1`

Min: `0`

📋 _Visible when Max Connections Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Connections (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute containing the maximum connections value per point.

📋 _Visible when Max Connections Input = Attribute_

⚡ PCG Overridable

</details>

#### Coincidence Prevention

<details>

<summary><strong>Prevent Coincidence</strong> <code>bool</code></summary>

When enabled, attempts to prevent selecting multiple connections that are in roughly the same direction from the source point.

Default: `true`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Coincidence Prevention Tolerance</strong> <code>double</code></summary>

The distance threshold for coincidence detection. Candidates closer than this to an already-selected connection direction may be skipped.

Default: `0.001`

Min: `0.00001`

📋 _Visible when Prevent Coincidence is enabled_

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

The maximum distance to search for neighbors.

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

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsProbing-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsProbing/Public/Probes/PCGExProbeClosest.h)
