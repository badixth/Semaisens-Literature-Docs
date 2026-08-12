# Track C, Brief 3 — Regional forecast

**Status: built.** Tested through five passes; two defects still open in `DEFECTS.md`.

Self-contained. **Attach nothing.** Every fact you need is inline. If something here contradicts your memory of an earlier phase, this file wins.

Run in a **fresh chat**. Second Track C surface. It reads the spine; it adds no rules to it.

---

## 1 · The question this screen answers

**Will the yield I expect actually arrive, and where is it going wrong?**

The national overview says *something is behind*. This screen says *by how much, in what, and with how much certainty*. If a reader leaves the overview to find out whether something is wrong, the overview failed. If they leave it to find out the size of a problem they already know about, that is this screen working.

---

## 2 · The forecast is not a point, and the docs already say so

`concepts/aggregation-model` states it plainly: **"The forecast is not a single point."** Every projection carries a central estimate, a **P10 to P90 band**, and a confidence label.

The current build shows **3,178 t**. One number, four significant figures, no spread. That is a divergence from the source of truth, not a matter of taste.

**Draw the band.** `ChartBand` exists for exactly this and `CHARTS.md` gives the reason: *the spread is data, so draw it.* A planter who sees a band knows what to plan against. A planter who sees 3,178 t plans against a figure nobody stands behind.

This is the same instinct as every other refusal in the product. A platform that will not price a tonne without calibration, and takes confidence as the minimum of its inputs, should not then state a forecast as though it were counted.

**Where a band cannot be computed, say so and show no point either.** A central estimate with no spread is not a weaker answer, it is a different and false one.

---

## 3 · The horizon trap

The docs give three horizons: **14-day, 30-day, end-of-season.**

**"End of season" only means something for rice.** Oil palm and rubber have no season end — they run production years. Pineapple runs batches that never share a boundary. Offering "end of season" across four crops repeats the mixed-crop defect in the time dimension.

Per crop, the long horizon is:

| Crop | The long horizon | Reads as |
| --- | --- | --- |
| **Rice** | End of season | This season's harvest — Main or Off, named |
| **Oil palm** | End of production year | The year's FFB, with the year named |
| **Rubber** | End of production year | The year's dry rubber, with panel and tapping cycle named |
| **Pineapple** | End of batch | Per batch, at its own age — never a calendar date |

14-day and 30-day are crop-agnostic and fine as written.

**A scope spanning crops carries no long-horizon total.** Same rule as tonnage: refuse it, with the reason.

---

## 4 · The mixed-stage problem

A regional forecast aggregates fields at different growth stages. A scheme where half the fields are at booting and half at ripening is not one forecast; it is two, summed.

That sum is legitimate — production adds up — but **the confidence in it does not**, and neither does any statement about the crop's condition. Two rules follow:

- **Sum the production, never the stage.** A region has no growth stage. It has fields at stages, and a distribution of them.
- **Comparisons align by phenology week, not calendar week.** Week 8 of Main 2024 is booting and week 8 of Main 2025 is booting. Aligning by calendar compares different biological stages and is the defect that has cost this project most.

---

## 5 · What may be compared with what

Straight from the crop cycle models, and every one of these has already produced a wrong verdict somewhere:

- **Rice:** Main to Main, Off to Off. **Never Main to Off** — the 15–25% gap is structural, not agronomic. Offering that comparison at all is a defect.
- **Oil palm:** Production Year to Production Year **at the same tree-age cohort**. A block at year 7 compares to another block at year 7, not to itself last year.
- **Rubber:** Production Year to Production Year on the **same panel and the same tapping cycle**. And the February-to-March wintering dip is expected — a forecast must not read it as decline. What is a signal is a dip that fails to recover within 8–10 weeks.
- **Pineapple:** batches at the **same age from planting**, never the same calendar date.

**An invalid comparison is refused in words, not greyed out.** Same treatment as everywhere else.

---

## 6 · Confidence, stated as coverage

Confidence is the minimum of its inputs — never the mean. One weak input drags the whole answer down, deliberately.

But **"Confidence low" is a modelling word and belongs in Review mode.** Product mode states what is actually missing: *"Two schemes have not reported since 8 June, so the band is wider than usual."* Same fact, expressed as something a reader can chase.

The docs name the drivers: coverage gaps from cloud or missing imagery, missing phenology stage, stale data. Say which one, by name.

---

## 7 · Charts

Budget per `CHARTS-product-map.md`. This surface is analytical, so it gets more than the overview — but not more than it needs.

| Mark | For | Rule |
| --- | --- | --- |
| **`ChartBand`** | The forecast itself. Primary mark on the screen. | P10–P90 behind the central line. Never a point alone. |
| **`ChartLine`** | The history the forecast continues from. | One band, one line, one card. |
| **`ChartChoropleth`** | Yield or health by administrative area. | **k = 5 per cell** — a cell below k collapses into "Other". Equal-area tiles where no geometry exists; no hand-drawn Malaysian outlines. |
| **`Sparkline`** | Direction inside a scheme row. | A mark, not a chart. Value printed beside it. |

**Nothing else.** No monetary axis anywhere — no price basis exists and the resolver has not reached `farm_calibrated`.

**Two charts in one card share their left padding.** The most common way a dashboard looks broken.

---

## 8 · Refusals

Reuse the spine's set. Do not write new copy for the same cases.

| Case | Behaviour |
| --- | --- |
| Long horizon across mixed crops | Refused with the reason. Per-crop shown instead. |
| Main-to-Off comparison | Refused. Structural gap, stated. |
| Rubber across panels or tapping cycles | Refused. Not comparable. |
| Choropleth cell below k | Collapses into "Other"; the total is unaffected and says so. |
| Band cannot be computed | No point estimate either. The reason is stated. |
| Money | Physical response only. Ringgit declined. |

---

## 9 · Copy rules

| Never show | Say instead |
| --- | --- |
| "Confidence low" | What is missing — "Muda Granary North last reported 8 June" |
| A bare tonnage with no spread | The band, or the reason there is none |
| "End of season" on a perennial | The production year, or the batch |
| `P10`, `P90` | "The likely range" — the percentiles belong in the trace |
| A combined tonnage | Per crop, with the crop and the unit named |

Missing data is an em dash with a reason. Never a zero.

---

## 10 · Acceptance tests

1. **Every forecast renders as a band.** Grep for a tonnage figure with no spread beside it; find none.
2. Where no band can be computed, **no central estimate is shown either**, and the reason is stated.
3. The long horizon is named per crop — season for rice, production year for oil palm and rubber, batch for pineapple.
4. A mixed-crop scope refuses the long-horizon total, in words.
5. Main-to-Off is not offered anywhere for rice.
6. Rubber comparison requires the same panel **and** the same tapping cycle; a mismatch is refused.
7. Oil palm comparison is offered at the same tree-age cohort, not the same block year-on-year.
8. Pineapple compares at the same age from planting; no calendar-date comparison exists.
9. A February-to-March rubber dip is not reported as decline, **and a dip failing to recover within 8–10 weeks still is.** Both halves must pass.
10. Two seasons compared align by phenology week. Week 8 is the same stage in both.
11. No region carries a growth stage. Stages appear only as a distribution.
12. A choropleth cell below k collapses to "Other" and the total is unchanged.
13. Confidence never appears as a word in Product mode; what is missing is named instead.
14. No monetary axis exists.
15. Every figure opens a trace with inputs, rule, job run and exclusions.
16. Nothing on this screen computes a rollup — it reads what the job wrote. Check the call path.
17. Charts in one card share left padding and their baselines line up.
18. No Product-mode copy contains an underscore, an enum value or a field name.
19. Dark and light pass at all three widths.
20. **Read it cold and time it.** A reader should reach "which crop, how much, how certain" in about fifteen seconds. Slower than the overview by design, but not by much.

---

## 11 · Out of scope

- **Yield production and assistance impact.** Their own briefs.
- **Changing a spine rule.** If one needs changing, it changes in the spine.
- **A price basis.** Still open, still refusing.
- **Field-level drilldown.** Scope narrows by region, scheme and crop only.
- **Bespoke Malaysian geography.** Projected paths from a real source, or equal-area tiles.

If a decision you need is not in this file and not resolvable from it, **stop and say "Decision required: *question*"** rather than picking a default. The mixed-crop range defect came from a brief that left a gap and a build that filled it reasonably; the reasonable fill was a 28-month window and a crushed axis.
