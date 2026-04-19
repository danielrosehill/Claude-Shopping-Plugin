---
description: Build or refresh the per-purchase spec.md through a guided intake conversation.
---

# Intake — Build the Spec for This Purchase (Israel)

You are running the intake for this Israeli purchase. The goal is to populate `spec.md` at the repo root with the per-purchase brief.

**Two-tier model:**

- **Standing preferences** (country, currency, AliExpress tolerance, trusted/avoided brands, markup threshold, preferred Israeli stores, etc.) live in **memory** via Mem0 MCP. Load them first with `/load-preferences` — don't re-ask.
- **Per-purchase spec** (`spec.md`) holds only what's specific to *this* decision: what you're buying, must-haves, hard nos, budget, timeline, priorities, context.

**One repo = one purchase.** **Israel defaults are applied** — country, ILS, 18% VAT — unless memory or the user overrides.

## Step 0: Load standing preferences

Before asking anything, run `/load-preferences`:

- If preferences load from Mem0 or the local fallback — the snapshot is now in `spec.md`. Don't re-ask those fields.
- If nothing's stored — say: "No standing preferences yet. I recommend `/save-preferences` first (one-time setup — applies to all future Israeli purchases). Alternatively I can ask the standing questions inline now, but you'll be asked again next time in a different workspace. Which do you prefer?"

## Step 1: Read existing state

1. Read `spec.md` — is Status "Configured" or still template?
2. Check `for-ai/` — any files already dropped in?

If `spec.md` is configured, ask whether to update or go to `/shortlist`.

## Step 2: Capture per-purchase fields

Conversational. Batch related questions. **Do not re-ask anything already loaded from standing prefs.**

### 1. What they're buying

- Short description (one sentence)
- Category label
- Primary use case

### 2. Must-haves

*Positive hard requirements. Disqualifies if missed.*

Ask: "What are your non-negotiable requirements for this purchase?"

### 3. Hard nos / Dealbreakers — **distinct from must-haves**

*Disqualifying characteristics. Disqualifies if present.*

Ask explicitly and separately: "Any dealbreakers specific to this purchase — things that rule a product out even if everything else matches?"

Standing avoided brands and standing avoided Israeli stores already apply. Ask here only for *new, this-purchase-specific* dealbreakers. Prompt for:

- Form factors or designs that are a non-starter for this use case
- Anti-features (no soldered RAM, no glossy screen, no cloud dependency)
- Channel exclusions specific to this category (e.g. "no AliExpress for this — counterfeit-prone category")
- New brand/store exclusions not in standing prefs

**Do not let the user skip this section.** Probe gently if they say nothing comes to mind.

### 4. Nice-to-haves

Soft preferences for tie-breaking.

### 5. Budget (this purchase)

- Target (₪)
- Ceiling (₪ — hard max)
- Flexibility

### 6. Timeline

- Urgency
- Deal-hunting

### 7. Priorities (this purchase)

Ranked factors for tie-breaking — this decision specifically. Common: landed cost, local warranty, manufacturer reputation, feature match, lead time.

### 8. Per-purchase overrides to standing preferences

Ask: "Anything about your usual prefs that should be overridden for *this* purchase?"

Common overrides for Israeli buyers:
- "Higher markup OK this time — niche item"
- "AliExpress off — need warranty on this one"
- "Domestic only — too urgent for international"
- "Lower lead-time tolerance — need it in 3 days"

Capture into the **Per-purchase overrides** table in `spec.md`.

### 9. Context / history

- Replacing something? What and why?
- Past products in this category — loved / hated?
- Compatibility constraints

## Step 3: Save

1. Write answers into `spec.md`, replacing placeholder text
2. Set `**Status**: Configured`
3. Set `**Last updated**: [today YYYY-MM-DD]`
4. Leave the **Standing preferences snapshot** section alone — `/load-preferences` owns it
5. Summarise in 4–6 lines
6. Prompt: "Spec saved. Drop sources into `for-ai/` if you have candidates in mind, or run `/shortlist` to evaluate candidates (product merit first, Israeli sourcing applied separately via `/source`)."

## Interview style

- Conversational, not interrogative
- Batch related questions
- Use `AskUserQuestion` for multiple-choice where it helps
- Accept partial answers
- **Never re-ask standing preferences** already loaded from memory
- **Explicitly probe for hard nos** — push a little if the user tries to skip

## Plugin recommendation (mention once)

If the user seems to be doing serious shopping work, mention once:

> For richer Israeli retailer browsing, the `/shopping:israel-search-*` commands work best when Playwright MCP and (optionally) Tavily MCP are configured — Playwright for bot-protected sites like Zap, Tavily as a faster fallback.

## Example opening

```
Welcome. Let me set up the spec for this purchase.

[Runs /load-preferences — reports: "Loaded standing prefs from Mem0 — Israel, ILS, 18% VAT, AliExpress tolerance: [value], Amazon-IL tolerance: [value], markup threshold [N]%, preferred stores: [list]."]

Good — I'll apply those. A few questions specific to this purchase:

What are you looking to buy? Short description is fine — e.g. "ergonomic office chair" or "14-inch laptop for dev".
```
