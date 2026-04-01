---
description: Smooth paths points.
icon: circle
---

# Path : Smooth

### Overview

This node smooths path data by gathering neighboring points and blending their properties/attributes onto each target point. The smoothing method determines which neighbors are gathered and how they're weighted; the BlendOps configuration determines what gets blended and how. This architecture makes smoothing fully attribute-agnostic - the same operation can smooth positions, colors, custom attributes, or any combination.

### How It Works

1. **Select Method**: Use the configured smoothing algorithm (Moving Average or Radius-based).
2. **Gather Neighbors**: For each point, identify neighboring points based on the method's criteria.
3. **Compute Weights**: Calculate influence weight for each neighbor based on the method's algorithm.
4. **Blend via BlendOps**: Use the configured BlendOps to blend properties/attributes from neighbors onto the target point.
5. **Apply Influence**: Scale the blending effect by the influence value.

**Usage Notes**

{% hint style="info" %}
### **BlendOps Required**

This node requires BlendOps to define what gets blended. The `Weight` blend mode is typically a good default for smooth results, but other modes can produce different effects using the same neighbor weights.
{% endhint %}

* **Attribute-Agnostic**: The smoothing method only gathers neighbors and computes weights. BlendOps control which properties/attributes are affected and how they're blended.
* **Preserve Endpoints**: Enable Preserve Start/End to keep path endpoints unaffected while smoothing interior points.
* **Influence**: Values less than 1 reduce the blending strength. Negative values can push away from the blended result.
* **Iterative Smoothing**: For very smooth results, chain multiple smooth nodes with lower amounts rather than one with a very high amount.

### Behavior

**Smoothing pipeline:**

```
1. Smoothing Method gathers neighbors + weights:
        ●───●───[●]───●───●
            ↑    ↑    ↑
   weight: 0.5  1.0  0.5

2. BlendOps blend selected attributes using weights:
   Position: weighted average of neighbors
   Color: weighted average of neighbors
   Custom: weighted average of neighbors
   (based on BlendOps configuration)

3. Influence scales the final blend:
   result = lerp(original, blended, influence)
```

### Inputs

| Pin                       | Type                | Description                           |
| ------------------------- | ------------------- | ------------------------------------- |
| **In**                    | Points              | Path points to smooth                 |
| **Filters**               | Filter Factories    | Filters for which points are smoothed |
| **Overrides : Smoothing** | Smoothing Factories | Override smoothing settings per-point |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Preserve Start</strong> <code>bool</code></summary>

Keep the first point of each path at its original values (no blending applied).

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Preserve End</strong> <code>bool</code></summary>

Keep the last point of each path at its original values (no blending applied).

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Smoothing Method</strong> <code>UPCGExSmoothingInstancedFactory</code></summary>

The algorithm used for gathering neighbors and computing weights.

| Option             | Description                                                  |
| ------------------ | ------------------------------------------------------------ |
| **Moving Average** | Gathers neighbors within a sliding window along the path.    |
| **Radius**         | Gathers points within a spatial radius, ignoring path order. |

⚡ PCG Overridable

</details>

***

#### Influence

<details>

<summary><strong>Influence Input</strong> <code>EPCGExInputValueType</code></summary>

Source for the influence value.

| Option        | Description                            |
| ------------- | -------------------------------------- |
| **Constant**  | Use the constant Influence value.      |
| **Attribute** | Read influence from a point attribute. |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Influence</strong> <code>double</code></summary>

How much to apply the blending result.

* `1.0` - Fully apply blended values
* `0.5` - Blend halfway between original and result
* `0.0` - No change
* Negative values push away from blended result

Default: `1.0`

📋 _Visible when Influence Input = Constant_

⚡ PCG Overridable

</details>

***

#### Smoothing Amount

<details>

<summary><strong>Smoothing Amount Type</strong> <code>EPCGExInputValueType</code></summary>

Source for the smoothing amount.

| Option        | Description                                   |
| ------------- | --------------------------------------------- |
| **Constant**  | Use the constant Smoothing value.             |
| **Attribute** | Read smoothing amount from a point attribute. |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Smoothing</strong> <code>double</code></summary>

The range for neighbor gathering. For Moving Average, this is the window size (number of points in each direction). For Radius, this is the search radius in world units.

Default: `5`

📋 _Visible when Smoothing Amount Type = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Scale Smoothing Amount Attribute</strong> <code>double</code></summary>

Multiplier applied to the smoothing amount. Useful for scaling attribute-driven smoothing values.

Default: `1`

⚡ PCG Overridable

</details>

***

#### Blending

<details>

<summary><strong>Blending Interface</strong> <code>EPCGExBlendingInterface</code></summary>

How attribute blending is configured.

| Option         | Description                                                     |
| -------------- | --------------------------------------------------------------- |
| **Individual** | Use per-attribute blend operations from input factories.        |
| **Monolithic** | Use the single blending configuration below for all attributes. |

Default: `Individual`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Blending Settings</strong> <code>FPCGExBlendingDetails</code></summary>

Configuration for attribute blending when using Monolithic mode.

📋 _Visible when Blending Interface = Monolithic_

//→ See TODO FPCGExBlendingDetails

</details>

#### Inherited Settings

This node inherits path processing settings from its base class.

→ See Path Processor Settings for: Path handling options.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/PCGExSmooth.h)
