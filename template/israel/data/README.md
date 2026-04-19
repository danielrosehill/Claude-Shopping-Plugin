# Data

Reference data the agent reads during shortlisting and sourcing. **Do not delete** — these files anchor the IL workflow.

| File | Purpose |
|------|---------|
| `shopping-rules.md` | VAT rules, Eilat pricing, markup tiers, translation notes, city map |
| `discovery-techniques.md` | Manual IL discovery playbook (Google `site:.il`, Zap, reverse-lookup) |
| `retailers.md` | Curated tier-1/2/3/4 IL retailers and their search-URL patterns |
| `hebrew-category-map.json` | Cached Hebrew nouns per product class |
| `israeli-stores.json` | ~830 IL vendors with categories + tech flag |

Maintain `israeli-stores.json` via `/shopping:israel-add-store <url> "<what they sell>"`.
