# CLAUDE.md

Orientation for an agent working in this repo. Read this before touching anything.

`START-HERE.md` is the human-facing version — what to run, in what order. This file is the agent-facing one: where truth lives, what is closed, and what not to reopen.

Last reconciled against the built prototype: **10 August 2026.**

---

## What this project is

**Semai / SemaiSens** — a remote-sensing agronomy platform for Malaysian agriculture. Four crops: paddy, oil palm, rubber, pineapple. Four audiences: estate managers, agronomists, farm owners, agency officers.

This repo holds two different kinds of file and they are not equal:

| Kind | What it is | Authority |
| --- | --- | --- |
| `concepts/`, `guides/`, `snippets/`, `api/`, `docs.json` | The Mintlify documentation site | **Source of truth.** Where docs and a brief disagree, the docs win. |
| `TRACK-*.md`, `DECISION-*.md`, `HANDOFF-*.md`, `DEFECTS-*.md`, `CONVENTION-*.md`, `CHARTS-product-map.md`, `START-HERE.md` | Build briefs, settled decisions, standing conventions, agent handoffs, defect logs | Derived. Patch these against the docs, never the reverse. |

The prototype itself lives in a Claude Design project, not here.

**Every root file carries a status line under its title.** Read it before acting on the file — several are consumed, superseded or deliberately unbuildable.

---

## Non-negotiable, closed sets

Do not extend these. They are closed by design and every brief restates them.

- **Eight guardrail categories:** Input validation, Preconditions, Refusals, Confirmations, Soft warnings, Rate and scope limits, Audit, Escalation. There is no ninth.
- **Nine telemetry event types.** There is no tenth. A UI interaction that is not one of the nine is not telemetry.
- **Eleven failure modes** and four degradation levels.
- **Eight functional roles:** `scout`, `agronomist`, `approver`, `estate_manager`, `estate_admin`, `regional`, `org_admin`, `on_call` (an overlay, not a primary). Plus `viewer`, which is an **org** role. "Farm owner" is a persona, not a role — if you see it in a role selector, that is a defect.
- **Human-readable ids only.** `fld_`, `blk_`, `zon_`, `ssn_`, `obs_`, `fnd_`. No UUIDs, no opaque suffixes.

## Standing conventions

- **Product mode carries no spec vocabulary.** No snake_case, no enum values, no field names anywhere a user can see. Review mode may.
- **Enum values are definitions; the labels file translates them.** Never make the user-facing label the enum value — reword the copy and the enum churns. This is the duplicated-authority defect and it has been paid for five times.
- **Anything the resolver computes has exactly one reader.** The labels file translates, never defines. This also governs controls: see scope authority below.
- **Refuse rather than mislead.** An em dash with a stated reason beats a fabricated figure. This is the product's signature habit, not a style preference.
- **Never average an average**, and never average across crops. Never sum across units — a total carries its unit or it is not a total. Confidence is the minimum of inputs, never the mean.
- **Never average a band, and never collapse one.** See the settled table.
- **k-anonymity at k = 5 on cohorts cut across schemes.** A scheme the reader's scope entitles them to is named however few fields it holds. This was refined on the regional forecast; the older "any cohort or breakdown" wording was too broad and contradicted what shipped.
- **A refused breakdown is not an absent one.** Refuse it visibly, restate the total, say the total is unaffected. Silence reads as "no data", which is a different and false claim.
- **Design system:** compose from existing components, iterate from the closest base, preserve base tokens, introduce none. Manrope, 4px spacing family, controls at 30 / 36 / 44px.

### Three standing files that briefs inherit from

An agent reading only this file will miss these. They are law, not reference.

| File | What it settles |
| --- | --- |
| `CONVENTION-scope-and-density.md` | Scope has one writable control per screen; the other renders derived. Three fates for a sentence — visible, tooltip, or trace. Nothing a reader must not miss goes behind hover. |
| `CHARTS-product-map.md` | Which mark belongs on which surface, and the chart budget per screen. The national overview gets the fewest marks in the product. |
| `guidelines/CHARTS.md` (design system) | What each mark is and how it behaves. The product map is the companion, not a replacement. |

---

## The stop rule

If a decision is needed that is not resolvable from the docs and the brief in front of you, **stop and say "Decision required: *question*"** rather than picking a reasonable default.

This is not caution for its own sake. A brief once said "this season resolved per field" without saying what happens with mixed crops; the build computed a union of eleven cycles, produced a 28-month window and a crushed axis. The fill was reasonable. That is exactly the problem.

---

## Settled — do not reopen

### Findings and severity

| Question | Answer | Record |
| --- | --- | --- |
| Can a person originate a finding? | Yes. One entity, `finding`, required `provenance` discriminator. | `concepts/finding-provenance` |
| Severity mechanism | **Acknowledgement, not a gate.** Findings commit immediately; a second name is recorded after. | same |
| Delivery | Per-organisation policy, bounded by the safety floor. `critical` never held; `high` held at most 4 hours. | same |
| Who may acknowledge | In-scope agronomist, not the author. Fallback to estate manager when no **other** in-scope agronomist has been active for 24 hours. | same |
| Raising ceilings | `scout` and `regional` cap at medium. `agronomist`, `approver`, `estate_manager` reach all bands. | same |
| Promotion | Reuses `promote_severity`. Appends. **`author_ref` does not change.** Writes its own audit row even though it is not a state transition. | same |

### Fields, blocks, effect

| Question | Answer | Record |
| --- | --- | --- |
| Blocks | May not overlap — rollups are area-weighted. Archive, never delete. | `TRACK-B-3` |
| Field completeness | Ecosystem and variety stay optional; the consequence becomes visible in three named states. | `TRACK-B-2` |
| Effect claims | Monetary figures only at `farm_calibrated`. NDRE mandatory on prime oil palm and tapped rubber. | `concepts/effect-methodology` |

### Track C — settled during the build, and each one cost rework

| Question | Answer | Record |
| --- | --- | --- |
| **Can a parent state one band for its children?** | **No. A band is a definition, not a measurement, so none is averaged and none is collapsed.** Where several resolve, name each with its hectares **and its own stage**, and position the reading by ground. Where a stage cannot be resolved, say "with no stage resolved" rather than falling back. | `TRACK-C-2`, built |
| Can a parent state one stage? | No. A crop in scope has a **distribution** of stages, not a stage. State the spread. | same |
| Scope authority | **One writable control per screen.** On Track C that is the page scope row; the app breadcrumb renders the resolved scope read-only. Operational surfaces are the reverse. | `CONVENTION-scope-and-density` |
| Do refusals recompute? | Yes. Narrowing scope recomputes the **refusals**, not only the figures. A refusal outliving its cause is a stale claim. | same |
| k = 5 | Cohorts cut across schemes. An entitled scheme is named however few fields it holds. | `TRACK-C-3`, built |
| Counted vs estimated | A production figure is counted and **carries no range**. A forecast is modelled and **always** carries one. They may not share an axis or a style. | `TRACK-C-4` |
| Short horizons on perennials | **Refuse rather than pro-rate.** Dividing an annual figure by the calendar is arithmetic, not a forecast, and oil palm output swings 30–50% across the year. | `TRACK-C-3`, built |
| Long horizon per crop | Season for rice, production year for oil palm and rubber, batch for pineapple. A mixed-crop scope carries no total, at **any** horizon — the unit is the reason, not the horizon. | same |

**Two decision files record superseded positions on purpose.** `DECISION-severity-option-P.md` is marked withdrawn; `DECISION-severity-option-R.md` carries an amended raising table plus the reasoning that led to the current model. They are kept so the wrong turns stay traceable. **Do not build from either.** The concept page is the live answer.

---

## Agronomic facts that have already caused defects

Each of these produced a wrong number or a wrong verdict at least once.

- **Nominal-is-best.** The band is the target. Deviation in *either* direction is loss. A higher index is not better — paddy falls through ripening, and high there means late, not healthy.
- **NDVI saturates above ~0.80.** Prime oil palm and tapped rubber sit there permanently, so effect measurement on those must run on NDRE or be refused, not degraded. Where a reading has saturated, **say so before naming its band** — a saturated figure ranks against nothing.
- **Young mature oil palm has not closed canopy.** Its expected index is lower than prime closed canopy, so reading a young stand against a prime band makes a normal stand look deficient. An estate mixing the two is not underperforming, it is young. This is the nominal-is-best error wearing a phase name, and it survived two rounds of the band fix because the collapse was happening one hop further down, inside a scheme rather than between schemes.
- **Rubber wintering** runs roughly February to March, extending into early April in some clones and northern states. Do not implement it as a muted date range: wintering recovers within 8–10 weeks, and a dip that fails to recover is Corynespora leaf fall, to which RRIM 2000-series clones are susceptible. **Expect the dip; flag failure to recover.**
- **Perennial planting date** is when the trees went in, often decades ago — not the start of the current production year. One field serving both makes every mature block read as newly planted.
- **Rice Main and Off seasons are not comparable.** The 15–25% gap is structural. Offering that comparison is a defect.
- **Rubber comparison** requires the same panel *and* the same tapping cycle.
- **Pineapple** has no single "this season" — batches overlap; compare at the same age from planting, never the same calendar date.
- **Comparisons align by phenology week**, not calendar week.

---

## Working with the docs

The Mintlify MCP connector (`https://mcp.mintlify.com`) can read, search, diff and edit the live docs. Deployment subdomain: `badixth-dc85e378`.

`checkout` opens a branch session; `save` opens a PR rather than committing to `main`, because agent review is enabled. **Leave it that way** and do not route around it via `execute_code`. Propose; let a human merge.

`search` across the whole docs set is the fastest way to check a cross-page claim, and `diff` is the only reliable way to confirm an edit actually landed.

`concepts/finding-provenance` is large enough that `read` will overflow a context window. Read the mounted `.mdx` directly, or delegate the read to a subagent.

---

## Testing the prototype

Playwright against the Claude Design preview. Two things that have cost time:

- **A child Design Component renders nothing standalone.** It must be loaded inside the `Semai Platform` shell, which is what hands it a store.
- **Reload the preview before concluding anything.** The tab caches a build. Three separate "it's broken" reports have turned out to be a stale preview or a mis-scaled screenshot coordinate.

Before reporting a control as inert, check whether a real click works — synthetic events do not always reach a React handler, and estate rows in the field picker expand rather than select.

---

## Known open gaps

**Track C:** briefs 1–3 built. Brief 4 (`TRACK-C-4-yield-production.md`) written, not built. Brief 5, assistance impact, not written — it ships as mostly refusals until a price basis exists, so it is deliberately last.

**The rate bands do not exist.** This is a finding, not a gap in the search. No Malaysian authority — DOA, RISDA, MPIB, or the reachable MPOB material — publishes a min/max for any crop-product pair. They publish **point recommendations**. `guides/prescription-maps` asks for a number that has to be set as policy rather than looked up. See `DECISION-rate-bands-do-not-exist.md`, which carries the sourced point recommendations and the open decision. Until it is answered, every band stays `decision_required` and **VRA refuses every prescription**. That is correct behaviour; do not relax it to make the feature look finished.

**`TRACK-B-4` is unbuildable** for the same reason.

**Expiry does not move the yield forecast.** Deliberate and still open.

**Per-estate delivery policy** is deferred; ships at organisation granularity.

**AC-15** — the acknowledgement fallback is not on `/snippets/role-model`. Recorded as a Docs Delta rather than patched, because it changes a second page's contract.

**Track B5**, the board view, is not written.

**Phase 11**, the dark/light and dead-control sweep, is held until Track C stops moving so it runs once over everything.

**Two prototype defects have outlived several reviews:** "Farm owner" in the role selector, and the two 17s. See `DEFECTS.md`.
