---
description: Evaluate candidate products against the spec on product merit only. Outputs a CONSIDER list before sourcing.
---

# Shortlist — Product Merit Evaluation (Brand-Agnostic)

Evaluate candidate products on **product merit only** — whether they meet the spec. Do **not** search Israeli retailers yet. Sourcing is a separate step (`/source`).

This is the third phase of the workflow: `/intake` → `/shortlist` → `/source` → `/recommend`.

## Pre-shortlist checklist

1. **Read `spec.md`** — verify Status is "Configured"
2. **Verify Standing preferences snapshot** in `spec.md` is populated; if empty, run `/load-preferences` first
3. If `spec.md` is missing must-haves / hard nos, stop and suggest `/intake`
4. **Check `for-ai/`** — user-provided sources (screenshots, PDFs, links, notes). If images/PDFs present, consider `/extract` first.

## Inputs

Two sources of constraint:

- **Per-purchase** (from `spec.md`): what's being bought, must-haves, hard nos, nice-to-haves, this-purchase priorities, per-purchase overrides
- **Standing** (from snapshot in `spec.md`, originally from Mem0 or fallback): trusted brands, avoided brands, review score floor, risk tolerance

Per-purchase overrides beat standing defaults.

## Candidate selection

Sources of candidates, in priority order:

1. **User-provided** — anything in `for-ai/` is a candidate (extract names, models)
2. **Spec-directed** — specific brands/models the spec mentions
3. **Discovery** — if the spec allows expanded search, research the market to find candidates matching the must-haves

If the user explicitly said "only evaluate my sources", skip step 3.

## For each candidate — product merit only

### 1. Identify

- Exact model name/number + manufacturer
- Cache lookup: `data/products/[brand]-[model-slug].json` — reuse if <7 days old

### 2. Must-haves check

For each must-have from `spec.md`: satisfied / partial / not satisfied.

Any must-have NOT satisfied → **DISQUALIFIED (missing must-have: X)**.

### 3. Hard nos check

Two sources:

- **Per-purchase hard nos** (from `spec.md`)
- **Standing avoided brands** (from the snapshot)

Any TRIGGERED → **DISQUALIFIED (hard no: X, or avoided brand: Y)**.

### 4. Foundational disqualification

Apply these regardless (unless spec explicitly overrides):

- Manufacturer red flags — fraud, safety recalls, defunct / unreachable, regulatory bans, import restrictions to Israel
- Product red flags — safety hazards, aggregate rating <2.5/5, widespread defect patterns, counterfeit-prone items where authenticity can't be verified
- Rating below the standing review score floor (default 3.5/5) → flag for acknowledgment, don't auto-disqualify unless it drops under 2.5

### 5. Manufacturer assessment

- Reputation + history
- Support / warranty track record
- **Presence in Israel** — do they have Israeli distributors, Israeli warranty support? This matters for the `/source` step later.

Spawn `manufacturer-research` sub-agent for unfamiliar brands.

### 6. Product assessment

- Build quality + materials
- Review aggregate across multiple sources (never rely on one platform)
- Known issues / failure modes
- Longevity / durability reports

### 7. Verdict

- **CONSIDER** — passes all gates; proceeds to `/source`
- **DISQUALIFIED** — record the specific reason

**Do NOT search Israeli retailers.** Shortlist is product-merit only. Where to buy comes next.

## Output: `from-ai/shortlist.md`

```markdown
# [Category] Shortlist (Product Merit)

**Date**: [YYYY-MM-DD]
**Spec summary**: [1-line from spec.md]
**Standing prefs applied**: [1-line from snapshot]

## Candidates Evaluated

### [Product name]

**Verdict**: CONSIDER / DISQUALIFIED ([specific reason])

- **Manufacturer**: [Name] — [reputation] — [Israel presence: Yes/No/Limited]
- **Reviews**: X.X/5 from [sources]

**Must-haves satisfied**:
- [Must-have 1]: ✓ / partial / ✗
- [Must-have 2]: ✓ / partial / ✗

**Hard nos**:
- [Hard no 1]: clear / TRIGGERED
- Standing avoided brands: clear / TRIGGERED ([brand])

**Strengths**: [bullets]
**Concerns**: [bullets]
**Notes for sourcing**: [any channel-specific considerations, e.g. "counterfeit-prone — be careful with AliExpress"]

[Repeat for each]

## Summary

- Evaluated: X
- Shortlisted (CONSIDER): X
- Disqualified: X (breakdown: must-haves, hard nos, foundational)
- Ready for `/source`: yes/no
```

## After shortlist

Tell the user:

- How many candidates evaluated
- How many shortlisted
- Top 2–3 by quick assessment (without pricing — sourcing is next)
- Any foundational flags worth surfacing
- Prompt: "Shortlist saved to `from-ai/shortlist.md`. Run `/source` to apply the Israeli sourcing waterfall — find where each shortlisted candidate is available and at what landed cost."
