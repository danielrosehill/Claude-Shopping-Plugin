---
name: new-workspace
description: Provision a new region-specific shopping workspace on disk. Use when the user wants to start researching a consumer purchase in a supported region (e.g. Israel) or spin up a generic shopping scaffold. Accepts a workspace name and optional --variant=<region>. Scaffolds the workspace, personalises CLAUDE.md from the user's global memory, and (by default) creates a GitHub repo.
disable-model-invocation: true
allowed-tools: Bash(mkdir *), Bash(cp *), Bash(cat *), Bash(git init *), Bash(git add *), Bash(git commit *), Bash(gh repo create *), Bash(gh auth status), Bash(git push *), Read
---

# Provision Shopping Workspace

Creates a new workspace for researching a consumer purchase. This plugin's commands (`/shopping:find-product`, `/shopping:israel-search-zap`, `/shopping:recommend`, etc.) are globally available once installed — this skill only provisions the **data scaffold** (spec, catalogs/, outputs/, region data) that those commands read from and write to.

## Arguments

`$ARGUMENTS` is parsed as:

- **First positional**: workspace name (kebab-case, used as directory and GitHub repo name). Required.
- **Second positional** (optional): target parent path. Defaults to the topic group under `~/repos/github/` that most specifically fits the subject — see `~/repos/github/README.md` for the group list. (`~/repos/github/my-repos/` is no longer the default; it holds only blog repos now.)
- **`--variant=<israel|generic>`** (optional): which region scaffold to copy. Default: `israel`.
- **`--local-only`** (optional): skip GitHub repo creation and push. Default: create a public GitHub repo and push.
- **`--private`** (optional): create the GitHub repo as private. Default: public.

### Examples

```
/shopping:new-workspace bluetooth-speaker --variant=israel
/shopping:new-workspace new-laptop --variant=israel
/shopping:new-workspace office-chair --variant=generic --local-only
```

## Procedure

### 1. Parse arguments

Extract workspace name, target parent path, variant, and flags from `$ARGUMENTS`. If workspace name is missing, ask the user for it before proceeding. Default variant: `israel`.

### 2. Resolve the scaffold path

The bundled scaffold lives at `${CLAUDE_SKILL_DIR}/../../template/<variant>/`. Confirm it exists. If the variant isn't one of the shipped regions, list the available variants (`israel`, `generic`) and stop.

### 3. Read ambient facts

Read `~/.claude/CLAUDE.md` if it exists. Extract OS, locale, timezone, currency, and user identity facts. These will personalise the workspace's CLAUDE.md at step 6.

### 4. Create the workspace directory

```bash
mkdir -p <target-parent>/<workspace-name>
cp -r ${CLAUDE_SKILL_DIR}/../../template/<variant>/. <target-parent>/<workspace-name>/
```

Do **not** copy any `.claude/` tree. The plugin's primitives are global.

### 5. Personalise CLAUDE.md

Open the new workspace's `CLAUDE.md` and:

- Replace any placeholder identity with the facts from step 3.
- Add a short header noting the workspace name and variant.
- If ambient facts include locale/currency, embed them so downstream commands can skip re-asking.

### 6. Prompt for workspace-specific facts

Ask the user only for facts this plugin can't infer:

- **Product category** (free-form, e.g. "bluetooth speaker", "14-inch laptop", "ergonomic chair") — written into the spec.
- **Budget ceiling** (optional) — written into the spec.
- **Generic variant only**: the target country/region and currency (write into `CLAUDE.md` as `SHOPPING_REGION` and `SHOPPING_CURRENCY`).

### 7. Initialise git and (optionally) publish

```bash
cd <target-parent>/<workspace-name>
git init
git add .
git commit -m "Initial workspace from shopping plugin"
```

Unless `--local-only` is set:

```bash
gh repo create <workspace-name> --<public|private> --source=. --push
```

Use `--public` by default, `--private` if flag was passed.

### 8. Print next steps

Tell the user:

- Workspace path.
- Variant chosen.
- Suggested first commands:
  - `/shopping:intake` — flesh out `spec.md`
  - For Israel variant: `/shopping:israel-search-zap` as the default first-pass, then `/shopping:recommend` to generate a ranked PDF.
  - For Generic variant: `/shopping:find-product`, then `/shopping:recommend`.
- Reminder that the workspace is **data** — they can delete/move it freely without losing the plugin's commands.

## Notes

- The scaffold path must be resolved via `${CLAUDE_SKILL_DIR}/../../template/` (not `${CLAUDE_PLUGIN_ROOT}` — that variable isn't exported in skill bash injection, only in hooks/MCP).
- Never copy `.claude/commands/`, `.claude/agents/`, or `.claude/skills/` into the new workspace. If the user wants workspace-local overrides, they can add them manually later.
- Don't hard-code any personal paths or identifiers here — everything comes from user memory or prompts.
