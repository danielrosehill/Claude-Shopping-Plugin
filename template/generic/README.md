# {WORKSPACE_NAME}

Generic shopping workspace — provisioned from the [shopping plugin](https://github.com/danielrosehill/shopping-plugin) when the target market doesn't yet have a dedicated region variant.

One repo = one purchase. See `CLAUDE.md` for the workflow.

## First-run setup

1. Edit `data/vendors.md` — list the local retailers you want to compare across.
2. Edit `data/tax-rules.md` — note the local VAT / sales tax rate and any quirks.
3. `/shopping:load-preferences`, then `/shopping:intake`.
