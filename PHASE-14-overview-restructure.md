# Phase 14 — the two overviews

**Status: built and verified.** Both overviews restructured; F1 and F2 closed. See `DEFECTS.md` passes 22, 23 and 24.

Self-contained. **Attach nothing.** Run in a **fresh chat**.

Two screens: **`Overview`** and **`National overview`**. Phases 12 and 13 have shipped and are verified — the gauges, the one-line provenance, the promoted labels, the real table, the stated column drop. **None of it moves.** See §6.

**This pass changes what a reader meets first. It does not change what any screen claims.**

---

## 1 · The two problems, stated plainly

**`Overview` shows its answer twice.** The headline says *"Six fields need a decision today."* Directly beneath it, a card lists those same six fields across roughly 40% of the screen, before the reader reaches a single number. The card is redundant with the sentence above it, and it is a preview of `What needs you`, which is a screen that already exists and is one click away.

**`National overview` opens with three blocks of exceptions.** Coverage, *Past the window it had*, *Outside its band* — all correct, all useful, and all of them arriving before the reader has any sense of what they are looking at. A reader who lands here does not yet know the shape of the estate they are being warned about.

**Neither is a correctness problem.** Both screens are right. They are ordered for someone who already knows the answer.

---

## 2 · The rule that governs the restructure

**You may rank estates and regions. You may not rank them by an index.**

This is the constraint that makes the obvious redesign wrong, so it is stated before the design.

In this product a region is effectively one crop — Kedah is paddy, Sabah is oil palm, Perak is rubber, Johor is pineapple. **A health ranking across them is a comparison across crops**, which is the defect the entire band programme exists to prevent, and Perak sits at 0.81 where NDVI has saturated and ranks against nothing.

**But three measures rank honestly across crops**, because they are counts of ground or of events rather than readings:

| Measure | Why it is safe | Where it already exists |
| --- | --- | --- |
| **Hectares outside their own band** | Each field judged against its own band, then the hectares summed. A count of ground, one unit. | *"Muda Granary North is reading outside its own band on 7.3 of its 14.1 ha"* |
| **Findings past their window** | A count of events. | *"Muda Granary North has 1 finding past the window it had"* |
| **Days since last report** | A count of days. | *"KADA Blok W4 has not reported since 5 June"* |

**Rank by hectares outside their own band.** It answers the question an agency officer actually has — *how much ground is not where it should be* — and the screen already computes it.

**Never a ranked index, never a combined production figure, never a health score.** A composite of the three above would be a fourth measure nobody could name the unit of.

---

## 3 · `National overview` — lead with the shape, then the exceptions

**New order:**

1. **The resolved scope, in words.** Already required by `CONVENTION-scope-and-density.md` rule 1 and still the fastest orientation on the screen: *"124 fields · 10 schemes · Kedah, Kelantan, Sabah, Perak and Johor · three organisations."*
2. **The verdict**, one sentence. What a reader should do about today.
3. **The regions, ranked by hectares outside their own band**, each with its hectares, its field count, its crop and its coverage state. This is the new primary content.
4. **Crop health**, with the gauges built in Phase 12. Unchanged.
5. **The exceptions** — coverage, past the window, outside its band — **kept in full, moved below.**
6. **The totals.** Unchanged.

**The exceptions are not being demoted in importance.** They are being placed after the thing that makes them legible. A reader who has just seen that Sabah holds 270 ha outside its band reads *"Ladang Sabah Timur is reading outside its own band on 120.0 of its 216.5 ha"* as detail. A reader who meets it first reads it as noise.

**Nothing is removed.** Every exception, every refusal and every coverage sentence stays on the surface, with its reason.

### Clicking a region

A region opens its own detail — its estates, its schemes, its exceptions — **scoped, not navigated away from.** Reuse the expansion pattern Phase 13 built on Estate group, which renders the real component rather than a copy, and which is already verified.

**Scope authority does not change.** The page scope row stays the only writable control on Track C; the breadcrumb stays derived and read-only. **Expanding a region is not a scope change** — if it becomes one, it is a second writable control and the convention is broken.

---

## 4 · `Overview` — decide what it is for

The queue card and `What needs you` are the same content at different lengths. **Two screens showing one list is the duplicated-authority defect in a new place.**

**Recommendation: `Overview` keeps the headline verdict and a short queue — the three most urgent, then a line reading *"11 more"* that opens `What needs you`.** The numbers, the crop mix and the health-by-region cards move up to meet the reader sooner.

**The test that decides whether this worked:** a reader should be able to answer *"what is the state of my estates, and what is the one thing I should do"* without scrolling. Today they can answer the second and not the first.

**If the honest answer is that `Overview` has no job once `What needs you` exists, say so** rather than shortening a card to justify a screen. That is a legitimate finding and it is better raised than designed around.

---

## 5 · What must not move

Enumerated because a restructure is where these get lost.

- **Every refusal, with its reason.** Including *"No combined figure"*, *"no total across crops"*, and the pineapple open-batch sentence.
- **Every coverage gap and its date**, including *"synced 10 d ago"*.
- **The production card's refusal**: *"1 of 5 estates has written one · no total across them"*, with the surviving figure naming its crop.
- **All fifteen gauges** on National overview — 5 paddy, 4 oil palm, 3 rubber, 3 pineapple — each drawing its band, with the three rubber rows **hollow** for saturation.
- **Zero bare `title` attributes.** Every promoted label stays visible text.
- **Provenance stays one line**, verdict visible, mechanism in the trace.
- *"A band is a definition rather than a measurement"* stays **off** the surface.
- **No spec vocabulary in Product mode.**

---

## 6 · Acceptance tests

1. **No figure changed anywhere.** Diff every rendered number on both screens against before.
2. Every refusal, coverage sentence and k-suppression that existed before still renders, with its reason.
3. `National overview` states the resolved scope in words above the fold, then a verdict, before any exception block.
4. Regions are ranked by **hectares outside their own band**, and the measure is named on the surface.
5. **No ranking anywhere is by index, by a health score, or by a figure combining crops.** Grep the sort keys.
6. Each region row states its hectares, its field count, its crop and its coverage state.
7. Expanding a region renders the **same component** as the equivalent list elsewhere — verify by `data-sc-name`, not by eye.
8. Expanding a region does **not** change the page scope row or the breadcrumb.
9. `Overview` shows at most three queue rows plus a count that opens `What needs you`.
10. Every item in §5 re-measured and unchanged — gauge count, hollow rubber rows, zero bare titles, one-line provenance.
11. At **390 · 834 · 1440**: zero horizontal overflow, zero clipped text, zero controls under 44 px at 390.
12. Colour passes AA in both themes.

**Method:** measure, do not look. Allow **2 seconds** after navigating. **Enumerate affordances before concluding one is absent** — query `[data-sc-name]`, `[role]`, `[aria-expanded]` rather than guessing at a label. Three probes in Phase 13 reported missing features that were present under different names.

---

## 7 · Out of scope

- **Anything on Fields workspace, Estate group or the board.** Phase 13 closed them.
- **Prescriptions and VRA.** Still blocked on the rate bands — every band is `decision_required` and the fail-closed rule refuses every prescription. That remains correct.
- **New tokens or new components.**
- **Renaming `Application plans`** to the documented noun, `prescription`. Recorded, still separate.

If a decision you need is not in this file and not resolvable from it, **stop and say "Decision required: *question*"** rather than picking a reasonable default.
