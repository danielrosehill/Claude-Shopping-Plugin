---
description: Re-run shortlist + sourcing against the current spec and write a new dated recommendations file
---

# Recompare

Re-run the candidate shortlist AND the sourcing waterfall against the current `spec.md`. Useful when the spec has changed, pricing has moved, stock has shifted, or new candidates have appeared since the last recommendation.

## Steps

1. **Read the current state:**
   - `spec.md` — including per-purchase overrides and the Standing preferences snapshot
   - `from-ai/shortlist.md` and `from-ai/sourcing.md` if present
   - The most recent file in `from-ai/recommendations/` (if any)

   Note any changes since the most recent recommendations file.

2. **Refresh the candidate set (shortlist phase):**
   - Re-read everything in `for-ai/` (especially `for-ai/catalogs/`)
   - Do NOT assume the previous shortlist is still valid
   - Apply must-haves, hard nos, standing avoided brands, foundational rules
   - Produce an updated CONSIDER list

3. **Refresh sourcing (source phase):**
   - Any price in `data/products/[brand]-[model].json` older than 7 days must be re-fetched
   - Re-run the region's sourcing waterfall for each CONSIDER candidate
   - Pick the best channel per candidate

4. **Score each candidate** against the current spec. Explicitly call out:
   - Previously-recommended models that no longer fit, and why
   - Pricing or availability changes that swap the best channel

5. **Write a NEW dated file** at `from-ai/recommendations/recommendations-YYYY-MM-DD.md`. **Never overwrite** an existing dated file.

   Structure: top pick + channel, runner-up + channel, budget alternative + channel, also-considered, one-line final recommendation, changes vs previous file.

6. **Offer to regenerate the PDF**: "Recommendations updated. Regenerate the PDF report? (/recommend)"
