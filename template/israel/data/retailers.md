# Israeli Tech Retailers

> **These URL patterns are last-known-good hints, not contracts.** Retailers change their search systems without notice. If a documented pattern returns 404 or redirects to homepage, fall back to the homepage-form approach (navigate, find `input[type=search]` / `input[name=q]`, submit query) and update the table below with what you discover.

## Tier 1 — Mainstream (big high-street chains)

| Retailer | Slug | Domain | Search URL Pattern (last verified) | Notes |
|----------|------|--------|------------------------------------|-------|
| KSP | ksp | ksp.co.il | `https://ksp.co.il/web/cat/search?text={query}` | Shows two prices: regular + Eilat (no VAT). Always use the HIGHER (regular) price. |
| Ivory | ivory | ivory.co.il | `https://www.ivory.co.il/catalog.php?q={query}` | Form action is `catalog.php` with `name=q`. The historical `catalogsearch/result/?q=` pattern (Magento default) does NOT work — Ivory is on a custom catalog. |
| Bug | bug | bug.co.il | `https://www.bug.co.il/search?q={query}` | Has bot detection — Playwright only. |
| TMS | tms | tms.co.il | `https://www.tms.co.il/search?q={query}` | Standard search. |
| Machsanei Hashmal (Pay&Go) | machsanei-hashmal | payngo.co.il | `https://www.payngo.co.il/search?q={query}` | Appliances + electronics |
| Kravitz | kravitz | kravitz.co.il | `https://kravitz.co.il/catalogsearch/result/?q={query}` | Magento search |
| iDigital | idigital | idigital.co.il | `https://www.idigital.co.il/search?q={query}` | Apple premium reseller |

## Tier 2 — Major

| Retailer | Slug | Domain | Search URL Pattern (last verified) | Notes |
|----------|------|--------|------------------------------------|-------|
| Ace | ace | ace.co.il | `https://www.ace.co.il/search?q={query}` | Big-box home improvement + electronics |
| Home Center | homecenter | homecenter.co.il | `https://www.homecenter.co.il/search?q={query}` | Home improvement + appliances |
| Office Depot | office-depot | officedepot.co.il | `https://www.officedepot.co.il/search?q={query}` | Office supplies + tech accessories |
| Audioline | audioline | audioline.co.il | `https://www.audioline.co.il/catalogsearch/result/?q={query}` | Audio equipment specialist |

## Tier 3 — Niche

| Retailer | Slug | Domain | Search URL Pattern (last verified) | Notes |
|----------|------|--------|------------------------------------|-------|
| Alpesek (UPS Specialist) | alpesek | alpesek.co.il | `https://www.alpesek.co.il/?s={query}` | UPS and power protection — WordPress search |
| Sollan (Batteries) | sollan | sollan.co.il | `https://www.sollan.co.il/?s={query}` | Battery specialist — WordPress search |

## Tier 4 — Brand Stores

| Retailer | Slug | Domain | Search URL Pattern (last verified) | Notes |
|----------|------|--------|------------------------------------|-------|
| Nitecore Israel | nitecore | nitecore.co.il | `https://nitecore.co.il/catalogsearch/result/?q={query}` | Official Nitecore (flashlights, chargers, batteries) |

## Aggregators (not in tier table — covered by their own skills)

| Aggregator | Domain | Skill | Notes |
|------------|--------|-------|-------|
| Zap | zap.co.il | `search-zap` | Canonical IL price-comparison aggregator. Try first for any cross-retailer query. |
| Google IL | google.co.il | `search-google-il` | Use as discovery surface — `site:.il`, `מחיר <hebrew>`, Google Shopping. |

## Common search-form patterns (when a URL fails)

If the documented URL pattern doesn't work, the retailer is most likely on one of these platforms:

| Platform | Default search URL | Form input |
|----------|-------------------|------------|
| Magento 2 | `/catalogsearch/result/?q={query}` | `input[name=q]` |
| WordPress / WooCommerce | `/?s={query}` | `input[name=s]` |
| Shopify | `/search?q={query}` | `input[name=q]` |
| Custom (KSP, Ivory) | varies | inspect `<form>` action |

When falling back to the homepage-form approach in Playwright:

```js
// Find any plausible search input
const inputs = document.querySelectorAll(
  'input[type=search], input[name=q], input[name=s], input[id*=search i], input[placeholder*=חיפוש]'
);
```

Read `form.action` to learn the actual search endpoint, then update this file with the discovered pattern.

## Domain Whitelist

In addition to the curated retailers above, the `data/israeli-stores.json` file contains 830+ Israeli stores with `is_tech` classification. Only navigate to domains from the curated list above OR stores marked `is_tech: true` or `is_tech: null` in the dataset. Never navigate to stores marked `is_tech: false`.
