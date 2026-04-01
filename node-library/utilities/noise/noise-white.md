---
description: White noise - fast, pure random.
icon: circle-dashed
---

# Noise : White

### Overview

This noise generates pure random values with no spatial correlation or coherence. Each position in 3D space is hashed to produce a random value independently of its neighbors, creating a completely incoherent, grainy pattern. White noise has no structure, no smooth transitions, and no recognizable features — just uniform randomness across all frequencies. It's the fastest noise to compute (simple hash function) and serves as a building block for other effects or whenever true spatial randomness is needed.

### How It Works

1. **Position Hashing**: Takes the 3D position coordinates and applies a hash function
2. **Random Value Generation**: The hash produces a pseudo-random value in the 0-1 range
3. **No Interpolation**: Unlike other noises, there's no smoothing or blending between values
4. **No Octaves**: White noise doesn't support octave layering (would be redundant as it already contains all frequencies)
5. **Inherited Processing**: Applies base noise settings (frequency, transform, contrast, remap curve, etc.)

The result is that adjacent positions have completely uncorrelated values, producing a "static" or "grain" appearance.

### Behavior

```
Characteristics:

- Completely random, no spatial correlation
- No smooth features (maximum high-frequency content)
- Very fast computation (single hash operation)
- Uniform distribution across value range
- Grainy, static-like appearance

Visual Representation (1D slice):

White Noise:     :.':.,:.':,:.:',.:'
Smooth Noise:    ~~~~~~~~~~~~
```

```
Comparison to Other Noises:

White:      Pure random, no structure
Value:      Interpolated random, some structure
Perlin:     Smooth gradients, organic structure
```

```
Frequency Content:

White noise contains ALL frequencies equally,
unlike smooth noises which have band-limited frequency.
```

```
Use Case Example:

Random dithering:   White noise × 0.1
Film grain:         White noise × 0.05
Breakup mask:       White noise > threshold
```

Good for: random dithering, film grain, static effects, breakup masks, random jitter, building block for other noise combinations

### Settings

#### Node-Specific Settings

White noise has no node-specific settings beyond the inherited base settings. It generates pure random values based only on position and seed.

#### Inherited Settings

This noise inherits common settings from the base noise configuration.

→ See Noise3D Factory Provider for: Weight Factor, Blend Mode, Invert, Remap Curve, Seed, Transform, Frequency, Contrast, and other shared noise properties.

**Note**:

* **Frequency** controls the sampling density. Higher frequency = more fine-grained randomness, lower frequency = more chunky randomness (due to coordinate quantization in the hash)
* **Seed** determines the random sequence. Different seeds produce different random patterns.
* **Contrast** can enhance or reduce the value range spread
* **Remap Curve** is particularly useful with white noise to reshape the value distribution (e.g., create more low values, cluster around mid-range, etc.)

**Tip**: White noise is often most useful when combined with other operations:

* Multiply by a small amount for subtle randomization/dithering
* Use as a breakup mask with thresholding
* Combine with smooth noise to add high-frequency detail
* Apply strong remap curves to create binary random patterns

### Outputs

| Pin         | Type    | Description                                                   |
| ----------- | ------- | ------------------------------------------------------------- |
| **Noise3D** | Noise3D | Noise factory that can be used by nodes requiring noise input |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExNoise3D-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExNoise3D/Public/Noises/PCGExNoiseWhite.h)
