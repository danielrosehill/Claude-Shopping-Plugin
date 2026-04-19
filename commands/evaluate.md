---
description: Quick one-off evaluation of a single product against the current Israeli purchase spec
argument-hint: <product URL, model name, or pasted listing>
---

# Evaluate a Single Product (Israel)

Evaluate the product in `$ARGUMENTS` against the current `spec.md`, the latest shortlist/sourcing, and the Israeli sourcing waterfall.

This is a **quick inline check**, not a full research cycle. Print the verdict in the response; do NOT write a file unless the user asks.

## Steps

1. **Read context first, every time:**
   - `spec.md` — must-haves, hard nos, nice-to-haves, per-purchase overrides, Standing preferences snapshot
   - The most recent file in `from-ai/recommendations/` (current top pick) if any
   - `from-ai/shortlist.md` and `from-ai/sourcing.md` if present — for context on what's already been evaluated

   Never skip this. A spec change since the last recommendation can invalidate otherwise-fine products.

2. **Resolve the product.** `$ARGUMENTS` may be:
   - A URL → fetch with WebFetch and extract model, price, key specs, seller, ships-to-IL flag (if applicable), seller rating (if AliExpress)
   - A model name only → look up specs from the manufacturer site first
   - A pasted listing → parse directly

   If a critical spec is missing, fetch it. Do not guess.

3. **Product-merit score:** mark each as **PASS / FAIL / PARTIAL** with a one-line reason:
   - Each must-have from `spec.md`
   - Each hard no (per-purchase AND standing avoided brands)
   - Manufacturer Israeli support presence

4. **Sourcing sanity check (Israeli waterfall, abbreviated):**
   - If the source is an Israeli retailer: VAT-included? KSP: use the higher price? Warranty?
   - If Amazon: ships to IL? Landed cost with shipping + 18% VAT + possible customs?
   - If AliExpress: respects spec's AliExpress tolerance? Seller rating meets threshold for branded?
   - Markup vs effective threshold (per-purchase override > standing default, typically 40%)

5. **Compare against the current top pick** from the latest recommendations file. Is this better, worse, or sideways? Include landed cost + sourcing channel in the comparison.

6. **Verdict** — end with one of:
   - ✅ **Buy** — clearly better than current top pick (better product OR better landed cost + lead time)
   - 🟡 **Consider** — viable alternative, trade-offs worth thinking about
   - ⚪ **Sideways** — equivalent to current pick
   - ❌ **Skip** — fails the spec, triggers a hard no, or is strictly worse

7. **Honesty rules:**
   - If the spec rules it out, say so plainly.
   - If ships-to-IL is unverified, say "unverified" rather than assume.
   - If AliExpress seller rating is missing for a branded item, that's an automatic 🟡 or ❌ depending on spec.

## Output

Print inline. Only write a file if the user explicitly asks.
