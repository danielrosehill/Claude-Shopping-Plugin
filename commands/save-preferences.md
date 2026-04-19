---
description: Persist standing shopping preferences to Mem0 (or a local fallback file) so every future shopping workspace can load them.
---

# Save Preferences — Persist Standing Shopping Preferences to Memory

Run a one-time setup (or update) for the user's standing shopping preferences and save them to memory (Mem0 MCP) so every future purchase workspace can `/load-preferences` them.

## When to run

- First time the user uses any shopping workspace — before `/intake`
- When standing preferences change (new trusted store, changed AliExpress stance, adjusted markup threshold)

## What to capture

Walk through these conversationally. Batch related questions. Offer region-appropriate defaults pulled from the workspace's `CLAUDE.md`.

### Geographic context

Confirm rather than re-ask unless different:

- Country
- Currency
- VAT rate
- VAT status: standard consumer / business-reclaimable / exempt

### Brand attitudes

- Manufacturers you trust across the board
- Manufacturers you avoid across the board (and briefly, why)

### Channel attitudes

- **AliExpress** — OK in general? Any limits (generic items only vs branded)? What seller rating floor (default >=95% for branded electronics)?
- **Amazon (shipping to your region)** — OK? Max lead-time tolerance?
- **Parallel imports / grey market** — OK, no, or depends on category?

### Vendor attitudes (local retailers)

- Preferred local stores (optionally with category mappings)
- Avoided local stores (and briefly, why)

### Thresholds

- Default markup ceiling over international RRP (typical: 30-50%; default: 40%)
- Review score floor (default 3.5/5)
- Risk tolerance with lesser-known brands (low / medium / high)

### Delivery

- Email for PDF report delivery

## Preferred storage: Mem0 MCP

If Mem0 MCP is configured:

1. Structure the answers as a small set of related memories, each tagged `shopping_preferences_<region>`
2. Use the installed Mem0 server's write tool — commonly `mcp__mem0__add_memory` — to persist
3. Report: "Saved N memories to Mem0 under tag `shopping_preferences_<region>`."

## Fallback: local file

If Mem0 MCP isn't available:

1. Write to `~/.claude/shopping-preferences-<region>.md` as structured markdown
2. Tell the user: "Saved locally. Install Mem0 MCP for cross-device persistence."

## Confirmation

After saving:

1. Echo back what was saved in a compact list
2. Tell the user: "Standing preferences saved. Run `/load-preferences` in any new purchase workspace to pull these in automatically."
3. If called as part of `/intake` bootstrap, return control and continue with `/intake`.
