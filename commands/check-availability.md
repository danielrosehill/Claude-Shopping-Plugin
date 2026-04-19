---
description: Check per-vendor stock and delivery-time for a given SKU.
---

# Check Availability

Quick availability check across the workspace's in-scope vendors. Unlike `/shopping:compare-vendors`, this one skips the full price-comparison pass — it just reports which vendors have stock and the earliest delivery.

## Arguments

`$ARGUMENTS`: `<brand> <model>` or a vendor-specific SKU.

## Behaviour

1. Identify the vendor set from the workspace region config.
2. For each vendor, fetch the product page and extract: stock status, warehouse location (if shown), soonest delivery date, whether reservations / back-orders are offered.
3. Emit a compact table sorted by earliest delivery.

## Output

```
## {product} — availability
Region: {region}

| Vendor | Stock | Soonest delivery | Notes |
|--------|-------|------------------|-------|
| ...    | In    | Tomorrow         | ...   |
```

Flag out-of-stock vendors separately so the user can decide whether to wait or switch vendors.
