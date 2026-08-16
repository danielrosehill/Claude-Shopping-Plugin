---
name: marketplace-plugins
description: The roster of marketplace-specific shopping plugins — which marketplaces have a dedicated plugin, what each one covers, and how to install it. Use when the user names a marketplace such as Amazon or AliExpress, when a price, stock or delivery answer needs that marketplace's own quirks to be correct, when they ask what shopping plugins exist or how they fit together, or when a generic fetch has already failed on a marketplace domain.
allowed-tools: Read, WebSearch, Bash(gh *), Bash(ls *)
---

# Marketplace-specific plugins

`shopping` is the **general** plugin: region-tuned consumer retail — local
vendors, language, VAT, regional aggregators, parallel-import conventions, and
the workspace scaffolding around a shopping decision.

Individual marketplaces get their **own plugins**. This skill is the roster, and
the handoff.

## Why the split

The knowledge that makes an answer *correct* on a given marketplace is specific,
volatile, and does not generalise: which fetch route reaches it, which of its
fields lie, how its filter grammar composes, what its delivery promise is
actually conditional on, which of its facts only render to a signed-in session.

Folding that into a general plugin makes the general plugin wrong everywhere
else, and makes the marketplace knowledge impossible to version independently —
Amazon changes its facet IDs on its own schedule, not on `shopping`'s.

So: one plugin per marketplace, each versioned and installed on its own, and
this list to find them.

**This supersedes the earlier design** in which marketplaces were data files
under `profiles/` read by `shopping`'s own commands. That approach is why
`amazon-search`, `brand-scrub` and `profiles/amazon-us.json` used to live here;
they now live in `amazon-us`. See [`profiles/README.md`](../../profiles/README.md).

## Roster

Keep this table current — it is the point of the skill. Add a row when a
marketplace plugin ships; do not let it go stale silently.

| Marketplace | Plugin | Repo | Covers |
| --- | --- | --- | --- |
| Amazon.com (US) | `amazon-us` | [`danielrosehill/Claude-Amazon-US-Plugin`](https://github.com/danielrosehill/Claude-Amazon-US-Plugin) | Fetch routing, verified listing data, Prime-aware delivery dates checked against the ZIP they were rendered for, filtered signed-in search, brand allow/blocklist |
| AliExpress (Israel) | `aliexpress-israel-skills` | [`danielrosehill/Aliexpress-Israel-Skills`](https://github.com/danielrosehill/Aliexpress-Israel-Skills) | ILS pricing, Choice, VAT and customs thresholds, image search, cart and export |

Candidates with no plugin yet: **Newegg (US)** — a stub profile survives at
[`profiles/newegg-us.json`](../../profiles/newegg-us.json), explicitly unverified.

## When to hand off

Hand off when the answer depends on the marketplace itself rather than on the
product:

- The user names the marketplace, or gives a URL or product ID belonging to it.
- The question is price, stock, delivery date or seller identity **on that
  marketplace** — as opposed to "what should I buy", which stays here.
- A generic fetch already failed on that domain, or returned something that
  looks right but cannot be trusted.

Stay in `shopping` for regional retail discovery, vendor comparison across
non-marketplace stores, and the workspace around a decision.

**If the marketplace has no plugin, say so plainly** rather than improvising
marketplace-specific behaviour here. Guessing a fetch route or a delivery caveat
produces a confident wrong answer, which is worse than "not covered yet". If it
is a marketplace the user works with regularly, that is a signal it should get a
plugin — say so.

## Installing

Check first whether it is already installed: its skills appear in the session's
skill list if so.

```
/plugin marketplace add danielrosehill/Claude-Code-Plugins
/plugin install amazon-us
```

Tell the user what installing gets them in a sentence and let them run it —
installing changes their configuration, so do not do it for them unasked. They
may need to reload the session before the new skills and commands appear.

## Reporting

When you hand off, say which plugin answered, so the result carries its
provenance. And carry that plugin's caveats through rather than flattening them
into a number — a delivery date that is non-Prime, a ZIP that could not be
confirmed, a price that excludes an untickable coupon all change the decision.
