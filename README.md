# SemaiSens — documentation and build record

The Mintlify documentation site for **Semai / SemaiSens**, a remote-sensing agronomy platform for Malaysian agriculture, together with the build briefs and decision records that drive its prototype.

Four crops — paddy, oil palm, rubber, pineapple. Four audiences — estate managers, agronomists, farm owners, agency officers.

## Where to start

| You are | Read |
| --- | --- |
| A person picking the project back up | **`START-HERE.md`** — current state, what to run next, what to check when it comes back |
| An agent about to change something | **`CLAUDE.md`** — where truth lives, which sets are closed, what not to reopen |
| Working on the docs site itself | `AGENTS.md` — Mintlify mechanics and the review gate |

## What is in here

**The documentation site** lives in `concepts/`, `guides/`, `snippets/`, `api/` and `docs.json`. This is the source of truth. Where a brief and the docs disagree, the docs win.

**The build record** lives at the repo root:

- `TRACK-*.md` — self-contained build briefs, one per surface. Pasted into a fresh chat with nothing attached.
- `DECISION-*.md` — settled positions, including two deliberately-kept wrong turns so the reasoning stays traceable.
- `CONVENTION-*.md`, `CHARTS-product-map.md` — standing rules every brief inherits.
- `DEFECTS.md` — append-only defect log with dates.
- `HANDOFF-*.md` — briefs written for a specialist agent, mostly consumed.

Every root file carries a status line under its title. Read it before acting on the file.

The prototype itself lives in a Claude Design project, not in this repo.

## Running the docs locally

```bash
npm i -g mint
mint dev
```

Run from the folder containing `docs.json`. `mint update` if the CLI is out of date; delete `~/.mintlify` and re-run if dev stops working.

## Publishing

Changes to the deploy branch publish automatically via the Mintlify GitHub app. **Agent review is enabled**, so agent-authored changes open a pull request rather than committing to `main`.
