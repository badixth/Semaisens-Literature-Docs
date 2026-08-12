# Methodology — Assistance impact

**Status: superseded as a build target, live as reference.** `concepts/effect-methodology` is the source of truth; this file is the fuller methodology it was drawn from and still carries detail the page does not.

How Semai measures whether an intervention worked, what it will claim, and — more importantly — when it will refuse to claim anything.

Written against the Mintlify docs repo at this revision. Amendment A (band resolver) and Amendment B (season scope) are assumed. Items are marked **[found]** where the literature supplies them and **[decided]** where this document does.

---

## 1 · The boundary — what Semai can and cannot observe

**A subsidy is not an intervention Semai can see.** It is a transfer between an agency and a farmer, outside the platform. What Semai observes is the **operation the subsidy paid for**.

```
agency disburses  →  operation happens  →  field responds
   (not observed)      (verifiable today)     (measurable)
```

Semai measures the **second arrow**. The agency must supply the first link — which operation its money paid for. Without that link the platform can say a field responded to an application, and cannot say a scheme worked.

**[decided]** This boundary is stated on every agency-facing surface. A platform that implies it can trace ringgit to yield, when it cannot see the ringgit, is making the kind of confident wrong claim that costs more than an honest gap.

### What qualifies as a measurable intervention

| Intervention | Measurable? | Why |
| --- | --- | --- |
| **Applied application plan** (`map.applied`) | **Yes** | Logged operation, bounded area, dated, already cross-checked against imagery **[found]** `guides/prescription-maps.mdx` |
| **Completed field check that changed a decision** | **Partial** | The check is logged; "changed a decision" is inferential. Measurable only where a plan or severity change followed within the window |
| **Irrigation, drainage, manual operation** | **Only if logged** | No entity exists today. Verifiable once logged as a source event |
| **Subsidy disbursement** | **No, not directly** | Outside the platform. Measurable only through the operation it funded, if the agency links them |
| **Advice given but not acted on** | **No** | Nothing happened; nothing to measure |

The honest core is `map.applied`. Everything else is an extension of it.

---

## 2 · What counts as effect

Five candidate measures, fastest to slowest. Each supports a different claim, and the difference matters.

| Measure | Horizon | Supports the claim | Does **not** support |
| --- | --- | --- | --- |
| **Band recovery** — did deviation from band close? | 1–3 clear passes | *"The field responded."* | *"Yield was saved."* |
| **Days to recovery** | Same | Comparing interventions of the same type | Cross-type comparison |
| **Trajectory change** — did the slope change after? | 3+ passes | *"The decline stopped."* More robust than a single reading | Attribution on its own |
| **Avoided loss** vs the rule card's yield-impact curve | Immediate | *"Modelled loss was averted."* | A monetary claim, unless the curve is farm-calibrated |
| **Yield delta** | One full cycle | *"Yield differed by X."* The real answer | Anything, without a yield map |

**[decided] Avoided loss is the weakest measure and needs a hard gate.** It is a counterfactual — it asserts a loss that never happened — computed against a curve whose own provenance may be `literature_v0`, which `concepts/risk-model.mdx` flags for annual agronomy review. Claiming averted ringgit against an unvalidated curve is precisely the impressive-unfounded claim to avoid.

**The gate:** avoided loss may be reported **only** where the yield-impact layer resolved at `farm_calibrated` — the curve has been tuned on this farm's own history. At `literature_v0`, `by_ecosystem` or `by_region`, the platform reports the response and declines the monetary figure.

**[found]** Yield delta is gated by the existing threshold in `guides/seasonal-analysis.mdx`: Pearson R² **above 0.6** means NDVI is a reliable proxy; **below 0.4** means non-canopy factors dominate. Below 0.4, an index-derived yield claim is not defensible on that field.

---

## 3 · The baseline — the central question

### Why before-and-after is usually wrong, and why here it is not

The obvious objection to same-field before-and-after is that **the band moves anyway**. A paddy field reading higher at week 12 than week 8 has not recovered; it has grown. Raw index before and after is confounded by growth stage and is worthless.

**Amendment A dissolves this.** Measure **deviation from band**, not the index. Deviation is stage-normalised by construction — the resolver has already accounted for crop, ecosystem, stage, variety and practice. A field 0.10 below band at booting and 0.02 below band at heading has genuinely improved, and the numbers say so directly.

**[decided] Primary baseline: deviation-from-band trajectory, same field, before and after.** Available on every field immediately, and honest because the band model makes it so.

### Fallback and corroboration

**Fallback — matched cohort. [decided]** Comparable fields that received nothing over the same window. Amendment A already supplies the matching keys: same crop, same ecosystem, same stage window, same region.

**The failure mode is selection bias, and it runs one way.** Fields that received an intervention were usually the worse ones — that is why they got it. A naive cohort comparison therefore **understates** the effect. Cohorts must be matched on **pre-intervention deviation**, not on crop and ecosystem alone.

**Corroboration — prior comparable cycle. [decided]** The strongest evidence and the rarest. Amendment B's comparability rules bind: Main against Main for paddy, same tree-age cohort for oil palm, same panel and tapping cycle for rubber, same age from planting for pineapple. Most fields have one cycle. Use it where it exists; never require it.

---

## 4 · Attribution windows, per crop

**[decided]**, grounded in the crop literature.

| Crop | Window | Reason |
| --- | --- | --- |
| **Paddy** | 2–3 clear passes, capped at the stage boundary or 30 days, whichever is shorter | Stages run in days. Past the boundary the crop has moved on and attribution collapses |
| **Oil palm** | 4–8 weeks | **[found]** NDVI lags water stress by 2–4 weeks on closed canopies; nutrient effects surface on NDRE first. No stage cap — the stage does not change inside a fortnight |
| **Rubber** | 4–8 weeks, **excluding February–April** unless a prior-year baseline exists | **[found]** Wintering drops NDVI 0.10–0.25 and recovers within 8 weeks. Any window overlapping it will read decline as failure and recovery as success, both spuriously |
| **Pineapple** | 6–10 weeks, and **measure on NDWI, not NDVI** | **[found]** CAM physiology delays the NDVI response to drought; NDWI and CWSI lead by 2–3 weeks |

**Two interventions inside one window cannot be separately attributed.** Report the combined response and say so.

---

## 5 · Confounders that must be controlled

| Confounder | Control |
| --- | --- |
| **Growth stage** | Controlled by construction — deviation from band, not index |
| **Ecosystem, variety, practice** | Controlled by the resolver's layers |
| **Weather** | Not controlled. Rainfall over the window must be reported beside any claim; a field that recovered during a rain event may have recovered regardless **[found]** `guides/seasonal-analysis.mdx` rainfall overlay |
| **Saturation** | **A hard blocker.** Above ~0.80 NDVI stops discriminating, so a closed oil palm or rubber canopy cannot show improvement on NDVI at all. Measure on NDRE or decline |
| **Wintering** | Excluded by window rule above |
| **CAM lag** | Index switched to NDWI for pineapple |
| **Soil background, water table** | Where the resolver flags the distortion, the claim carries it |
| **Cloud gaps** | Fewer clear passes widens confidence and, below the minimum, blocks the claim |

**Saturation deserves emphasis.** It means the two perennial crops cannot demonstrate effect on their default index at their most valuable stage. Any impact reporting on prime oil palm or tapped rubber must run on NDRE, and a scheme report that silently used NDVI there is measuring nothing.

---

## 6 · When no claim can be made

**This is the most important section.** The platform's habit is to refuse rather than mislead — an em dash where a figure would be incomparable, a disabled control that explains itself, a proof pack that says *"we could not confirm this."* Impact inherits that habit.

**No claim is made when any of these hold:**

| Condition | What is shown instead |
| --- | --- |
| Fewer than **2 clear passes before and 2 after** | *"Not enough clear imagery in the window to tell. Next capture 19 Jun."* |
| The index is **saturated** at this stage | *"NDVI cannot separate change on a closed canopy. Measured on NDRE."* — or declined if NDRE is unavailable |
| A **recurring window** (wintering) overlaps and no prior-year baseline exists | *"February to April on rubber is leaf fall. We cannot separate the treatment from the season this year."* |
| The resolver flags an **active distortion** | The distortion is named and the claim is withheld or downgraded |
| Band resolved at **`crop_default`** | *"No ecosystem is set for this field, so 'expected' is a guess. Set it and we can measure this properly."* |
| **R² below 0.4** on a yield claim | *"Canopy did not predict yield on this field. A yield figure here would not mean what it appears to."* |
| Cohort smaller than **k = 5** | Collapsed to "Other" **[found]** `concepts/aggregation-model.mdx` |
| **Two interventions overlap** in the window | Combined response reported; neither attributed |
| Yield-impact layer is **not `farm_calibrated`** | Response reported; monetary avoided-loss withheld |

**[decided]** A withheld claim is displayed, never hidden. The same rule the aggregation model applies to stale rollups: *"Stale rollups are always visible; hiding them would let a stakeholder mistake old data for the current state."*

---

## 7 · How it aggregates

**No new aggregation category is needed.** The five in `concepts/aggregation-model.mdx` cover it.

**Response rate is a Rate. [found]** *"Sum(numerator events) / Sum(denominator events)"* — the same shape as the existing *"response rate to alerts"* and *"verification acceptance rate"*.

**Magnitudes are Intensive and area-weighted.** Mean deviation closed, days to recovery. *"Sum(metric × area) / Sum(area)"*, never a mean of means.

**The crop rule, and the way out.** Effect *magnitude* on paddy at ripening and oil palm at prime are different quantities and must never be averaged together — the same error as a cross-crop index average, which this product already refuses to compute. But a **response rate is dimensionless**: *did it respond, yes or no*. Rates aggregate across crops; magnitudes do not.

**[decided]** Therefore the scheme-level headline is a **rate**, not a magnitude. Per-crop magnitudes sit beneath it.

**Confidence is the minimum of its inputs [found]**, and inherits every existing degradation factor — coverage gaps, rule-card version drift, missing phenology, variety mix unknown — plus one new one: **the share of contributing fields whose bands resolved below `by_ecosystem`**. A scheme measured mostly on fields with no ecosystem set is a low-confidence scheme, and should say so.

---

## 8 · What the agency sees

Three claim tiers, each with its evidence, plus the honest fourth.

**Tier 1 · Responded.** The field's deviation from band closed within the attribution window.
*Evidence:* deviation before and after, passes used, rainfall over the window, distortions checked and cleared.

**Tier 2 · Likely helped.** It responded, and a matched cohort — matched on pre-intervention deviation — did not.
*Evidence:* cohort definition, size, matching keys, and the cohort's own trajectory.

**Tier 3 · Yield effect measured.** A completed cycle with an uploaded yield map and R² ≥ 0.6.
*Evidence:* the scatter, the residual map, and the reconciliation against forecast **[found]** `guides/estate-group.mdx`.

**Tier 0 · Cannot tell yet**, with the reason from §6 and what would change it.

**The question an agency actually asks** is not *"how many ringgit did we save"* — it is *"where should the next round go."* That is answered by **response rate sliced by ecosystem, region and crop**: where interventions of this type are responding, and where they are not. A rate is defensible on far less evidence than a monetary figure, and it is the more actionable answer.

**[decided]** Semai does not publish a ringgit-per-ringgit return figure. It publishes response rates, per-crop magnitudes, and yield effects where a yield map exists — and names what it could not measure.

---

## 9 · Open questions

1. What links a subsidy to the operation it funded — an agency-supplied reference on the source event, or a separate reconciliation import?
2. Should a **failed** intervention — applied correctly, no response — feed `farm_history`, or only successful ones? Excluding failures would bias the calibration optimistically.
3. Does an intervention on one block count as an intervention on its field for cohort purposes?
4. Where a field receives the same intervention every cycle, is the baseline the previous cycle or the untreated cohort? The two answer different questions.
5. What is the minimum cohort size for a defensible Tier 2 claim, given k = 5 is a privacy floor rather than a statistical one?
6. Should an intervention that prevented a decline — no visible improvement, but the cohort declined — count as a response? This is the hardest case and the most common in a disease season.

---

## 10 · Docs delta

**Nothing in the corpus connects an operation to its outcome.** `concepts/verification-model.mdx` ends at *"an operation of the reported type occurred on the reported parcel within the imagery window."* It states plainly what it does not prove — identity, product, off-parcel activity — but does not name the larger gap: it never asks whether the operation worked.

**`farm_history` is defined as an input and never as an output.** `concepts/risk-model.mdx` describes it as *"auto-tuned by the platform after 1–2 seasons"* and `guides/estate-group.mdx` says reconciliation *"feeds the farm_history layer"* — but no page defines what is written, when, or by what rule. Impact measurement depends on that mechanism and it is undocumented.

**No entity for a non-VRA field operation.** Irrigation, drainage and manual applications are named in the crop literature as mitigations, but only `map.applied` is loggable. The most common real-world interventions on smallholder paddy cannot be recorded, let alone measured.

**R² thresholds appear once and are load-bearing.** The 0.6 and 0.4 figures in `guides/seasonal-analysis.mdx` are stated in a table cell and cited nowhere else, yet they are the only documented test of whether an index-derived yield claim is defensible.

**Yield map upload is documented but has no entity.** *"Field Settings > Data Uploads > Yield Map"* exists in `guides/seasonal-analysis.mdx`; Layer 1 lists it under open questions with no schema and no owner. Tier 3 claims are impossible until it has one.
