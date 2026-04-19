# Purchase Spec — Israel

*This single file is the source of truth for **this purchase**. One repo = one purchase. Populate via `/intake`, or edit directly.*

**Standing preferences** (country, AliExpress tolerance, trusted/avoided brands, markup threshold, preferred Israeli stores, etc.) live in **memory** — not here. Load them with `/load-preferences` before running `/intake`. This file captures only what's specific to this decision.

**Status**: Not yet configured
**Created**: [Date]
**Last updated**: [Date]

---

## What I'm buying

[Short description — e.g. "14-inch laptop for technical writing and light dev work"]

**Category**: [Free-form label — e.g. "laptop", "drill", "office chair"]

**Primary use case**: [The one thing this product needs to do well]

---

## Must-haves

*Hard positive requirements. A product that fails any of these is disqualified.*

- [ ] [Requirement 1]
- [ ] [Requirement 2]
- [ ] [Requirement 3]

---

## Hard nos / Dealbreakers

*Disqualifying characteristics — distinct from must-haves. Standing "avoided brands" and "standing avoided Israeli stores" from memory also apply; anything here extends them for this purchase.*

- [ ] [Dealbreaker 1 — e.g. "No soldered RAM"]
- [ ] [Dealbreaker 2 — e.g. "No glossy screens"]
- [ ] [Dealbreaker 3 — e.g. "No AliExpress for this category (counterfeit-prone)"]

---

## Nice-to-haves

- [Feature 1]
- [Feature 2]

---

## Budget (this purchase)

**Target**: ₪[Amount]
**Ceiling**: ₪[Hard upper limit]
**Flexibility**: [Hard limit / Flexible for the right product / Very flexible]

---

## Timeline

**Urgency**: [ASAP / Within a week / Within a month / No rush]
**Deal-hunting**: [Yes / No]

---

## Priorities (this purchase)

*Ranked factors for tie-breaking.*

1. [e.g. "Landed cost"]
2. [e.g. "Local warranty"]
3. [e.g. "Lead time"]

---

## Per-purchase overrides to standing preferences

*Leave blank to inherit from memory. Populate only what differs for this purchase.*

| Standing preference | Override for this purchase |
|---------------------|----------------------------|
| Markup threshold | [e.g. "Willing to go to 60% — niche item"] |
| AliExpress tolerance | [e.g. "Not for this — need warranty"] |
| Amazon-IL lead time tolerance | [e.g. "Max 1 week this time"] |
| International shipping | [e.g. "Domestic only — urgent"] |

---

## Context / history

**Replacing**: [If applicable — what and why]
**Past experience**: [Relevant products owned]
**Compatibility constraints**: [Ecosystem, accessories, space]
**Other notes**: [Anything else]

---

## Standing preferences snapshot

*Populated automatically by `/load-preferences`. Read-only audit of the memory-backed preferences applied at intake time. Edit via `/save-preferences`, not here.*

- **Country**: [from memory — default: Israel]
- **Currency**: [from memory — default: ILS]
- **VAT rate**: [from memory — default: 18%]
- **VAT status**: [from memory]
- **Standing trusted brands**: [from memory]
- **Standing avoided brands**: [from memory]
- **Markup threshold (default)**: [from memory — default: 40%]
- **AliExpress tolerance**: [from memory — Yes/No/Generic only/Branded if seller ≥95%]
- **Amazon-IL tolerance**: [from memory — Yes/No/Max lead time]
- **Parallel imports / grey market**: [from memory]
- **Preferred Israeli stores**: [from memory]
- **Avoided Israeli stores**: [from memory]
- **Review score floor**: [from memory — default: 3.5/5]
- **Risk tolerance**: [from memory]
- **Email for PDF**: [from memory]

---

*Spec populated via `/intake`. Standing preferences loaded via `/load-preferences` from Mem0 MCP (if configured).*
