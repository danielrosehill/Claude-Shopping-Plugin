# Compare — Side-by-Side Product Comparison (Israel)

Generate a focused comparison of 2–4 specific products without going through the full shortlist/source/recommend flow. Useful when the user has already narrowed down candidates and wants a quick side-by-side with Israeli sourcing.

## Usage

- Inline: "compare the TP-Link AX3000 vs Netgear Nighthawk"
- Or run `/compare` and be prompted

## Before comparing

1. Read `spec.md` — to apply priorities and standing preferences when calling a winner
2. Verify Standing preferences snapshot is populated
3. Check `for-ai/` — user may have dropped sources for these specific products

## Process

1. **Identify products** — ask if not specified
2. **For each product, run the waterfall abbreviated**:
   - Israeli domestic best price + store (using `search-main-tech-stores` / `search-major-retailers` / `search-by-category` as appropriate)
   - Amazon-IL best price + landed cost (if shippable + spec allows)
   - AliExpress best price + landed cost (only if branded seller ≥95% and spec allows)
3. **Aggregate** one row per product across the comparison axes
4. **Render** as a markdown table on screen + save to `from-ai/compare.md`

## Default comparison axes

| Axis | Notes |
|------|-------|
| Landed price (₪, best channel) | Best of domestic / Amazon-IL / AliExpress |
| Best channel | Which won |
| Lead time | Per channel |
| Warranty | Per channel |
| Rating aggregate | X.X/5 from N sources |
| Spec highlights | 3–5 key specs |
| Markup vs RRP | Israeli best vs international RRP |
| Must-haves met | From `spec.md` if configured |
| Hard nos clear | From `spec.md` + standing avoided brands |

## Output format

Write `from-ai/compare.md`:

```markdown
# Comparison: [Product A] vs [Product B] vs [Product C]

**Date**: [Date]
**Products**: [Count]
**Spec priorities**: [From spec.md]

## Quick summary

| | [Product A] | [Product B] | [Product C] |
|---|---|---|---|
| **Best landed price (₪)** | ... | ... | ... |
| **Best channel** | [Domestic / Amazon-IL / AliExpress] | ... | ... |
| **Lead time** | 1–3 days | 1–2 weeks | 3–5 weeks |
| **Warranty** | 1y local | Manufacturer intl | None / seller |
| **Rating** | X.X/5 | X.X/5 | X.X/5 |
| **Markup vs RRP** | +X% | +X% | +X% |

## Must-haves

| Must-have | [A] | [B] | [C] |
|-----------|-----|-----|-----|

## Hard nos

| Hard no | [A] | [B] | [C] |
|---------|-----|-----|-----|

## Specs

| Spec | [A] | [B] | [C] |
|------|-----|-----|-----|

## Quick verdict

[1–2 sentences naming the leader and why, referencing the spec's priorities]
```

## Guidelines

- Tables first — this is a scan tool
- Highlight meaningful differences, not just lists
- Apply spec priorities + standing prefs when calling a winner
- Be opinionated

## After comparison

Ask: "Want the full flow instead (`/shortlist` → `/source` → `/recommend` for a PDF), or dig deeper on any of these?"
