---
description: Does a rolling blending of properties & attributes.
icon: circle
---

# Path : Attribute Rolling

### Overview

This node segments paths into discrete ranges based on filter conditions, writing rich metadata about each range to point attributes. It can also apply rolling attribute blending within those ranges.

The node walks along the path and tracks whether each point is "inside" or "outside" a range. Range boundaries are detected via filters, and each point receives metadata about its range membership, boundary status, and position within its range.

### How It Works

1. **Initialize State**: The rolling state starts either inside or outside a range based on the initial value settings.
2. **Walk the Path**: Points are processed in order (or reverse if enabled), checking filters at each point.
3. **Detect Boundaries**: When a filter passes, the range state changes (Toggle mode) or a range starts/stops (Start/Stop mode).
4. **Track Metadata**: For each point, track which range it belongs to, whether it's a boundary point, and its index within the range.
5. **Apply Blending**: If blend operations are connected, blend attributes from a reference point to points within ranges.

### Range Metadata

The core strength of this node is the range metadata it generates. The output attributes (`RangeIndex`, `IsInsideRange`, `IndexInsideRange`, `RangeStart`, `RangeStop`, `RangePole`) let you:

* **Segment paths** into numbered sections based on attribute conditions
* **Mark boundary points** for special treatment downstream
* **Create per-range indices** for gradient effects or LOD transitions
* **Flag inside/outside status** for conditional filtering
* **Identify "poles"** (start or end points) for spline tangent handling

This metadata is available for downstream nodes regardless of whether you also use the blending capabilities.

### Range Control Modes

#### Toggle Mode

A single filter set controls the range state. Each time a point passes the filter, the state flips between inside and outside.

```
Initial: Outside (bInitialValue = false)
Filter passes at ● points:

  ○───○───●───○───○───○───●───○───○───●───○───○
         [=== Range 0 ===]         [Range 1]
  Outside │     Inside    │ Outside │ Inside │ Out
          Start          Stop      Start   Stop
```

Toggle mode is ideal when your condition naturally alternates (e.g., "every time we cross a certain height threshold").

#### Start/Stop Mode

Separate filter sets explicitly control when ranges start and stop. A start filter begins a range, and a stop filter ends it.

```
Start filter: ▶    Stop filter: ■

  ○───○───▶───○───○───■───○───▶───○───■───○
          [== Range 0 ==]   [= Range 1 =]

Multiple starts without a stop: only the first start counts.
Multiple stops without a start: ignored.
```

Start/Stop mode gives explicit control when start and stop conditions are fundamentally different (e.g., "start at intersection points, stop at path endpoints").

### Initial Value Subtleties

The initial value setting determines whether the path begins inside or outside a range. This has significant implications:

| Mode                    | Behavior                                                                                                                                                                            |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Constant**            | Always start in the specified state (inside if `true`, outside if `false`). First filter hit will toggle/change state.                                                              |
| **Constant (Preserve)** | Start in the specified state, but if the first point already passes the filter, don't toggle. Useful when you want the filter to define ranges but not double-trigger at the start. |
| **From Point**          | Use whatever state the first point's filter result indicates. The path begins in the state matching the first point.                                                                |

**Example - Constant vs Constant (Preserve):**

```
Filter passes at ● points, Initial = false (Outside)

Constant:          ●───○───○───●───○
                   [Range 0]   [R1]
                   ↑ First point toggles to Inside

Constant(Preserve): ●───○───○───●───○
                    Outside    [R0]
                    ↑ First point matches filter but doesn't toggle
                      because we're preserving the initial state
```

### Value Control (for Blending)

When blend operations are connected, **Value Control** determines which point provides the reference values:

| Mode            | Reference Point                              | Use Case                                                 |
| --------------- | -------------------------------------------- | -------------------------------------------------------- |
| **Previous**    | The point immediately before the current one | Cumulative/rolling effects that propagate along the path |
| **Range Start** | The first point of the current range         | All points in a range inherit from its starting point    |
| **Pin**         | Points passing a separate "Pin" filter       | Explicit control over which points serve as references   |

### Behavior

```
Toggle Mode with Range Index output:

Filter: ●    Initial: Outside

  ○───○───●───○───○───●───○───○───●───○
  -1  -1   0   0   0   1   1   1   2   2
       └─────────┘└─────────┘└─────────┘
        Range 0    Range 1    Range 2

Points outside ranges have RangeIndex = -1 (or offset value).
Points inside ranges are numbered sequentially.
```

### Inputs

| Pin                  | Type               | Description                                           |
| -------------------- | ------------------ | ----------------------------------------------------- |
| **In**               | Points             | Path points to process                                |
| **Filters : Toggle** | Filter Factories   | Filters that toggle rolling on/off (Toggle mode)      |
| **Filters : Start**  | Filter Factories   | Filters that start a rolling range (Start/Stop mode)  |
| **Filters : Stop**   | Filter Factories   | Filters that stop a rolling range (Start/Stop mode)   |
| **Filters : Pin**    | Filter Factories   | Filters that pin reference points (Pin value control) |
| **Blending**         | Blend Op Factories | Blend operations to apply during rolling              |

### Settings

#### Node-Specific Settings

<details>

<summary><strong>Range Control</strong> <code>EPCGExRollingRangeControl</code></summary>

How rolling ranges are defined.

| Option         | Description                                                                                       |
| -------------- | ------------------------------------------------------------------------------------------------- |
| **Toggle**     | Single filter set that flips state on each pass. Simpler setup, good for alternating conditions.  |
| **Start/Stop** | Separate filter sets for explicit range boundaries. More control, good for asymmetric conditions. |

Default: `Toggle`

</details>

<details>

<summary><strong>Value Control</strong> <code>EPCGExRollingValueControl</code></summary>

How the reference value for blending is determined.

| Option          | Description                                                                                          |
| --------------- | ---------------------------------------------------------------------------------------------------- |
| **Previous**    | Each point blends from the previous point. Creates cumulative/propagating effects.                   |
| **Range Start** | All points in a range blend from the range's first point. Creates uniform inheritance within ranges. |
| **Pin**         | Points passing the Pin filter become references. Explicit control over reference points.             |

Default: `Previous`

</details>

<details>

<summary><strong>Initial Value Mode</strong> <code>EPCGExRollingToggleInitialValue</code></summary>

How the initial rolling state is determined.

| Option                  | Description                                                                                           |
| ----------------------- | ----------------------------------------------------------------------------------------------------- |
| **Constant**            | Use the constant Initial Value setting. First filter hit changes state.                               |
| **Constant (Preserve)** | Use constant, but don't change if first point already matches the filter. Prevents double-triggering. |
| **From Point**          | Start in whatever state the first point's filter result indicates.                                    |

Default: `Constant`

</details>

<details>

<summary><strong>Initial Value</strong> <code>bool</code></summary>

Starting state: `true` = inside a range, `false` = outside a range.

Default: `true`

📋 _Visible when Initial Value Mode ≠ From Point_

⚡ PCG Overridable

</details>

<details>

<summary><strong>Reverse Rolling</strong> <code>bool</code></summary>

Process the path from end to start instead of start to end. Ranges are detected and numbered in reverse order.

Default: `false`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Blend Outside Range</strong> <code>bool</code></summary>

Apply blend operations to points outside ranges as well as inside.

Default: `false`

</details>

<details>

<summary><strong>Blend Stop Element</strong> <code>bool</code></summary>

Include the stop boundary point in blending operations. By default, the stop point is excluded from the range's blending.

Default: `false`

📋 _Visible when Blend Outside Range = false_

</details>

***

#### Output Attributes

These attributes are written per-point and provide rich metadata for downstream filtering, conditional logic, or visualization.

<details>

<summary><strong>Write Range Start</strong> <code>bool</code></summary>

Mark points that begin a range with a boolean flag.

Default: `false` · Attribute: `RangeStart`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Range Stop</strong> <code>bool</code></summary>

Mark points that end a range with a boolean flag.

Default: `false` · Attribute: `RangeStop`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Range Pole</strong> <code>bool</code></summary>

Mark points that are either a start or stop (any boundary) with a boolean flag. Useful when you need to identify all boundary points regardless of type.

Default: `false` · Attribute: `RangePole`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Range Index</strong> <code>bool</code></summary>

Write which range each point belongs to. Points outside ranges get `-1` (plus any offset).

Default: `false` · Attribute: `RangeIndex`

* **Index Offset**: Shift the range index values. Since `-1` indicates "outside", an offset of `1` makes the first range `1` instead of `0`, and outside becomes `0`.

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Is Inside Range</strong> <code>bool</code></summary>

Simple boolean flag indicating if the point is inside any range.

Default: `false` · Attribute: `IsInsideRange`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Write Index Inside Range</strong> <code>bool</code></summary>

Write the point's sequential index within its range (0, 1, 2, ...). Resets to 0 at each new range start. Points outside ranges get `0`.

Default: `false` · Attribute: `IndexInsideRange`

⚡ PCG Overridable

</details>

#### Inherited Settings

This node inherits path processing settings from its base class.

→ See Path Processor Settings for: Path handling options.

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExElementsPaths-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExElementsPaths/Public/Elements/PCGExAttributeRolling.h)
