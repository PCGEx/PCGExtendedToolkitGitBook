---
description: Each seed paths to randomly selected goal(s).
icon: function
---

# Goal Picker : Random

### Overview

This goal picker randomly selects goals for each seed point. It can pick a single random goal, a fixed number of random goals, or a random number of random goals. The randomization uses a configurable seed value for reproducible results.

### How It Works

1. **Initialize Random**: Sets up random number generator with the local seed.
2. **Determine Count**: Gets the number of goals to pick (1, fixed, or random).
3. **Select Goals**: Randomly chooses goal indices without duplicates.
4. **Return Goals**: Returns the selected goal point(s).

**Usage Notes**

* **Single Mode**: Each seed connects to one randomly selected goal.
* **Fixed Mode**: Each seed connects to a specified number of random goals.
* **Random Mode**: Each seed connects to a random number of random goals.
* **Reproducible**: Same LocalSeed value produces the same random selections.

### Behavior

**Random Goal Selection:**

```
Seeds: [S0, S1, S2]
Goals: [G0, G1, G2, G3, G4]

Single Mode:
   S0 → G2 (random)
   S1 → G0 (random)
   S2 → G4 (random)

Fixed Mode (Num Goals = 2):
   S0 → G2, G1
   S1 → G0, G3
   S2 → G4, G2

Random Mode (Num Goals = 3 max):
   S0 → G2, G1 (picked 2)
   S1 → G0 (picked 1)
   S2 → G4, G2, G3 (picked 3)
```

### Settings

<details>

<summary><strong>Local Seed</strong> <code>int32</code></summary>

Seed value for the random number generator. Same seed produces the same random selections.

Default: `0`

</details>

<details>

<summary><strong>Goal Count</strong> <code>EPCGExGoalPickRandomAmount</code></summary>

How many random goals to select per seed.

| Option              | Description                                   |
| ------------------- | --------------------------------------------- |
| **Single**          | Pick one random goal per seed                 |
| **Multiple Fixed**  | Pick a fixed number of random goals per seed  |
| **Multiple Random** | Pick a random number of random goals per seed |

Default: `Single`

</details>

<details>

<summary><strong>Num Goals Type</strong> <code>EPCGExInputValueType</code></summary>

Whether the goal count is a constant or read from an attribute.

| Option        | Description                          |
| ------------- | ------------------------------------ |
| **Constant**  | Use the fixed Num Goals value        |
| **Attribute** | Read the count from a seed attribute |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Num Goals</strong> <code>int32</code></summary>

Number of random goals to select per seed. For Random mode, this is the maximum possible count.

Default: `5`

📋 _Visible when Goal Count is not Single and Num Goals Type = Constant_

</details>

<details>

<summary><strong>Num Goals (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

Attribute containing the number of goals to pick (converted to int32).

📋 _Visible when Goal Count is not Single and Num Goals Type = Attribute_

</details>

#### Inherited Settings

→ See Goal Picker : Default for Index Safety settings.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPathfinding-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPathfinding/Public/GoalPickers/PCGExGoalPickerRandom.h)
