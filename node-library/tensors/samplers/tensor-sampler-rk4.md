---
description: Samples the field using Runge-Kutta 4 method.
icon: function
---

# Tensor Sampler (RK4)

### Overview

The RK4 (Runge-Kutta 4) tensor sampler uses a fourth-order numerical integration method to sample the tensor field. This classical method takes four intermediate samples per step to compute a more accurate direction than single-point sampling. RK4 provides consistent, high-quality results with fixed step sizes, making it predictable and well-suited for smooth tensor fields.

### How It Works

1. **Initial Sample**: Samples the tensor field at the current position.
2. **Midpoint Samples**: Takes two additional samples at the midpoint of the step using the initial direction.
3. **Endpoint Sample**: Takes a final sample at the projected endpoint.
4. **Weighted Average**: Combines all four samples using the RK4 weighting formula (1/6, 2/6, 2/6, 1/6) to produce the final direction.

**Usage Notes**

* **Fixed Step Size**: Unlike Adaptive RK, this sampler uses consistent step sizes. Best for fields with uniform complexity.
* **Higher Accuracy**: More accurate than single-point sampling due to the four-sample integration, reducing drift in curved fields.
* **Predictable Performance**: Fixed computation cost per step makes performance easy to estimate.

### Behavior

```
RK4 Integration per Step:

Position P₀ ──────────────────────────────────> P₁
            │
            ├─ k1: Sample at P₀
            ├─ k2: Sample at P₀ + 0.5*k1
            ├─ k3: Sample at P₀ + 0.5*k2
            └─ k4: Sample at P₀ + k3

Final direction = (k1 + 2*k2 + 2*k3 + k4) / 6

Result: Smoother path through curved tensor fields
        compared to single-sample methods.
```

### Settings

This sampler inherits all settings from the base tensor sampler.

#### Inherited Settings

<details>

<summary><strong>Radius</strong> <code>double</code></summary>

Base radius for tensor sampling operations.

Default: `1`

</details>

<details>

<summary><strong>Min Step Fraction</strong> <code>double</code></summary>

Minimum step size as a fraction of the base radius.

Default: `0.1`

Range: `0.01` to `1.0`

</details>

<details>

<summary><strong>Max Step Fraction</strong> <code>double</code></summary>

Maximum step size as a fraction of the base radius.

Default: `1.0`

Range: `0.1` to `2.0`

</details>

<details>

<summary><strong>Error Tolerance</strong> <code>double</code></summary>

Error tolerance for sampling accuracy.

Default: `0.01`

Range: `0.001` to `0.5`

</details>

<details>

<summary><strong>Max Sub Steps</strong> <code>int32</code></summary>

Maximum number of sub-steps per sample for improved accuracy.

Default: `4`

Range: `1` to `16`

</details>

→ See Tensor Sampler (Default) for complete inherited settings documentation.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsTensors-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsTensors/Public/Samplers/PCGExTensorSamplerRK4.h)
