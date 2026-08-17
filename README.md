# shopping-plugin

Claude Code plugin for region-specific consumer shopping — find local products, compare vendors, check availability, and generate ranked purchase recommendations against a personal spec. Distinct from the general `procurement-tools` plugin: this one is tuned to local retail quirks (language, VAT, regional pricing aggregators, parallel-import conventions).

## Which plugin do I want?

This plugin is the **place-specific** half of the buying stack. It is about *where
you buy*: how a given retailer's catalogue, language, currency, tax treatment,
stock data and delivery conventions actually work.

The **`procurement-tools`** plugin is the other half. It is the marketplace-agnostic
*process*: working out what the thing is and what it is called, writing a spec,
generating the search vocabulary, judging candidates against it, and choosing.

| You are doing this | Use |
| --- | --- |
| Finding who stocks it, at what price, in stock when | **`shopping`** |
| Comparing vendors for a SKU you have already settled on | **`shopping`** |
| Israeli retailers, Hebrew terms, Zap, ILS, parallel imports | **`israel-shopping`** |
| One named marketplace's search syntax and quirks | that marketplace's own plugin (`amazon-us`, `aliexpress-israel-skills`, `grainger`, …) |
| I don't know what this thing is called, or what to search for | **`procurement-tools`** → `define-spec` |
| Turning a want into a spec, then judging candidates against it | **`procurement-tools`** |
| Surveying a whole product category before choosing | **`procurement-tools`** → `mkt-*` |

The two halves are designed to be installed together. Start in `procurement-tools`
when the question is "what should I buy"; come here once it is "where do I get it".

## What you get

### Primitives (always available once the plugin is installed)

**Generic commands** (`/shopping:*`):
- `find-product` — locate a product across configured vendors, return a ranked price table
- `compare-vendors` — side-by-side comparison of a known SKU across stores
- `check-availability` — per-vendor stock and delivery-time check
- `intake` — turn an informal product brief into a structured `spec.md`
- `recommend` — read the workspace spec + catalog data and emit a ranked recommendation (Typst PDF)
- `add-product` — ingest a new catalog screenshot / listing into the workspace dataset
- `catalog-to-json` — extract structured product/price data from catalog screenshots
- `compare` — targeted comparison of specific products across all stores in scope

**Israel-region commands** have moved to the [`israel-skills`](https://github.com/danielrosehill/Israel-Skills-Plugin) plugin. Install it alongside `shopping` for IL-specific retail discovery (Zap, KSP/Ivory/Bug/TMS, Hebrew term resolution, ILS currency, etc.).

### Marketplace plugins — moved out

Marketplace-specific research now lives in **dedicated per-marketplace plugins**,
one repo each, versioned independently. The `marketplace-plugins` skill carries
the roster and hands off to whichever applies:

| Marketplace | Plugin |
| --- | --- |
| Amazon.com (US) | [`amazon-us`](https://github.com/danielrosehill/Claude-Amazon-US-Plugin) |
| AliExpress (Israel) | [`aliexpress-israel-skills`](https://github.com/danielrosehill/Aliexpress-Israel-Skills) |

**Breaking change in 2.0.0:** `amazon-search`, `brand-scrub` and
`profiles/amazon-us.json` were removed from this plugin and now live in
`amazon-us`. Install it to get them back:

```
/plugin install amazon-us
```

Why: Amazon changes its facet IDs on its own schedule, marketplace skills and
general shopping skills were competing to match the same requests, and someone
who wants Israeli retail discovery has no use for Amazon extraction contracts.
Rationale in full at [`profiles/README.md`](profiles/README.md).

### Provisioning skill

- `/shopping:new-workspace <name> [--variant=israel|generic] [--local-only] [--private]`

Scaffolds a new shopping workspace (spec template, catalogs dir, outputs dir, region data), personalises `CLAUDE.md` from `~/.claude/CLAUDE.md`, and (by default) creates a public GitHub repo.

## Pattern

Primitives live in the plugin → globally available from any cwd.
Workspace scaffolds are provisioned as **data** → no `.claude/` tree inside provisioned workspaces.
Plugin updates never touch your workspace data.

See [PLAN.md in Claude-Workspace-Reshaping-190426](https://github.com/danielrosehill/Claude-Workspace-Reshaping-190426) for the full pattern spec this plugin follows.

## Variants

- `israel` — full Israeli scaffold: Hebrew term resolution hints, Zap-first discovery order, VAT/Eilat pricing rules, store categories, catalog folders tuned for IL retailers (`ksp/`, `ivory/`, `bug/`, `ali/`).
- `generic` — region-agnostic scaffold for a future region. Start here when the target market doesn't yet have a dedicated variant; fill in retailer list + currency + tax rules under `data/`.

More region variants will land under `template/<region>/` as the shopping plugin grows.

## Install

Via the danielrosehill marketplace:

```
/plugin marketplace add danielrosehill/Claude-Code-Plugins
/plugin install shopping
```

## License

MIT.
