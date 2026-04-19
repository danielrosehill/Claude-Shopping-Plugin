# {WORKSPACE_NAME} — Shopping Workspace (Generic)

This workspace was provisioned by `/shopping:new-workspace` with `--variant=generic`. Use this when your target market doesn't yet have a dedicated regional variant in the shopping plugin. One repo = one purchase.

## Region

- **Country**: {SHOPPING_REGION}
- **Currency**: {SHOPPING_CURRENCY}
- **Tax / VAT**: see `data/tax-rules.md` (fill in)
- **Vendors in scope**: see `data/vendors.md` (fill in)

Replace the placeholders during your first run, or via `/shopping:intake`.

## How to use this workspace

1. **Fill in `data/vendors.md`** with the local retailers you'll search across, plus their canonical search-URL patterns.
2. **Fill in `data/tax-rules.md`** with the local VAT/sales-tax rate and any "ex-tax pricing" gotchas.
3. **Set up standing preferences** (one-time, persists across all your shopping workspaces): `/shopping:save-preferences`, then `/shopping:load-preferences` at the start of each new workspace.
4. **Define this purchase**: `/shopping:intake` — populate `spec.md`.
5. **Discover candidates**: drop catalog screenshots into `catalogs/{vendor}/`, or use `/shopping:find-product`.
6. **Shortlist + recommend**: `/shopping:shortlist` -> `/shopping:recommend` (writes a Typst PDF to `outputs/`).

## Layout

```
spec.md                         # the per-purchase brief
data/                           # vendors.md, tax-rules.md (fill in)
catalogs/{vendor}/              # screenshots / listings per vendor
for-ai/                         # working notes for the agent
from-ai/                        # agent outputs
outputs/                        # final PDFs
```

## When to graduate to a region variant

If the same region keeps coming up, contribute a dedicated variant under `template/<region>/` in the shopping plugin — bake in the local vendors, tax rules, and search aggregators so future workspaces don't need to be configured from scratch.
