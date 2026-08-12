# The base model for nutrient bands

**Status: research complete, and it answers the ingestion question.** 11 August 2026. Companion to `RESEARCH-the-band-is-on-the-tissue.md`, which established that the band sits on the plant rather than on the bag. This file answers the two questions that were left open: **can our own sensors reach that band**, and **does the pattern generalise past oil palm**.

**Both answers change the design.** The first is a firm no, with five independent reasons and three of them physical. The second is a partial yes with a schema consequence — the key we assumed is wrong for three of the four crops.

---

## 1 · Satellite cannot place a reading inside a tissue band. Build ingestion.

This is a refusal, not a deferral. More ground samples and better models do not fix three of these five.

**1 · The sensor cannot see the tissue the band is defined on.** Frond 17 sits deep in the phyllotactic spiral, shaded and interior. The remote-sensing literature itself prefers **frond 9** as more appropriate for mature palm — so the frond whose band is published is not the frond a sensor can see. Pineapple is worse: the diagnostic tissue is the **basal white portion** of the D leaf, which is neither green nor at the canopy surface. **This is a field-of-view mismatch, not a modelling problem.**

**2 · The signal is not in our bands.** N-specific absorption sits at **1325–1575 nm**, outside every UAV multispectral camera and not covered by Sentinel-2's red edge. And for P, K, Mg, Ca and the micronutrients there is **no absorption feature at all in 400–2500 nm** — every published model for them is a borrowed correlation with chlorophyll or water, and that borrowed correlation is itself cultivar- and site-specific. **A red-edge index measures chlorophyll. Calling its output nitrogen is target-variable substitution built into the physics.**

**3 · Our canopies defeat the measurement, in exactly the way our own rules predict.** Prime oil palm and tapped rubber are saturated. At 9 m triangular spacing a 20 m Sentinel-2 pixel holds about **5.7 palms plus uncovered inter-row** — soil, frond piles, cover crop, harvest paths. So at prime closed canopy the palm component is saturated and **the only part of the pixel still varying is the contaminant.** We already refuse effect measurement on saturated readings; this is the same refusal one layer deeper.

**4 · The error is larger than the band.** The frond-17 optimum N band is **0.40 percentage points wide**. The best genuine cross-site canopy RMSE anywhere in the literature — in rice, which is far better served than our perennials — is **0.46 %N**. The largest oil palm study (SAR, n = 1,116) implies about **0.24 %N**, which is 60% of the band and 2.4× either marginal band.

**5 · Nothing transfers.** Published central tendency is R² ≈ 0.81. Every genuine cross-site, cross-season or cross-cultivar test lands at **0.03 – 0.35**. One maize study went 0.95 train → 0.47 test → **0.20 across cultivar**. A grassland study showed the *model you would choose* flips depending on the validation scheme. And no oil palm satellite-to-leaf-N study has ever been tested across a site or a season — the largest has **36 sub-pixel plots at one estate, split randomly thirty times**.

### Two consequences specific to this product

**Our own multi-nutrient rule removes the diagnosis entirely.** If only the rating of the most deficient nutrient is valid, and P, K, Mg and Ca have no spectral feature, then satellite **cannot even identify which nutrient is most deficient.** The rule does not degrade gracefully here.

**The unsolvable direction is the expensive one.** Excess N is where the money and the emissions are. The best oil palm classifier achieved *"nearly perfect classification when samples from both excessive levels are merged"* — the surface that would justify **cutting** fertiliser is the one nobody can resolve.

### The market has already reached this conclusion

Twenty-three vendors audited. **Not one claims a leaf nutrient concentration in % dry matter from imagery.** Zero instances of a concentration, a unit, a critical value or a sufficiency range attached to any imagery-derived product.

**Yara is the instructive case.** N-Sensor outputs **kg N/ha uptake**, expressed relative to the field's own average — never a concentration. Their own calibration statement: *"these trials look at… the actual leaf nitrogen contents through laboratory analysis. Only once it is confirmed that there is a good correlation between the two will a new calibration be released."* **Laboratory leaf N% is the reference the model is validated against, never the output.** They have no calibration for oil palm, rubber or pineapple.

**And the design worth copying is OneSoil's:** *"Build VRA map based on Soil Sampling Results… Your lab workflow stays the same. OneSoil improves how you define sampling zones."* **The satellite decides where to sample; the lab decides what the number is; the platform turns the lab result into a rate map.** It is the only shipped design in the survey that never has to refuse something it already promised.

---

## 2 · What satellite legitimately buys, and it is not nothing

Oil palm estates already stratify into **Leaf Sampling Units**. Remote sensing is a good instrument for **defining and re-defining those strata** — a same-date, within-block relative ranking that decides where the sampler walks. Plus trajectory and anomaly detection, canopy-gap and leaf-retention monitoring, and NDMI and SAR for water and structure.

**That is a real feature, it is honest, and it makes leaf analysis cheaper rather than pretending to replace it.**

The refusal writes itself in the product's existing voice:

> *"Nutrient status is not measured from satellite. The published band is defined on frond 17 tissue, which this sensor cannot see, and the achievable error is wider than the band. What this map gives you is sampling priority, not a nutrient reading."*

---

## 3 · The pattern generalises — but the key we assumed is wrong

**We assumed `crop × nutrient × age`. That is right for oil palm and wrong for the other three.**

| Crop | Band exists? | Provenance | **Discriminator** |
| --- | --- | --- | --- |
| **Oil palm** | Yes, deficient / good / excessive | PPI/IPNI — industry institute, not MPOB | **Palm age** *and* **soil group** |
| **Rubber** | **Yes — and it is Malaysian** | **RRIM**, Pushparajah & Tan 1972 via FFTC EB 398 | **Clone group** |
| **Rice** | Yes, but only in the international source | IRRI + PPI. **DOA copied it and kept only the optima** | **Stage × plant part** |
| **Pineapple** | **Nothing Malaysian at all** | International only, and mutually contradictory | **Leaf section × mass basis × cultivar** |

> **A fixed third column collapses three of the four crops.** The key is `crop × nutrient × (whichever discriminator that crop's literature uses)`, and the discriminator is named per crop, not assumed.

### Rubber is the best-provenanced standard we have found anywhere

**Better than the oil palm table**, because it is a Malaysian national research institute rather than a fertiliser-industry one — and it ships with its own deficit-to-rate lookup.

Pushparajah (1994), FFTC Extension Bulletin 398, Table 1, % dry matter — four classes with an explicit over-supply class, the same nominal-is-best shape:

| Nutrient | Low | Medium | High | Very high |
| --- | --- | --- | --- | --- |
| N, clone group 1 | < 3.2 | 3.21–3.50 | 3.51–3.70 | > 3.71 |
| N, clone group 2 | < 3.3 | 3.31–3.70 | 3.71–3.90 | > 3.91 |
| N, clone group 3 | < 2.9 | 2.91–3.20 | 3.21–3.40 | > 3.41 |
| P | < 0.19 | 0.20–0.25 | 0.26–0.28 | > 0.28 |
| Mg | < 0.2 | 0.20–0.25 | 0.26–0.30 | > 0.30 |
| Mn, ppm | < 45 | 45–150 | 151–300 | > 300; **> 500 toxic** |

**The three N clone groups span Low thresholds of 2.9 to 3.3%.** A clone read against the wrong group is the young-mature oil palm error wearing a clone name.

**Its rate table is keyed on leaf *and* soil**, not leaf alone — *"Reference to soil analysis will indicate that the K reserves in the soil are adequate and thus K fertilizers need not be used."* A rubber surface fed only a leaf figure could not use it.

**And it carries a published refusal condition.** Year-to-year variation *"could cover the range from deficiency to sufficiency"* even in unfertilised plots, therefore *"reference must be made to the analysis of leaves from control plots."* **A single-year rubber leaf figure with no control plot is not interpretable against these bands.**

**Rubber sampling is anchored to leaf age, not to a date** — around 100 days after leaf formation, window 90–160. If we ever ingest rubber leaf analysis, **days since refoliation is a mandatory field or the value is uninterpretable.** And leaf diagnosis is *"Not for phosphorus and calcium"* — a P or Ca verdict from rubber tissue is a refusal case by the source's own rule, even though the same source prints P ranges.

### Rice — DOA shipped a band collapse

`Pakej Teknologi Padi` 2008, Jadual 10, gives optimum tissue N by stage: **2.9–4.2% at tillering, 2.2–2.5% at flowering, and 0.6–0.8% labelled "Bunting (70–85 HLT)" — booting.**

**In the source it copied — Dobermann & Fairhurst 2000, Table 6 — that third column is `Maturity / Straw`.** DOA relabelled a straw-at-maturity figure as a booting-stage leaf figure. **Reading a healthy booting leaf against 0.6–0.8% would call the stand massively excessive.**

It also kept only the middle column. The source's full structure is **deficient < 2.5% · optimum 2.9–4.2% · excess > 4.5%** at tillering; the Malaysian copy has the optima and neither bound. And the tissue is unnamed on every row except Fe and Mn, which still carry *"(Y leaf)"* and *"(Shoot)"* — the fossil evidence that plant part varied row by row in the original.

**This is our own band-collapse defect, already shipped in a government document.** It is the strongest possible argument that the platform must carry tissue and basis on every band rather than trusting a table.

### Pineapple — the tissue itself is contested

There is **no Malaysian pineapple leaf standard of any kind.** MPIB's manual is a weeks-after-planting schedule with visual indicators only. MARDI's manual is login-walled and is the last place one could hide.

Internationally there are **two incompatible conventions on the same leaf** — the Hawaiian technique samples the white basal section, the French technique the whole leaf. Same units, same leaf: **K roughly doubles going basal (22–30 → 43–65 g/kg) while Ca falls about threefold (8–12 → 2.2–4.0).** There is no single correction factor. Half the world's thresholds are fresh mass and half dry mass, roughly 12:1 apart and not constant. And two cultivars in the same manual differ nearly **2× in optimum K**.

**A pineapple band without its leaf section, its mass basis and its cultivar is not a band.**

---

## 4 · So the base model is five layers, and the last one is small

| | Layer | What it holds | State |
| --- | --- | --- | --- |
| **1** | **Stratify** | Satellite decides *where* to sample. Relative, same-date, within-block. **Never a nutrient verdict.** | Buildable now, from what we hold |
| **2** | **Measure** | Laboratory tissue analysis, ingested. Carries tissue, leaf age, sampling date, control plot where the crop requires it | **The ingestion decision — answered: yes** |
| **3** | **Judge** | `crop × nutrient × discriminator` → deficient / optimum / excessive, with units, basis, tissue and citation | Oil palm and rubber now. Rice from the international source. **Pineapple refuses** |
| **4** | **Derive** | Published function from deficit to rate | Oil palm: four, three Malaysian. Rubber: a two-key lookup. Rice: per-site. **Pineapple: none** |
| **5** | **Attribute** | Whoever chose what the literature leaves open | **PR #5** |

**Layer 5 shrinks but does not vanish.** The agronomy is derivable and citable; what stays policy is the economic risk appetite — Foster picks the final rate by economic criterion, *20% return for plantations against 100% for smallholders*. That is a business decision and always will be.

---

## 5 · Rules that follow, and belong wherever bands are implemented

- **A band carries its tissue, its mass basis, its discriminator and its citation, or it is not a band.** Rice's shipped defect is what happens when it does not.
- **The discriminator is named per crop**, never assumed. Age, clone, soil group, stage-and-part, leaf-section-and-basis-and-cultivar.
- **The multi-nutrient rule is not crop-specific.** It is restated verbatim for rice in the IRRI source and for oil palm in Foster via Goh. Write the refusal once, for all crops.
- **Where a crop's own literature forbids a verdict, refuse it** — P and Ca from rubber tissue, zinc from pineapple D-leaf, which is *"not diagnostic of zinc deficiency"*.
- **Non-overlapping bands on the same nutrient are normal.** Oil palm frond 17 critical N is **2.55–2.65% inland and 2.85–2.95% coastal** — they do not overlap, and that is the discriminator doing its job.

---

## 6 · What would close the remaining gaps, and what it costs

1. **RM 120 and two emails.** The Malaysian Rubber Board library sells **RRIM (1990), *Manual for diagnosing nutritional requirements for Hevea*** (RM 60) and **Chang & Teoh (1981)** (RM 60). The first is the version the industry standardised on and the only thing that will settle whether the clone groups were revised for the **RRIM 2000-series clones our docs already care about**. **This is the cheapest high-value action in the whole programme.**
2. **OCR of MPOB Bulletin 72.** Live, no text layer.
3. **MARDI's pineapple manual**, login-walled — the last place a Malaysian pineapple standard could exist.
4. **A second reading of FFTC EB 398.** All seven tables are scanned bitmaps read visually once. **Two values break otherwise monotonic columns and are almost certainly scan errors** — a `100` and a `9200` in the potassium table. Do not encode either without a second reading.

---

**Housekeeping, unrelated to the research:** a subagent wrote three PNG page renders into the docs folder as a working step and removed them. Nothing was left behind, but **delete permission on that folder is now enabled** and you may want to turn it back off.
