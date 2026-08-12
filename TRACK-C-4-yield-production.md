# Track C, Brief 4 — Yield production

**Status: built.** Tested at three widths and through the Phase 12 density pass.

Self-contained. **Attach nothing.** Every fact you need is inline. If something here contradicts your memory of an earlier phase, this file wins.

Run in a **fresh chat**. Third Track C surface. It reads the spine; it adds no rules to it.

---

## 1 · The question this screen answers

**How much actually came off, and is it coming off evenly?**

The regional forecast is forward-looking — *will the yield arrive*. This screen is backward-looking — *what arrived, and from where*. The second half of that sentence is the reason the screen exists.

An estate averaging 4.5 t/ha with every block at 4.5 is a different business from one averaging 4.5 with half its blocks at 3 and half at 6. **Only one of them has a problem to fix, and a mean cannot tell you which.** Every design decision below follows from that.

---

## 2 · What is inherited, and is not up for renegotiation

Three surfaces have now been built against these. They work. Do not redesign them.

**From the spine.** Never average an average — area-weighted always. Never average across crops. Never sum across units; a total carries its unit or it is not a total. Confidence is the minimum of inputs. Access does not relax on the way up: a field the reader cannot see is **in** the totals and **out** of every list, filter and export.

**From the band work.** A band is a definition, not a measurement, so **bands are never averaged and never collapsed**. Where several resolve, name each with its hectares and its own stage, and position the reading by ground. Where a stage cannot be resolved, say *"with no stage resolved"* rather than falling back. A crop in scope has a distribution of stages, not a stage.

**From `CONVENTION-scope-and-density.md`.** One writable scope control per screen; on Track C that is the page row, and the breadcrumb renders the resolved scope read-only. One line above the fold states the scope in words. Refusals recompute when scope narrows. Three fates for a sentence: visible if it changes what the reader believes about the number, tooltip only if it defines a term and could be deleted safely, trace or docs if it explains method. **Nothing a reader must not miss goes behind hover.**

**k = 5, as already decided on the forecast:** the threshold is for **cohorts cut across schemes**. A scheme the reader's scope entitles them to is named however few fields it holds. That decision is made; reuse the wording.

**Money stays gated at `farm_calibrated`.** No ringgit axis. No price basis exists.

---

## 3 · Counted is not estimated, and they may not share an axis

This is the trap unique to this surface, and it will be walked into.

A production screen wants to show **actual against forecast**. The two are not the same kind of number:

| | What it is | Carries a range? |
| --- | --- | --- |
| **Production to date** | Declared harvest, counted | **No.** A count is a count. |
| **Forecast** | Modelled | **Always.** Never a point. |

Putting a counted figure and a modelled figure on one axis, styled alike, tells a reader they are the same class of claim. They are not — one can be audited against a weighbridge ticket and the other cannot.

**Rules:**

- A production figure **never** carries a P10–P90 band, because there is nothing uncertain about it. If a range appears beside a production figure, something has gone wrong upstream.
- Where actual and forecast appear together, the forecast carries its band and the actual does not, and the difference is visible without reading a legend.
- **The variance between them is stated as a figure with its own words**, never as a bare percentage. *"1,240 t against a forecast of 1,500 t — 260 t short"* beats *"−17%"*, which invites a reader to treat a modelled denominator as ground truth.

---

## 4 · The window is part of the figure

*"Production"* with no window is not a quantity. Every figure on this screen names the window it sums over, and the window is crop-shaped — the same shapes the forecast already uses:

| Crop | The window | Reads as |
| --- | --- | --- |
| **Rice** | The named season | Main Season 1 · 2026, harvested to date |
| **Oil palm** | The production year | Production Year 2026, to the last declared harvest |
| **Rubber** | The production year, on a named panel and tapping cycle | Panel B · d/3 |
| **Pineapple** | The batch | Per batch, at its own age from planting |

**A part-harvested window says so.** *"1,240 t from 34 of 59 fields harvested"* — never a total that reads as final when it is not. A season three-quarters in is the most misleading number this screen can produce, because it looks exactly like a finished one.

**A scope spanning crops carries no production total.** Same rule as everywhere: refuse it, with the unit as the reason, at every window.

---

## 5 · Charts — and one of them is genuinely dangerous

Budget per `CHARTS-product-map.md`. This is an analytical surface, so it gets the most marks in the product. It also gets the only mark that can leak a farm.

| Mark | For | Rule |
| --- | --- | --- |
| **`ChartBoxplot`** | Comparing estates or schemes. The primary mark. | See the four rules below. This is the one that needs care. |
| **`ChartHistogram`** | The shape of one estate's blocks. | Where the question is *is this estate uniform*, one estate at a time. |
| **`ChartBarGrouped`** | The same measure across two or three things — season against season, panel against panel. | **Never four series.** And only across things the comparability rules permit. |
| **`ChartLine`** | Cumulative harvest through the window. | One line per crop. Never across crops. |

### 5.1 Four rules for the boxplot, one of which is a privacy rule

**A boxplot below k is a scatter plot of named farms.** Five points, an estate label, and a reader who knows the area can identify every one. The box hides nothing when n is small. **Below five fields, the box is refused in words and the total stays** — the same refusal already written for the choropleth, reused verbatim.

**Outlier dots are individual fields, and individual fields are banned on this surface.** This is the subtle one. A boxplot renders outliers as separate points by construction, and a hoverable outlier with a tooltip is a field-level drilldown through the back door — it unpicks the same rule the forecast refused a field picker to protect. Either **suppress outlier rendering** on Track C, or render them as unlabelled, unhoverable marks with a stated note that individual fields are not identified here. Do not let the chart library's default decide this.

**Never a boxplot across crops.** Two units on one axis is the tonnes-versus-kilogrammes defect drawn rather than summed.

**State n on every box.** A box over 34 fields and a box over 6 are not comparable at a glance and will be compared at a glance.

### 5.2 And the rule the map already carries

**Two charts in one card share their left padding**, and their baselines line up. The most common way a dashboard looks broken.

---

## 6 · Yield per hectare is a different question from production

Both belong on this screen, and the forecast has already built the pattern to keep them apart:

> *"A ratio of two sums — 2,053 t over 426.7 ha. The figure at the top of this screen is the sum itself, and the two answer different questions: how much is coming, and how hard the ground is working for it."*

Reuse that shape exactly. **The rule label must match the operation** — a rate is a ratio of two sums, never a mean of per-child rates, because a mean of rates is averaging an average and looks identical in the output.

**And per-hectare is not comparable across crops.** 4.81 t/ha of paddy and 18.3 t FFB/ha of oil palm are not two values of one measure. One crop at a time.

---

## 7 · What may be compared with what

Unchanged from the forecast, because they are properties of the crop, not of the screen. Every one of these has produced a wrong verdict somewhere:

- **Rice:** Main to Main, Off to Off. **Never Main to Off** — the 15–25% gap is structural. Offering it is the defect.
- **Oil palm:** production year to production year **at the same tree-age cohort**. And note for this surface specifically: a young-mature stand and a prime closed-canopy stand have different yield potential, so an estate mixing them is not underperforming, it is young. Say which.
- **Rubber:** same panel **and** same tapping cycle. The February-to-March wintering dip is expected and must not read as decline; a dip failing to recover within 8–10 weeks is a signal.
- **Pineapple:** same age from planting, never the same calendar date.

**Comparisons align by phenology week, not calendar week**, and **an invalid comparison is refused in words**, not greyed out.

---

## 8 · Refusals

Reuse the spine's set and the forecast's wording. Do not write new copy for cases that already have some.

| Case | Behaviour |
| --- | --- |
| Production total across mixed crops | Refused. The unit is the reason, at every window. |
| Boxplot over fewer than five fields | Box refused in words; the total stands and says so. |
| Cohort breakdown below k | Refused visibly; total unaffected; never silence. |
| A window not yet complete | Stated as partial, with the count harvested of the count in scope. |
| Money | Physical response only. Ringgit declined with the reason. |
| Production figure with no declared harvest | Em dash and the reason. **Never a zero** — nothing harvested and nothing reported are different facts. |

That last one matters more here than anywhere else on Track C. On a production screen a zero is a plausible reading, so the failure is silent.

---

## 9 · Copy rules

| Never show | Say instead |
| --- | --- |
| A production figure with a range | The figure. It is counted. |
| "−17%" against forecast | "260 t short of the forecast" |
| A completed-looking partial total | "…from 34 of 59 fields harvested" |
| "Confidence low" | What is missing, named |
| A combined tonnage | Per crop, with the crop and unit named |
| A mean where the spread is the point | The distribution |

Sentence case. Middot as separator. Missing data is an em dash with a reason, never a zero.

---

## 10 · Acceptance tests

1. **No production figure carries a range.** Grep for a counted figure with a spread beside it; find none.
2. Every production figure names its window, and the window is crop-shaped.
3. A part-harvested window states how many fields of how many have been harvested.
4. A mixed-crop scope refuses the production total, with the unit as the reason, at **every** window.
5. **A boxplot over fewer than five fields is refused in words** and the total stands.
6. **Outlier points identify no field** — not by label, not by tooltip, not by click.
7. Every box states its n.
8. No boxplot, histogram or bar spans two units.
9. Per-hectare figures open a trace naming **a ratio of two sums**, and it names the sum it sits beside.
10. No rate is computed as a mean of per-child rates. Seed unequal fields and confirm the two differ.
11. Bands are never averaged or collapsed; each is named with its hectares and its own stage, and an unresolved stage says so.
12. The stage spread is stated; no single stage is asserted for a mixed crop.
13. Main-to-Off is not offered for rice. Rubber requires same panel and same tapping cycle. Oil palm compares at the same tree-age cohort. Pineapple compares at the same age from planting.
14. A wintering dip is not reported as decline, **and** a dip failing to recover in 8–10 weeks still fires. Both halves.
15. Exactly one writable scope control; the breadcrumb is derived and never disagrees.
16. Narrowing recomputes the figures **and the refusals**.
17. One line above the fold states the resolved scope in words.
18. Every figure opens a trace with inputs, rule, job run, data through **and exclusions** — including an empty exclusion list that says it is empty.
19. Nothing on this screen computes a rollup. Check the call path, not the output.
20. No duplicate sentence anywhere. No underscore, enum value or field name in Product mode. No monetary axis.
21. No tooltip holds a refusal, a coverage gap, a unit or a band position. Every tooltip opens on focus and tap, not hover alone.
22. **The screenshot test.** Close every tooltip and disclosure, screenshot the screen, and confirm every conclusion a reader would draw is still supported by what is visible.
23. Dark and light pass at all three widths.
24. **Read it cold and time it.** A reader reaches "how much came off, and is it even" in about twenty seconds. Slower than the forecast by design — this screen is for someone who has already decided to look.

---

## 11 · Out of scope

- **Assistance impact.** Its own brief, and it comes last — without a price basis it ships as mostly refusals.
- **Changing a spine rule.** If one needs changing, it changes in the spine.
- **A price basis.** Still open, still refusing.
- **Field-level drilldown**, including through a chart. See §5.1.
- **Bespoke Malaysian geography.** Equal-area tiles, or a projected path from a real source.

Two carried in and still open — do not paper over them:

- The **two 17s**: "fields at serious or urgent" and "findings acted on inside their window" are different quantities sharing a value.
- **"Farm owner" in the role selector.** A persona, not a functional role, with no defined ceiling. Flagged four times now.

If a decision you need is not in this file and not resolvable from it, **stop and say "Decision required: *question*"** rather than picking a default. The mixed-crop range defect came from a brief that left a gap and a build that filled it reasonably; the reasonable fill was a 28-month window and a crushed axis.
