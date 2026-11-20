# {WORKSPACE_NAME} — Shopping Workspace (Israel)

This workspace was provisioned by `/shopping:new-workspace` with `--variant=israel`. One repo = one purchase.

## Region

- **Country**: Israel
- **Currency**: ILS (₪)
- **VAT**: 18% (standard consumer)
- **Searching**: use the `israel-shopping` plugin's `find-product` skill (Zap, trusted vendors per category, .co.il search, imports, branch stock). Its `data/trusted-vendors.json`, `data/hebrew-category-map.json` and `docs/search-strategies.md` hold the vendor list, Hebrew terms, VAT/Eilat rules and discovery playbook (see `data/README.md`).

## How to use this workspace

1. **Set up standing preferences** (one-time, persists across all your shopping workspaces):
   - `/shopping:save-preferences` if you haven't already
   - `/shopping:load-preferences` at the start of each new workspace

2. **Define this purchase**:
   - `/shopping:intake` — populate `spec.md` with what you're buying, must-haves, hard nos, budget, timeline.

3. **Discover candidates**:
   - Drop catalog screenshots into `catalogs/{vendor}/` (e.g. `catalogs/ksp/`, `catalogs/ivory/`, `catalogs/bug/`, `catalogs/ali/`).
   - Or run the `israel-shopping` plugin's `find-product` skill for live search (Zap, trusted vendors, .co.il, imports, branch stock).

4. **Shortlist + source + recommend**:
   - `/shopping:shortlist` — produce a CONSIDER list against the spec.
   - `/shopping:israel-source` — apply the IL waterfall (tier-1 tech -> tier-2 broad -> niche specialists -> Amazon-IL -> AliExpress).
   - `/shopping:recommend` — generate the ranked Typst PDF report into `outputs/`.

5. **Update the spec / re-run**:
   - `/shopping:update-spec` — change requirements
   - `/shopping:recompare` — re-run shortlist + sourcing and write a new dated recommendations file.

## Layout

```
spec.md                         # the per-purchase brief
data/                           # IL pricing rules, retailers, Hebrew map, store DB
catalogs/{vendor}/              # screenshots / listings per vendor
for-ai/                         # working notes for the agent
from-ai/                        # agent outputs (shortlist, sourcing, dated recommendations)
outputs/                        # final PDFs and exportable artefacts
```

## Notes

- This workspace is **data**. Plugin updates never touch it.
- The plugin's commands are globally available — you can invoke them from this workspace OR from anywhere else.
- Personal standing preferences (AliExpress tolerance, trusted brands, etc.) live in the gateway `memory` server or under the plugin's `$CLAUDE_USER_DATA/shopping/` data directory, not in this repo.
