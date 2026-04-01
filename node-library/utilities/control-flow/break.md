---
description: Tracks recursion depth and conditions to control recursive subgraph execution.
icon: circle
---

# Break

### Overview

This control flow node manages recursion counters and exit conditions for recursive subgraphs, acting as a "break" mechanism. It tracks iteration counts, maintains state across recursion cycles, applies tag management, and optionally tests data conditions to determine when to continue or stop recursion. The node can create new trackers, update existing ones, or dynamically choose based on context.

### How It Works

1. **Mode Selection**: Determines whether to Create, Update, or CreateOrUpdate a recursion tracker
2. **Tracker Processing**: Manages counter state, tracking current iteration and remaining count
3. **Condition Testing**: Optionally tests input data against filters to determine continuation
4. **Counter Update**: Increments or decrements the recursion counter based on settings
5. **Output Routing**: Routes data to Continue or Stop pins based on remaining iterations and conditions
6. **State Output**: Provides tracker data with updated state for next iteration

### Behavior

```
Simple Workflow (Outside Subgraph):

Mode: Create
MaxCount: 5

Iteration 0: Create tracker (Remainder = 5) → Continue
  └─ Enter subgraph with tracker...
```

```
Update Workflow (Inside Subgraph):

Mode: Update
CounterUpdate: -1 (decrement)

Iteration 1: Tracker in (Remainder = 5)
  → Update (Remainder = 4) → Continue → Loop back

Iteration 2: Tracker in (Remainder = 4)
  → Update (Remainder = 3) → Continue → Loop back

Iteration 3: Tracker in (Remainder = 3)
  → Update (Remainder = 2) → Continue → Loop back

Iteration 4: Tracker in (Remainder = 2)
  → Update (Remainder = 1) → Continue → Loop back

Iteration 5: Tracker in (Remainder = 1)
  → Update (Remainder = 0) → Stop → Exit subgraph
```

```
CreateOrUpdate Workflow (Inside Subgraph):

Mode: CreateOrUpdate
MaxCount: 5
RemainderOffsetWhenCreateInsteadOfUpdate: -1

First time (no tracker input):
  → Create tracker (Remainder = 5 - 1 = 4) → Continue

Subsequent iterations:
  → Update tracker as normal
```

```
Extra Outputs:

MaxCount: 10
Current Remainder: 7

Progress = (10 - 7) / 10 = 0.3 (30% complete)
Index = 10 - 7 = 3 (iteration 3)
Remainder = 7 (iterations left)

One Minus enabled:
Progress = 1 - 0.3 = 0.7 (70% remaining)
```

```
Branch Type:

Separate Continue/Stop output pins
Data flows to one pin or the other based on condition

Simple Type:
All data to single output pin
Tracker attribute "Continue" = true/false
```

```
Additional Data Testing:

bDoAdditionalDataTesting = true
Test Data input: Point collections
Tracker Filters: Filter factories

If ANY test data passes filters → Continue = true
If NO test data passes filters → Continue = false
```

Good for: iteration limits, recursive loop control, subgraph breaks, depth tracking, condition-based exits

### Settings

<details>

<summary><strong>Type</strong> <code>EPCGExRecursionTrackerType</code></summary>

Determines the output pin structure and control flow style.

| Type                 | Description                                                     |
| -------------------- | --------------------------------------------------------------- |
| **Simple**           | Single output pin, continuation controlled by tracker attribute |
| **Branch** (default) | Separate Continue/Stop output pins for explicit routing         |

**Simple**: All data flows through one pin; check the tracker's "Continue" attribute to determine state **Branch**: Data is routed to Continue or Stop pins based on recursion state

Simple can update multiple trackers at once, Branch works with a single tracker.

Default: `Branch`

</details>

<details>

<summary><strong>Mode</strong> <code>EPCGExRecursionTrackerMode</code></summary>

Determines how the tracker is initialized and managed.

| Mode                           | Description                       | Use Case                               |
| ------------------------------ | --------------------------------- | -------------------------------------- |
| **Create**                     | Create a new tracker              | Use outside subgraph to initialize     |
| **Update**                     | Update an existing tracker        | Use inside subgraph for each iteration |
| **Create or Update** (default) | Create if empty, otherwise update | Use inside subgraph for convenience    |

**Create**: Initializes a fresh tracker with MaxCount iterations remaining **Update**: Expects existing tracker input, updates counter and tests conditions **CreateOrUpdate**: Convenience mode that auto-creates on first run, then updates

Default: `CreateOrUpdate`

</details>

<details>

<summary><strong>Continue Attribute Name</strong> <code>FName</code></summary>

Name of the boolean attribute set on the tracker to indicate whether recursion should continue.

This attribute is read by logic that checks whether to loop again or exit.

Examples:

* `"Continue"` (default): Standard continuation flag
* `"KeepGoing"`: Custom name
* `"Valid"`: Semantic naming

📋 _Visible when Mode != Update_

Default: `"Continue"`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Max Count</strong> <code>int32</code></summary>

Maximum number of recursion iterations before stopping.

The tracker starts with this value as the remainder and decrements (or increments) based on CounterUpdate until reaching zero.

Examples:

* `20` (default): Allow 20 iterations
* `100`: Deep recursion
* `5`: Short loop

📋 _Visible when Mode != Update_

Default: `20`

Range: `0` to max int

⚡ PCG Overridable

</details>

<details>

<summary><strong>Add Tags</strong> <code>FString</code></summary>

Comma-separated tags to add to the tracker.

Useful for marking tracker state or categorizing recursion types.

Examples:

* `"Iteration,Active"`: Add multiple tags
* `"RecursionDepth5"`: Semantic tag
* `""` (default): No tags added

Default: `""` (empty)

⚡ PCG Overridable

</details>

<details>

<summary><strong>Remove Tags</strong> <code>FString</code></summary>

Comma-separated tags to remove from the tracker.

Useful for clearing tags from previous iterations.

Examples:

* `"Temporary,Debug"`: Remove multiple tags
* `"OldState"`: Clear previous state tag

📋 _Visible when Mode != Create_

Default: `""` (empty)

⚡ PCG Overridable

</details>

<details>

<summary><strong>Counter Update</strong> <code>int32</code></summary>

Value to add to the recursion counter each update.

Negative values decrement the remainder (standard countdown), positive values increment (countup).

Examples:

* `-1` (default): Countdown (20 → 19 → 18 ... → 0)
* `1`: Count up
* `-2`: Decrement by 2 each iteration

📋 _Visible when Mode != Create_

Default: `-1`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Output Progress</strong> <code>bool</code></summary>

If enabled, creates a "Progress" output pin with normalized progress value (0 to 1).

Progress = (MaxCount - Remainder) / MaxCount

Examples:

* At iteration 3 of 10: Progress = 0.3
* At iteration 10 of 10: Progress = 1.0

Useful for driving curves, lerps, or progress-dependent operations.

📋 _Visible when Mode != Create_

Default: `false`

</details>

<details>

<summary><strong>One Minus</strong> <code>bool</code></summary>

Output (1 - Progress) instead of Progress.

Inverts the progress value, showing remaining portion instead of completed portion.

Examples:

* At iteration 3 of 10: Normal = 0.3, One Minus = 0.7
* At iteration 10 of 10: Normal = 1.0, One Minus = 0.0

📋 _Visible when Mode != Create and Output Progress = true_

Default: `false`

</details>

<details>

<summary><strong>Output Index</strong> <code>bool</code></summary>

If enabled, creates an "Index" output pin with the current iteration index.

Index = MaxCount - Remainder

Examples:

* Iteration 1 (Remainder = 9 of 10): Index = 1
* Iteration 5 (Remainder = 5 of 10): Index = 5

📋 _Visible when Mode != Create_

Default: `false`

</details>

<details>

<summary><strong>Output Remainder</strong> <code>bool</code></summary>

If enabled, creates a "Remainder" output pin with the current remaining iteration count.

Shows how many iterations are left before stopping.

Examples:

* Iteration 3 of 10: Remainder = 7
* Iteration 9 of 10: Remainder = 1

📋 _Visible when Mode != Create_

Default: `false`

</details>

<details>

<summary><strong>Force Output Continue</strong> <code>bool</code></summary>

Overrides the Continue attribute to a valid value.

Use this when the tracker receives attribute sets that may already have a boolean with the same name but incorrect value.

📋 _Visible when Mode != Create_

Default: `false`

</details>

<details>

<summary><strong>Do Additional Data Testing</strong> <code>bool</code></summary>

Enables collection-level filtering on a separate set of test data.

If enabled, reads "Test Data" input and applies "Tracker Filters". If no test data passes the filters, the tracker returns Continue = false.

Useful for condition-based exits: "stop if no valid data remains."

📋 _Visible when Mode != Create and Type = Simple_

Default: `false`

</details>

<details>

<summary><strong>Add Entry When Creating From Existing Data</strong> <code>bool</code></summary>

Add metadata entry when creating a tracker from existing attribute set data.

Technical setting for metadata management.

📋 _Visible when Mode != Create_

Default: `false`

</details>

<details>

<summary><strong>Remainder Offset When Create Instead Of Update</strong> <code>int32</code></summary>

An offset applied to the remainder when creating a tracker in CreateOrUpdate mode.

The default value (-1) assumes the tracker is created from inside a subgraph, meaning one iteration has already passed.

Examples:

* `-1` (default): MaxCount = 10 → Initial Remainder = 9
* `0`: MaxCount = 10 → Initial Remainder = 10

📋 _Visible when Mode = CreateOrUpdate_

Default: `-1`

</details>

<details>

<summary><strong>Group Branch Pins</strong> <code>bool</code></summary>

Cosmetic setting to group Continue/Stop pins together in the node UI.

For organizational purposes.

Default: `false`

</details>

### Inputs

| Pin                            | Type             | Description                                                          |
| ------------------------------ | ---------------- | -------------------------------------------------------------------- |
| **In**                         | Any              | Input data to pass through                                           |
| **Tracker** (optional)         | Param Data       | Existing tracker to update (Mode = Update or CreateOrUpdate)         |
| **Tracker Filters** (optional) | Filter Factories | Filters for tracker testing (when Do Additional Data Testing = true) |
| **Test Data** (optional)       | Point Data       | Data collections to test against filters                             |

### Outputs

**Branch Type:**

| Pin                      | Type          | Description                                           |
| ------------------------ | ------------- | ----------------------------------------------------- |
| **Continue**             | Same as input | Data when recursion should continue                   |
| **Stop**                 | Same as input | Data when recursion should stop                       |
| **Tracker**              | Param Data    | Updated tracker state                                 |
| **Progress** (optional)  | Param Data    | Normalized progress (0-1) when Output Progress = true |
| **Index** (optional)     | Param Data    | Current iteration index when Output Index = true      |
| **Remainder** (optional) | Param Data    | Remaining iterations when Output Remainder = true     |

**Simple Type:**

| Pin                      | Type          | Description                                       |
| ------------------------ | ------------- | ------------------------------------------------- |
| **Out**                  | Same as input | All data (check tracker's Continue attribute)     |
| **Tracker**              | Param Data    | Updated tracker state                             |
| **Progress** (optional)  | Param Data    | Normalized progress when Output Progress = true   |
| **Index** (optional)     | Param Data    | Current iteration index when Output Index = true  |
| **Remainder** (optional) | Param Data    | Remaining iterations when Output Remainder = true |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExFoundations-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExFoundations/Public/Elements/ControlFlow/PCGExRecursionTracker.h)
