---
description: Probabilistic matching based on random chance.
icon: circle-dashed
---

# Match : Random

### Overview

This match rule introduces randomness into the matching process by generating a random value for each match test and comparing it against a threshold. Matches succeed probabilistically based on the threshold value - a threshold of 0.5 means approximately 50% of tests will pass, while 0.8 means 80% will pass. The random seed ensures reproducibility, and the threshold can be constant or read from attributes for per-element probability control.

### How It Works

1. **Seed Initialization**: Uses RandomSeed to initialize the random number generator
2. **Threshold Determination**: Reads threshold from either:
   * **Constant**: Fixed probability value (0-1)
   * **Attribute**: Per-candidate or per-target threshold value
3. **Random Generation**: Generates a random value between 0 and 1 for each match test
4. **Threshold Comparison**: Compares random value against threshold:
   * **Normal**: Match if random < threshold
   * **Inverted**: Match if random >= threshold (when bInvertThreshold is true)
5. **Strictness Application**: Applies inherited Strictness setting
6. **Inversion**: Applies inherited Invert flag if enabled

Each match test generates a new random value, making the matching stochastic while remaining deterministic (same seed produces same results).

### Behavior

**Threshold Examples (with RandomSeed = 42):**

```
Threshold = 0.0:  Never matches (0% probability)
Threshold = 0.25: Matches ~25% of the time
Threshold = 0.5:  Matches ~50% of the time (default)
Threshold = 0.75: Matches ~75% of the time
Threshold = 1.0:  Always matches (100% probability)
```

**Random Value vs Threshold:**

```
Random Value = 0.3, Threshold = 0.5 → 0.3 < 0.5 → Match ✓
Random Value = 0.7, Threshold = 0.5 → 0.7 >= 0.5 → No Match ✗
```

**Inverted Threshold:**

```
With bInvertThreshold = true:
Random Value = 0.3, Threshold = 0.5 → 0.3 >= 0.5 → No Match ✗
Random Value = 0.7, Threshold = 0.5 → 0.7 >= 0.5 → Match ✓
(Effectively inverts the probability)
```

**Attribute-Based Threshold:**

```
Candidate[0] has Threshold = 0.9 → High match probability
Candidate[1] has Threshold = 0.1 → Low match probability
Candidate[2] has Threshold = 0.5 → Medium match probability
```

**Deterministic Randomness:**

```
Same RandomSeed always produces the same sequence of random
matches, allowing reproducible stochastic matching.
```

**Multiple Base Inversions:**

```
- bInvertThreshold: Inverts the probability (high becomes low)
- bInvert (inherited): Inverts the final match result
- Combined: Both inversions can be applied together
```

Good for: stochastic sampling, probabilistic filtering, random selection, Monte Carlo methods, chance-based effects

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Random Seed</strong> <code>int32</code></summary>

Seed value used for random number generation. The same seed always produces the same sequence of random values, ensuring reproducible results.

* **Different seeds**: Produce different random patterns
* **Same seed**: Always produces identical random sequence
* **42** (default): Common default seed value

Change the seed to get different random outcomes while maintaining determinism.

Default: `42`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Threshold Input</strong> <code>EPCGExInputValueType</code></summary>

Determines where the threshold value comes from.

| Option                 | Description                                                                     |
| ---------------------- | ------------------------------------------------------------------------------- |
| **Constant** (default) | Uses the fixed Threshold value for all match tests                              |
| **Attribute**          | Reads threshold from the ThresholdAttribute for per-element probability control |

Using Attribute allows different elements to have different match probabilities.

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Threshold (Attribute)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute to read the threshold value from when ThresholdInput is set to Attribute. Value is expected to be in the 0-1 range.

Examples:

* `"MatchProbability"`: Reads per-element probability
* `"@Data.Chance"`: Reads from data domain
* `"Weight"`: Uses normalized weight as probability

Only used when ThresholdInput = Attribute.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Threshold (Constant)</strong> <code>double</code></summary>

Fixed threshold value (0-1) used when ThresholdInput is set to Constant. Represents the probability of matching.

* **0.0**: Never matches (0% probability)
* **0.25**: Matches 25% of the time
* **0.5** (default): Matches 50% of the time
* **0.75**: Matches 75% of the time
* **1.0**: Always matches (100% probability)

Only used when ThresholdInput = Constant.

Default: `0.5`

Range: `0.0` to `1.0`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert Threshold</strong> <code>bool</code></summary>

Inverts the threshold comparison, effectively reversing the probability.

* **false** (default): Match if random < threshold (normal behavior)
* **true**: Match if random >= threshold (inverted probability)

With inversion:

* Threshold 0.8 becomes 20% probability instead of 80%
* Threshold 0.2 becomes 80% probability instead of 20%

Different from the inherited bInvert flag, which inverts the final match result after all other processing.

Default: `false`

⚡ PCG Overridable

</details>

#### Inherited Settings

This match rule inherits common settings from the base match rule configuration.

→ See Match Rule Factory Provider for: Strictness, Invert

**Important**: This rule is stochastic (random) but deterministic (reproducible with same seed). Each match test generates a new random value, so multiple tests between the same elements may produce different results unless you cache matches.

**Tip**: Use attribute-based thresholds to create spatially-varying or importance-weighted random matching. For example, assign higher probabilities to elements in regions of interest.

### Outputs

| Pin            | Type       | Description                                          |
| -------------- | ---------- | ---------------------------------------------------- |
| **Match Rule** | Match Rule | Match rule factory for probabilistic random matching |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExMatching-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExMatching/Public/Matching/PCGExMatchRandom.h)
