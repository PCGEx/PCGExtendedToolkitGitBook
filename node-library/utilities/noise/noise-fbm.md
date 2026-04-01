---
description: Fractal Brownian Motion with variants (ridged, billow, warped).
icon: circle-dashed
---

# Noise : FBM

### Overview

This noise implements Fractal Brownian Motion (fBm), a technique that layers multiple octaves of noise at progressively higher frequencies and lower amplitudes to create natural-looking, self-similar patterns. It offers five variants that transform the base pattern into different characteristic looks, from smooth terrain to sharp mountain ridges to billowing clouds, making it one of the most versatile noise types for procedural generation.

### How It Works

1. **Octave Layering**: Combines multiple layers of base noise, each at a different frequency and amplitude
2. **Frequency Scaling**: Each octave's frequency is multiplied by Lacunarity (typically doubling)
3. **Amplitude Scaling**: Each octave's amplitude is multiplied by Persistence (typically halving)
4. **Variant Application**: Applies variant-specific transformations:
   * **Standard**: Direct summation of octaves
   * **Ridged**: Inverts and sharpens values to create ridges
   * **Billow**: Takes absolute value for symmetric bulges
   * **Hybrid**: Weighted combination with first octave dominating
   * **Warped**: Offsets sample positions based on noise values for organic distortion
5. **Inherited Processing**: Applies base noise settings (frequency, transform, contrast, remap curve, etc.)

### Behavior

```
Octave Layering (Standard fBm):

Octave 1 (freq=1, amp=1.0):   ~~~~~~~~~~~
Octave 2 (freq=2, amp=0.5):   ~·~·~·~·~·~
Octave 3 (freq=4, amp=0.25):  ~:~:~:~:~:~
Combined:                      ≈≈≈≈≈≈≈≈≈≈≈ (natural, detailed)
```

```
Variant Comparison:

Standard:  ~~~~~~~~~~~~~~~  (smooth hills and valleys)
Ridged:    /\/\/\/\/\/\/\  (sharp mountain ridges)
Billow:    ○○○○○○○○○○○○○  (puffy clouds, bulbous)
Hybrid:    ~~~  ~~  ~~~    (combination of smooth and detailed)
Warped:    ≈~≈~≈~≈~≈~≈    (organically distorted)
```

```
Octaves Effect:

1 Octave:  ~~~~~~~~~~~~    (smooth, large features)
4 Octaves: ≈≈≈≈≈≈≈≈≈≈≈≈    (good detail)
16 Octaves: :::::::::::    (extremely detailed, noisy)
```

```
Persistence Effect:

Low (0.2):  ~~~~ (smooth, low octaves dominate)
Med (0.5):  ≈≈≈≈ (balanced detail)
High (0.9): :::: (rough, high octaves prominent)
```

Good for: terrain generation, cloud simulation, organic textures, procedural surfaces

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Octaves</strong> <code>int32</code></summary>

Number of noise layers to combine. Each octave adds finer detail at progressively higher frequency.

* **1**: Single layer (essentially basic noise)
* **4** (default): Good balance of detail and performance
* **8**: High detail
* **16**: Maximum detail (expensive)

Each octave approximately doubles computation cost.

Default: `4`

Range: `1` to `16`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Lacunarity</strong> <code>double</code></summary>

Frequency multiplier between successive octaves. Controls how quickly detail scales up with each layer.

* **1.5**: Gentle progression, similar-sized features
* **2.0** (default): Standard doubling, natural look
* **4.0**: Aggressive scaling, more contrast between scales

Default: `2.0`

Range: `1.0` to `4.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Persistence</strong> <code>double</code></summary>

Amplitude multiplier between successive octaves, controlling how much each layer contributes to the final result.

* **0.2**: Low persistence, smooth (large features dominate)
* **0.5** (default): Balanced, natural fractal distribution
* **0.9**: High persistence, rough (small features prominent)

Also called "gain" in some implementations.

Default: `0.5`

Range: `0.0` to `1.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Variant</strong> <code>EPCGExFBMVariant</code></summary>

The fractal variant to use, each producing distinctly different characteristics.

| Option                  | Description                                                                                                                      |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| **Standard fBm**        | Classic Fractal Brownian Motion - smooth, natural hills and valleys. Good for terrain heightmaps.                                |
| **Ridged Multifractal** | Inverts and sharpens the noise to create sharp ridges. Perfect for mountain ranges and crystalline patterns.                     |
| **Billow (Turbulence)** | Takes absolute value to create symmetric bulges. Ideal for clouds, smoke, and puffy organic shapes.                              |
| **Hybrid Multifractal** | Weighted combination where the first octave has more influence. Creates interesting terrain with both smooth and detailed areas. |
| **Domain Warped**       | Distorts the sampling space using the noise itself. Produces highly organic, flowing patterns.                                   |

Default: `Standard fBm`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Ridge Offset</strong> <code>double</code></summary>

Offset value for the Ridged Multifractal variant, controlling the sharpness and height of ridges.

* **0.5**: Gentler, broader ridges
* **1.0** (default): Natural sharp ridges
* **2.0**: Very pronounced, extreme ridges

Default: `1.0`

Range: `0.0` to `2.0`

📋 _Visible when Variant = Ridged Multifractal_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Warp Strength</strong> <code>double</code></summary>

Domain distortion strength for the Domain Warped variant. Higher values create more organic, flowing distortion.

* **0.1**: Subtle warping
* **0.5** (default): Moderate organic distortion
* **2.0**: Extreme warping (may lose structure)

Default: `0.5`

Range: `0.0` to `2.0`

📋 _Visible when Variant = Domain Warped_

⚡ PCG Overridable

</details>

#### Inherited Settings

This noise inherits common settings from the base noise configuration.

→ See Noise3D Factory Provider for: Weight Factor, Blend Mode, Invert, Remap Curve, Seed, Transform, Frequency, Contrast, and other shared noise properties.

### Outputs

| Pin         | Type    | Description                                                   |
| ----------- | ------- | ------------------------------------------------------------- |
| **Noise3D** | Noise3D | Noise factory that can be used by nodes requiring noise input |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExNoise3D-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExNoise3D/Public/Noises/PCGExNoiseFBM.h)
