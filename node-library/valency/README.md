---
hidden: true
icon: grid-round-2-plus
---

# Valency

**Valency is WFC-style constraint solving on cluster topology.** It assigns collection entries to Vtx based on connectivity constraints — orbitals, cages, bonding rules — then solves for valid configurations across the entire cluster. The system sits at the intersection of clusters and asset staging, using both as its foundation.

{% hint style="warning" %}
Valency is an advanced system that builds on clusters and asset staging. Familiarity with both is recommended before diving in.
{% endhint %}

### Sections

<table data-view="cards"><thead><tr><th>Section</th><th>Contents</th></tr></thead><tbody><tr><td><a data-mention href="rules/">rules</a></td><td>Bonding rules, socket rules, orbital set definitions</td></tr><tr><td><a data-mention href="valency-staging/">valency-staging</a></td><td>Solvers — constraint solver, entropy solver, priority solver</td></tr><tr><td><a data-mention href="valency-patterns/">valency-patterns</a></td><td>Pattern matching and replacement on solved clusters</td></tr></tbody></table>

Once a solve completes, utility nodes extract the results — writing socket transforms from solved module assignments or exposing orbital data as point attributes for downstream processing.

### Concepts

For understanding orbitals, cages, bonding rules, and the solve process:

* [Valency Concepts](../../working-with-pcgex/valency/valency/)
