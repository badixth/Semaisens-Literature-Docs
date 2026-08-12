# Proposal — putting the four standing rules into the docs

**Status: decided and half-built.** Written 13 August 2026, following `AUDIT-crop-literature.md` and the decision to patch the docs rather than withdraw the rules.

**Rule 2's open decision was answered: option A, refuse inside the wintering window.** Rules 2, 3 and 4 are built in [PR #11](https://github.com/badixth/Semaisens-Literature-Docs/pull/11). **Rule 1 (nominal-is-best) is still unbuilt** — it touches the same sections of the four ecosystem pages that [PR #10](https://github.com/badixth/Semaisens-Literature-Docs/pull/10) rewrites, so it waits for #10 to merge.

The two corrections to our own rules stand and still need patching into `CLAUDE.md`: **drop the unsourced 15–25%**, and **split panel-and-cycle into an index rule and a yield rule**. Plus the saturation carve-out on nominal-is-best when Rule 1 is built.

The audit found four rules in `CLAUDE.md` that the crop tree does not contain. Three are absent; one is contradicted. This file says exactly what I propose to write for each, what it rests on, and where I need a decision.

**It also proposes two corrections to our own rules.** Writing them down forced a precision the shorthand did not have, and in two places the shorthand is wrong.

---

## Rule 1 — Nominal-is-best

**What `CLAUDE.md` says.** *"The band is the target. Deviation in either direction is loss. A higher index is not better — paddy falls through ripening, and high there means late, not healthy."*

**What the docs say.** Nothing. Across 108 files, every alert is one-sided-low. Two above-band rules exist, both on weed pages, both unquantified, both justified by weed biomass rather than phenology.

### The problem with writing the rule as it stands

*"Deviation in either direction is loss"* is not true as stated, and if I write it into four ecosystems pages it will be wrong in a way somebody has to unpick later.

An above-band reading is not loss. **It is a question the platform currently does not ask.** And the answer differs by crop and by where in the curve the reading sits.

### What I propose to write

One sentence at the top of each ecosystems page, and one crop-specific table:

> A reading **above** its band is a finding, not the absence of one. The band is a description of what healthy looks like at this phase, not a floor.

| Crop | An above-band reading usually means |
| --- | --- |
| Paddy | Through **ripening**, the crop is late — the canopy should be falling as grain fills, and a field still at heading values at day 110 has delayed maturity, not vigour. Through the **first 30 days**, weed greenness. |
| Oil palm | On an **immature or young-mature** stand, inter-row cover crop or a wrong planting date. A closed-canopy stand cannot read meaningfully above its band — see the saturation carve-out. |
| Rubber | Inside the **wintering window**, the block has not wintered on schedule: a clone difference, a northern-state or East Malaysia shift, or a wrong planting date. Outside it, saturation. |
| Pineapple | Weed greenness, **bright-sand (BRIS)** inflation, or **silver mulch**. All three are already documented on the page; none currently fires. |

**The saturation carve-out, which the rule needs and does not have.** Above the saturation point the index does not discriminate, so an above-band reading there is not a finding either — it is an unreadable number. The rule is: *below saturation, above-band is a question; at saturation, no reading ranks against anything.* Without this, adding nominal-is-best to oil palm and rubber would generate a permanent false signal on every prime block.

**Sourcing.** The paddy ripening decline is in the docs already, in every ecosystem table (irrigated: 0.75–0.90 at heading, 0.40–0.65 at ripening). The pineapple causes are in the docs already. The oil palm and rubber cases are inference from the same tables. **No external citation.** I would write these as interpretive guidance rather than as sourced fact, which is what they are.

**No decision needed.** I think this version is right and the `CLAUDE.md` version is a compression of it.

---

## Rule 2 — Comparisons align by phenology week, not calendar week

**What `CLAUDE.md` says.** *"Comparisons align by phenology week, not calendar week."*

**What the docs say — and this is the contradiction.** `rubber-ecosystems.mdx`:

> *"It compares each pixel to the same block on **the same calendar week last year** and automatically absorbs the wintering dip."*

Repeated four times across the crop as *"same block year-over-year"*.

**The same page contains the refutation.** It documents that wintering *"is typically February to March, extending into early April in some clones and northern states (Perlis, Kedah, north Perak)"* and that *"Sabah and Sarawak run slightly later and less synchronized."* Every operational rule then uses a flat Feb–Apr with no East Malaysia adjustment.

### What actually goes wrong

Rubber is a plateau crop for eleven months of the year. **Calendar-week alignment is fine for those eleven months** — that is worth saying plainly, because the rule as written in `CLAUDE.md` implies the whole comparison model is wrong, and it is not.

It breaks in one window. If wintering ran three weeks later this year than last, a calendar-week comparison in March puts a bare block against a leafy one and reports an anomaly of −0.20 that is entirely timing. The docs claim the opposite — that the comparison *"automatically absorbs the wintering dip"* — which is true only if the dip falls in the same weeks both years, which the same page says it does not.

### What I propose to write

Replace the *"automatically absorbs"* claim with:

> Anomaly compares the block to itself on the same calendar week last year. Outside the wintering window this is sound — rubber sits on a plateau and the calendar is a good proxy for stage.
>
> **Inside the wintering window it is not.** Wintering shifts by clone, by state, and between Peninsular and East Malaysia, so the same calendar week can be bare leaf-fall one year and full canopy the next. A calendar-aligned anomaly across that boundary measures the shift in timing, not the health of the block.

And then a statement of what the platform does. **This is the decision below.**

### Decision required

Three options, and I do not think this is mine to pick.

| | What it says | Cost |
| --- | --- | --- |
| **A. Refuse** | The anomaly view states no figure for a rubber block inside its wintering window, and says why. | Cheapest, truest to the product's habit. Loses the view for roughly ten weeks a year on every rubber block. |
| **B. Anchor on observed onset** | The platform detects the block's own leaf-fall onset each year and aligns year-over-year on weeks-since-onset rather than calendar week. | Correct. Needs two years of history before it can work, and it is a build commitment we have not scoped. |
| **C. Flag, do not refuse** | The figure is shown with a standing caution for the window. | Cheapest to build. **I would argue against it** — a caution on a number that may be wholly artefact is the "silence reads as no data" failure in reverse. |

My reading: **A now, B later**, with the docs written so that B is a refinement of A rather than a contradiction of it. But it is a product decision.

---

## Rule 3 — Rice Main and Off seasons are not comparable

**What `CLAUDE.md` says.** *"Rice Main and Off seasons are not comparable. The 15–25% gap is structural. Offering that comparison is a defect."*

**What the docs say.** Nothing. The rice tree has no Malaysian season concept at all — no *main season*, no *off season*, no *musim*. Meanwhile six pages build season-over-season diagnostics without a guard: *"repeat in the same locations season after season"*, *"revisit chronic weak zones each season"*, *"target them with pre-emergence herbicide next season"*.

### Correction to our own rule — the 15–25% has no source I can find

I searched for it and could not source it. What I **can** source:

- Malaysia's agricultural seasons are formally enumerated as **annual, main, and off-season**, and paddy statistics are reported separately by season ([FEWS NET Malaysia Country Book](https://help.fews.net/fde/v3/malaysia-country-book), last updated December 2025, from *Perangkaan Agromakanan*, Ministry of Agriculture).
- Peninsular rice is planted September–October and harvested December–March; **Sarawak is planted October–November and harvested March–April** (same source). The two regions are a month apart.
- The off-season falls in the Southwest Monsoon and depends more heavily on controlled irrigation.
- Published work on the granaries finds the **technology yield gap itself differs by season** — around 1.6 t/ha in the wet season against a consistently larger ~2.2 t/ha in the dry season — and attributes the two to different causes (varietal shift in one, sub-optimal water and nitrogen in the other).

That is enough to justify the guard and **not** enough to justify "15–25%". I propose we **drop the number from `CLAUDE.md`** and keep the rule, rather than write an unsourced figure into the docs. Writing it in would be precisely the failure the audit catalogued.

### What I propose to write

A short section on `rice-ecosystems.mdx`, plus one line on each of the six pages that invite the comparison:

> Malaysian paddy runs two crops a year, and they are not the same crop. The main season and the off season differ in water regime, radiation, and how much of the water is under irrigation control, and Malaysian agricultural statistics report them separately for that reason.
>
> **Compare main against main and off against off.** A main-against-off comparison measures the difference between two seasons, not the difference between two years of management. The platform does not offer it.

Plus: the Peninsular / Sarawak one-month offset stated once, since it is the same shape as the rubber East Malaysia problem and the docs currently have neither.

---

## Rule 4 — Rubber comparison requires the same panel and the same tapping cycle

**What `CLAUDE.md` says.** *"Rubber comparison requires the same panel and the same tapping cycle."*

**What the docs say.** Nothing keys a computation to either. Panels appear only as management objects; tapping frequency is a free-text scout note.

### Correction to our own rule — this rule is about yield, not about the index

Writing it out made the problem obvious. **Panel and tapping cycle govern latex yield. They barely touch canopy greenness.** A block moved from d/2 to d/3 tapping loses latex immediately and looks identical from orbit.

So the rule as written is too broad, and if I put it on an ecosystems page next to the NDVI bands it will read as an index rule and be wrong. Two rules, not one:

- **Index comparison** on rubber is governed by wintering and phase — Rule 2.
- **Latex yield comparison** on rubber carries its panel and its tapping system, or it is not a comparison.

### What I propose to write

Not on the ecosystems page. On the diagnosis page and wherever yield is compared:

> A latex yield figure is a function of the tapping system that produced it. A block on d/2 and a block on d/3 are not comparable, and neither is one block either side of a panel change. **A rubber yield comparison names its panel and its tapping system, or it is not a comparison.**
>
> Tapping intensity is currently recorded as a free-text scout note. Until it is a field, the platform cannot enforce this and does not claim to — the comparison is offered with the panel and system shown, and the reader judges.

That last sentence matters. It is the honest position given the data model, and it is a smaller claim than refusing.

---

## Summary of what changes

| | Docs change | Also changes `CLAUDE.md` |
| --- | --- | --- |
| 1 · Nominal-is-best | Four ecosystems pages: above-band is a question, with crop-specific causes and a saturation carve-out | Yes — *"deviation in either direction is loss"* is too strong; add the saturation carve-out |
| 2 · Phenology week | Rewrite rubber's *"automatically absorbs the wintering dip"* | Yes — calendar week is sound outside the wintering window; the rule is narrower than stated |
| 3 · Main and Off | New section on `rice-ecosystems.mdx` plus a guard on six pages | Yes — **drop the unsourced 15–25%** |
| 4 · Panel and cycle | Rubber diagnosis and yield surfaces, not the ecosystems page | Yes — split into an index rule and a yield rule |

**One decision blocks the PR:** Rule 2, options A / B / C.

Everything else I can write on the reasoning above.

---

## One pattern worth naming separately

**East Malaysia is missing from both crops that need it.** Rubber wintering *"runs slightly later and less synchronized"* in Sabah and Sarawak, and no rule adjusts for it. Sarawak paddy is planted a month later than Peninsular and harvested a month later, and the rice tree has no season concept at all.

Two crops, same gap, same consequence: a Sarawak block judged against a Peninsular calendar. Worth its own defect rather than being buried inside these two rules.
