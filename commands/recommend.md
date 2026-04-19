---
description: Generate the ranked recommendation report (Typst PDF) from spec + catalogs.
---

Read `spec.md` for the user's requirements and all catalog images in `catalogs/`.

Analyze every product against the spec. Consider: wattage vs current speaker, speech/podcast clarity, portability, durability, price, and RRP deviation (local price vs international RRP).

Generate a ranked recommendation as a Typst file (`recommendations.typ`) and compile it to `recommendations.pdf`. Highlight the top 5 picks with reasoning for each. Include a price comparison table and a bottom-line verdict.

If $ARGUMENTS is provided, use it as additional constraints (e.g. "budget under 300 NIS", "only local stores").