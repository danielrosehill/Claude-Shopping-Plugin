# Israeli Shopping — Discovery Techniques

These are the manual techniques that work well for finding Israeli listings for a product. Half the battle is figuring out the **accepted Hebrew retail term** for the product class. Once you have that, every retailer search starts working. The other half is knowing which surface to query first.

The shopping-plugin commands emulate these patterns rather than going straight to a single retailer's search box.

---

## The terminology problem (read this first)

Israeli retailers index their catalogues in Hebrew. Google Translate often gives a literal translation that **does not match retail listings** — for example, "charging station" translates to `תחנת טעינה`, but the term retailers actually use is `עמדת טעינה`.

A single product class can have two or three accepted Hebrew terms. A search that returns nothing isn't necessarily evidence the product isn't sold — it's often evidence you used the wrong Hebrew noun.

Before searching any retailer:

1. Check `data/hebrew-category-map.json` for a known mapping.
2. If the class isn't mapped, run the **reverse-lookup technique** (below) and cache the result back into the map.
3. Always carry **all** known Hebrew variants forward into the search — try each.

---

## Technique 1 — Reverse-lookup from a known SKU listing

**When to use:** the product class has no entry in `hebrew-category-map.json`, or the existing entry returns weak results.

**How it works:** instead of guessing the Hebrew term, find ONE Israeli listing for a representative SKU in the same product class, then read the canonical Hebrew noun off the page title, URL slug, or breadcrumbs.

**Workflow:**

1. Pick a representative SKU you know is sold in Israel (e.g. an Anker Prime model for desktop chargers, an Eneloop pack for batteries).
2. Google: `"<exact brand model>" site:.il` — almost always returns at least one IL listing.
3. Open the top result and look at:
   - **Page title** — usually starts with the Hebrew product class noun (e.g. `עמדת טעינה עוצמתית 6 יציאות Anker Prime...`)
   - **URL slug** — Hebrew nouns appear url-encoded (e.g. `%D7%A2%D7%9E%D7%93%D7%AA-%D7%98%D7%A2%D7%99%D7%A0%D7%94` decodes to `עמדת-טעינה`)
   - **Breadcrumbs** — the category path is often the cleanest source
4. Extract the Hebrew noun(s). Note plural vs singular (Hebrew retail listings often use singular even when the product is plural).
5. Cache to `hebrew-category-map.json` so the next caller doesn't re-derive.

**Example:** searching for the term "desktop charging tower" — the Lev Hamisrad listing for Anker Prime A2683 has the URL `lev-hamisrad.co.il/עמדת-טעינה-עוצמתית-6-יציאות-Anker-Prime-...`, so the canonical Hebrew is `עמדת טעינה` (or `עמדת טעינה עוצמתית` for higher-wattage variants).

**Why this beats Google Translate:** retailers use idiomatic Israeli retail jargon. Google Translate doesn't know that. A real listing does, by definition.

---

## Technique 2 — Google with `site:.il`

**When to use:** discovery mode, or when retailer-specific searches return nothing.

**Patterns:**

| Pattern | Example | What it surfaces |
|---|---|---|
| `<brand model> site:.il` | `Anker A2343 site:.il` | All IL pages mentioning that exact SKU |
| `<brand model> site:co.il` | `Anker A2343 site:co.il` | Commercial-only IL pages (less news, more shops) |
| `"<exact phrase>" site:.il` | `"GaN 250W" site:.il` | Niche specs, surfaces specialist retailers |
| `<hebrew term> site:.il` | `עמדת טעינה Anker site:.il` | IL retailers indexed under the Hebrew term |

**Pros:** surfaces listings on stores that aren't in `israeli-stores.json`, including small specialist retailers, brand resellers, and forum mentions that link to listings.

**Cons:** mixes news, blogs, and review sites with retail listings. Filter results by domain.

---

## Technique 3 — Hebrew price/buy prefix

**When to use:** when the Hebrew term is known but retailer-specific searches are weak; useful for surfacing aggregator pages and small-shop listings.

**Patterns:**

| Pattern | Example |
|---|---|
| `מחיר <hebrew product>` | `מחיר עמדת טעינה Anker Prime` |
| `קנה <hebrew product>` | `קנה עמדת טעינה Anker Prime` |
| `<hebrew product> בארץ` | `עמדת טעינה בארץ` |
| `<hebrew product> ישראל` | `עמדת טעינה Anker ישראל` |

`מחיר` (price) is the strongest signal — pages that contain the word "price" next to the product noun are almost always commercial listings, not blogs.

`קנה` (buy) is similar but slightly noisier.

`בארץ` ("in the country", Israeli idiom for "in Israel") and `ישראל` are useful when Google is returning too many international results.

**Caveat:** these patterns can surface irrelevant options (price-comparison stubs, dropshipping fronts, expired listings). Always sanity-check before quoting prices.

---

## Technique 4 — Google Shopping IL

**When to use:** quick survey across many retailers when you want a price-ranked list and don't need the deep filter control of Zap.

**URL:** `https://www.google.com/search?q=<query>&tbm=shop&gl=il&hl=he`

- `tbm=shop` — Google Shopping vertical
- `gl=il` — geolocation Israel (forces IL pricing/retailers)
- `hl=he` — Hebrew interface (returns Hebrew listings)

**Pros:** very fast, ranks by relevance + price, shows retailer logos.
**Cons:** Google Shopping IL coverage is patchy — many IL retailers don't feed Google Shopping. Use as a complement to Zap, not a replacement.

---

## Technique 5 — Zap aggregator

**When to use:** the canonical first-pass for any product that's sold by multiple IL retailers. Zap indexes prices across all tier-1 + tier-2 retailers and many specialists.

**URL:** `https://www.zap.co.il/search.aspx?keyword=<hebrew query>`

Zap returns a comparison page with all vendor prices in one table. Click through to the specific retailer for the actual listing.

**When Zap doesn't help:** new SKUs not yet indexed, niche products from a single specialist, second-hand items.

---

## Technique 6 — Try multiple Hebrew variants

When you have several candidate Hebrew terms (from the map or from reverse-lookup), search **each** rather than picking one. Different retailers tag the same product class differently.

Example for "desktop charging tower":

- `עמדת טעינה` — most listings, generic
- `עמדת טעינה עוצמתית` — KSP's term for high-wattage models
- `מטען שולחני` — "desktop charger", used by Bug and TMS
- `תחנת טעינה` — alt phrasing, less common in retail
- `מטען רב-ערוצי` — "multi-channel charger"

Combine with the brand name: `<hebrew term> Anker Prime` is far higher-signal than either token alone.

---

## Technique 7 — Brand store fallback

For products from a brand with an official Israeli distributor (Nitecore, Anker, UGREEN, Eneloop/Panasonic), check the brand's `.co.il` site directly. Often:

- Cheaper than mainstream chains
- Has the full SKU range (mainstream chains carry only the bestsellers)
- Authoritative warranty info

Check `data/israeli-stores.json` for `is_tech: true` brand stores before falling back to general retailers.

---

## Discovery order — recommended default

For a typical "find me prices for X in Israel" query, walk the techniques in this order:

1. **Resolve Hebrew term** — map lookup, then reverse-lookup if missing (Technique 1).
2. **Zap** (Technique 5) — fastest comprehensive view.
3. **Per-retailer search** at the four mainstream chains using the Hebrew variants — only if Zap misses or you need a specific retailer's price.
4. **Google `site:.il`** (Technique 2) — to catch specialist retailers Zap doesn't index.
5. **`מחיר` + Hebrew prefix** (Technique 3) — last-resort discovery for poorly-indexed products.
6. **Google Shopping IL** (Technique 4) — sanity-check the price ranking from steps 2–5.

Skip steps that the user explicitly rules out, but otherwise default to running 1 + 2 minimum.
