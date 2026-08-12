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

## Three kinds of band, and they are not interchangeable

**Added 11 August 2026, after the word caused a real misreading between an agent and a build.** "Band" is doing three jobs in this product. Every one of them is a range with a floor and a ceiling that a value gets compared against, so the collision is easy and it is expensive.

| | A range on… | Keyed by | Where it lives | State |
| --- | --- | --- | --- | --- |
| **Index band** | a **reading** — NDVI, NDRE | crop × ecosystem × phase | `guides/Crop/*/*-ecosystems.mdx` | **Sourced, documented, live.** Refined per field from season history |
| **Rate band** | an **input** — kg of product | crop × product × soil group | `concepts/risk-model` → Bands | **Three peat rows sourced.** Everything else `decision_required` |
| **Tissue band** | a **measurement of the plant** — % N in frond 17 | crop × nutrient × a crop-specific discriminator | **Rubber and pineapple are live in the docs and unscoped. Oil palm and rice have none.** | `MODEL-nutrient-band.md`, `AUDIT-crop-literature.md` §1 |

**Corrected 12 August 2026.** That row read *"Researched, not yet in the docs"* until the crop tree was read. It was wrong. Five four-tier Whorl 2 tables ship today at `guides/Crop/Rubber/nutrient/*.mdx` and six D-leaf targets at `guides/Crop/Pineapple/nutrient/*.mdx` — **all eleven carrying no site, soil, clone, cultivar, age or season qualifier**, and both rubber pages render five nutrient verdicts side by side. The tissue-band track is therefore a **correction of two crops and a build of two**, and the correction is the urgent half.

**They may never be compared, summed, substituted or averaged into one another.** An index reading outside its band says the crop looks wrong. A rate outside its band says the operator asked for too much fertiliser. A tissue value outside its band says the plant is short of a nutrient. **Three different claims, three different consequences, one word.**

**Say which kind, every time.** In a brief, in a defect, in a commit message, on a screen. *"The rice bands"* is ambiguous and has already cost a round trip — *"the rice index bands"* is not.

**The practical tell:** if a refusal fires when somebody *types* a number, it is a rate band. If it fires when a *satellite* reports a number, it is an index band. If it fires when a *laboratory* reports a number, it is a tissue band.

---

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

**Four of them were ours, not the literature's — established 12 August 2026 by the first full read of `guides/Crop/`. All four are now in the docs, and writing them down corrected three of them.** PRs #11 and #12. Cite the docs, not this file.

| Rule | Where it now lives | What writing it down changed |
| --- | --- | --- |
| **Nominal-is-best** | `snippets/band-resolution` plus all four ecosystems pages | **Gained the saturation carve-out.** Without it the rule fires permanently on every prime oil palm and tapped rubber block. |
| **Rice Main and Off are not comparable** | `rice-ecosystems.mdx` → *Main season and off season are not the same crop* | **Lost the 15–25% figure** — see below. |
| **Comparisons align by phenology week** | `rubber-ecosystems.mdx` → *Anomaly is refused inside the wintering window* | **Narrowed.** Calendar week is sound for the eleven months rubber sits on a plateau. It breaks in one window, and there the answer is a refusal. |
| **Rubber: same panel and same tapping cycle** | `rubber-diagnosis.mdx` → §4 | **Split.** It is a **yield** rule, not an index rule — see below. |

Two more the audit settled and this file should not forget: **"refuse" occurred zero times in the whole corpus** before PR #11, and **no NDRE band table exists for any crop**, so every *"switch to NDRE"* instruction still has no destination. The pages now say so instead of pretending otherwise.

- **Nominal-is-best.** The band is not a floor. **A reading above the band is a finding, not the absence of one.** Paddy is the case that matters: the canopy is *supposed* to fall through ripening, so a field still at heading values at day 110 has delayed maturity, not a better crop. **Do not write this as "deviation in either direction is loss"** — that is too strong and it was the old wording. An above-band reading is a *question*, and the answer is usually vegetation that is not the crop, or a wrong planting date. **The carve-out is load-bearing: at saturation no above-band finding fires at all**, because a saturated figure ranks against nothing. That carve-out covers oil palm from year 6 and rubber's entire tapping life, so those two crops get the least from this rule and paddy and pineapple get the most.
- **NDVI saturates above ~0.80.** Prime oil palm and tapped rubber sit there permanently, so effect measurement on those must run on NDRE or be refused, not degraded. Where a reading has saturated, **say so before naming its band** — a saturated figure ranks against nothing.
- **Young mature oil palm has not closed canopy.** Its expected index is lower than prime closed canopy, so reading a young stand against a prime band makes a normal stand look deficient. An estate mixing the two is not underperforming, it is young. This is the nominal-is-best error wearing a phase name, and it survived two rounds of the band fix because the collapse was happening one hop further down, inside a scheme rather than between schemes.
- **Rubber wintering** runs roughly February to March, extending into early April in some clones and northern states. Do not implement it as a muted date range: wintering recovers within 8–10 weeks, and a dip that fails to recover is Corynespora leaf fall, to which RRIM 2000-series clones are susceptible. **Expect the dip; flag failure to recover.**
- **Perennial planting date** is when the trees went in, often decades ago — not the start of the current production year. One field serving both makes every mature block read as newly planted.
- **Rice Main and Off seasons are not comparable.** The difference is structural — different water regime, different radiation, and the off season leans harder on controlled irrigation. Offering that comparison is a defect. **Do not quote a percentage.** This file carried "the 15–25% gap" for weeks and it has no source anybody has been able to find; it was dropped on 13 August 2026 rather than written into the docs. What *is* sourced (FEWS NET Malaysia Country Book, from *Perangkaan Agromakanan*): the two seasons are reported separately in Malaysian statistics, and Sarawak runs about a month behind Peninsular at both planting and harvest. What is **not** refused: a weak zone repeating season after season, which compares a patch against the rest of the same field in the same season.
- **Rubber panel and tapping cycle govern *yield*, not the index.** A block moved from d/2 to d/3 loses latex the following week and looks identical from orbit. So: **a latex yield comparison names its panel and its tapping system, or it is not a comparison** — and it does *not* govern NDVI comparison, which is set by wintering and stand age. Filing this next to the index bands makes it read as an index rule and it will be wrong. The platform cannot enforce it either: tapping frequency and panel position are free-text scout notes, not fields.
- **Pineapple** has no single "this season" — batches overlap; compare at the same age from planting, never the same calendar date.
- **Comparisons align by phenology week**, not calendar week — but the rule is narrower than it sounds. On a plateau crop the calendar is a fine proxy and calendar-week comparison is sound. It breaks where a stage can shift relative to the calendar, which on rubber is the wintering window, and there **the platform refuses the anomaly rather than reporting it.** A future onset-anchored comparison would be a refinement of that refusal, not a reversal; the docs are written so it can be.

**Two more, added 11 August 2026.** These have not caused a defect yet because the product does not hold leaf analysis — but they are the same shape as the ones above and they will bite the moment nutrient work starts. See `RESEARCH-the-band-is-on-the-tissue.md`.

- **A multi-nutrient diagnosis is not a set of independent verdicts.** If more than one nutrient reads deficient, or any reads very deficient, **only the rating of the most deficient nutrient is valid** — the diagnostic system only works near the optimum (Foster 2003, via Goh/AAR). Rendering five nutrient verdicts side by side when three read deficient is a fabricated claim of the same family as averaging a band. Name the worst one and **refuse the rest**, with the reason.
- **A tissue band is site-specific, exactly as an index band is stage-specific.** Foster's computed K band at one site is 0.77–0.86%; the generic mature band is 0.90–1.20%. They disagree **by design** — optimum concentration varies with soil, terrain, palm age, climate, season and sampling method. **A single global band per crop reintroduces the collapse the whole band programme exists to prevent.** A tissue band carries its scope or it is not a band.

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

**The rate bands mostly do not exist — but VRA no longer refuses everything. Corrected 13 August 2026.** Malaysian authorities publish **point recommendations**, not min/max, so eleven crop-product rows stay `decision_required`. Two things have changed since that was written:

- **Three oil palm peat rows are `sourced` and validate** — MOP 4.0–6.0, urea 0.5–0.6, rock phosphate ceiling 1.0, all **kg/palm/yr** (MPOB–SOPPOA 2016). Note the unit: a per-hectare rate against a per-palm band must refuse on the **unit**, before any min/max check.
- **Attribution is live** (PR #5). A rate with no band but a named entitled author is permitted and labelled as attributed. `agronomist`, `approver`, `estate_manager` may set one; `scout` may not. **A rate with neither a band nor an author is still refused** — that is the part not to relax.

**The current VRA blocker is propagation, not the bands.** `concepts/risk-model` refuses in its fail-closed section what it permits in its attribution section forty lines later; `guides/prescription-maps` Input validation still demands the min/max; both advisor pages have never heard of attribution; and Step 5 of the VRA guide prints three unsourced example rates on a page whose whole point is that unsourced rates refuse. See `AUDIT-crop-literature.md` and `DECISION-rate-bands-do-not-exist.md`.

**`TRACK-B-4` is stale rather than unbuildable.** It was written around validation only and needs rewriting around attribution before it is used.

**Expiry does not move the yield forecast.** Deliberate and still open.

**Per-estate delivery policy** is deferred; ships at organisation granularity.

**AC-15** — the acknowledgement fallback is not on `/snippets/role-model`. Recorded as a Docs Delta rather than patched, because it changes a second page's contract.

**Track B5 and B6 are both built.** *Where the work is* is drag-writable, with every illegal move refused **in words on the card** rather than snapping back. Two things settled during the build and worth not reopening: **acceptance is the assignee's act** — a manager dragging a card to In flight is refused, because acceptance is what stops the two-hour on-call escalation — and **an undone move leaves a trace**, stated on the surface as *"Both moves stay in the record."*

**The task lifecycle is now enumerated** at `/guides/field-scouting` → *Task lifecycle*: four resting states, `draft` · `assigned` · `accepted` · `completed`, each cited. **Overdue is an event, not a state. Refusal returns a task to `draft` carrying its history, and is not a state either.** And the rule that stops the next surface reinventing it: **a board's columns are not the states** — they read state plus two attributes, and the split between To do and Waiting to be accepted is whether the assignee has been asked. Three things stay open there: mark-started-on-behalf, a per-role permission matrix, and whether the lifecycle covers work items beyond scout tasks.

**Phase 11 has run.** Colour in both themes, three widths, dead controls, all sixteen screens. Six findings, none of them screen-by-screen: three colour tokens, one colour semantic, one missing phone layout, one silent disabled control. See `DEFECTS.md`. Two things it settled — **there are no dead controls**, and dark mode is sound; the failures are light-mode contrast and the absence of any responsive breakpoint.

**Method note for the next sweep.** Measure, do not look. Contrast, overflow and handler presence are computed values, and a screenshot pass would have missed all six. Allow a 2-second settle after navigating or the probe reads an unrendered screen, and read the `body` background before trusting any colour result — the preview silently reverts to light on reload.

**Two prototype defects have outlived several reviews:** "Farm owner" in the role selector, and the two 17s. See `DEFECTS.md`.
