---
description: Amplitude of a vertex, based on neighboring connections.
icon: circle-dashed
---

# Vtx : Amplitude

### Overview

This sub-node computes amplitude metrics for vertices based on the spatial extent of their connections to neighbors. Amplitude represents how far edges extend from a vertex in various directions. The node can output minimum, maximum, and range values as either scalar lengths or per-component vectors, plus a signed amplitude value relative to an up direction.

> This is a [vtx-property-provider.md](vtx-property-provider.md "mention")

### How It Works

1. **Analyze Connections**: Examine all edges connected to each vertex
2. **Compute Amplitudes**: Calculate min/max/range of edge extents
3. **Determine Sign**: Optionally compute signed amplitude relative to up vector
4. **Write Attributes**: Output computed values to vertex attributes

**Usage Notes**

* **Length vs Individual**: Length mode outputs a single scalar (edge length), while Individual mode outputs per-component (X,Y,Z) vectors.
* **Sign Calculation**: The sign output measures whether neighbors are predominantly above or below the vertex relative to the up vector.

### Behavior

```
Amplitude Calculation:

Vertex A with edges to neighbors:
  → B at offset (100, 0, 50)   length = 111.8
  → C at offset (-50, 30, 0)   length = 58.3
  → D at offset (0, -80, 20)   length = 82.5

Length Mode:
  MinAmplitude = 58.3  (shortest edge)
  MaxAmplitude = 111.8 (longest edge)
  Range = 53.5         (max - min)

Individual Mode (Absolute):
  MinAmplitude = (0, 0, 0)      (min per component)
  MaxAmplitude = (100, 80, 50)  (max per component)

Sign (Up = Z):
  Average neighbor Z = (50 + 0 + 20) / 3 = 23.3
  Sign = positive (neighbors trend upward)
```

### Settings

#### Minimum Amplitude

<details>

<summary><strong>Write Min Amplitude</strong> <code>bool</code></summary>

When enabled, writes the minimum amplitude value to an attribute.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Min</strong> <code>FName</code></summary>

Name of the attribute to write minimum amplitude to.

Default: `MinAmplitude`

📋 _Visible when Write Min Amplitude = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Absolute (Min)</strong> <code>bool</code></summary>

When enabled, uses absolute values for component-wise minimum calculation.

Default: `true`

📋 _Visible when Write Min Amplitude = true and Min Mode = Individual_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Mode (Min)</strong> <code>EPCGExVtxAmplitudeMode</code></summary>

How the minimum amplitude is computed.

| Option         | Description                         |
| -------------- | ----------------------------------- |
| **Length**     | Output scalar edge length           |
| **Individual** | Output per-component (X,Y,Z) vector |

Default: `Length`

📋 _Visible when Write Min Amplitude = true_

⚡ PCG Overridable

</details>

#### Maximum Amplitude

<details>

<summary><strong>Write Max Amplitude</strong> <code>bool</code></summary>

When enabled, writes the maximum amplitude value to an attribute.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max</strong> <code>FName</code></summary>

Name of the attribute to write maximum amplitude to.

Default: `MaxAmplitude`

📋 _Visible when Write Max Amplitude = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Absolute (Max)</strong> <code>bool</code></summary>

When enabled, uses absolute values for component-wise maximum calculation.

Default: `true`

📋 _Visible when Write Max Amplitude = true and Max Mode = Individual_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Mode (Max)</strong> <code>EPCGExVtxAmplitudeMode</code></summary>

How the maximum amplitude is computed.

| Option         | Description                         |
| -------------- | ----------------------------------- |
| **Length**     | Output scalar edge length           |
| **Individual** | Output per-component (X,Y,Z) vector |

Default: `Length`

📋 _Visible when Write Max Amplitude = true_

⚡ PCG Overridable

</details>

#### Amplitude Range

<details>

<summary><strong>Write Amplitude Range</strong> <code>bool</code></summary>

When enabled, writes the amplitude range (max - min) to an attribute.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Range</strong> <code>FName</code></summary>

Name of the attribute to write amplitude range to.

Default: `AmplitudeRange`

📋 _Visible when Write Amplitude Range = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Absolute (Range)</strong> <code>bool</code></summary>

When enabled, uses absolute values for component-wise range calculation.

Default: `true`

📋 _Visible when Write Amplitude Range = true and Range Mode = Individual_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Mode (Range)</strong> <code>EPCGExVtxAmplitudeMode</code></summary>

How the amplitude range is computed.

| Option         | Description                         |
| -------------- | ----------------------------------- |
| **Length**     | Output scalar range                 |
| **Individual** | Output per-component (X,Y,Z) vector |

Default: `Length`

📋 _Visible when Write Amplitude Range = true_

⚡ PCG Overridable

</details>

#### Amplitude Sign

<details>

<summary><strong>Write Amplitude Sign</strong> <code>bool</code></summary>

When enabled, writes a signed amplitude value relative to an up direction.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Sign</strong> <code>FName</code></summary>

Name of the attribute to write amplitude sign to.

Default: `AmplitudeSign`

📋 _Visible when Write Amplitude Sign = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Sign Output Mode</strong> <code>EPCGExVtxAmplitudeSignOutput</code></summary>

How the sign value is computed and output.

| Option                | Description                                    |
| --------------------- | ---------------------------------------------- |
| **Raw**               | Raw dot product with up vector                 |
| **Size**              | Dot product multiplied by edge size            |
| **Size (Normalized)** | Dot product multiplied by normalized edge size |
| **Sign**              | Simple sign value (0, 1, or -1)                |

Default: `Size`

📋 _Visible when Write Amplitude Sign = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Absolute (Sign)</strong> <code>bool</code></summary>

When enabled, uses absolute value for sign calculation.

Default: `true`

📋 _Visible when Write Amplitude Sign = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Up Mode</strong> <code>EPCGExVtxAmplitudeUpMode</code></summary>

How the reference up direction is determined.

| Option                | Description                            |
| --------------------- | -------------------------------------- |
| **Average Direction** | Use average direction to all neighbors |
| **Custom Up Vector**  | Use a specified up vector              |

Default: `Average Direction`

📋 _Visible when Write Amplitude Sign = true_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Up Input Type</strong> <code>EPCGExInputValueType</code></summary>

Whether the up vector comes from a constant or attribute.

Default: `Constant`

📋 _Visible when Write Amplitude Sign = true and Up Mode = Custom Up Vector_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Up Vector (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Attribute selector for the up vector.

📋 _Visible when Write Amplitude Sign = true, Up Mode = Custom Up Vector, and Up Input Type ≠ Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Up Vector</strong> <code>FVector</code></summary>

Constant up vector for sign calculation.

Default: `(0, 0, 1)`

📋 _Visible when Write Amplitude Sign = true, Up Mode = Custom Up Vector, and Up Input Type = Constant_

⚡ PCG Overridable

</details>

### Outputs

| Pin     | Type         | Description                                              |
| ------- | ------------ | -------------------------------------------------------- |
| **Out** | Vtx Property | Vertex property factory for use with Vtx Properties node |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsClusters-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsClusters/Public/Elements/Meta/VtxProperties/PCGExVtxPropertyAmplitude.h)
