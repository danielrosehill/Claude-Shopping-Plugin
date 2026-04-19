---
description: Update spec.md with an addition, edit, or removal — diff-only response
argument-hint: <what to add / change / remove>
---

# Update the Spec

Update `spec.md` at the repo root based on `$ARGUMENTS`.

## Steps

1. **Read `spec.md`** to understand the current state.
2. **Classify** the change as an **addition**, **edit**, or **removal**, and decide which section it belongs in:
   - What I'm buying
   - Must-haves (hard positive requirements)
   - Hard nos / Dealbreakers (disqualifying characteristics)
   - Nice-to-haves
   - Budget / Timeline / Priorities
   - Per-purchase overrides to standing preferences
   - Context / history

   If the change is about a **standing preference** (e.g. "I want to lower my markup threshold generally"), redirect: "That's a standing preference — use `/save-preferences` to update memory. Want me to just override for this purchase instead?"

3. **Check for contradictions.** If the change conflicts with an existing line, edit that line rather than leaving both. The spec must stay internally consistent.

4. **Phrasing:**
   - Must-haves → firm language ("must", "required")
   - Hard nos → firm negative ("must not", "excluded")
   - Nice-to-haves → soft language ("ideally", "preferred")
   - Per-purchase overrides → explicit about what's being overridden and for how long ("for this purchase only")

5. **Apply the edit** with the Edit tool. Do not rewrite the whole file if a targeted edit will do.

6. **Show the user the diff only** — the lines that changed. No summary, no restatement of the full spec.

7. **Flag downstream impact.** If the change likely invalidates the current top pick in the latest `from-ai/recommendations/recommendations-*.md`, say so explicitly. Do NOT update the recommendations file — that's `/recompare`'s job.

8. **Update `Last updated`** in `spec.md` to today's date.
