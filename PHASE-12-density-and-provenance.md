# Phase 12 — density and provenance

**Status: built and verified.** Fifteen band gauges, one-line provenance, D1 and F1 closed. See `DEFECTS.md` passes 18, 19 and 23.

Self-contained. **Attach nothing.** Run in a **fresh chat**.

Cross-surface, like Phase 11. It changes **where things sit and what draws them** — not what any screen claims. If this pass changes a single figure, something has gone wrong.

---

## 0 · One correctness fix first, and it is not a design change

**`Overview` → Production forecast reads `National 5,410 t`.** The card lists five estates. One carries a figure — Ladang Sabah Timur, **4,820 t**. The other four are em-dashes: *"No forecast written for this estate"* twice, *"No rollup has been written for this estate yet"* twice.

**The parent is 590 t larger than the only child it names**, and the scope is four crops over 576.6 ha. One click away, `National overview` refuses this exact figure:

> *"Paddy, oil palm, rubber and pineapple are not the same measurement. A single figure across them would not be a weak number, it would not be a number."*

Two screens, one question, opposite answers. Either it is summing tonnes of paddy with tonnes of FFB — the never-sum-across-units defect — or it is oil palm alone, mislabelled `national`.

**Fix this before anything else in this file.** If the honest answer is that no total exists at this scope, the card refuses and states what it can: the one estate that has a forecast, with its figure and its confidence. **Do it first so nobody later mistakes a correctness bug for a density decision.**

---

## 1 · What this pass is actually fixing

The product is optimised for **being right** and not yet for **being read**. Every figure can defend itself, and that defence currently renders at the same weight as the figure.

The cause is in the briefs, not the build. They specified what must be **true** — *state the spread, say so before naming its band, name each band with its hectares and its own stage* — and each rule was faithfully rendered as a sentence. Sixteen screens of faithfully-rendered sentences is a document.

**The fix is not to delete the sentences. It is to let a mark carry what a paragraph is carrying now.**

`CHARTS-product-map.md` already made this argument and it has never been built:

> *"The row currently reads 'Inside the 0.54–0.74 band for paddy at panicle initiation / booting' — nine words a reader assembles into a mental picture. **The gauge is that picture.**"*

> *"**Saturation is drawn, not just said.** Where the index has saturated, the gauge shows the reading pressed against a ceiling it cannot discriminate past. Rubber at 0.81 should look inconclusive, not excellent."*

**That is this pass in two quotes.** The specification exists; the screens predate it.

---

## 2 · Inherited, and not up for renegotiation

**Nothing on any screen changes what it claims.** No figure moves, no refusal is dropped, no scope changes.

**A band is a definition, not a measurement.** Never averaged, never collapsed. Where several resolve, each is named with its hectares and its own stage.

**Nominal-is-best.** Deviation in either direction is loss. **Never a bare index on a scale** — `CHARTS-product-map.md`: *"A chart with a 0-to-1 axis and no band drawn is the nominal-is-best defect in visual form, and worse than the prose version because a scale implies more is better. Every index mark carries its band or it does not ship."*

**NDVI saturates above ~0.80.** A saturated reading ranks against nothing and must be **drawn** as inconclusive.

**The three fates**, from `CONVENTION-scope-and-density.md` Part 2:

| Fate | Test |
| --- | --- |
| **Visible** | *Would a reader draw a wrong conclusion without it?* |
| **Tooltip** | *Could you delete it and still read the figure correctly?* ≤ 12 words. Hover **and** focus **and** tap. |
| **Trace or docs** | *Is it about the method rather than the result?* |

**Nothing a reader must not miss goes behind hover.** On a phone the middle fate does not exist at all — promote it or drop it.

**Design system:** compose from existing components, iterate from the closest base, preserve base tokens, **introduce none**.

---

## 3 · Provenance is context, not news

Every page carries some combination of **Last updated · Data through · Confidence**, and on `Estate group` they are three full-width cards with a four-row breakdown beneath — at the same visual weight as the findings.

**The pattern already exists in the product.** The top bar reads *"Data up to 14 Jun · checked this morning · synced 1 d ago."* The pages ignore it.

**Collapse to one line, in that pattern, on every page that carries it.** The split that keeps it honest:

| | Where it goes |
| --- | --- |
| **Confidence verdict** — low / medium / high | **Visible**, beside the figure it qualifies. *"Confidence low"* on a forecast changes what a reader should believe. |
| **What set it** — which four inputs, which was weakest, what the job saw | **Trace.** *"Confidence is the lowest of these, never their average"* is method. |
| **Job run time, data-through date, sync age** | **One line.** Three cards become one sentence. |

**Do not drop the freshness exception.** *"synced 10 d ago"* on KADA Kelantan East is not routine provenance — it is a coverage gap, it changes the reading, and it stays visible wherever it appears.

---

## 4 · The gauge — build the mark that was already specified

**`ChartGauge` with the band drawn behind it, on every crop-health row.** The design system carries it: the band as tall teeth, the reading as the rule, the figure as **signed distance from the band**, a hollow rule past a ceiling the index cannot discriminate above, and thinned teeth where the band is not calibrated for that crop and stage.

**One row per band, not one per crop.** Paddy resolves five bands, oil palm four, rubber and pineapple three. A crop does not get a gauge; **each resolved band gets a row, carrying its own hectares and its own stage.** That is the settled rule, and drawing it makes *"no single stage describes this crop"* visible rather than asserted.

**What this replaces.** The paddy row on `National overview` currently renders as body copy:

> *"Five bands resolve for paddy — 0.55–0.75 at tillering on 412.6 ha, 0.52–0.72 at tillering on 268.4 ha, 0.40–0.65 with no stage resolved on 9.9 ha, 0.35–0.60 at tillering on 5.5 ha and 0.65–0.80 at panicle initiation / booting on 4.2 ha. A band is a definition rather than a measurement, so none is averaged and this reading is positioned by ground instead: 693.3 ha read inside their own band, 4.2 ha read below theirs and 3.1 ha read above theirs…"*

**Eighty words become five rows and one line.** What stays on the surface: the five rows, each with its hectares and stage, and the position-by-ground summary — *693.3 ha inside · 4.2 below · 3.1 above*. What goes to the trace: *"a band is a definition rather than a measurement, so none is averaged"* — that is the **rule**, not this instance, and it is being restated on every crop row on every screen.

**Rubber is the test case.** It currently says *"NDVI has saturated on this ground — above 0.80 it stops discriminating, so this figure ranks against nothing."* The mark draws that: hollow rule, pressed at the ceiling. **Keep a short sentence too** — saturation changes what the reader believes, so it does not live in the drawing alone. But it can be six words instead of twenty-six.

---

## 5 · The text triage, worked

Apply the three fates. **Trace where it explains method; keep where it changes belief.**

| Sentence, as it stands | Fate | Why |
| --- | --- | --- |
| *"A band is a definition rather than a measurement, so none is averaged and this reading is positioned by ground instead"* — on **every** crop row, **every** screen | **Trace** | The rule, restated four times per screen. Once, in *How this was built*. |
| *"Fields here are spread across four stages — 39 at tillering, 18 at stem elongation, 1 at panicle initiation / booting and 1 at ripening / maturity — so no single stage describes this crop"* | **Split** | The distribution is drawn by the rows. Keep the count of stages; the enumeration goes to the trace. |
| *"NDVI has saturated on this ground — above 0.80 it stops discriminating, so this figure ranks against nothing"* | **Visible, shortened** | Changes belief. The mark carries most of it; six words carry the rest. |
| *"Each child's reading multiplied by its hectares, added, then divided by the hectares. Never the plain mean of the readings — that quietly weights a 3 ha block the same as a 182 ha one."* | **Trace** | Already in a trace. Correct. Leave it. |
| *"Ladang Johor Selatan holds one block with more than one open batch, and production is not split between them"* | **Visible** | A refusal with its cause. Stays. |
| *"One scheme has not reported since 5 June"* | **Visible** | Coverage gap. Stays, everywhere. |
| The boxplot caption on **Yield production**, ~100 words | **Split** | *"No point is drawn for an individual field"* is a **privacy refusal** — visible. *"Young mature palms have not closed canopy and carry less per hectare by definition"* changes what the reader believes about the number — visible. The k-mechanics, the IQR definition and the same-scale note are method — trace. |

**One rule to carry out of this table:** where a sentence appears on more than one row of the same screen, it is a rule and belongs in the trace. **Restatement is the tell.**

---

## 6 · What must not move, whatever the density pressure

Enumerated because a density pass is exactly where these get lost.

- **Every refusal, with its reason.** A refusal is an answer and renders as one.
- **Every coverage gap**, and the date it started.
- **Every unit**, and every *"no total across crops"* banner.
- **Band position** — inside, above, below, with hectares. This is on the Visible list by name and it is **currently hover-only in fourteen places on Fields workspace** (native `title` on bare bar segments). A native `title` opens on neither tap nor focus. **Promote all fourteen or drop the segments.**
- **k-suppression**, and the total that survives it.
- **Saturation and non-calibration**, in words as well as in the mark.

---

## 7 · Two small ones

**The `Ask Semai` button overlaps chart content.** On `Yield production` it sits on top of the boxplot's own title. Floating controls do not overlap the thing they are floating above.

**`MEAN INDEX —` is a refusal card at KPI weight** on `Fields workspace` — a card whose content is *"not comparable across four crops."* The refusal is right and must stay; **a quarter of the KPI row is the wrong place for it.** Make it a line under the row, not a peer of the numbers.

---

## 8 · Acceptance tests

**Test 1 is the one that says the job was done correctly.**

1. **No figure changed anywhere.** Diff every rendered number on all sixteen screens against before. Any difference is a defect in this pass, not an improvement.
2. Every refusal that existed before still renders, with its reason, in words.
3. Every crop-health row draws a gauge with its band behind it. **No index mark anywhere renders without its band.**
4. Paddy shows **five rows**, oil palm four, rubber three, pineapple three — each with its own hectares and its own stage.
5. A saturated reading draws as inconclusive — hollow past the ceiling — **and** says so in words.
6. Where a band is not calibrated for the crop and stage, the mark shows it.
7. *"A band is a definition rather than a measurement"* appears **at most once per screen**, and not on the surface. Grep for it.
8. Provenance renders as **one line** per page. `Last updated`, `Data through` and `Confidence` are no longer three cards anywhere.
9. Confidence's **verdict** is visible beside its figure; **what set it** is in the trace.
10. *"synced 10 d ago"* and every other coverage exception is still visible.
11. Zero content is reachable only by hover. Grep for `title` attributes with no `aria-label` — **currently 21, of which 14 have no visible text.**
12. `Ask Semai` overlaps nothing.
13. At **390**, everything Phase 11 closed is still closed: 390 / 390, zero clipping, zero controls under 44 px, no truncated refusal.
14. At **834 and 1440**, no new overflow.
15. Colour still passes AA everywhere, both themes.

**Method:** measure, do not look. Allow **2 seconds** after navigating. Read the `body` background before trusting a colour result — the preview reverts to light on reload.

---

## 9 · Out of scope

- **Any change to what a screen claims.** See test 1.
- **The table rebuild on Fields workspace** — column sort, crop column, checkbox selection, shape thumbnails. That is the next pass and it is a different job.
- **The Overview and National overview restructure** — leading with scannable numbers, ranking regions by hectares outside their own band. Depends on this pass settling what the marks look like.
- **New tokens or new components.** Compose from what exists; the band mark is already in the design system.
- **The scout drawer.** Built and clean.
- **Renaming `Application plans`** to the documented noun. Real, recorded, separate.

If a decision you need is not in this file and not resolvable from it, **stop and say "Decision required: *question*"** rather than picking a reasonable default.
