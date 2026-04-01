---
description: Adaptive step size based on field curvature. More accurate in curved regions.
icon: function
---

# Tensor Sampler (Adaptive RK)

### Overview

The Adaptive RK (Runge-Kutta) tensor sampler dynamically adjusts its step size based on the local curvature of the tensor field. In regions where the field changes rapidly, it takes smaller steps for accuracy. In smooth regions, it takes larger steps for efficiency. This provides better results than fixed-step sampling when tensor fields have varying complexity.

### How It Works

1. **Curvature Estimation**: Estimates the local field curvature by comparing tensor directions at nearby sample points.
2. **Step Size Calculation**: Computes an appropriate step size inversely proportional to curvature.
3. **Adaptive Sampling**: Takes smaller steps in curved regions, larger steps in smooth regions.
4. **Error Control**: Uses the error tolerance setting to balance accuracy vs. performance.

**Usage Notes**

* **Best For**: Tensor fields with varying curvature — smooth in some areas, sharply curved in others.
* **Performance**: May be slower than fixed-step sampling due to curvature estimation, but produces more accurate results.
* **Step Bounds**: The Min/Max Step Fraction settings from the base sampler control the allowed step size range.

### Behavior

```
Tensor Field with Variable Curvature:

Smooth region:    ────────────────    (large steps)
Curved region:    ╭───╮               (small steps)
                  │   │
                  ╰───╯

Adaptive sampler uses fewer samples in smooth areas,
more samples in curved areas for uniform accuracy.
```

### Settings

This sampler inherits all settings from the base tensor sampler.

#### Inherited Settings

<details>

<summary><strong>Radius</strong> <code>double</code></summary>

Base radius for tensor sampling operations.

Default: `1`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Min Step Fraction</strong> <code>double</code></summary>

Minimum step size as a fraction of the base radius. In highly curved regions, steps won't go below this size.

Default: `0.1`

Range: `0.01` to `1.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Step Fraction</strong> <code>double</code></summary>

Maximum step size as a fraction of the base radius. In smooth regions, steps won't exceed this size.

Default: `1.0`

Range: `0.1` to `2.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Error Tolerance</strong> <code>double</code></summary>

Error tolerance for step size adaptation. Lower values produce more accurate results with smaller steps; higher values allow larger steps with potential accuracy trade-offs.

Default: `0.01`

Range: `0.001` to `0.5`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Sub Steps</strong> <code>int32</code></summary>

Maximum number of sub-steps per sample for improved accuracy.

Default: `4`

Range: `1` to `16`

⚡ PCG Overridable

</details>

→ See Tensor Sampler (Default) for complete inherited settings documentation.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsTensors-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsTensors/Public/Samplers/PCGExTensorSamplerAdaptive.h)
