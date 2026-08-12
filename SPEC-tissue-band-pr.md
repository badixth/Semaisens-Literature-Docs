# Spec — the tissue band PR

**Status: written, not pushed. The Mintlify connector was down when this was staged, 13 August 2026.** Push as-is when it returns; nothing here needs re-deciding.

Implements the decision of 13 August: **caveat the unscoped tissue bands, and refuse a verdict near a boundary.** Plus the multi-nutrient validity rule from `RESEARCH-the-band-is-on-the-tissue.md` §7.1, which was settled and unbuilt.

---

## Why "near a boundary" is not a percentage

The obvious implementation is a margin — refuse within 10% of a tier edge. **It does not survive contact with the evidence.**

Foster's site-computed oil palm K band is **0.77 – 0.86%**. The generic mature band is **0.90 – 1.20%**. The lower bounds differ by 0.13 on a generic tier 0.30 wide — a **43% disagreement**. Any margin small enough to be useful is far too small to catch the case that motivated the rule.

So the rule is not a margin. **It uses the structure the tables already have.**

### Rubber — four tiers, and two of them are not verdicts

Every rubber table has `Deficient · Marginal · Optimal · High`. The **Marginal** tier already exists because the transition is not sharp — it is the table's own admission of the uncertainty. On an unscoped band it cannot carry a verdict.

| Tier | Resolves? | Why |
| --- | --- | --- |
| Deficient | **Yes** | Well below any plausible site-calibrated floor. |
| Marginal | **No** | This is the boundary zone by construction. The platform returns the value and the tier, and states that a generic band cannot grade it. |
| Optimal | **Yes** | |
| High | **No** | Not because of scope — because **no page in the corpus says what High means or what to do about it.** Publishing a rating with no consequence is not a verdict. |

### Pineapple — a single target, so no grading at all

Pineapple gives one target range per nutrient and no tiers. A range without tiers supports **inside or outside**, not *deficient*. The platform states the value and whether it falls in the generic target, and does not grade the distance. A value marginally outside an unscoped target is not a deficiency finding.

---

## New snippet — `snippets/tissue-diagnosis.mdx`

Frontmatter: `title: "Reading a Tissue Analysis"` · `sidebarTitle: "Tissue Diagnosis"` · `hidden: true` · description: *"How the platform reads a leaf tissue result against a generic band, why only one nutrient verdict is valid at a time, and where it refuses to grade."*

Body:

> A **tissue band** is a range on a laboratory measurement of the plant — % N in a rubber Whorl 2 leaf, % K in a pineapple D-leaf. It is not an index band and not a rate band. It fires when a *laboratory* reports a number.
>
> ## Only one nutrient verdict is valid at a time
>
> A lab report returns every nutrient it was asked for. **That is not a set of independent verdicts.**
>
> Critical-level foliar diagnosis is calibrated near the optimum. Once a plant is short of more than one nutrient, the nutrients interact and the individual ratings stop meaning what they mean in isolation. Established for oil palm by Foster (2003, via Goh / AAR): *"if any nutrient is found to be very deficient, or more than one nutrient is deficient, then the deficiency rating of only the most deficient nutrient is considered to be valid… This implies that the system only works if the nutritional state of the palm is near the optimum."* The reasoning is a property of critical-level systems generally, not of oil palm, so it applies to the rubber Whorl 2 and pineapple D-leaf tables on the same terms.
>
> **The platform names the single most deficient nutrient and refuses the rest, with the reason.** It does not render five verdicts side by side. A refused verdict is stated, not hidden — the value is still shown, and the reason for withholding the rating is shown with it.
>
> ## These bands are generic, and are not site-calibrated
>
> Every tissue band in this documentation is a **generic range for the crop.** None carries a site, soil group, clone, cultivar, stand age, season or sampling-method qualifier, because no published Malaysian source supplies one.
>
> That is a real limit, not a formality. Optimum concentration varies with all of those things. Foster's site-computed oil palm K band is 0.77 – 0.86% where the generic mature band is 0.90 – 1.20% — **a leaf at 0.88% reads deficient on one and high on the other.** The two disagree by design.
>
> ## So the platform refuses to grade the middle
>
> A generic band supports a verdict where the value is unambiguous, and not where it is close to a line the band cannot place precisely.
>
> - **On rubber**, `Deficient` and `Optimal` resolve. **`Marginal` does not** — that tier exists because the transition is not sharp, and on a generic band it cannot be read as a rating. **`High` does not either**, because no page states what High means or what to do about it.
> - **On pineapple**, the targets are single ranges with no tiers, so they support *inside* or *outside* the generic target and nothing finer. The platform does not grade how far outside.
>
> Where a rating is refused, the measured value and the band are both shown. **The refusal is a statement about the band, not about the sample.**
>
> ## What would lift these limits
>
> A tissue band carries its scope or it is not a band. Scoping these would need the source tables they descend from — for rubber, most likely the RRIM clone-group tables, which key the band to clone group. Until those are in hand the generic ranges stay, marked as generic.
>
> ## Related
>
> - [How a band resolves](/snippets/band-resolution)
> - [Rubber Diagnosis](/guides/Crop/Rubber/rubber-diagnosis)
> - [Pineapple Diagnosis](/guides/Crop/Pineapple/pineapple-diagnosis)

---

## Per-page edits

### The five rubber nutrient pages

`nutrient/nitrogen.mdx`, `phosphorus.mdx`, `potassium.mdx`, `magnesium.mdx`, `calcium.mdx` — insert immediately **after** each four-tier table:

> <Warning>
>   This is a **generic band for the crop**, with no site, soil, clone, age or season qualifier. Optimum concentration varies with all of those, so `Marginal` is not a verdict and `High` has no stated consequence — the platform reports the value and refuses the rating in both tiers. And where more than one nutrient reads deficient, **only the most deficient rating is valid.** See [Reading a tissue analysis](/snippets/tissue-diagnosis).
> </Warning>

### The six pineapple nutrient pages

`nitrogen.mdx`, `potassium.mdx`, `magnesium.mdx`, `calcium.mdx`, `boron.mdx`, `zinc.mdx` — append to each `**D-leaf analysis**` bullet:

> This is a generic target with no site, soil, cultivar or season qualifier, so it supports *inside* or *outside* and no finer grade — see [Reading a tissue analysis](/snippets/tissue-diagnosis).

### `Rubber/field-walk-protocol.mdx`

At the Whorl 2 sampling instruction (*"cut Whorl 2 leaves from 20 trees across the block for tissue analysis (N, P, K, Mg, Ca)"*), append:

> Sample all five together — but read them one at a time. Where more than one reads deficient, **only the most deficient rating is valid** and the others are refused. See [Reading a tissue analysis](/snippets/tissue-diagnosis).

### `Rubber/rubber-diagnosis.mdx` §2

After *"This is the reference for nutrient sampling."*:

> The table below moves from a **visible symptom** to a likely nutrient, one symptom at a time. It is a decision tree, not a verdict set. When a laboratory result comes back covering several nutrients at once, a different rule applies: **name the most deficient and refuse the rest.** See [Reading a tissue analysis](/snippets/tissue-diagnosis).

### `Pineapple/field-walk-protocol.mdx` and `pineapple-diagnosis.mdx`

Same one-line pointer at the D-leaf definition.

---

## Not in this PR

**Oil palm and rice still have no tissue bands at all.** Oil palm instructs frond 17 sampling on seven pages and states no concentration anywhere. Rice mandates recording an LCC or SPAD reading at every scout stop, and averaging it across the field, with **no threshold anywhere in the tree to read it against** — the scout is asked to collect a number no page can interpret.

Those are builds, not corrections, and they are the other half of the tissue track. The rice LCC gap is the more urgent of the two because the workflow already demands the reading.
