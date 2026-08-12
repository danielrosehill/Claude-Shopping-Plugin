# Marketplace profiles

A profile is **encoded context, not an integration**. It exists because almost no
consumer marketplace offers a buy-side API, and the ones that do gate it behind a
seller or affiliate relationship. That is not an impediment — the exploratory work a
profile front-loads (which filter does what, which brands are real, what identifier to
quote) yields better recommendations than scraping would, and it only has to be done
once per marketplace instead of once per purchase.

Deliberately **not** one plugin per marketplace. Profiles are data files here; the
`shopping` commands read them. Adding a marketplace is adding a file, not a repo.

## What a profile has to answer

| Section | The question it settles |
| --- | --- |
| `identifier` | What is the canonical handle for a product here, and what does it look like? |
| `search` | How is a search URL constructed — query, filter and sort parameter names? |
| `filters` | Which facet tokens map to which human labels, with their caveats? |
| `recipes` | The composed URLs worth keeping, and what they collapse to in practice. |
| `trust_rubric` | How do you tell a real product from junk *on this specific marketplace*? |
| `access` | What API exists, what it really costs, and which fetch route works. |
| `traps` | What fails silently. |

The `trust_rubric` is the part that carries the value, and it is marketplace-shaped
rather than generic. On Amazon the axis is first-party vs third-party sellers and
brand existence off-platform, because the catalogue is open. On a single-seller
retailer that axis collapses entirely and the useful questions become stock accuracy,
grey-import status and warranty origin.

## Profiles are time-bound

Facet IDs, sort keys and filter availability change, and some are session-dependent —
the same URL shows different facets signed-in versus anonymous. Every profile carries
`verified` and `verified_how`. Treat anything older than a few months as a hypothesis
and re-derive it the same way it was derived the first time.

## Deriving one

The method that produced `amazon-us.json`, reusable as-is:

1. `verify_egress` for the target country. Wrong-country failures return HTTP 200 with
   plausible content, so this is the only signal that exists.
2. Fetch a broad search page for a category with many competing sellers. It will be
   large — mine it from the saved tool-result file, never into context.
3. Extract every filter link with its label. On Amazon that is markdown `[label](url)`
   pairs whose URL carries the filter parameter; the label is the sidebar text.
4. Recompose the tokens you care about into one URL and re-fetch it. This is the step
   that catches a token that parses but does not compose.
5. Record what you confirmed and what you did not, per key. `amazon-us.json` marks one
   sort value confirmed and four unconfirmed rather than presenting five in one voice.

## Session dependence — the trap that produces a wrong profile

Derive against the **real signed-in session**, not just an anonymous fetch. On Amazon
the two disagree: an anonymous page hides the Prime filter and the curated brand facets
entirely, and shows a degraded free-shipping stand-in in their place. Both passes are
recorded in `amazon-us.json` under `session_dependence`, because the anonymous result
looked complete and was not.

## Current profiles

| Profile | State |
| --- | --- |
| `amazon-us.json` | Verified 2026-08-12, both anonymous and signed-in passes |
| `newegg-us.json` | **Placeholder — not derived.** Slot and shape recorded; every field is a hypothesis except the price regex carried over from the `purchasing` plugin |

The set is deliberately tech-heavy and expected to stay that way.

B&H (`bhphotovideo.com`) is the remaining candidate: a first-party single seller, so the
seller axis collapses and the weight shifts to ships-to-Israel handling, tax treatment
and the affiliate product feed behind the `affportal.bhphoto.com` login.

## Brands are a profile output

`p_123` brand IDs are stable and global, so the brand facet is harvestable into a durable
allowlist and blocklist that transfers across every future search. That is what the
`brand-scrub` skill does; `examples/brands.example.json` is a seeded starting point.
Scrub brands from the **facet rail**, never from result cards — there is no dependable
per-card brand element, and the obvious attempt captures the "Amazon's Choice" badge
instead.
