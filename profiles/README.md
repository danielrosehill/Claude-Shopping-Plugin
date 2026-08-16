# Marketplace profiles

> **Superseded, 2026-08-16.** This directory used to hold one profile per
> marketplace, read by `shopping`'s own commands. The pattern is now **one plugin
> per marketplace** — see the [`marketplace-plugins`](../skills/marketplace-plugins)
> skill for the roster.
>
> `amazon-us.json`, along with the `amazon-search` and `brand-scrub` skills that
> read it, moved to
> [`danielrosehill/Claude-Amazon-US-Plugin`](https://github.com/danielrosehill/Claude-Amazon-US-Plugin).
> Its derivation methodology went with it, at `profiles/README.md` there.

## Why the pattern changed

The original note here argued the opposite: *"Deliberately not one plugin per
marketplace. Adding a marketplace is adding a file, not a repo."* That was
reasonable while there was one profile, and it stopped being reasonable once the
Amazon material grew to two skills, two tested extractors and a 300-line profile
with its own session-dependence findings.

Three things broke:

- **Versioning.** Amazon changes its facet IDs on its own schedule. Shipping
  that as part of `shopping` means every Amazon fix is a `shopping` release.
- **Install weight.** Someone who wants Israeli retail discovery has no use for
  Amazon extraction contracts, and vice versa.
- **Skill collision.** Marketplace skills and general shopping skills compete to
  match the same user requests, and the model picks between them with nothing to
  go on.

A profile is still the right *format* for encoding a marketplace. It is just
that the profile and the skills that read it belong together, in their own
repository.

## What is still here

[`newegg-us.json`](newegg-us.json) — a **PLACEHOLDER, never derived**. Every
field in it is an unverified hypothesis and it explicitly says so. Do not
compose a search URL from it or trust a result set produced with it.

It stays as a record of the shape a second US tech marketplace would take. If
Newegg becomes worth doing properly, the work is to derive it and ship it as its
own plugin, following the six-step method now documented in the `amazon-us`
plugin's `profiles/README.md`.
