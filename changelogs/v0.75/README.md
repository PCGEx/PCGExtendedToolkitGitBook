---
description: Tracking changes since 0.74.4
icon: tag
---

# v0.75

{% hint style="info" %}
## Cont'd stability + reinforced testing

There's now around \~**1200 tests** covering the most critical aspects of PCGEx. Low level math, serialization, distribution etc.

It's still ridiculous coverage when you look at the codebase size; but going through that automated testing process did help uncover quite a number of small bugs and mathematical typos across the last few updates.
{% endhint %}

## Cluster Decomposition

> I moved a single broken old feature I'm positive nobody ever used into its own full fledged module : `PCGExElementsClustersDecomp`

It follows the same "Refine" template : a simple [cluster-decomposition](../../node-library/clusters/analyze/cluster-decomposition/ "mention") node with a battery of different swappable behaviors. The most useful one is by far [decomp-max-boxes-extended.md](../../node-library/clusters/analyze/cluster-decomposition/decomp-max-boxes-extended.md "mention")

<figure><img src="../../.gitbook/assets/image (322).png" alt=""><figcaption></figcaption></figure>

<div><figure><img src="../../.gitbook/assets/image (321).png" alt=""><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (323).png" alt=""><figcaption></figcaption></figure></div>

<div><figure><img src="../../.gitbook/assets/image (324).png" alt=""><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (327).png" alt=""><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (325).png" alt=""><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (326).png" alt=""><figcaption></figcaption></figure></div>

***

## Staging and Collections just got a lot better

This last update has some major feature rounding up around collections.&#x20;

There's a **completely new UI** for managing collection entries! The previous array still exists but is low-key hidden through menus — I'm not removing it just yet because I get very little feedback on who uses it/how and the new approach might not be a desirable default : make your voice heard if that's the case.

<figure><img src="../../.gitbook/assets/image (328).png" alt=""><figcaption></figcaption></figure>

Supports:

* Multi-selection editing
* `Ctrl+D` : duplicate selection in place
* `Del` : Delete selection
* Drag'n drop to reorder

#### Actor Collection Serialization

Actor collections now have the ability to serialize any  properties, including nested components values. You can pick actors from existing levels and add them to a collection, they will be spawned with all their uniqueness.

This requires your to use [staging-spawn-actors.md](../../node-library/staging/staging-spawn-actors.md "mention") in order to benefit from it, and it's not supporting setting properties from attribute (yet).

<figure><img src="../../.gitbook/assets/image (319).png" alt=""><figcaption></figcaption></figure>

#### PCG Data Asset Collections

[pcg-data-asset-collection](../../node-library/staging/collections/pcg-data-asset-collection/ "mention") made their way to PCGEx a few updates ago, but they now offer the possibility to pick a Level directly : that level will be exported in-place as a custom PCGDataAsset, _with full actor serialization support_; allowing you to load and rebuild more complex setups at runtime.&#x20;

> It's a tradeoff of course because it makes the collection a data owner as opposed to a simple proxy but it's very handy for small constructs.

<figure><img src="../../.gitbook/assets/image (318).png" alt=""><figcaption></figcaption></figure>

#### Level Collections

[level-collection.md](../../node-library/staging/collections/level-collection.md "mention") are collections of levels that can be used with [staging-spawn-level.md](../../node-library/staging/staging-spawn-level.md "mention"). This is very niche usecase as it allows you to spawn level instances directly -- it's geared toward editor-only work as runtime doesn't have the same optimized cooking processes (_at runtime it justs spawns actors it's not very efficient_)

<figure><img src="../../.gitbook/assets/image (317).png" alt=""><figcaption></figcaption></figure>

#### Other tweaks

[staging-type-filter.md](../../node-library/staging/staging-type-filter.md "mention")got updated as well and can now split points to pins matching their entry types. It came as a natural consequence of supporting more data types with their dedicated spawners.

<figure><img src="../../.gitbook/assets/image (316).png" alt=""><figcaption></figcaption></figure>

***

### Smooth Manifold Projection

Cell-finding nodes support a new way of building the DCEL the algorithms rely on to find cells on clusters that don't work well with a simple planar projection.&#x20;

It has limitations and will generate wrong results if the topology isn't "smooth" enough; but it can find cells the planar/best fit approach cannot.

<figure><img src="../../.gitbook/assets/image (320).png" alt=""><figcaption></figcaption></figure>

***

## Misc

{% hint style="warning" %}
## Noise 3D Ranges have changed

Moved the old \[-1:1] to \[0:1] and added scaling on consumers.
{% endhint %}

### Helpful Dataviz

I've added some fancy debug drawing on some settings that require a tad bit too much appetite for abstraction, or because the math/settings pairing can be unintuitive:

<figure><img src="../../.gitbook/assets/image (310).png" alt=""><figcaption><p><a data-mention href="../../node-library/filters/edge-filters/edge-filter-endpoints-check.md">edge-filter-endpoints-check.md</a></p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (311).png" alt=""><figcaption><p><a data-mention href="../../node-library/filters/vtx-filters/vtx-filter-adjacency.md">vtx-filter-adjacency.md</a></p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (312).png" alt=""><figcaption><p><a data-mention href="../../node-library/filters/vtx-filters/vtx-filter-adjacency.md">vtx-filter-adjacency.md</a></p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (313).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (314).png" alt=""><figcaption></figcaption></figure>

