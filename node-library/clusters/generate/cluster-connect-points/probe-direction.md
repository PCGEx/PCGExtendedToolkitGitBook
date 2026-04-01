---
description: Probe in a given direction.
icon: circle-dashed
---

# Probe : Direction

### Overview

This per-point probe searches for neighbors in a specific direction within an angle tolerance. The direction can be constant for all points or read from an attribute, and optionally transformed by each point's rotation. This creates directional connectivity where edges follow a preferred vector, useful for flow patterns, alignment constraints, or oriented structures.

<figure><img src="../../../../.gitbook/assets/image (282).png" alt=""><figcaption></figcaption></figure>

### How It Works

1. **Direction Setup**: Gets the probe direction (constant or from attribute)
2. **Transform Application**: Optionally rotates direction by the point's transform
3. **Candidate Search**: Finds candidates within the search radius
4. **Angle Filtering**: Filters candidates within the MaxAngle cone of the direction
5. **Best Selection**: Selects the best match based on alignment or distance preference

**Usage Notes**

* **Unsigned Check**: When enabled, treats opposite directions as equivalent - useful for bidirectional alignment without caring which way points
* **Component-Wise Angles**: Allows different tolerances for pitch, yaw, and roll when matching direction
* **Chained Processing**: When enabled, this probe runs after other probes have processed, potentially finding different results

### Behavior

```
Direction Probe (MaxAngle=45°):

    Direction: Forward (→)
    Angle tolerance cone:

              ╱│╲
             ╱ │ ╲  45°
            ╱  │  ╲
    ───────●───┼───────→ direction
            ╲  │  ╱
             ╲ │ ╱  45°
              ╲│╱

    Candidates:

         •(outside)
          ╲
           ╲
            ●───────→•(best aligned)
           ╱       ╱
          ╱       •(in cone, closer)
         •(outside)

    With Favor=Closest: connects to closer •
    With Favor=Best Alignment: connects to best aligned •
```

### Settings

#### Direction Configuration

<details>

<summary><strong>Direction Input</strong> <code>EPCGExInputValueType</code></summary>

Determines whether the direction is a constant vector or read from an attribute.

| Option        | Description                                |
| ------------- | ------------------------------------------ |
| **Constant**  | Use the constant direction specified below |
| **Attribute** | Read the direction from a point attribute  |

Default: `Constant`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Direction</strong> <code>FVector</code></summary>

The constant direction vector to probe in.

Default: `(1, 0, 0)` (Forward)

📋 _Visible when Direction Input = Constant_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Direction (Attr)</strong> <code>FPCGAttributePropertyInputSelector</code></summary>

The attribute containing the direction vector per point.

📋 _Visible when Direction Input = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

When enabled, inverts the direction read from the attribute.

Default: `false`

📋 _Visible when Direction Input = Attribute_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Transform Direction</strong> <code>bool</code></summary>

When enabled, rotates the direction vector according to each point's transform. This allows the probe direction to follow the point's orientation.

Default: `true`

⚡ PCG Overridable

</details>

#### Angle Tolerance

<details>

<summary><strong>Use Component-Wise Angle</strong> <code>bool</code></summary>

When enabled, uses separate angle thresholds for pitch, yaw, and roll instead of a single max angle.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Angle</strong> <code>double</code></summary>

The maximum angle deviation from the probe direction to accept a connection. Candidates outside this cone are rejected.

Default: `45`

Range: `0` - `180`

📋 _Visible when Use Component-Wise Angle is disabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Angles</strong> <code>FRotator</code></summary>

Separate angle thresholds for pitch, yaw, and roll axes.

Default: `(45, 45, 45)`

📋 _Visible when Use Component-Wise Angle is enabled_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Unsigned Check</strong> <code>bool</code></summary>

When enabled, uses absolute angle values, treating opposite directions as equivalent. Useful when you want alignment regardless of which way the vector points.

Default: `false`

⚡ PCG Overridable

</details>

#### Selection

<details>

<summary><strong>Favor</strong> <code>EPCGExProbeDirectionPriorization</code></summary>

Determines how to prioritize among candidates within the angle tolerance.

| Option               | Description                                         |
| -------------------- | --------------------------------------------------- |
| **Best Alignment**   | Favor candidates that best align with the direction |
| **Closest Position** | Favor the closest candidates within the tolerance   |

Default: `Closest Position`

</details>

<details>

<summary><strong>Do Chained Processing</strong> <code>bool</code></summary>

When enabled, this probe processes candidates after other probes have run, potentially yielding different results based on remaining candidates.

Default: `false`

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

The maximum distance to search for neighbors.

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

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsProbing-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsProbing/Public/Probes/PCGExProbeDirection.h)
