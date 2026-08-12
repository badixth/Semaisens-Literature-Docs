# Decision required — the agronomic min/max does not exist

**Status: live. Option D adopted and shipped; the sourcing question stays open. Corrected 13 August 2026.**

**Two claims in this file were true when written and are now wrong. Do not act on them.**

- *"VRA refuses every prescription"* — **no longer true.** Three oil palm peat rows are `sourced` and validate, and attribution (PR #5, merged) permits a rate carrying a named entitled author. A rate with **neither** a band nor an author is still refused; that part is unchanged.
- *"Still open: which functional roles may set an attributed rate"* — **closed.** Decided 11 August 2026 on `concepts/risk-model`: `agronomist`, `approver` and `estate_manager` may; `scout` may not. `Operator` was never one of the eight and is gone from the docs.

**What is still genuinely open** is the sourcing, not the mechanism: eleven crop-product rows remain `decision_required`, and filling them is blocked on documents listed under *What to chase next*. See `ACQUISITION-list.md`.

**And a live defect this file's fix created.** PR #5 amended the attribution section but not the fail-closed section above it, so `concepts/risk-model` now refuses in one place what it permits forty lines later. `guides/prescription-maps` Input validation and both advisor pages were never told either. That propagation is unwritten work, not a decision.

> **Three corrections, from the research in `RESEARCH-the-band-is-on-the-tissue.md`. Read them before using anything below.**
>
> **The title of this file is too strong.** MPOB publishes ranges for **oil palm on peat** — MOP 4.0–6.0 kg/palm/yr, urea 0.5–0.6, rock phosphate ceiling 1.0, each with a stated agronomic reason. The claim holds for mineral soils only. Landed as docs PR #6.
>
> **The rice LCC lookup was published**, in *Pakej Teknologi Padi* (2008): LCC No.3 / SPAD < 32 → 50 kg urea/ha off-season, 75 main season. DOA **removed it** from the 2022 successor. It is a trigger with a point dose, not a band — but "not published" was the wrong reason to refuse.
>
> **MPOB Bulletin 72 is reachable, not missing.** It has no text layer. OCR of that one document is the highest-value outstanding action.
>
> **And the reframe that matters most: the band exists, on the plant rather than on the bag.** Leaf tissue optimum ranges are published for oil palm with a deficient / good / excessive structure — the same nominal-is-best shape this product already uses — together with four published functions from tissue deficit to rate, three of them Malaysian. **The guardrail was validating the wrong quantity.** Options A–D below remain live, but D now covers a much smaller surface than when it was written.

Research pass on the Handoff 3 gap. **The finding is structural and it changes the shape of the guardrail, so it needs a decision rather than more searching.**

---

## What was found

**No Malaysian extension authority publishes a minimum or a maximum application rate for any crop-product pair.** DOA, RISDA and MPIB all publish **point recommendations** — one figure per application event, per stage. Not one primary document retrieved contains a min, a max, a permitted range, or a "do not exceed" figure.

The single genuine published range in the whole exercise is **ethephon on pineapple, 40–50 mL of prepared solution per plant**, from MPIB.

This matters because `guides/prescription-maps` says a rate must fall inside *"the crop-specific agronomic min/max declared in the Risk Model."* **That number is not a fact that can be looked up. It is a policy that has to be set.**

## Decision required

> **How is a band derived from a point recommendation, and who owns that derivation?**

Three shapes, and they are not equivalent:

**A · Deviation from recommendation.** Keep the sourced point and refuse beyond ±*x*% of it. Honest about what the source is, and the tolerance is visibly a platform decision rather than a borrowed fact. Needs one number chosen and defended, and the same *x* probably cannot serve urea on rice and ethephon on pineapple.

**B · An agronomist sets a min/max per row**, recorded as `status: decided_by` with a name and a date, distinct from `sourced`. Most defensible in front of an agronomist, because it does not dress a judgement as a citation. Slowest, and it needs a person.

**C · Keep failing closed.** Every band stays `decision_required`, VRA refuses every prescription. Correct, and currently shipped, but it is a guardrail that has never permitted anything — which is not yet a product.

**D · Attribution — the rate is not validated, it is signed.** Proposed 11 August after studying a working competitor. No band is invented at all: where none exists, a rate may proceed if it carries a named author who was entitled to set it, and the platform renders it as a judgement rather than as a citation. **The guardrail's question changes from *is this rate inside a band* — which we cannot answer — to *is this rate attributable to somebody entitled to set it*, which we can answer today for every crop.**

**Adopted. [PR #5](https://github.com/badixth/Semaisens-Literature-Docs/pull/5) is merged and live**, amending the fail-closed rule on `concepts/risk-model` so that *no source* and *no accountability* stop being the same condition. **A rate with neither a band nor an author is still refused.**

Four constraints are written into it so attribution cannot decay into the fabricated default this file exists to refuse:

- **Restricted products and regulatory limits are excluded.** Pesticides, high-nitrogen formulations, buffer zones and statutory maxima refuse regardless of who signs. Where the constraint is law, a name is not authority.
- **A carried-forward rate carries its origin.** Reuse is not authorship, and a figure copied map to map is how a judgement becomes a house standard nobody has read.
- **Sourced and attributed are distinguishable on the prescription itself**, not only in a settings table.
- **No fifth `status` value.** Attribution is recorded on the prescription; the band stays `decision_required` until somebody sources it.

**Why the competitor study changed the answer.** Terra Oracle runs a working VRA product and does **not** validate rates against agronomic bands — three of its four zoning algorithms are pure statistics, the fourth is driven by a soil laboratory, and the rate column has no minimum, maximum or warning. The agronomist owns the number; the platform draws zones and writes the machine file. See `RESEARCH-vra-competitor-terra-oracle.md`. **Nobody in this market has solved the band problem, because nobody has had to own it.**

**A remains the destination.** Every attributed rate is one an agronomist can later promote to `sourced` when MPOB Bulletin 72 or the DOA LCC lookup is in hand, and the audit trail will show which figures were judgement and which became citation.

**Second decision, smaller — now answered by D.** `status` offers `sourced | decision_required | undefined | agronomy_reviewed`. A judgement rate is **not** a band and does not need a fifth value; it is recorded on the prescription instead.

**Still open, and D depends on it:** which functional roles may set an attributed rate. The `prescription-maps` role matrix names Operator, Agronomist, Approver/Manager and Read-only, and **`Operator` is not one of the eight functional roles.** That mismatch must be resolved before attribution can be enforced, because the entitlement is the whole of the guardrail.

---

## What is now sourceable

Real material, with citations. Point recommendations, not bands.

### Rice — *Rice Check: Padi*, Jabatan Pertanian Malaysia, 1st ed. 2022, ISBN 978-983-047-315-4. Check Utama 5, pp. 28–30

| Product | Rate | Basis | Stage |
| --- | --- | --- | --- |
| Urea (46-0-0) | 80 kg/ha | per pass | Active tillering |
| NPK 17.5:15.5:10 or 17:20:10 | 140 kg/ha | per pass | Vegetative, 3-leaf |
| same compound | 100 kg/ha | per pass | Panicle initiation |
| NPK 17:3:25 + 2MgO | 100 kg/ha | per pass | Panicle initiation |
| NPK 17:3:25 + 2MgO | 50 kg/ha | per pass | Heading / flowering |
| MOP (60% K₂O) | 30 kg/ha | per pass | First application |
| MAP (11-52-0) | 55 kg/ha | per pass | First application |

**Two things that must travel with these figures or they will be misused.**

**The rate column is headed `Bantuan Pemberian Kerajaan`** — government assistance allocation. These are *subsidised input quantities*, not agronomic optima. Treating 140 kg/ha as a ceiling misrepresents the source, and an agronomist will know it.

**DOA defers the nitrogen rate to an instrument.** Catatan 5 directs the reader to the Leaf Colour Chart and the DOA "LCC Padi" app to determine the N rate, and the LCC-to-rate lookup is not published in the book. An N guardrail that ignores LCC is out of step with DOA's own method. **Obtaining that lookup is the highest-value next step for rice.**

Also settled: **no TSP or CIRP row for rice.** DOA's only P source is MAP. Do not fill that row. And rates do not vary by granary, by season, or by soil — only *timing* varies, by planting method and variety maturity class.

### Rubber — RISDA, *Amalan Pertanian Baik* (Pembajaan / Kadar Pembajaan) and RISDA course document, 31 Mar–1 Apr 2014

| Product | Rate | Basis |
| --- | --- | --- |
| CIRP (32% P₂O₅) | 113 g/tree into the planting hole | at planting |
| RISDA 1 — 10.7:16.6:9.5:2.4 | 75 g → 389 g/tree across months 2–39 | per pass, immature |
| RISDA 4 — 10.0:6.0:21 | 700 g at 45 mo, 800 g at 52 and 56 mo | per pass, mature |

Density: 550 trees/ha planted, ≥500 at start of tapping.

**RISDA publishes no straight urea, TSP or MOP for rubber** — the whole schedule runs on two proprietary compounds. Any straight-product rate would be a nutrient-equivalence substitution invented by us. Those rows stay undefined.

### Pineapple — MPIB / LPNM, *Pineapple Farm Management*. Peat soil; Moris, Gandul, N36, Josapine

| Product | Rate | Basis |
| --- | --- | --- |
| **Ethephon (prepared solution)** | **40–50 mL/plant** | per pass — **a real band** |
| BCN mixed, NPK 30:1:32 | 14 g/plant per pass · 42 g/plant per cycle | per pass / season |

Density 43,500 plants/ha standard. BCN composition is ammonium sulphate 72%, MOP 27%, CIRP 1%, which converts to 438.5 kg AS/ha per pass and 1,315.4 kg/ha per cycle at standard density. **Store the per-plant figure as authoritative and convert at read time** — the per-hectare number is density-dependent.

**Ethephon's active-ingredient concentration is not stated by MPIB.** The 40–50 mL is prepared solution, not formulated product, so the a.i. basis cannot be closed from this source. It needs the Malaysian pesticide registration label. Until then the one real band on the page is in the wrong unit for the schema.

### Oil palm — nothing sourced

Every row remains undefined. **MPOB's publications tree is currently returning 404** — Bulletin 72 ("Oil Palm Fertiliser Recommendation for Sabah Soils", May 2016), the TT series, and the publications index all failed. That is the single highest-value missing document and the one most likely to answer whether MPOB bands by soil group.

Every per-palm borate figure in circulation traces to a boron vendor — U.S. Borax, Borochemie, Agromate, Yara — not to MPOB. All refused.

---

## The three structural questions, answered

**Rice N — stage-specific or season total?** **Stage-specific, unambiguously**, as four discrete events with named stages and day windows. Urea appears at exactly one of them. There is no season total in the document; any total is our arithmetic. So the band schema's `by_stage` shape is right for rice — but see the LCC caveat, which is the real recommendation.

**Oil palm — by soil group or single?** **Unanswerable from MPOB while its site is down.** Soil-group differentiation is real and long-established in the Malaysian literature — the Hew & Ng (1968) nine-group schedule reproduced in later work splits inland mineral, coastal alluvial and peat with materially different rates. But every Malaysian soil-differentiated schedule actually retrieved was **points, not bands**, which is the same finding again.

**Rubber — immature vs mature inside the band?** There is no band, so the distinction lives in the rate, and it is sharp: RISDA changes both **product and rate** at the 44/45-month boundary, from a P-heavy compound at 389 g/tree to a K-heavy one at 700 g/tree. A guardrail applying one band across both stands would be **wrong by a factor of two at the boundary**.

---

## What to chase next, in order

1. **MPOB Bulletin 72** and MPOB Technology No. 13, in print or via MPOB's library. The whole oil palm section depends on it.
2. **The DOA LCC-to-N-rate lookup.** It is DOA's actual nitrogen recommendation.
3. **The Malaysian pesticide label for registered Ethrel**, to close the ethephon a.i. basis.
4. **MRB / RRIM technical recommendations**, if straight products are needed for rubber. No MRB primary document was reachable; everything surfaced for "mature rubber NPK per tree" was Indian or Sri Lankan and was excluded.
5. **LPNM's *Manual Tanaman Nanas Tanah Mineral*.** Everything sourced for pineapple is peat only.

One unreconciled figure, recorded rather than averaged away: a MARDI e-Buletin lead for MD2 pineapple (300 N / 250 P₂O₅ / 595 K₂O kg/ha) could not be retrieved, and its K₂O is roughly **double** what MPIB's BCN schedule delivers. Two Malaysian sources, one crop, a 2× divergence. Someone should reconcile it before either is trusted.

---

Until the decision above is made, **the fail-closed rule stands and VRA continues to refuse every prescription.** That is the correct behaviour and it should not be relaxed to make the feature look finished.
