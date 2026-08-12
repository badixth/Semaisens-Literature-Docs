# Phase 17 — creating a prescription

**Status: written, not built.** This is the next thing to paste.

Self-contained. **Attach nothing.** Run in a **fresh chat**.

**The last hole in the product.** Semai can read a prescription, move one through six stages and export it. It cannot make one. This closes that.

---

## 0a · Read this first — "band" means three things

**Corrected 11 August 2026, after this brief caused a misreading.** Everything in this file that says *band* means **rate band** — a range on an input, in kilograms of product, keyed on crop × product × soil group, living in `concepts/risk-model` → Bands.

**It never means the index band**, which is the healthy NDVI or NDRE range by crop, ecosystem and phase, lives in `guides/Crop/*/*-ecosystems.mdx`, is **fully sourced and already live in the product**, and is what every gauge on every reading surface draws.

**Nothing in this brief touches the index bands.** They keep validating, they keep refusing, they keep being refined per field from season history. If a change here would stop a paddy index band from firing, that change is wrong.

The third kind — the **tissue band**, a range on a laboratory measurement of the plant — does not exist in the docs yet and is out of scope entirely. See `CLAUDE.md` → *Three kinds of band*.

**The tell, if you are unsure which one you are looking at:** a rate band refuses when somebody **types** a number. An index band refuses when a **satellite** reports one.

---

## 0 · What changed, and why this is buildable now

`VRA maps` has been read-only because every rate band is `status: decision_required` and the fail-closed rule refused everything. **That is no longer the whole story.**

`concepts/risk-model` now carries **two routes to a permitted rate**, and only two:

| Route | Condition | What the platform is claiming |
| --- | --- | --- |
| **Validated** | A `sourced` band exists and the rate falls inside it | *This rate is within a published recommendation, and here is the citation.* |
| **Attributed** | No band exists, and the rate carries a named author who was entitled to set it | *Nobody has published a range for this. A named person decided this figure, on this date, for this map.* |

**A rate with neither a band nor an author is still refused.** The fail-closed rule is intact — what changed is that *no source* and *no accountability* stopped being the same condition.

**And three bands are now `sourced`** — oil palm on peat: MOP **4.0–6.0**, urea **0.5–0.6**, rock phosphate **ceiling 1.0**, all kg/palm/yr. **They are the only validated rows in the product**, they are peat-only, and their unit is per palm rather than per hectare. A prescription submitted per hectare against them is rejected before the min/max check runs.

**So this build will exercise both routes on the first day**, which is exactly what it should do.

---

## 1 · The flow is already documented — build to it

`/guides/prescription-maps` carries a seven-step creation flow. **Do not redesign it.** Quoted, abbreviated:

1. **Open the VRA tab** on a field. Existing maps for that field are listed.
2. **Start a new map.** *"If you arrived here from a scout completion or risk card, the base index and target hazard are pre-filled."*
3. **Choose the base index** — NDVI, NDRE, NDWI.
4. **Set the number of management zones** — *"Choose between 2 and 5 management zones… The platform auto-clusters pixels using k-means and displays the resulting zone map immediately."*
5. **Review and adjust rates per zone.**
6. **Submit for review, or export** if you hold export permission.
7. **Export in your equipment's format.**

**Required at Input validation, verbatim:**

> *"Prescription requires: `field_id`, active crop cycle, `index_basis` (NDVI, NDRE, etc.), product, per-zone rates, and unit. Rates must fall within the crop-specific agronomic min/max declared in the Risk Model. Zones must cover the target field with no unintentional gaps or overlaps. **The advisor pre-fills the draft from the latest calibration; the user reviews rather than authors.**"*

**That last clause is the same design as the scout drawer**, which shipped and is clean. **Reuse it.** The drawer opens pre-filled from what the platform knows, and the human reviews, adjusts and signs.

**The lifecycle is six stages** — *generated → reviewed → approved → exported → applied → verified* — and the prototype already renders all six with nine plans across them. **This brief adds the first stage, not the rest.**

---

## 2 · The rate is where this brief earns its keep

Every zone rate renders as **one of three states, and the state is visible on the prescription itself** — not in a settings table.

| State | Renders as | Example |
| --- | --- | --- |
| **Validated** | The rate, its band, and the citation | *"4.8 kg MOP/palm/yr · inside the 4.0–6.0 band · MPOB, peat, 2016"* |
| **Attributed** | The rate, and **who decided it** | *"140 kg/ha · not sourced from any published recommendation. Set by Siti Rahman on 3 August, for this map only."* |
| **Refused** | No rate, and why | *"No published recommendation exists for this crop and product, and nobody has set a rate for it."* |

**The third is not a failure state.** It is the correct answer for most crop-product pairs today, and it must read as an answer rather than as a gap.

### Four constraints, each of which is how attribution decays if it is missed

**Restricted products refuse regardless of who signs.** Pesticides and high-nitrogen formulations sit under the existing safety floor; buffer zones near waterways, regulated boundaries and statutory maxima are law rather than agronomy. **Where the constraint is law, a name is not authority.** Attribution covers fertiliser and soil amendments only.

**A carried-forward rate carries its origin.** A figure entered once will be copied to the next map and the one after, until it is a house standard nobody has read the reasoning for — *which is the fabricated default the whole page refuses, arrived at by drift.* So a rate reused from an earlier map states **who set it, when, and on which map**, and the reader is told it is being reused rather than decided. **Reuse is not authorship.**

**Attribution is recorded on the prescription, not on the band.** Do not add a fifth `status` value. The band stays `decision_required` until somebody sources it.

**Sourced and attributed must be distinguishable at a glance.** The table will hold a mix for a long time, and a citation and a judgement that look identical at the point of use are worse than either alone.

---

## 3 · Zoning refuses on a saturated index — and the product already says so

The prototype's own draft copy reads:

> *"This field is at prime, closed canopy, where the canopy has closed. Crop health flattens out above about 0.80, so every part of the field reads the same whether it is short of nitrogen or not."*

**That sentence is a refusal that has not been wired to anything.** Zoning divides a field by variation in the base index. **Where the index has saturated there is no variation to divide by**, so k-means will return five zones of noise and the map will look precise and mean nothing.

**Refuse zoning on a saturated index, in words, and name the alternative** — NDRE where the crop and stage support it, per the standing rule that effect measurement on prime oil palm and tapped rubber runs on NDRE or is refused.

This is the same refusal the gauges already draw as a hollow rule. **Wire it to the thing it should have been governing.**

---

## 4 · The screens

**One new surface and two edits. No new components.**

### 4.1 · The creation drawer

**Reuse `New check`'s pattern and placement** — centred, 640 px, pre-filled where the platform knows something, refusing in words where it does not.

Order, which is also the reading order:

1. **The field and its cycle.** Refuse in words on a closed cycle, beside the list, per the pattern settled three times.
2. **The base index**, with the saturation refusal of §3 where it applies.
3. **The zones** — 2 to 5, k-means, rendered on the field.
4. **The rates**, one per zone, each carrying its state from §2.
5. **The product and unit.** A unit mismatch against a sourced band refuses **before** the min/max check.
6. **Submit for review**, or save as a draft.

**The drawer opens pre-filled or it opens on field selection.** Never an empty form.

### 4.2 · `VRA maps` gains its origin

A `New VRA map` action in the header, opening the drawer. The nine existing plans and their six stages are untouched.

### 4.3 · The field's VRA tab

The documented entry point is *"Navigate to Fields, select your field, and click the VRA tab."* **If that tab does not exist, add the action to the field detail instead** and record the difference rather than inventing a tab.

---

## 5 · Guardrails — all eight, none new

| Category | What applies |
| --- | --- |
| **Input validation** | `field_id`, active cycle, `index_basis`, product, per-zone rates, unit. Zones cover the field with no unintentional gaps or overlaps. **Rates validated *or* attributed** — §2. |
| **Preconditions** | *"Field must have a recent calibration (satellite pass within the cadence declared in the Crop Cycle Model). Buffer zones near waterways and regulated boundaries must be defined and honored."* |
| **Refusals** | Rates outside a sourced min/max. Missing or expired calibration imagery. Prescriptions on closed cycles. Application inside a regulatory buffer. **Restricted products above the safety floor, regardless of author.** Zoning on a saturated index. |
| **Confirmations** | **Export** — leaves the platform, effectively irreversible. Apply-to-fleet. Edit-after-export. *"Drafting, editing, and deleting a pre-export prescription commit optimistically with undo."* |
| **Soft warnings** | *"Rates near the top of the agronomic band (not over). Draft on a field that already has a recent active prescription for the same product. Draft during a busy application window for the estate."* |
| **Rate and scope limits** | *"Fleet-apply capped at N fields per action (org-configurable)."* **N is org policy — do not invent it.** |
| **Audit** | *"Every prescription is versioned. Log includes actor, source, `index_basis` snapshot, rate table, product, export format, timestamps, and any confirmation acknowledgments."* **Plus `set_by`, `set_by_role`, `set_at` and the reason on every attributed rate.** |
| **Escalation** | Deviation > 20% on as-applied → estate manager. Failed cross-check → agronomy review. *"Prescriptions rejected at export twice by different reviewers escalate to the org admin."* |

**Telemetry stays at nine event types.** `map.generated`, `map.reviewed`, `map.approved`, `map.applied` and `map.verified` are named in the docs. **Opening the drawer is not telemetry.**

---

## 6 · Roles and approval — both decided, build to them

**Decided 11 August 2026 and landed on `/guides/prescription-maps`.** An earlier draft of this brief told you to stop here. **You no longer need to.**

**`Operator` and `Read-only` are gone, and no ninth role was added.** The permissions were kept and redistributed across the eight:

| Role | Generate | Review | Approve | Export | Upload as-applied |
| --- | :-: | :-: | :-: | :-: | :-: |
| `scout` | | | | | ✓ |
| `agronomist` | ✓ | ✓ | | ✓ post-approval | ✓ |
| `approver` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `estate_manager` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `viewer` (org) | | | | | |

**Uploading as-applied data is `scout` work** — a record of what actually went on the ground is ground truth by definition. **A `viewer` cannot review**, because review attaches comments and a comment is a write.

**Who may set an attributed rate:** `agronomist`, `approver`, `estate_manager`. **`scout` may not** — its function is ground-truth work, not diagnosis, which is the same reason it cannot raise a finding above `medium`.

### The approver may not be the author

**A prescription that one person writes and the same person approves has had a single reader, and it puts fertiliser on the ground.** This mirrors the findings acknowledgement rule and binds harder here — a finding is a claim, a prescription is an act.

**Fallback, so a small estate is not deadlocked:** where no other entitled person has been active on the estate for 24 hours, approval falls to `estate_manager` and the record shows it took the fallback route. Where the author *is* the `estate_manager`, it falls to an in-scope `approver`. **If neither exists, the prescription waits — it is never self-approved.**

`map.approved` records **both names**: who wrote it and who approved it.

**This is a refusal, and it renders as one:** *"You wrote this prescription, so you cannot approve it. It needs a second name."*

---

## 7 · What must not regress

- **All gauges** — 15 National overview, 15 How a figure is built, 12 Fields workspace, 1 field detail.
- **Every refusal already on the surface**, including the VRA lines on `What needs you` — *"3 rate zones off — no base rate yet"*.
- **The six-stage lifecycle** and the nine existing plans.
- **The attribution copy already on the screen** — *"Semai proposes the rates; a person decides them, and approval is what makes a file possible"*, and *"Drafted by Ali Ismail, confirmed by Ali Ismail, approved by Siti Rahman."*
- **`New check` and `New field`** — both centred, both clean.
- **Whatever Phase 16 left.** If the repetition pass has landed, its word ceilings hold: **600 for operational screens, 900 for reading surfaces.** This drawer counts against `VRA maps`.

---

## 8 · Acceptance tests

1. A prescription can be created end to end from a field, and appears in the six-stage list as `Generated`.
2. **Every zone rate renders as validated, attributed or refused**, and which one is visible on the prescription without opening anything.
3. A rate against **oil palm on peat** validates against 4.0–6.0 MOP and states the citation.
4. **A per-hectare rate against a per-palm band refuses on the unit, before the min/max check.**
5. **A rate with no band and no author refuses**, in words, naming what is missing.
6. **A rate with no band and a named author proceeds**, and renders as a judgement — never as a citation.
7. **A restricted product refuses regardless of author**, and says that a name is not authority here.
8. A rate reused from an earlier map **states who set it, when, and on which map**.
9. **Zoning on a saturated index refuses in words** and names the alternative.
10. A closed cycle refuses **beside the field list**, not by destroying it.
11. Export carries a confirmation and states that it leaves the platform.
12. Draft, edit and delete commit with undo.
13. **No fifth `status` value** exists on the bands table. Grep it.
14. Audit records `set_by`, `set_by_role`, `set_at` and the reason on every attributed rate.
14a. **A `scout` cannot generate or approve**, and **a `viewer` cannot review**. Both refuse in words.
14b. **The author cannot approve their own prescription** — refused in words, with the fallback named. `map.approved` carries two names.
15. **No spec vocabulary in Product mode** — grep for `index_basis`, `field_id`, `set_by`, `decision_required`, `sourced`.
16. Every item in §7 re-measured and unchanged.
17. At **390 · 834 · 1440**: zero horizontal overflow, zero clipped text, zero controls under 44 px at 390.
18. Colour passes AA in both themes.

**Method:** measure, do not look. Allow **2 seconds** after navigating. **Query marks and components by `[data-sc-name]`, and enumerate affordances by `[role]` and `[aria-expanded]` before concluding one is absent** — this application has no `<main>` element, and a probe scoped to it returns zero on every screen.

---

## 9 · Out of scope

- **Filling any band.** The table stays as it is; three peat rows sourced, everything else `decision_required`.
- **Leaf-analysis ingestion.** Answered as yes in `MODEL-nutrient-band.md`, and it is its own track — bigger than a phase.
- **The as-applied upload and the verification cross-check.** Documented, downstream, separate.
- **The tissue band.** Waiting on the acquisition list.
- **New tokens or components.**

If a decision you need is not in this file and not resolvable from it, **stop and say "Decision required: *question*"** rather than picking a reasonable default.
