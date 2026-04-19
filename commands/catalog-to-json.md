Read the catalog screenshot(s) at the path provided in $ARGUMENTS.

Extract every visible product into a structured JSON array and write it to `products.json` (create if missing, merge if existing — deduplicate on brand + model + store).

Each product object should have:
```json
{
  "brand": "JBL",
  "model": "Flip 7",
  "store": "ksp",
  "currency": "NIS",
  "price": 399,
  "sale_price": 349,
  "eilat_price": 296,
  "category": "portable-speaker",
  "notes": "any extra info visible in the screenshot"
}
```

Omit fields that aren't visible in the screenshot (e.g. `eilat_price` if not shown). Use "USD" for AliExpress, "NIS" for Israeli stores.

After writing, print a summary table of what was added/updated and the total product count in `products.json`.

If no $ARGUMENTS path is given, scan all images in `catalogs/` and rebuild `products.json` from scratch.