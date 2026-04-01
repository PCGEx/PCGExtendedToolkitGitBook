---
description: Shrink path from its beginning and end.
icon: circle
---

# Path : Shrink

### Overview

This node removes points from the start and/or end of paths, effectively shortening them. You can shrink by a fixed distance or by a point count, with options for how to handle the cut point when shrinking by distance.

### How It Works

1. **Select Endpoints**: Determine which ends of the path to shrink (start, end, or both).
2. **Calculate Shrink Amount**: Based on mode, determine how many points or how much distance to remove.
3. **Apply Stop Conditions**: Optionally stop shrinking early if points pass certain filters.
4. **Perform Cut**: Remove points and optionally create new endpoints at the exact cut location.
5. **Preserve Metadata**: Optionally keep original endpoint attributes on the new endpoints.

**Usage Notes**

* **Distance vs Count**: Distance mode is more precise for controlling exact path length. Count mode is simpler when you know how many points to remove.
* **Cut Types**: When shrinking by distance, "New Point" creates an interpolated point at the exact cut location. Other options snap to existing points.
* **Stop Conditions**: Use filters to prevent shrinking past important points (e.g., stop when reaching a tagged point).
* **Closed Loops**: This node is designed for open paths. A warning is shown when used on closed loops (which can be suppressed).

### Behavior

**Shrink by distance from both ends:**

```
Before:     ●───●───●───●───●───●───●
            ├──100──┤       ├──100──┤
            (shrink)         (shrink)

After:          ⊕───●───●───⊕
                ↑            ↑
           (new point)  (new point)
```

**Shrink by count (2 points from start):**

```
Before:     ●───●───●───●───●───●───●
            0   1   2   3   4   5   6

After:              ●───●───●───●───●
                    2   3   4   5   6
```

### Inputs

| Pin                 | Type             | Description                                         |
| ------------------- | ---------------- | --------------------------------------------------- |
| **In**              | Points           | Path points to shrink                               |
| **Stop Conditions** | Filter Factories | Filters that prevent shrinking past matching points |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Shrink Endpoint</strong> <code>EPCGExShrinkEndpoint</code></summary>

Which ends of the path to shrink.

| Option            | Description                      |
| ----------------- | -------------------------------- |
| **Start and End** | Shrink from both ends.           |
| **Start**         | Shrink only from the path start. |
| **End**           | Shrink only from the path end.   |

Default: `Start and End`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Settings Mode</strong> <code>EPCGExShrinkConstantMode</code></summary>

Whether both endpoints use the same settings or separate settings.

| Option       | Description                                               |
| ------------ | --------------------------------------------------------- |
| **Shared**   | Both start and end use the primary settings.              |
| **Separate** | Start uses primary settings, end uses secondary settings. |

Default: `Shared`

📋 _Visible when Shrink Endpoint = Start and End_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Shrink Mode</strong> <code>EPCGExPathShrinkMode</code></summary>

How the shrink amount is measured.

| Option       | Description                                             |
| ------------ | ------------------------------------------------------- |
| **Count**    | Remove a specific number of points.                     |
| **Distance** | Remove points up to a specific distance along the path. |

Default: `Distance`

⚡ PCG Overridable

</details>

***

#### Distance Settings

<details>

<summary><strong>Primary Distance Details</strong> <code>FPCGExShrinkPathEndpointDistanceDetails</code></summary>

Distance-based shrink settings for the first endpoint (or both if shared).

| Property         | Description                                         |
| ---------------- | --------------------------------------------------- |
| **Amount Input** | Constant or Attribute source for distance.          |
| **Distance**     | Distance to shrink from the endpoint. Default: `10` |
| **Cut Type**     | How to handle the cut point (see below).            |

**Cut Type Options:**

| Option              | Description                                                  |
| ------------------- | ------------------------------------------------------------ |
| **New Point**       | Create a new point at the exact cut location (interpolated). |
| **Previous (Ceil)** | Use the existing point before the cut distance.              |
| **Next (Floor)**    | Use the existing point after the cut distance.               |
| **Closest (Round)** | Use whichever existing point is closest to the cut distance. |

📋 _Visible when Shrink Mode = Distance_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Secondary Distance Details</strong> <code>FPCGExShrinkPathEndpointDistanceDetails</code></summary>

Distance-based shrink settings for the second endpoint when using separate settings.

📋 _Visible when Shrink Mode = Distance, Shrink Endpoint = Start and End, and Settings Mode = Separate_

⚡ PCG Overridable

</details>

***

#### Count Settings

<details>

<summary><strong>Primary Count Details</strong> <code>FPCGExShrinkPathEndpointCountDetails</code></summary>

Count-based shrink settings for the first endpoint (or both if shared).

| Property         | Description                               |
| ---------------- | ----------------------------------------- |
| **Value Source** | Constant or Attribute source for count.   |
| **Count**        | Number of points to remove. Default: `10` |

📋 _Visible when Shrink Mode = Count_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Secondary Count Details</strong> <code>FPCGExShrinkPathEndpointCountDetails</code></summary>

Count-based shrink settings for the second endpoint when using separate settings.

📋 _Visible when Shrink Mode = Count, Shrink Endpoint = Start and End, and Settings Mode = Separate_

⚡ PCG Overridable

</details>

***

#### Advanced Settings

<details>

<summary><strong>Endpoints Ignore Stop Conditions</strong> <code>bool</code></summary>

When enabled, the path's original endpoint points themselves won't trigger stop conditions. This allows shrinking to start even if the first point would normally match a stop condition filter.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Preserve First Metadata</strong> <code>bool</code></summary>

When creating a new start point from shrinking, inherit attributes from the original first point instead of interpolating.

Default: `false`

</details>

<details>

<summary><strong>Preserve Last Metadata</strong> <code>bool</code></summary>

When creating a new end point from shrinking, inherit attributes from the original last point instead of interpolating.

Default: `false`

</details>

***

#### Warnings

<details>

<summary><strong>Quiet Closed Loop Warning</strong> <code>bool</code></summary>

Suppress the warning when attempting to shrink a closed loop path. Shrinking closed loops may produce unexpected results.

Default: `false`

</details>

#### Inherited Settings

This node inherits path processing settings from its base class.

→ See Path Processor Settings for: Path handling options.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/PCGExPathShrink.h)
