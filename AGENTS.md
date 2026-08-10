# Agent instructions

**Read `CLAUDE.md` first.** It is the real orientation file for this repo — where truth lives, which sets are closed, what is settled, and the stop rule. This file only covers the Mintlify mechanics that sit underneath it.

If the two ever disagree, `CLAUDE.md` wins.

---

## What this repo is

The Mintlify documentation site for **Semai / SemaiSens**, a remote-sensing agronomy platform for Malaysian agriculture, plus the build briefs and decision records that drive its prototype.

- Pages are MDX with YAML frontmatter. Configuration lives in `docs.json`.
- The Mintlify MCP server (`https://mcp.mintlify.com`) reads, searches, diffs and edits the live docs. Deployment subdomain: `badixth-dc85e378`.
- **Agent review is on.** `save` opens a PR rather than committing to `main`. Leave it that way and do not route around it via `execute_code`. Propose; let a human merge.
- `diff` is the only reliable way to confirm an edit landed.

## Authority

`concepts/`, `guides/`, `snippets/`, `api/` and `docs.json` are the **source of truth**. Everything at the repo root — briefs, decisions, conventions, defect logs — is **derived**. Patch the derived files against the docs, never the reverse.

## Style

- Sentence case for headings. Active voice.
- Code formatting for file names, commands, paths, field names and enum values.
- Bold for UI elements: click **Settings**.
- One idea per sentence.

**Product-mode copy is different and stricter.** Anything a user can see carries no spec vocabulary — no snake_case, no enum values, no field names. Review mode may. This is enforced by grep in several acceptance tests, so it is not a preference.

## The habit that matters most

**Refuse rather than mislead.** An em dash with a stated reason beats a fabricated figure, and a refusal is an answer that renders as one — never a blank, never a zero, never silence. This runs through the whole product and the docs describe it as behaviour rather than as tone.

## The stop rule

If a decision is needed that is not resolvable from the docs and the brief in front of you, **stop and say "Decision required: *question*"** rather than picking a reasonable default. `CLAUDE.md` explains what this rule cost before it existed.
