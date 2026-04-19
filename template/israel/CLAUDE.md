# {WORKSPACE_NAME} — Shopping Workspace (Israel)

This workspace was provisioned by `/shopping:new-workspace` with `--variant=israel`. One repo = one purchase.

## Region

- **Country**: Israel
- **Currency**: ILS (₪)
- **VAT**: 18% (standard consumer)
- **Sourcing waterfall**: see `data/retailers.md`
- **Discovery playbook**: see `data/discovery-techniques.md`
- **Pricing rules** (VAT detection, Eilat pricing, markup tiers): see `data/shopping-rules.md`
- **Hebrew term map**: `data/hebrew-category-map.json`
- **Curated store DB** (~830 vendors): `data/israeli-stores.json`

## How to use this workspace

1. **Set up standing preferences** (one-time, persists across all your shopping workspaces):
   - `/shopping:save-preferences` if you haven't already
   - `/shopping:load-preferences` at the start of each new workspace

2. **Define this purchase**:
   - `/shopping:intake` — populate `spec.md` with what you're buying, must-haves, hard nos, budget, timeline.

3. **Discover candidates**:
   - Drop catalog screenshots into `catalogs/{vendor}/` (e.g. `catalogs/ksp/`, `catalogs/ivory/`, `catalogs/bug/`, `catalogs/ali/`).
   - Or use the live search commands: `/shopping:israel-search-zap` (default first-pass), `/shopping:israel-search-main-tech-stores`, `/shopping:israel-search-by-category`, `/shopping:israel-search-google-il`.

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
- Personal standing preferences (AliExpress tolerance, trusted brands, etc.) live in Mem0 / `~/.claude/`, not in this repo.
