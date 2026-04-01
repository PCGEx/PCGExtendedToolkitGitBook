---
description: Base infrastructure for data matching rules.
icon: arrow-down-from-arc
---

# Match Rule

### Overview

This is the foundational factory provider system for match rules in PCGExtendedToolkit. Match rules determine whether data elements from one dataset correspond to elements in another dataset based on various criteria. The system provides a flexible framework where different rule types (attribute matching, tag matching, spatial overlap, etc.) can be created and combined to define complex matching logic for operations like data pairing, filtering, and association.

### How It Works

1. **Factory Pattern**: Match rules use the factory pattern to create reusable rule instances
2. **Base Configuration**: All match rules inherit common settings (Strictness, Invert)
3. **Test Interface**: Each rule implements a `Test()` method that evaluates whether two data elements match
4. **Scope Context**: Matching occurs within a scope that provides context about the datasets being compared
5. **Recursion Support**: Some rules support transitive/recursive matching for multi-hop relationships
6. **Result Inversion**: The bInvert flag allows rules to be negated (match becomes no-match)

Match rules are used by nodes that need to establish correspondences between datasets, such as point-to-point matching, edge connections, or data filtering operations.

### Common Settings

All match rule types inherit these base settings:

<details>

<summary><strong>Strictness</strong> <code>EPCGExMatchStrictness</code></summary>

Controls how strictly the match criteria must be satisfied.

| Option            | Description                                                                           |
| ----------------- | ------------------------------------------------------------------------------------- |
| **Any** (default) | Match succeeds if any condition is met (logical OR for multi-condition rules)         |
| **All**           | Match succeeds only if all conditions are met (logical AND for multi-condition rules) |

The interpretation depends on the specific rule type. For single-condition rules, this may have no effect. For multi-attribute or multi-tag rules, it determines whether partial matches count as success.

Default: `Any`

⚡ PCG Overridable

</details>

<details>

<summary><strong>Invert</strong> <code>bool</code></summary>

When enabled, inverts the match result. Elements that would normally match will not match, and vice versa.

* **false** (default): Normal matching behavior
* **true**: Inverted matching (match becomes no-match)

Useful for creating exclusion rules or "does not match" conditions without having to create separate rule types.

Default: `false`

⚡ PCG Overridable

</details>

### Match Rule Types

Different match rule types inherit from this base and add their own specific criteria:

* **Attribute-to-Attribute**: Match based on comparing attribute values between datasets
* **By Index**: Match elements at corresponding indices
* **Copy Tags**: Match based on tag copying/propagation rules
* **Overlap**: Match based on spatial or volumetric overlap
* **Random**: Random probabilistic matching
* **Shared Tag**: Match elements sharing common tags
* **Tag-to-Attribute**: Match tags against attribute values

Each type is documented separately with its specific settings and behavior.

### Usage Pattern

Match rules are typically:

1. Created as factory providers (these nodes)
2. Connected to nodes that perform matching operations
3. Evaluated for each pair of candidate elements
4. Combined with other rules for complex matching logic

The matching system supports combining multiple rules to create sophisticated data association patterns.

### Recursion

Some match rule types support recursive/transitive matching, where matches can propagate through multiple hops:

* **SupportsRecursion()**: Whether the rule type can handle recursive matching
* **WantsRecursion()**: Whether recursion is both supported and enabled in the rule's config
* **GetMaxRecursionDepth()**: Maximum depth for recursive matching (-1 = unlimited)

This allows for establishing indirect relationships, like "friend of a friend" patterns.

### Outputs

| Pin            | Type       | Description                                           |
| -------------- | ---------- | ----------------------------------------------------- |
| **Match Rule** | Match Rule | Match rule factory that can be used by matching nodes |

***

[![Static Badge](https://img.shields.io/badge/Source-PCGExMatching-473F69)](https://github.com/Nebukam/PCGExtendedToolkit/blob/main/Source/PCGExMatching/Public/Core/PCGExMatchRuleFactoryProvider.h)
