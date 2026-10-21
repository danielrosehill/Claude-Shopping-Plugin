---
description: Pull standing shopping preferences (region, channel tolerances, trusted brands, markup threshold) from the gateway memory server into the current workspace's spec.md.
---

# Load Preferences — Pull Standing Shopping Preferences from Memory

Load the user's standing shopping preferences into the current session so they can be applied during `/intake`, `/shortlist`, `/source`, and `/recommend`.

## Migration from legacy path (one-time)

Before resolving the data directory, check for legacy data at these paths:
- `~/.claude/shopping-preferences-<region>.md` (for every region the user has set up)

If a legacy file exists AND the corresponding file at the new path does NOT exist, move it to the new location and delete the legacy file. Tell the user: "Migrated shopping-preferences-<region>.md from ~/.claude/ to <new>."

## Path resolution

Resolve the plugin's data directory as `$CLAUDE_USER_DATA/shopping/` if `CLAUDE_USER_DATA` is set; otherwise `$XDG_DATA_HOME/claude-plugins/shopping/` if `XDG_DATA_HOME` is set; otherwise `~/.local/share/claude-plugins/shopping/`. Create the directory if it doesn't exist. See the canonical convention in the `meta-tools:plugin-data-storage` skill. The local preferences file lives at `<plugin-data-dir>/shopping-preferences-<region>.md`.

## What "standing preferences" means

Things that apply to **every** purchase — not to this specific one:

- **Geographic context**: country, currency, VAT rate, VAT status (consumer / business-reclaimable / exempt)
- **Brand attitudes**: permanently trusted manufacturers, permanently avoided manufacturers (with reasons)
- **Channel attitudes** (region-specific):
  - AliExpress tolerance (Yes / No / Generic only / Branded OK if seller >=95%)
  - Amazon tolerance (Yes / No / Max lead time)
  - Parallel imports / grey market (OK / No / Depends)
- **Vendor attitudes** (region-specific): preferred local stores, avoided local stores
- **Thresholds**: default markup ceiling (% over international RRP — default 40%), review score floor (default 3.5/5), risk tolerance
- **Delivery**: email for PDF report

These are stored in **memory** (the `memory` server on the MCP gateway, recommended), not in any per-purchase repo.

## Preferred storage: gateway `memory` server

The recommended persistence layer is the `memory` upstream on the MCP gateway (tools `mcp__gateway__memory__*`). The tools are deferred — load them with `ToolSearch` (`select:mcp__gateway__memory__recall,mcp__gateway__memory__list_memories`) before the first call.

1. Query memories tagged `shopping_preferences_<region>` (e.g. `shopping_preferences_israel`)
2. Parse into the fields listed above
3. Write the snapshot into the **Standing preferences snapshot** section of `spec.md`
4. Report to the user: "Loaded preferences from memory — AliExpress: Yes (seller >=95%), markup threshold: 40%, etc."

### Tool calls

- `mcp__gateway__memory__recall` — `query: "shopping preferences <region>"`, `namespaces: ["personal"]` (or whichever namespace `/save-preferences` used)
- `mcp__gateway__memory__list_memories` — `namespace: "personal"`, then keep the records tagged `shopping_preferences_<region>`

## Fallback 1: local file

If the gateway `memory` tools don't resolve, check `<plugin-data-dir>/shopping-preferences-<region>.md`. If present, populate the snapshot section of `spec.md` from it.

## Fallback 2: region defaults only

If no memory and no local file, populate the snapshot with the region's defaults (country, currency, VAT, markup threshold 40%, review floor 3.5/5) and flag brand/channel/vendor attitudes as "Not set". Tell the user to run `/save-preferences` for future purchases.

## After loading

1. Write the loaded values into `spec.md` → **Standing preferences snapshot** section
2. Report a 5-8 line summary: key channel tolerances, markup threshold, any standing avoided brands.
3. Prompt next step: `/intake` if spec isn't configured yet, or `/shortlist` if it already is.

## What NOT to do

- **Do not write per-purchase information here.** Must-haves, hard nos for this purchase, budget, timeline — those belong in `spec.md` via `/intake`.
- **Do not write to memory from this command.** `/load-preferences` is read-only. Writes go through `/save-preferences`.
