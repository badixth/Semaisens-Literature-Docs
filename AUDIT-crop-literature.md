# Audit — the crop literature tree

**Status: complete. Findings 2–5 and D-L1, D-L2, D-L6 actioned in PRs #9–#12. The rest open.** First full read of `guides/Crop/` — 108 files across four crops, 12 August 2026. Three subagents, one per crop group, instructed to quote rather than summarise.

**The findings below are recorded as the audit found them and are not rewritten as they are fixed.** What has shipped:

| Finding | Fixed by |
| --- | --- |
| 2 · Nominal-is-best absent from all 108 files | [PR #12](https://github.com/badixth/Semaisens-Literature-Docs/pull/12) — with the saturation carve-out the rule needed |
| 3 · Rubber compared by calendar week | [PR #11](https://github.com/badixth/Semaisens-Literature-Docs/pull/11) — anomaly refused inside the wintering window |
| 4 · Bands collapsed on all four crops | [PR #10](https://github.com/badixth/Semaisens-Literature-Docs/pull/10) |
| 5 · "Switch to NDRE" with no destination | Not fixed — **stated as a gap** on the snippet and both perennial pages, rather than papered over. The NDRE bands still do not exist. |
| Main/Off absent from the rice tree | [PR #11](https://github.com/badixth/Semaisens-Literature-Docs/pull/11) |
| Panel and tapping cycle absent | [PR #11](https://github.com/badixth/Semaisens-Literature-Docs/pull/11), as a **yield** rule |

**Finding 1 — the tissue bands — is untouched and is now the largest open item.**

This file records what the literature layer **actually says**, against what the briefs have been assuming it says. Several standing rules in `CLAUDE.md` are not inherited from this layer. Two are contradicted by it.

**Read the first section before writing another brief.** The rest is defect inventory.

---

## The five findings that change something

### 1. The rubber and pineapple tissue bands are already in the docs — and they are already in violation

`CLAUDE.md` records tissue bands as *"Researched, not yet in the docs."* That is wrong. Five complete four-tier Whorl 2 tables ship today at `guides/Crop/Rubber/nutrient/*.mdx`:

| Nutrient | Deficient | Marginal | Optimal | High |
| --- | --- | --- | --- | --- |
| N | < 3.0 | 3.0 – 3.3 | 3.3 – 3.7 | > 3.7 |
| P | < 0.20 | 0.20 – 0.23 | 0.23 – 0.28 | > 0.28 |
| K | < 1.1 | 1.1 – 1.3 | 1.3 – 1.7 | > 1.7 |
| Mg | < 0.20 | 0.20 – 0.24 | 0.24 – 0.30 | > 0.30 |
| Ca | < 0.5 | 0.5 – 0.7 | 0.7 – 1.0 | > 1.0 |

Plus soil pairings — P confirmed by *"soil P (Bray-1) < 15 ppm"*, Mg by *"soil Mg < 0.5 me/100g"*, K:Mg *"2:1 to 3:1"*.

Pineapple has six D-leaf targets, scattered one per page: N 1.5–1.7%, K 3.0–3.5%, Mg 0.3–0.4%, Ca 0.15–0.30%, B 20–40 ppm, Zn 8–15 ppm. Fe has none.

**Two standing rules are broken by what is already published.**

- **A tissue band carries its scope or it is not a band.** None of these eleven bands carries a site, soil group, clone, cultivar, stand age or season qualifier. One global band per nutrient per crop is exactly the collapse the band programme exists to prevent.
- **A multi-nutrient diagnosis is not a set of independent verdicts.** `Rubber/field-walk-protocol.mdx` instructs *"cut Whorl 2 leaves from 20 trees across the block for tissue analysis (N, P, K, Mg, Ca)"* and `rubber-diagnosis.mdx` lays five nutrient rows side by side. There is no "name the worst, refuse the rest" rule anywhere in the corpus.

**Oil palm has zero tissue figures.** Seven pages instruct frond 17 sampling and not one states a concentration, a critical level or a unit. **Rice has zero** — and worse, `Rice/field-walk-protocol.mdx` mandates *"Take the LCC or SPAD reading here. Record the reading, not just 'pale' or 'green'."* then *"Average LCC or SPAD reading across all stops"*, with no threshold anywhere in the tree to interpret it against. The scout is asked to collect a number no page can read.

So the tissue-band track is not a greenfield. It is a **correction of two crops and a build of two**, and the correction is the urgent half.

### 2. Nominal-is-best is not in the literature layer. At all. On any crop.

I have been treating this as inherited. It is not. Across 108 files there is **no statement that the band is a target, that deviation in either direction is loss, or that a high reading is a problem.** Every alert in the corpus is one-sided-low: *below baseline*, *decline*, *drop*, *dip*, `−0.05`, `−0.10`, `−15%`.

**Exactly two above-band rules exist, both on weed pages, both unquantified:**

- `Rice/biotic/weeds/grasses.mdx` — *"trigger if NDVI is well above the expected baseline for that stage (weeds contributing to greenness)"*, and *"a field ahead of the baseline may be weedy, not vigorous."*
- `Pineapple/biotic/weeds.mdx` — *"NDVI above phase baseline in early months (weeds add greenness) then a drop when weeds are killed or the pineapple falls behind."*

Both are justified by weed biomass, never by phenology. **The ripening argument — high means late, not healthy — appears nowhere.** The rice ripening rows say only *"Decline expected as grain fills."*

The band tables are two-sided ranges, so the *shape* supports the rule. The rules do not. **Nominal-is-best is a product decision with no doc backing, and it should be recorded as one rather than cited to the literature.**

Same for rubber's tissue `High` rows: N > 3.7, K > 1.7, Mg > 0.30 are published, and no page says what High means or fires on it.

### 3. Rubber's documented comparison rule is calendar week — and the same page proves it wrong

`Rubber/rubber-ecosystems.mdx`: *"It compares each pixel to the same block on **the same calendar week last year** and automatically absorbs the wintering dip."* Repeated four times across the crop as *"same block year-over-year"*.

This contradicts the standing rule that **comparisons align by phenology week**. And the docs contain their own refutation: the same page says the wintering window shifts by clone, by northern state (*"Perlis, Kedah, north Perak"*), and that *"Sabah and Sarawak run slightly later and less synchronized."* Yet every operational rule in the crop uses a flat **Feb–Apr** with **no East Malaysia adjustment anywhere**. A Sarawak block is judged against a Peninsular window.

**And the panel-and-cycle rule is not in the docs at all.** `CLAUDE.md` states *"Rubber comparison requires the same panel and the same tapping cycle."* Panels appear only as management objects; tapping frequency appears only as a free-text scout note. Nothing keys a computation to either. That rule is ours, not the literature's.

### 4. The docs collapse bands. On all four crops. In the source of truth.

Every ecosystems page carries an at-a-glance *"Typical peak NDVI"* column and an environment-independent phase/age table. **Both are collapses**, and they disagree with the per-environment tables they summarise.

Oil palm is the clearest. Three of four at-a-glance figures match neither of the rows they summarise — they take a floor from the 15–25 row and a ceiling from the 6–15 row. Reclaimed's floor, 0.60, is the arithmetic mean of 0.58 and 0.62. And the generic stand-age table's prime row, **0.75 – 0.88, is character-identical to mineral upland's** — so the "generic" band silently *is* the mineral band, and a peat block read against it carries a floor 0.07 too high. **That is the young-mature error one axis over**, and line 28 instructs the reader to use the generic table first: *"Use the age bands below as the primary lens."*

Rubber's generic table disagrees with mineral upland at five rows of six, and both claim primacy — one is *"the primary lens"*, the other *"the baseline used by the default rubber preset"*. Nothing says which a gauge draws. Pineapple: generic disagrees with mineral upland at every row.

Oil palm also differences two bands and gets it wrong: *"Peak plateau ~0.05 lower than mineral upland"* — the actual difference is 0.07 at the floor.

### 5. "Switch to NDRE" has no destination. Four crops, zero NDRE bands.

Every crop asserts NDVI saturation and prescribes NDRE. **No NDRE band table exists anywhere in the corpus.** NDMI is not mentioned once. Consequences:

- `Oil-Palm/nutrient/nitrogen.mdx` fires *"when block-average NDRE drops below the stand-age baseline"* — there is no NDRE stand-age baseline.
- Rice states the number exactly — *"NDVI saturates above ~0.8 in every ecosystem"* — then publishes an irrigated heading band of **0.75 – 0.90**, straddling it, and nine of its ten numeric alerts are NDVI deltas, several firing in the saturated window.
- Oil palm, rubber and pineapple assert saturation **with no number at all** and key the switch to **stand age**, not to a reading. Oil palm says *"from year 6 onward, regardless of environment"* while the same page puts closure at year 5–6 on mineral and *"year 7 – 8"* on reclaimed.
- `Rubber/abiotic/drought.mdx` is the strongest statement in the corpus — *"NDVI saturates on mature rubber; use it only for immature blocks or catastrophic drought"* — and every mature-block alert in the crop is still an NDVI alert.

**The word "refuse" appears zero times in 108 files.** The corpus's strongest position is degrade-to-another-index, which is the opposite of the product's signature habit.

---

## What the corpus does have, and it is good

Thirteen citable grounds for a refusal on oil palm alone, all phrased as capability limits rather than obligations. The best of them:

- *"No reliable satellite signature at the block scale — individual deformed fronds are not detectable at Sentinel resolution."* (`Oil-Palm/nutrient/boron.mdx`)
- *"Diagnosis is a soil test and leaf analysis task; imagery is confirmatory only."* (`Oil-Palm/nutrient/phosphorus.mdx`)
- *"Standard optical imagery does not measure elevation change."* (`Oil-Palm/abiotic/peat-subsidence.mdx`)
- *"Red Ring is confirmed only by cutting the trunk."* (`Oil-Palm/biotic/diseases/red-ring.mdx`)
- *"Satellite cannot distinguish the two; ground diagnosis required."* (`Rubber/biotic/diseases/pestalotiopsis.mdx`)
- *"Do not compare pineapple NDVI directly against paddy or oil palm."* (`Pineapple/pineapple-ecosystems.mdx`) — the only cross-crop prohibition in the corpus
- *"Do not wait for NDVI to drop before acting on drought in pineapple. NDVI is a lagging indicator."*
- *"Satellites are excellent at abiotic and good at nutrient. Biotic detection needs ground truth."* (`Rice/rice-abiotic-stress.mdx`)

**Every one is written to a human reader. None names a product behaviour.** Converting these into refusals is the single highest-value piece of work this audit surfaces — the reasoning is already written and sourced-in-spirit; only the consequence is missing.

---

## Provenance: ~130 band cells, four sentences

**There is not one citation in 108 files.** No author, no year, no title, no DOI, no URL, no reference section. I grepped all four crops.

The whole layer rests on four near-identical `<Info>` blocks, one per ecosystems page:

| Crop | The entire provenance |
| --- | --- |
| Rice | *"published Sentinel-2 and Landsat rice studies and IRRI agronomy references"* |
| Oil palm | *"published Sentinel-2 and Landsat oil palm studies and MPOB agronomy references"* |
| Rubber | *"published Sentinel-2 and Landsat rubber studies and MRB / RRIM / IRRDB agronomy references"* |
| Pineapple | *"published Sentinel-2 pineapple studies and MPIB / Del Monte agronomy references"* |

No study named. No MPOB, MRB or MPIB document named. **And the five rubber tissue tables and six pineapple D-leaf targets sit on pages with no Info block at all — twenty-six numeric boundaries with zero provenance of any kind.**

Worth noting for the rice tree specifically: **no Malaysian authority appears anywhere.** No MARDI, DOA, MADA, IADA, KADA. No Malaysian variety — MR219 and MR297 are absent. No Main/Off season structure. It is generic pan-Asian IRRI agronomy in a Malaysian product.

---

## Defects, ranked

### Fix before the next build touches these surfaces

**D-L1 · Banned pesticide actives. → Fixed in [PR #9](https://github.com/badixth/Semaisens-Literature-Docs/pull/9).**

The audit found monocrotophos on two oil palm pages, described as *"low environmental impact"*. A grep of the whole corpus against the Malaysian DOA banned list found **eight recommendations across four crops, five of them with no hedge at all**:

| Page | Active | Status per DOA, 26 Aug 2025 |
| --- | --- | --- |
| Oil palm — bagworms, nettle caterpillars | Monocrotophos | Registration terminated **31 December 2025** |
| Rice — gall midge | Carbofuran | Banned 2023 |
| Rice — grass weeds, broadleaf weeds | Butachlor | Banned |
| Pineapple — thrips | Methomyl | Banned **1974** |
| Pineapple — symphylids | Chlorpyrifos | Agricultural use withdrawn 2023; registration ended 1 July 2026 |
| Pineapple — weeds | Paraquat | Banned 2020 |

Source: [Senarai racun makhluk perosak yang telah diharamkan dan dihadkan penggunaan](https://www.doa.gov.my/doa/resources/aktiviti_sumber/sumber_awam/maklumat_racun_perosak/pendaftaran_rmp/senarai_racun_perosak_haram_terhad_ogos2025.pdf), Bahagian Kawalan Racun Perosak dan Baja, Jabatan Pertanian.

PR #9 replaces each with a registered alternative, names the ban rather than deleting silently, and adds `snippets/pesticide-registration` carrying the rule once: **a named active is a description of practice, not a clearance to use it**, and the platform does not check registration status.

**This is also the corpus's first real citation.** Two DOA sources now appear in the docs. Before PR #9 there were none in 108 files.

**D-L2 · The collapsed tables. → Fixed in [PR #10](https://github.com/badixth/Semaisens-Literature-Docs/pull/10).** The NDVI numbers are removed from the at-a-glance column on all four crops and from the environment-independent table on oil palm, rubber and pineapple. The per-environment tables are untouched — **no band a gauge draws has changed.** New snippet `snippets/band-resolution` carries the rule once. Also resolves **D-L6** (the `> 25 years` gap now says so on the page) and pineapple's double-modelled ratoon row, and corrects the wrong "~0.05 lower than mineral upland" arithmetic.

**D-L3 · Rubber tissue bands carry no scope.** Eleven bands, four crops' worth of consequence, no qualifier. Either scope them or mark them `decision_required` as the rate bands are.

**D-L4 · The multi-nutrient side-by-side.** Both rubber pages that render five verdicts at once need the "name the worst, refuse the rest" rule.

### Structural, will produce a wrong number

**D-L5 · Rubber's wintering trough sits entirely below its own prime band.** Trough 0.55 – 0.70; prime tapped 0.78 – 0.90. For six-plus weeks a year a healthy block reads out-of-band **by design**, and nothing in the docs suppresses the band during that window.

**D-L6 · Senescent rows missing from every environment table.** Oil palm `> 25 years` and rubber `> 25 years` exist only in the collapsed generic table. A 27-year-old peat block resolves to no band in its own environment.

**D-L7 · Rubber peat band promised and absent.** Frontmatter advertises *"mineral upland, hilly / sloping, coastal lowland, and peat environments"*. There is no peat table, no peat row, no peat option in the picker — only a prose instruction to expect *"peak NDVI ~0.05 lower"*.

**D-L8 · Clones and cultivars named in alert rules that are not selectable.** `RRIM 2005` (Corynespora), `RRIM 600` (powdery mildew, TPD), `Perola`, `Smooth Cayenne`, `Cayenne`. Each has a *"Susceptible-clone alert"* keyed to a flag that cannot be set.

**D-L9 · Threshold units mixed four ways within single crops.** Absolute deltas (`−0.08`/`−0.10`/`−0.15`/`−0.20`), percent-of-baseline (`5% below`), one standard-deviation-below-mean, and unquantified *"below your baseline"* under four undefined baseline names (dry-baseline, fertile-field, fertile-strip, ecosystem). A 5% shortfall on a pineapple establishment band (0.12) is 0.006; on fruit development (0.75) it is 0.0375. Not the same rule.

**D-L10 · One 0.10 delta serves five causes on oil palm** — bud rot, fusarium, ganoderma, bagworms, nettle caterpillars — and the corpus admits once that it cannot discriminate: *"same as basal Ganoderma. The satellite cannot distinguish the two."* It is also absolute, not band-relative: a 0.10 drop from 0.88 stays inside the mineral prime band; from 0.68 it exits the peat band.

**D-L11 · Three rice pages claim a frontmatter `rule_card` that does not exist.** `rice-drought.mdx`, `blast.mdx`, `bacterial-leaf-blight.mdx` each assert a machine-readable contract powering Risk Monitoring, Activity & Alerts and *Ask the advisor*. Every file's frontmatter contains only `title`, `sidebarTitle`, `description`.

**D-L12 · Incompatible stage vocabularies, every crop.** Oil palm bands run `3–6 / 6–15 / 15–25`; the diagnosis page runs `3–8 / 8–18 / 18+`; the field-walk protocol follows diagnosis. A 16-year-old block is *late mature* on one page and *peak yield* on another, and `potassium.mdx` says *"K demand peaks during peak yield (8 to 18 years)"* then points at a band page with no 8–18 band. Rice has three vocabularies across three pages; a scout's recorded stage cannot be joined to a band. Pineapple's diagnosis page uses lowland months against a highland table.

**D-L13 · Rice submergence fires exactly where the ecosystems page forbids it.** Ecosystems: *"Sudden NDVI drops on flood-prone fields often reflect a flood pulse, not crop damage. Cross-check with the NDWI layer and recent rainfall before flagging."* Submergence: *"trigger on a drop of 0.20 or more between consecutive passes in flood-prone fields"* — no NDWI condition.

**D-L14 · Rice K-deficiency signal is indistinguishable from the expected band behaviour.** `potassium.mdx` fires on *"Later-season NDVI decline while the crop still has weeks to grain-fill"*; the ecosystems page says of that window *"Decline expected as grain fills."* Nothing separates them.

**D-L15 · Main/Off season non-comparability is absent from the rice tree**, while six pages build season-over-season diagnostics (*"repeat in the same locations season after season"*, *"revisit chronic weak zones each season"*). The 15–25% structural gap appears nowhere. This is an open invitation to a defect we have already named.

### Smaller, still worth a pass

- Five oil palm drought lag windows on one page: 24–30, 12–24, 6–18, 12–18, 6–12 months.
- Oil palm nettle caterpillar defoliation: *"20 to 30%"* on line 13, *"30 to 50%"* on line 33.
- Peat water table 50–70 cm on five pages, 40–60 cm on `acid-sulfate.mdx`, no precedence rule.
- Ganoderma census keyed to 8 years while the same page puts replant onset at *"4 to 6 years"*.
- Ganoderma satellite signature described as *"ring-shaped"* on the ecosystems page and as unstructured clustering on its own page.
- The K/Mg discriminator stated three ways on oil palm; two of them (frond position, soil type) fail on the pages' own evidence — only spot-vs-whole-frond morphology works.
- Mn-toxicity vs Mg-deficiency separated *"by soil pH (Mn toxicity below pH 4.5)"* while Mg availability *"drops sharply below pH 4.5"* — the discriminator is null in its operating range.
- Coastal alluvial defined as high-pH on `iron.mdx` and pH 3.5 on `acid-sulfate.mdx`, one band for both.
- Rubber wintering duration: 4–8 / ~8 / 6–8 / 6–10 / 6–12 weeks across five pages; failure threshold 8–10 or 10.
- Rubber failure-to-recover attributed to Corynespora, when powdery mildew and Pestalotiopsis claim the same signal and one page says *"Satellite cannot distinguish the two."*
- Rubber opening age: year 5, 5–6, 6–7, 7–8 across four pages. P page's *"< 5 cm/year"* girth flag cannot reach the 50 cm tapping threshold on schedule.
- Same trigger, two severities: latex −15% is Medium on `potassium.mdx`, High on `tpd.mdx`.
- Rubber rates are per tree; lime and seed rates are per hectare; the only stand density (400–500 trees/ha) lives on a third page.
- Pineapple ratoon modelled as both a phase row and an offset, giving different bands; the block settings step invokes both.
- Pineapple MD2 harvests at 14–18 months while the generic table calls 14–18 *fruit development* — the generic curve is a Sarawak/Moris curve wearing a universal label.
- Pineapple black rot storage *"7 to 10 C"* overlaps the chilling-injury zone (*"keep fruit above 8 C"*).
- Rice direct-seeding offset stated as 5–10 days and 7–10 days in one file.
- Rice tungro alert runs days 21–45; the same page says infection *"within the first 30 days"* is what matters.
- Broken link: `rice-abiotic-stress.mdx` points at `/guides/Crop/rice-ecosystems`, missing `/Rice`.
- Oil palm dangling references: no calcium page, no zinc page, and *"Crown Disease"* named as a differential five times with no page.
- Three oil palm disease pages are for regions the platform does not serve — *"Not present in Southeast Asia or Africa"*, *"Rare in Southeast Asia"* ×2 — and all three carry 0.10 cluster alerts that would fire on other causes here.

---

## What this does not change

The paddy index bands the prototype draws against are correct and match `rice-ecosystems.mdx` exactly. That was verified before this audit and holds after it.

The rate-band position is unchanged and reinforced. The corpus adds fourteen product rates across the crops — MOP 2–4 kg/palm/yr, kieserite 1–2 kg/palm/yr, borax 50–200 g/palm/yr, urea 100–400 g/tree/yr, MOP 500–900 kg/ha/cycle and so on — and **every one is a point recommendation with a calibrate-elsewhere qualifier**, none keyed on soil group, none with a stated harm above the ceiling. `DECISION-rate-bands-do-not-exist.md` stands.

**One exception worth recording.** `Pineapple/nutrient/boron.mdx` is the only place in 108 files with the shape of a rate band: *"Broadcast borax 5 to 15 kg/ha, incorporated. **Do not exceed 20 kg/ha; B toxicity is a real risk.**"* A range, a hard ceiling, and a named harm. It is uncited. It is one row.

Note the mirror on oil palm, which is worse: `boron.mdx` gives *"50 to 200 g per palm per year"* — a 4× range — on a page that says *"the toxicity window is narrow"* and *"Do not apply B annually as a routine."* A 4× envelope and a narrow toxicity window cannot both be true.
