---
hidden: true
icon: atom-simple
---

# Valency

{% hint style="warning" %}
## Work In Progress&#x20;

This section is under development. The Valency system may change.&#x20;
{% endhint %}

Valency is PCGEx's constraint-solving system for modular assembly. It places compatible modules at cluster vertices based on connection rules, similar to Wave Function Collapse but operating on cluster topology.

### Core Concept

Valency assigns modules to cluster Vtx such that:

* Each Vtx gets exactly one module
* Connected Vtx have compatible modules
* Constraints propagate through the cluster

### Key Components

| Component         | Purpose                              |
| ----------------- | ------------------------------------ |
| **Modules**       | Assets with defined connection rules |
| **Orbitals**      | Connection points on modules         |
| **Bonding Rules** | Compatibility between orbitals       |
| **Palettes**      | Collections of available modules     |
| **Cages**         | Constraint containers at Vtx         |

### Prerequisites

Valency builds on:

* Clusters -The topology being solved
* Asset Staging -Module spawning

### Current Documentation

Detailed Valency documentation exists in:

* `working-with-pcgex/valency/fundamentals/`
* `working-with-pcgex/valency/your-first-valency-system.md`
* `working-with-pcgex/valency/orbitals-and-orbital-sets.md`

These pages cover the current implementation. This conceptual section will be updated when the system stabilizes.

### Related

* Clusters -Foundation for Valency
* Asset Staging -Module collections
