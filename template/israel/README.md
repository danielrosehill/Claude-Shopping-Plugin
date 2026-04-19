# {WORKSPACE_NAME}

Shopping workspace for an Israeli consumer purchase, provisioned from the [shopping plugin](https://github.com/danielrosehill/shopping-plugin).

One repo = one purchase. The spec, the catalog screenshots, and the agent's recommendations all live here. The plugin's commands (`/shopping:*`) are installed globally and operate on the files in this directory.

## Quick start

```
/shopping:load-preferences      # pull standing prefs from Mem0
/shopping:intake                # populate spec.md
# drop catalog screenshots into catalogs/{vendor}/
/shopping:israel-search-zap     # live IL price discovery
/shopping:shortlist             # narrow to CONSIDER candidates
/shopping:israel-source         # apply the IL sourcing waterfall
/shopping:recommend             # produce the ranked PDF
```

See `CLAUDE.md` for the full workflow and data conventions.

## Layout

| Path | What lives here |
|------|-----------------|
| `spec.md` | Per-purchase brief — must-haves, hard nos, budget, timeline |
| `data/` | IL pricing rules, retailer playbooks, Hebrew term map, store DB |
| `catalogs/` | Per-vendor catalog screenshots / listings (`catalogs/ksp/`, `catalogs/ali/`, etc.) |
| `for-ai/` | Working notes for the agent |
| `from-ai/` | Agent outputs — shortlist, sourcing, dated recommendations |
| `outputs/` | Final PDFs and exportable artefacts |
