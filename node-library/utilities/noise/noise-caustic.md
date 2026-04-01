---
description: Caustic noise - water light patterns.
icon: circle-dashed
---

# Noise : Caustic

### Overview

This noise simulates the caustic light patterns created by light refracting through water or other transparent mediums with rippled surfaces. It generates the characteristic bright focal points and dark areas that appear at the bottom of pools or on surfaces beneath moving water. The noise can be animated and layered to create convincing water light effects.

### How It Works

1. **Wave Layer Generation**: Creates multiple overlapping sine-wave based patterns
2. **Layer Accumulation**: Combines WaveLayers number of offset wave patterns, each with slightly different frequencies
3. **Caustic Focusing**: Applies intensity and focus parameters to create bright focal points where waves converge
4. **Time Animation**: When Time parameter changes, shifts the wave patterns to simulate moving water
5. **Inherited Processing**: Applies base noise settings (frequency, transform, contrast, remap curve, etc.)

### Behavior

```
Single Wave Layer (simplified):

~~~  ~~~  ~~~  Gentle waves
 ‖    ‖    ‖   Light focuses where waves converge
 ■    ■    ■   Bright caustic spots
```

```
Multiple Layers (WaveLayers = 3):

Layer 1: ~~~  ~~~  ~~~
Layer 2:  ~~~  ~~~  ~~~  (offset)
Layer 3:   ~~~  ~~~  ~~~ (offset)
Result:  ■  ■■ ■  ■     Complex caustic pattern
```

```
Intensity Effect:

Low (1.0):  ·  ·  ·  (subtle, evenly distributed)
Medium (2.0): ●  ●  ●  (default, clear focal points)
High (4.0):  ●● ●● ●● (very bright, sharp contrasts)
```

```
Focus Effect:

Low (1.0):  (●) (●) (●) (broad, blurred spots)
Medium (2.0): ● ● ●     (clear focal points)
High (8.0):  •• •• ••   (sharp, intense focal points)
```

Good for: underwater effects, magical effects, sci-fi patterns, water surfaces

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Wave Layers</strong> <code>int32</code></summary>

Number of overlapping wave patterns to combine. More layers create more complex, organic-looking caustic patterns.

* **1**: Simple, regular pattern
* **3** (default): Realistic caustic complexity
* **8**: Very complex, chaotic patterns

Higher values increase computation cost but create richer patterns.

Default: `3`

Range: `1` to `8`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Wavelength</strong> <code>double</code></summary>

Base wavelength of the wave patterns, controlling the size of caustic features. Larger wavelengths create larger, more spread-out light patterns.

* **0.1**: Very fine, tight caustic patterns
* **1.0** (default): Natural caustic scale
* **10.0**: Large, broad light patterns

Default: `1.0`

Range: `0.1` to `10.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Time</strong> <code>double</code></summary>

Time parameter for animation. As this value increases, the caustic pattern shifts and moves, simulating the motion of water ripples.

* **0.0** (default): Static pattern (no animation)
* **Increasing values**: Animated pattern (use with AnimationSpeed)

Typically driven by a time variable or attribute for animated effects.

Default: `0.0`

Minimum: `0.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Animation Speed</strong> <code>double</code></summary>

Multiplier for the Time parameter, controlling how quickly the caustic pattern moves and shifts.

* **0.0**: No movement
* **1.0** (default): Natural water motion speed
* **5.0**: Very fast, energetic movement

Default: `1.0`

Range: `0.0` to `5.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Intensity</strong> <code>double</code></summary>

Controls how bright the caustic focal points become. Higher intensity creates stronger contrast between bright spots and dark areas, emphasizing where light rays converge.

* **0.5**: Subtle, even distribution
* **2.0** (default): Clear caustic effect
* **4.0**: Very intense, dramatic focal points

Default: `2.0`

Range: `0.5` to `4.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Focus</strong> <code>double</code></summary>

Sharpness of the caustic focal points. Higher values create tighter, more defined bright spots where light concentrates.

* **1.0**: Broad, blurred spots
* **2.0** (default): Clear focal points
* **8.0**: Very sharp, pinpoint bright areas

Default: `2.0`

Range: `1.0` to `8.0`

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

[![Static Badge](https://img.shields.io/badge/Source-PCGExNoise3D-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExNoise3D/Public/Noises/PCGExNoiseCaustic.h)
