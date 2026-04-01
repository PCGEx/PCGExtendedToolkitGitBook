---
icon: gem
---

# PCGEx Pro

**PCGEx is free and staying free.** The 200+ node core — MIT-licensed, open source, production-ready — is the foundation for everything. That doesn't change. Pro exists so that foundation can keep growing.

## What is PCGEx Pro?

PCGEx Pro is a paid plugin that embeds the full free core and adds niche companion modules on top. If you're already using PCGEx, switching to Pro is seamless — uninstall the free version, install Pro, done. No migration, no redirectors, no project changes.

The additional modules are specialized tools that require significant development investment and serve workflows narrower than the general-purpose core.

### Current Modules

| Module | Status | What it does |
|--------|--------|--------------|
| **ZoneGraph Interop** | Complete | Bridges PCGEx cluster data with Unreal's ZoneGraph system |
| **Constraint Solver** | In development | WFC and dungeon-architect-style constraint solving for procedural layout generation |

These are the same kind of low-level, composable tools as the free core. They just target domains that are narrower and take considerably more time to build and maintain.

{% hint style="info" %}
Pro is not a ready-made solution. It's specialized tooling — same philosophy, different scope.
{% endhint %}

## Why Pro Exists

Maintaining and expanding an open-source toolkit of this scale is a tremendous time investment. The free core stays robust because it's the foundation everything else is built on. Pro is how the continued development of *all of it* becomes sustainable.

No features will be removed from the free core to push people toward Pro. No degradation, no artificial limits. Pro modules are extensions that depend on the core — they don't replace any part of it.

## Purchasing

### FAB Listing

PCGEx Pro is sold on FAB — Epic's marketplace.

| Tier | Price |
|------|-------|
| Personal | $49.99 |
| Professional | $149.99 |

These tiers are FAB's standard structure. The price is expected to increase gradually as Pro grows. If you buy at the current price and it goes up later, that doesn't affect you — you already have your license.

### Source Access

FAB distributes Pro as a single bundled `.uplugin`. Behind the scenes, each module lives in its own repository. After purchasing, you can request access to the [PCGEx GitHub org](https://github.com/PCGEx) — you'll be added to the Pro team, which gives you access to the individual repos.

## License Terms

PCGEx Pro follows **FAB's standard per-seat EULA**. Here's what that means in practice:

**One purchase = one user.** A 10-person studio needs 10 licenses.

**You own what you bought.** Your purchased version works forever. You can ship commercial games with it, modify the source for your project, and keep your local copy indefinitely. There is no expiration.

**No subscription.** There is no recurring fee. You get rolling updates as long as your engine version is actively supported. When an engine version reaches end of life, updates for that version wind down naturally.

**Don't redistribute.** The source is for your use — not for public sharing.

### Rolling Updates

There are no yearly editions or version-locked releases. PCGEx Pro is a single, continuously updated product. As new engine versions ship, Pro is updated to match. One listing, one product, ongoing updates.

### Studios & Custom Licensing

For teams that need volume licensing or custom terms, reach out directly on [Discord](https://discord.gg/Aze3puAg6T) or [GitHub](https://github.com/Nebukam). These are handled case-by-case.

## Community & Supporters

### Contributors

People who contribute meaningfully to PCGEx — code, bug reports, community support — may receive a complimentary Pro license. This is handled personally, not through an automated system. If you've contributed and feel this applies, reach out.

### Patreon

[Patreon](https://www.patreon.com/c/pcgex) continues as an independent way to support PCGEx development. Long-standing members whose cumulative contributions reach approximately $25 (half the FAB Personal tier price) will receive a complimentary Pro license. Reach out on Discord when you hit that threshold.

Patreon is not required to use Pro, and Pro is not required to use the free core. They're parallel paths.

## The Line Between Free and Paid

The free core covers everything generic: cluster construction, pathfinding, filtering, sampling, staging, topology, tensors, paths, spatial operations, metadata, blending, sorting, partitioning. That's the vast majority of what PCGEx does, and it stays free.

Pro covers modules that are **niche** (specialized workflows, not general-purpose PCG), **development-intensive** (months of focused work each), and **dependent on the free core** (extensions, not replacements).

The goal is straightforward: keep giving away the broadly useful stuff, charge fairly for the deeply specialized stuff, and use the revenue to make all of it better.

## Related

- [FAQ](pcgex-pro-faq.md) — Common questions about purchasing, licensing, and access
- [About](about.md) — Project history and design philosophy
- [Supporters](supporters.md) — People and studios supporting PCGEx
