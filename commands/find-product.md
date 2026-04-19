---
description: Find a product across the vendors configured for this workspace's region and return a ranked price table.
---

# Find Product

Region-agnostic product search. Reads the workspace's region configuration (from `CLAUDE.md` or `data/region.md`) and dispatches to the correct region-specific search commands.

## Arguments

`$ARGUMENTS`: the product name or free-text description. Brand + model gives the tightest results.

## Behaviour

1. Read the workspace's region setting.
2. If region = `israel`: run `/shopping:israel-search-zap` as first-pass, then fall through to `/shopping:israel-search-main-tech-stores`, `/shopping:israel-search-by-category`, `/shopping:israel-search-google-il` per the IL discovery order.
3. If region is generic / unset: use the vendor list declared in `data/vendors.md` (workspace-local) and run per-vendor search via Playwright MCP or Tavily MCP.
4. Return a ranked price table (cheapest first, VAT-inclusive where applicable), plus any noteworthy specialist hits.

## Output

```
## {product} — price comparison
Region: {region}
Vendors queried: {count}

| # | Price (inc. tax) | Vendor | Delivery | Link |
|---|------------------|--------|----------|------|
| 1 | ...              | ...    | ...      | ...  |
```

Call out anomalies (ex-VAT listings, ex-duty listings, unusually low prices worth double-checking).
