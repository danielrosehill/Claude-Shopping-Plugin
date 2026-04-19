---
description: Pull standing shopping preferences (region, channel tolerances, trusted brands, markup threshold) from Mem0 into the current workspace's spec.md.
---

# Load Preferences — Pull Standing Shopping Preferences from Memory

Load the user's standing shopping preferences into the current session so they can be applied during `/intake`, `/shortlist`, `/source`, and `/recommend`.

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

These are stored in **memory** (via Mem0 MCP, recommended), not in any per-purchase repo.

## Preferred storage: Mem0 MCP

The recommended persistence layer is [Mem0](https://mem0.ai), exposed as an MCP server.

1. Query memories tagged `shopping_preferences_<region>` (e.g. `shopping_preferences_israel`)
2. Parse into the fields listed above
3. Write the snapshot into the **Standing preferences snapshot** section of `spec.md`
4. Report to the user: "Loaded preferences from Mem0 — AliExpress: Yes (seller >=95%), markup threshold: 40%, etc."

### Typical Mem0 MCP tool calls

Tool names vary by Mem0 implementation. Commonly:

- `mcp__mem0__search_memory` — query with `"shopping preferences <region>"`
- `mcp__mem0__list_memories` — filter by tag
- `mcp__mem0__get_memory` — by ID

Use whichever read tool the installed Mem0 server exposes.

## Fallback 1: local file

If Mem0 MCP isn't available, check `~/.claude/shopping-preferences-<region>.md`. If present, populate the snapshot section of `spec.md` from it.

## Fallback 2: region defaults only

If no memory and no local file, populate the snapshot with the region's defaults (country, currency, VAT, markup threshold 40%, review floor 3.5/5) and flag brand/channel/vendor attitudes as "Not set". Tell the user to run `/save-preferences` for future purchases.

## After loading

1. Write the loaded values into `spec.md` → **Standing preferences snapshot** section
2. Report a 5-8 line summary: key channel tolerances, markup threshold, any standing avoided brands.
3. Prompt next step: `/intake` if spec isn't configured yet, or `/shortlist` if it already is.

## What NOT to do

- **Do not write per-purchase information here.** Must-haves, hard nos for this purchase, budget, timeline — those belong in `spec.md` via `/intake`.
- **Do not update Mem0 from this command.** `/load-preferences` is read-only. Writes go through `/save-preferences`.
