---
description: Side-by-side comparison of a known SKU across the workspace's configured vendors.
---

# Compare Vendors

Given a known SKU or brand+model, build a side-by-side comparison across vendors in scope for the workspace's region.

## Arguments

`$ARGUMENTS`: `<brand> <model>` — e.g. `JBL Flip 7` or `Anker Prime A2343`.

## Behaviour

1. Resolve the region from the workspace's `CLAUDE.md`.
2. Fan out to the relevant region-specific search commands (or the generic vendor list) to pull the SKU's listing page from each vendor that carries it.
3. Capture per vendor: price, tax treatment, stock, delivery lead time, warranty, notable promos.
4. Produce a comparison table ranked by landed price.

## Output

Table columns: Vendor, Price (inc. tax), Stock, Lead time, Warranty, Notes, Link.

Flag:
- Vendors where the SKU is listed but out of stock
- Vendors where the shown price is ex-tax / ex-shipping
- Any vendor that is unusually cheap (likely grey import, worth verifying warranty coverage)
