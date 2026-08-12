# Terra Oracle's VRA, and what it means for our rate bands

**Status: research, and it carries a recommendation.** Driven live in the portal on 11 August 2026, read-only — no control that saves, exports, deletes or bills was pressed. The account held one demo field, so this is what they showcase rather than a customer's production data.

Written to answer three questions: **are the three names one thing, how does a working competitor solve the rate problem, and can we default a rate across crops.** The third answer is no, and the second explains why nobody else has had to answer it.

---

## 1 · The three names are one entity, and one of them is ours

| Name | Where it lives |
| --- | --- |
| **Prescription** | The docs. `"Prescription requires: field_id, active crop cycle, index_basis…"` |
| **VRA map** / **prescription map** | The docs. `/guides/prescription-maps`: *"VRA (Variable-Rate Application) maps, also called prescription maps…"* |
| **Application plan** | **The prototype only.** Not evidenced anywhere in the documentation. |

**Prescription and VRA map are synonyms and the docs say so outright.** *"Application plan"* is a name the build invented and nothing backs it. Terra Oracle calls the artefact a **VRA map** and the section **VRA Planning**, which matches the industry.

**Recommendation: rename the screen to `VRA maps` and use *prescription* for the record in specs.** One user-facing name, one spec name, no third.

---

## 2 · What Terra Oracle actually is

A soil-lab-driven precision-ag portal. Four sections: **Fields · Field Data · Field Intelligence · VRA Maps**, plus an LLM advisor scoped to *Field Data, Soil Analysis, VRA Planning, Agronomy Q&A, Platform Help*.

**The pipeline runs from a lab result, not from a satellite.** `Field Data` ingests an **External Analysis** record with two attachments — a **field data file** and **lab results** — which becomes a layer. `Field Intelligence` then draws that layer on the field with a time slider, alongside NDVI, weather and satellite.

### The VRA screen, measured

For `VRA 192 — Field 526 — Mg`:

| Control | Value observed |
| --- | --- |
| Source | **Soil Analysis** ▸ scan date *23 March 2026* · **NDVI** tab present |
| Property | **Ca · K · Mg · P · S · pH** |
| Zone algorithm | **Agronomic · Equal Interval · Equal Area · Standard Deviation** |
| Number of zones | 5 |
| Smart boundaries | on |
| Trim percentage | 0 |
| Minimum application area | on, **1.00 ha** |
| Export | **Download VRA · John Deere Operations Center · CNH FieldOps** |

The zone table:

| Soil Mg from | to | Area | **Rate** | Amount |
| --- | --- | --- | --- | --- |
| 0.00 | 48.00 | 0.62 ha | **50.00** | 31.46 |
| 48.00 | 77.00 | 10.64 ha | **30.00** | 319.48 |
| 77.00 | 106.00 | 22.11 ha | **20.00** | 442.33 |
| 106.00 | 135.00 | 4.77 ha | **10.00** | 47.74 |
| 135.00 | ∞ | 0.00 ha | **0.00** | 0.00 |
| | | **38.17 ha** | | **841.04** |

`Amount = Area × Rate`, summed. **The rate is inverse to the soil value** — the less magnesium in the ground, the more you put on, and above a sufficiency ceiling you put on none.

---

## 3 · The finding that matters: they did not solve our problem, they declined to own it

**Three of their four zone algorithms contain no agronomy at all.** Equal Interval, Equal Area and Standard Deviation are statistical cuts of whatever distribution you hand them. They would divide any array of numbers into five zones.

**The fourth, labelled `Agronomic`, has the internal value `LAB`.** The agronomic algorithm *is* the lab-driven one. Agronomy enters this product through a soil laboratory, not through a published rate table.

**And the rate column is not validated against anything.** 50 · 30 · 20 · 10 · 0 are round numbers, editable per zone, with no minimum, no maximum, no warning and no refusal anywhere on the screen.

> **Terra Oracle's answer to "what rate is correct?" is: the agronomist decides. The platform draws the zones, records the decision and writes the file the tractor reads.**

That is a coherent product position, and it is worth taking seriously precisely because **it is not the position our docs currently take.** `guides/prescription-maps` says a rate *"must fall within the crop-specific agronomic min/max declared in the Risk Model"* — a validation the market leader does not attempt.

### Why their approach does not transfer to us as-is

**Their input is a measured quantity of a nutrient in the soil. Ours is a vegetation index.**

| | Terra Oracle | Semai today |
| --- | --- | --- |
| Input | Soil Mg, ppm, from a lab | NDVI / NDRE / NDMI / SAR |
| Inference | deficit against a sufficiency level → kilograms to add | **?** |
| Length of that chain | short, physical, unit-preserving | long, and uncalibrated for Malaysian crops |

**Soil chemistry to fertiliser rate is nearly arithmetic.** Index to fertiliser rate is a research question, and it is the one no Malaysian authority has published — which is the same wall `DECISION-rate-bands-do-not-exist.md` hit from the other side.

**So the honest read: we cannot copy their VRA, because we do not hold their input.** Doing so is a product decision — ingest soil lab results — not a documentation gap. It is a real option and it is the one that would unblock VRA fastest, but it changes what the platform is.

**Also worth noting, without overclaiming from one field:** their `NDVI date` dropdown was **empty** on this field while the soil dropdown was populated. On this evidence the index-driven path is the weaker half of their product. Our band, stage and saturation work has no counterpart anywhere in what was visible.

### What they have that we do not, and it is not agronomy

**Machine export as first-class.** *Download VRA*, *Export to John Deere Operations Center*, *Export to CNH FieldOps.* Our docs name file formats — ISO-XML, Shapefile, GeoJSON — but no platform integrations. A prescription that cannot reach the machine is a PDF.

**A minimum application area.** 1.00 ha, on by default. A guardrail about **whether the work is physically executable**, which is a category our eight do not really cover and which costs nothing to adopt.

---

## 4 · The answer to "should we default a rate across crops"

**No. Not a default, and never interchangeable across crops.** Three reasons, in order of how badly it goes wrong.

**It has no unit.** Rice urea is *80 kg/ha per pass*. RISDA 4 on mature rubber is *700 g/tree*. Pineapple BCN is *14 g/plant per pass*. There is no number that means anything in all three. A default across crops is a figure with the unit stripped off, which is the never-sum-across-units defect turned into a policy.

**The docs already refuse it, in the strongest language in the corpus.** `concepts/risk-model`:

> *"Where no band exists for a crop and product, the platform declines the prescription. **It does not fall back to 'any rate is acceptable.'**"*
> *"A missing guardrail fails closed. That is the difference between **we have not sourced this yet** and **any rate is acceptable here**, and the second one puts fertiliser on the ground."*

**And it is the failure mode this whole product exists to prevent.** A default rate is a plausible number with no author. Every other surface in Semai refuses rather than fabricate — an em dash with a stated reason beats a fabricated figure. A default rate would be the one place we did the opposite, and it would be the place with physical consequences.

### The three honest paths, and one of them is now clearly best

**A · Deviation from a sourced point.** We hold real DOA point recommendations for rice. Refuse beyond ±*x*% of the cited figure, with the tolerance visibly a platform decision. **Per crop and per product, never one *x* across all of them.** Carries the recorded caveat that DOA's rice column is headed `Bantuan Pemberian Kerajaan` — a subsidy allocation, not an agronomic optimum.

**B · An agronomist sets a min/max per row**, recorded as `decided_by` with a name and a date, distinct from `sourced`. This is the fourth `status` value the decision file already identified as missing.

**C · The platform does not own the rate at all.** The user enters rates per zone; Semai records who, when and against what evidence, then exports. **This is what Terra Oracle does.**

**Recommendation: C, with B as the path to A.**

The reasoning is that **C changes the question to one we can actually answer.** Today the guardrail asks *"is this rate inside a band?"* and we have no band, so every prescription refuses and the feature has never permitted anything — which, as the decision file says, is not yet a product. Under C the guardrail asks:

> **"Is this rate attributable to a named person who was entitled to set it?"**

That we can answer today, for every crop, with no new agronomy at all. And it fits the grain of everything else already built: acceptance is the assignee's act, a finding carries its author, a promotion writes its own audit row, an undone move leaves a trace. **A rate carries a name** is the same move in a new place.

**The refusal changes rather than disappears**, which is the important part:

> *Before:* "No band exists for this crop and product, so this prescription is refused."
> *After:* "This rate is not sourced from any published recommendation. It was set by **Siti Rahman** on 3 August and applies only to this map."

The second is honest, shippable, auditable, and it does not dress a judgement as a citation. **And it fails closed in the way that matters** — a rate with no name still refuses.

**A stays the destination.** Every row set under C is a row an agronomist can later promote to `sourced` when MPOB Bulletin 72 or the DOA LCC lookup is in hand, and the audit trail will show exactly which figures were judgement and which became citation.

---

## 5 · What to do next, in order

1. **Rename `Application plans` → `VRA maps`.** One entity, one name. Cheap and it stops the third vocabulary spreading.
2. **Take the decision in §4** — this needs you, not another brief. It is the only thing blocking `TRACK-B-4`.
3. **If C is chosen, rewrite `TRACK-B-4` around attribution rather than validation.** The flow is fully documented already and the seven creation steps stand; only the guardrail's question changes.
4. **Adopt the minimum application area** as a soft warning. Cheap, physical, and we do not have it.
5. **Open the question of soil lab ingestion** separately. It is the only route to Terra Oracle's actual capability, and it is a change of product scope rather than a gap to be filled.

---

## 6 · Limits of this study

One demo field, one VRA map, one crop nutrient. **No login was created and no credential was entered** — the session was already authenticated in the browser. Nothing that saves, exports, deletes or bills was pressed, so the export integrations were observed as controls and not exercised. Rate-entry validation was inspected in the rendered form and not tested by submitting a value, which means **a server-side rate check cannot be ruled out** — only that nothing on the surface indicates one.

**Sources:** `portal.terraoracle.ai/vra/maps/detail/192/`, `/vra/maps/`, `/field-data/`, `/field-intelligence/526/`.
