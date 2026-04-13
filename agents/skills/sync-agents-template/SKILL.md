---
name: sync-agents-template
description: Plan and merge agent tooling from github.com/nnajiabraham/agents_dot_file into another repo—.cursor, agents/skills, docs subtrees, ignore files, AGENTS.md—with update vs adapt modes and explicit conflict policies. Use when the user wants to bootstrap or refresh Cursor/agent config from that template.
---

# Sync agents template

Bring **github.com/nnajiabraham/agents_dot_file** (default branch `main`) into a **target repository** so the target gets the same agent surface the template maintains: `.cursor/`, `agents/skills/`, agent-oriented docs, ignore patterns, and a merged `AGENTS.md`.

This is **not** a generic git merge of two histories. It is a **path-directed sync** with a written plan, user checkpoints, and configurable behavior when paths already exist.

## When to use

- The user wants to **copy or refresh** template agent config into the repo they have open (or a path they name).
- The user says **“sync agents_dot_file”**, **“merge agents template”**, or similar.
- After a template change on `agents_dot_file`, the user wants **other repos** updated the same way without re-specifying every step.

## Inputs to collect (before editing files)

1. **Target root** — Default: current workspace / repo root. Confirm if monorepo or subdirectory.
2. **Template source** — Default: `https://github.com/nnajiabraham/agents_dot_file`, ref `main`. Allow override (fork, tag, SHA).
3. **Sync mode**
   - **`update`** — Align with the template: copy template files, merge ignores, merge `AGENTS.md` using the structure below. Minimal discovery of the target (only what’s needed to resolve paths and obvious breaks).
   - **`adapt`** — Same as `update`, plus **target-specific adaptation**: fill or rewrite `docs/agents/project.md`, `docs/agents/dev-workflow.md`, and `docs/agents/coding-conventions.md` using the target’s README, Makefiles, package manifests, and layout so links and commands are accurate for *this* repo.
4. **Docs scope** — Default: **template subtrees only** — sync paths that exist under the template’s `docs/` (e.g. `docs/agents/`, `docs/prompts/`). Do **not** delete or replace unrelated trees such as `docs/rfcs/` unless the user explicitly asks for a broader scope. For **conflicts** under those subtrees (or when expanding scope), use the **conflict policy** below (`ask` / `template` / `target` / `merge`).
5. **Conflict policy** — For any path that **exists in both** template and target and **differs**, choose one (per run or per file class):

   | Policy | Behavior |
   |--------|----------|
   | `ask` | Stop at conflicts; list files; wait for user per file or class. **Default when unclear.** |
   | `template` | Template version replaces target for that path. |
   | `target` | Keep target; skip copying that path from template. |
   | `merge` | For Markdown, produce a merged document: keep template section order where possible, preserve target-only sections (e.g. “Spotube quick reference”), dedupe headings. For non-Markdown, treat as `ask` unless user approves a binary/template choice. |

   The user may mix policies (e.g. `template` for `agents/skills/**`, `merge` for `AGENTS.md`, `target` for `.cursor/mcp.json`).

6. **`agents/skills/` layout policy** — Choose explicitly:
   - **`additive`** (default): add/update files from the template; do **not** delete target-only skills or paths missing from the template.
   - **`mirror`**: make `agents/skills/` match the template (add/update **and** remove skills not present in the template).

7. **`.cursor/mcp.json` and secrets** — Copy **verbatim** from the template **only after explicit user approval** (the template may contain placeholder tokens such as `YOUR_GITHUB_PAT`). If the user has not approved, **omit or skip** `.cursor/mcp.json` and note it in the plan.

Do **not** encode automatic removal of `.cursor/rules` or other Cursor rule folders unless the user requests it.

## Template paths to consider

Use the template tree as the checklist (adjust if the template adds files):

| Area | Typical paths |
|------|----------------|
| Cursor | `.cursor/*` (e.g. `mcp.json`) |
| Skills | `agents/skills/**` |
| Docs | `docs/agents/**`, `docs/prompts/**` (and any new doc folders under template `docs/`) |
| Root | `AGENTS.md` |
| Ignores | `.gitignore`, `.cursorignore` (if present in template) |

## Workflow

### 1) Plan (no writes yet)

1. Fetch template at the chosen ref (shallow clone or archive is fine).
2. Inventory **template paths** vs **target paths** for each area above.
3. List **actions**: add / update / skip / conflict.
4. For each conflict, note recommended resolution using the chosen **conflict policy**; if policy is `ask`, stop and present a table.
5. Call out **MCP / PAT placeholders** and require approval before copying `.cursor/mcp.json`.
6. Present the plan to the user and get **explicit go-ahead** before applying.

### 2) Apply

1. Copy or merge files per plan. Prefer normal file tools; preserve executable bits if any.
2. **`adapt` mode**: After template files are in place, update `docs/agents/project.md` (what the repo is, where things live), `docs/agents/dev-workflow.md` (real install/build/test commands), and lightly adjust `docs/agents/coding-conventions.md` to match stack and house rules.
3. **`AGENTS.md`**: Merge template structure (“start here”, progressive disclosure links to `docs/agents/*`) with any **target-specific** quick reference blocks (ports, Make targets, env files, CI gotchas). Use conflict policy `merge` unless user chose otherwise.
4. **Ignores**: **Union** template and target: keep target-specific entries, add template blocks not already present, dedupe lines.
5. Run **secret scanning** / repo hooks if available; if a copied example trips the hook (URLs, tokens), **fix or ask** — do not commit secrets.

### 3) Verify

1. List `git status`; ensure no accidental deletion of unrelated `docs/` trees.
2. Optionally run `make test` / `make lint` if the target repo uses them and the user cares (agent config changes usually don’t require it).
3. Summarize changed paths and recommend commit message (e.g. `chore(agents): sync from agents_dot_file`).

## Confirmation checkpoints

- Before copying **`.cursor/mcp.json`**: user must approve verbatim copy (and understand PAT placeholders).
- Before **`template` wins** on a large subtree (e.g. all of `agents/skills/`): confirm no target-only skills will be lost—or scope the replace.
- After **`adapt`**: user should skim `docs/agents/project.md` and `dev-workflow.md` for accuracy.

## Error handling

- **Template fetch fails**: Retry or ask for alternate ref/fork URL.
- **Ambiguous conflicts** with policy `ask`: do not guess; list paths and options.
- **User revokes MCP copy**: leave existing `mcp.json` untouched or document “skipped by user request.”
