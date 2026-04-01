---
description: Theta/Yao graph spanner - connects to nearest in angular cones.
icon: circle-dashed
---

# G-Probe : Theta Graph

### Overview

This global probe creates a Theta graph (or Yao graph variant) by dividing the space around each point into angular cones and connecting to the nearest neighbor within each cone. This produces a geometric spanner with provable path quality guarantees while maintaining sparse connectivity. The resulting graph has at most NumCones edges per point but ensures efficient paths between any two points.

<figure><img src="../../../../.gitbook/assets/image (294).png" alt=""><figcaption></figcaption></figure>

### How It Works

1. **Cone Setup**: Divides 360° around the cone axis into NumCones equal angular sectors
2. **Neighbor Search**: For each point, finds neighbors within the search radius
3. **Cone Assignment**: Assigns each neighbor to the cone containing its direction vector
4. **Best Selection**: Selects the best neighbor in each cone (nearest for Yao, projected-nearest for Theta)
5. **Edge Creation**: Creates edges to the selected neighbors

**Usage Notes**

* **Theta vs Yao**: Theta graph uses the neighbor with shortest projected distance onto the cone bisector; Yao graph uses the actual nearest neighbor in each cone. Theta typically produces slightly sparser graphs
* **Cone Count**: More cones (higher NumCones) create denser graphs with better spanner properties but more edges. 6-8 cones is typical for good balance
* **Cone Axis**: The axis perpendicular to which cones are arranged. Use UpVector for horizontal connectivity patterns, or another axis for different orientations

### Behavior

```
Theta Graph with 6 cones:

    Cones around point (top view):

            ╲   2   ╱
             ╲     ╱
          1   ╲   ╱   3
               ╲ ╱
         ───────●───────
               ╱ ╲
          6   ╱   ╲   4
             ╱     ╲
            ╱   5   ╲


    Connection per cone:

         •               Cone 1: connect to nearest •
          ╲              Cone 2: (empty)
           ╲             Cone 3: connect to nearest •
            ●────────•   Cone 4: connect to nearest •
           ╱             Cone 5: (empty)
          ╱              Cone 6: connect to nearest •
         •

    Result: At most 6 edges per point (one per cone max)
```

### Settings

#### Cone Configuration

<details>

<summary><strong>Num Cones</strong> <code>int32</code></summary>

The number of angular cones to divide space into. Each cone spans 360°/NumCones. More cones create denser graphs with better path guarantees; fewer cones create sparser graphs.

* **4-6**: Sparse, may have longer paths
* **6-8**: Good balance (recommended)
* **8+**: Dense, near-optimal paths

Default: `6`

Range: `4` - `32`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Cone Axis</strong> <code>FVector</code></summary>

The axis around which cones are arranged. Cones extend perpendicular to this axis. Use UpVector (0,0,1) for horizontal cone patterns, or adjust for different connectivity orientations.

Default: `(0, 0, 1)` (Up)

⚡ PCG Overridable

</details>

<details>

<summary><strong>Use Yao Variant</strong> <code>bool</code></summary>

When enabled, uses Yao graph construction which selects the actual nearest neighbor in each cone. When disabled, uses Theta graph construction which selects based on projected distance onto the cone bisector. Yao graphs are slightly denser but simpler to understand.

Default: `false` (Theta)

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

The maximum distance to search for neighbors in each cone.

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

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsProbing-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsProbing/Public/Probes/PCGExGlobalProbeTheta.h)
