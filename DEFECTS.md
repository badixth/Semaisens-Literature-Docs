# Defects — the running log

**Status: live. Append-only.**

One log for the whole prototype. Newest pass at the top. **Do not overwrite a pass** — the record of what was fixed, and how many attempts it took, is worth more than a tidy current list. Three earlier defect files were collapsed into this one on 10 August 2026.

Everything here was driven live in the prototype with Playwright after a full reload, not read from screenshots. Figures were re-derived by hand from the rendered DOM.

---

## Open

| Id | Surface | What | Since |
| --- | --- | --- | --- |
**Nothing open.**

---

## Pass 26 — G1 closed, and two probe errors of my own

**The mark is on the field detail**, under *Where this field stands*: a `BandGauge` with the band at **0.65–0.80** on a **0.0–1.0** track, the reading as a rule, the label **`0.10 below`**, and the sentence *"Read against the 0.65–0.80 band for paddy at booting. 0.10 below it."*

**The wording is fixed too, and I nearly filed it as unfixed.** `Where it stands` renders as:

> **Where it stands** · *Slightly below · against the 0.65–0.80 band for paddy at booting* · **−0.10**

The **value is `−0.10`** and the ladder word sits in the description as evidence for it — exactly as reported. My first probe truncated the string before the figure and returned only *"Slightly below"*.

### Two errors of mine, same family, and the rule that follows

**One — a truncated read reported as a missing fix.** I cut a 140-character block at 90 and concluded a value was absent when it was 40 characters further on.

**Two — a synthetic click reported as a dead end.** With a field selected, `element.click()` on the scope breadcrumb did nothing, and I was one step from recording *"you cannot return to the field list."* A **real** Playwright click on the same control resets scope to `All fields` and restores the picker with all twelve fields. `CLAUDE.md` already warns that synthetic events do not always reach a React handler; this is the seventh instance in this project.

**The rule, now stated for the third time in this log and evidently still needed:** *before recording an absence — of a value, a control, a component or a route — confirm the probe can detect that thing where it is known to exist.* Every false finding in passes 18, 20, 25 and 26 was a probe that could not have returned a positive.

### One observation, not a defect

**The nav item does not return to the picker.** With a field selected, clicking `Field analytics` re-renders that field's detail. The route back is the **scope breadcrumb**, which is correct — the breadcrumb is the single writable scope control on operational surfaces, and the screen honouring its scope is the convention working.

But a demo operator will reach for the nav item first. Worth knowing before presenting; not worth changing, because the alternative breaks scope authority.

### Recorded from the build side, and it changes how their results should be read

The build reports the interaction suite is **order-dependent** — tests 26, 27, 28 and 30 pass individually and fail in some full-run orders; 31 does the reverse. Cause identified as each test restoring a session captured after the previous test already wrote to the store, with `bandTiers` committing a write. **Until a real per-test store snapshot exists, a single test's verdict should be read from an individual run.**

Also fixed there: six tests were failing on a cold store with *"a div is on top of it"* — the walkthrough offer at z-index 70, one intro card reported as six separate occlusion defects. `coachSeen` and `fieldId` now reset.

---

---

## Pass 25 — Phase 15, and a correction I owe the record

### Everything in the brief landed

| | Before | After |
| --- | --- | --- |
| **Field analytics** | 10 fields as static text, **zero** field controls | **13 controls**, every field selectable |
| **VRA maps** | named `Application plans` | **renamed**, nine plans across the six stages, 15 controls |
| **Proof packs** | read-only | **9 controls** — `Open Field P1`, `Subsidy claim`, `Takaful claim`, `NADMA relief`, `Send to the subsidy office` |
| **Regional forecast** | no marks | **`SpreadMark`** component present |
| **Yield production** | one boxplot over a histogram | **boxplot gone** |

**Field analytics does what its copy always promised.** Selecting Muda A2 takes the screen from 527 to 886 elements, renders a `FieldDetail`, and sets the scope control to `Semai Demo Organisation › Muda Granary North › Muda A2` — which is exactly the sentence that used to point at nothing.

**The Yield production card was rewritten around the histogram**, and its empty state distinguishes absence from suppression, which is the harder half:

> *"Nothing has been declared against this window, so there is no spread to draw and **nothing is withheld**. The shape resolves on the first declared harvest."*

Its subtitle now reads *"the mean cannot tell you whether an estate is uniform; **the shape can**"* — the argument survived the change of mark.

### G1 · The field detail draws a bare index

The most zoomed-in reading surface in the product renders:

> `Mean NDVI` **0.55** · `NDVI` **0.55** — *"slightly below"*

**No `BandGauge`. No band range anywhere on the screen** — zero `0.xx–0.xx` strings. Components present are `FieldDetail`, `SeverityBadge`, `AttentionCard` and nothing else.

This is the case the mark exists for: **one field, one crop, one stage, one band, one reading.** Every other reading surface has it — National overview 15, How a figure is built 15, Fields workspace 12. `CHARTS-product-map.md` is explicit and this is its own acceptance test 2:

> *"**Never a bare index on a scale.** A chart with a 0-to-1 axis and no band drawn is the nominal-is-best defect in visual form... **Every index mark carries its band or it does not ship.**"*

**And the wording is softer than the product's own standard.** Fields workspace says *"0.10 below the band for paddy at booting"* — a signed distance against a named band. Here it says *"slightly below"*, on the screen where precision is easiest.

### A correction I owe the record

**I reported three dead screens in the previous pass. I had only verified one.**

`Field analytics` was measured against the whole document and was genuinely inert — seven controls, none of them a field. **`VRA maps` and `Proof packs` were measured with a `main`-scoped selector, and this application has no `<main>` element**, so that probe returns zero on every screen in the product, including ones I know carry controls.

Both may have had working controls before the brief. **I cannot now prove either way**, and the brief may have asked for work that was already done.

**The rule this makes explicit, after five near-misses of the same family:** a probe that returns zero must be validated against a screen known to be non-zero **before** the zero is reported as a finding. A null result is a claim like any other.

---

**Nothing else open.**

---

## Pass 24 — F2 closed

Both directions of deviation now draw in **`rgb(192,57,43)`** on every `GroundRow`. Blue is gone from the bars entirely — measured across all five regions. Deviation reads as one kind of thing regardless of direction, which is what nominal-is-best requires.

The split survives as a **2 px gap** between the two outside segments rather than as a second hue, and the direction is stated in words on every row — *"150.0 ha reading below the band that resolved for it · 68.0 ha above it."*

**One consequence, recorded and accepted:** on the narrow rows the two outside segments read as one block. Kedah's are 10 px and 7 px with 2 px between them. That is the right trade — the bar's job is *how much ground is outside*, which is also the sort key, and the direction lives in the sentence. **If the split is ever wanted in the mark itself, use tint or hatch within the same hue — do not reintroduce a second hue.**

Fifteen gauges and all four `Counted by field` summaries intact.

---

## Pass 23 — F1 closed at the right level

**The band rows no longer claim a position.** They state the band and the hectares that resolved to it, and nothing more:

> `0.58–0.74 · vegetative` — 214.8 ha
> `0.58–0.75 · fruit development` — 38.5 ha
> `0.25–0.50 · late vegetative` — 26.0 ha
> **Counted by field: 279.3 ha inside their own band**
> *"The bands above divide the same 279.3 ha — **a band row states how much ground resolved to it, not how that ground read.**"*

**That sentence is the fix.** The row stopped making a claim it could not support, rather than the summary being bent to match it — and the distinction between *how much ground resolved to a band* and *how that ground read* is now stated on the surface, which is what will stop the fourth recurrence.

The gauge follows: the band region still draws, and **no reading rule is drawn**, because a row of many fields has no single reading to place. Consistent.

**All four crops reconcile** against their own totals, and nothing contradicts them:

| Crop | Counted by field | Total |
| --- | --- | --- |
| Paddy | 693.3 inside · 4.2 below · 3.1 above | 700.6 ✓ |
| Oil palm | 1,742.5 inside · 270.0 below · 68.0 above | 2,080.5 ✓ |
| Rubber | 276.0 inside · 96.2 below | 372.2 ✓ |
| Pineapple | 279.3 inside | 279.3 ✓ |

Fifteen gauges intact.

### F2 · The two directions of one failure are drawn as two severities

Measured on every `GroundRow` bar:

| Segment | Colour |
| --- | --- |
| Inside its band | `rgb(22,163,74)` green |
| **Below** its band | `rgb(192,57,43)` red |
| **Above** its band | `rgb(0,117,255)` **blue** |

**Red is the product's problem colour** — severity chips, negative deltas, overdue. **Blue is `Low / For info`** on the same severity ladder. So the bar says below-band is a problem and above-band is informational.

**Nominal-is-best says both are loss.** High on paddy at ripening means late, not healthy; high on rubber means saturated and ranking against nothing. Above-band is not the milder direction, and the palette currently claims it is.

**Fix:** one hue for deviation in both directions, distinguished by **side** — the bar already places below left of above — or by tint and hatch rather than by a hue that carries a different meaning elsewhere. The design system's **divergent scale** is built for exactly this: distance from an optimum in both directions. Phase 11 settled the governing rule — *colour marks a state that needs a decision* — and both directions need one.

### Standing, not changed

The verdict slot still opens with the coverage caveat — *"One scheme has not reported since 5 June"* — as the largest text on the page. Raised as a question in pass 22, not filed, and unchanged. Recorded so the next reader knows it was seen and left.

---

---

## Pass 22 — Phase 14 built clean, and an older defect surfaced underneath it

**Phase 14 passes every test.** The order is right, the ranking is honest, the expansion reuses `GroundRow`, the scope row does not move, and all fifteen gauges survive. **But reconciling the arithmetic to check test 1 turned up something that has been on this screen since Phase 12 and that I did not catch in pass 18.**

### F1 · Four crops, four mismatches, in both directions

Each crop states its bands as rows, then a position-by-ground summary. Measured row by row against each row's gauge:

| Crop | What the rows say | What the summary says | Gap |
| --- | --- | --- | --- |
| **Paddy** | 696.4 ha `in band` · 4.2 ha at **−0.07** | 693.3 inside · 4.2 below · **3.1 above** | **3.1 ha called above. No row shows above.** |
| **Oil palm** | **All 2,080.5 ha `in band`** across four rows | 1,742.5 inside · 270.0 below · 68.0 above | **338.0 ha called outside. No row shows outside.** |
| **Rubber** | All 372.2 ha `in band` (three rows, all drawing hollow) | 276.0 inside · **96.2 below** | **96.2 ha called below; its own row says `in band`.** |
| **Pineapple** | 253.3 ha `in band` · 26.0 ha at **+0.15** | **279.3 ha inside** | **26.0 ha called inside; its own row states a signed distance above.** |

**Pineapple is the clearest to read and the hardest to defend.** The row says `0.25–0.50 · late vegetative · 26.0 ha · +0.15`, its gauge draws a rule outside the band, and eight lines below the screen says all 279.3 ha is inside.

**The likely cause is the collapse defect, one level further down again.** A band row is an aggregate — one band and the total hectares that resolved to it — and its `in band` label describes the aggregate, while the summary counts by field. So a row of 1,650 ha reads `in band` while holding fields that do not. **That is a band row stating one position for hectares that do not share one**, which is the rule *never collapse a band* at a level nobody has checked yet.

This is the **third** recurrence of the same shape: first between schemes, then inside Felda Wilayah J2, now inside a band row. `CLAUDE.md` already warns about exactly this — *"it survived two rounds of the band fix because the collapse was happening one hop further down."*

**One alternative reading, and the build should settle it rather than me.** Pineapple's row draws a **dark** rule, `rgb(26,31,28)`, where paddy's genuine below-band row draws **red**, `rgb(192,57,43)`. If dark is the *inconclusive / not calibrated* variant, the summary may be right to exclude it from below-and-above — but then it is still wrong to count it as **inside**. Either way the two disagree.

**Decision required if it is not resolvable: which side is authoritative, the row or the summary?**

### Consequence for Phase 14, which is small

The new ranking is built from the **summary** figures, so it inherits whichever answer is right. Johor reads **218.0 ha**, all of it from Felda Wilayah J2; Ladang Johor Selatan contributes 0.0 while holding the 26.0 ha at +0.15. **If the row is right, Johor is 244.0.** The order does not change — Johor leads either way — so this is a figure to correct, not a ranking to rebuild.

### My miss

Pass 18 checked the gauge count, the three variants and the row text. **It never reconciled the rows against the summary beneath them**, which is the one arithmetic on that card. Counting marks is not checking a figure.

---

## Pass 22a — what Phase 14 got right

**Order, as specified:** resolved scope in words → verdict → `Where the ground is` → crop health → the exceptions in full → totals. Every exception sentence, refusal and coverage caveat survived.

**The ranking is honest and says so.** Johor 218.0 · Sabah 120.0 · Perak 96.2 · Kedah 7.3 · Kelantan 0.0, with the measure named on the row — *"Ranked by hectares reading outside their own band — each field judged against the band that resolved for its own crop at its own stage, and the hectares then added."* And the rule stated on the surface:

> *"No region is ranked by a reading. A region here is effectively one crop, so an index league table across regions would be a comparison across crops — and there is no combined health score on this platform to build one from."*

**Two caveats ride on the rows that need them**, which is better than a footnote: Kelantan's `0.0 ha` carries *"268.4 ha of Kelantan has not reported for 10 days, so the position above is that last reading rather than today's"*, and Perak carries *"276.0 ha of Perak reads at the ceiling of its index, where sitting inside a band is not evidence of health."* **A zero that means "we cannot see" is the most dangerous figure on a ranking**, and it is labelled.

**The expansion reuses the component.** `GroundRow` goes 5 → 8 on opening Johor, and the scope row and breadcrumb do not move. `aria-expanded` on every region.

**Overview** shows three queue rows — *"14 things across 6 fields · the 3 that can wait least, in queue order"* — and keeps a job.

### P-54 is right, and it is an application of the settled rule rather than a relaxation

`CLAUDE.md` already says: *k = 5 on cohorts cut across schemes; a scheme the reader's scope entitles them to is named however few fields it holds.* **Sabah is one scheme.** Naming it is what the settled rule requires, and withholding it while the same 120.0 ha appears eight lines below under Ladang Sabah Timur would be a refusal that protects nothing — worse than none, because it claims a protection it is not providing.

Both halves are stated on the surface. **The one caveat: the withholding branch is unreachable on this seed, so it is built and untested.** Worth a seed that exercises it before anyone relies on it.

**P-53** — hectares outside their own band as the sole sort key, no composite. Correct; a composite of the three honest measures would be a fourth measure with no unit.

---

**Everything else is closed.** Passes 16–21.

**Pass 21 · E1 closed, and the tablet layout is now a stated refusal.** At **834** five columns render — selection, Field, Band position, Where it sits, Findings — and five are `display: none`. The drop is named on the surface:

> *"Narrow screen · the shape, crop, stage, raw index and capture date are not shown. Crop and stage are named in each row's sentence, and every one of them is on the wider screen."*

**Drop and say which, plus where the information still is.** Zero overflow and zero scrollers at **1440, 834 and 390** — the 1130-into-458 squeeze is gone. At 390 the table becomes cards; no header renders, so there are no columns to account for. 12 `BandGauge` survive at every width.

Worth noting they kept **Where it sits** over **Crop**, against my suggested priority, and justified it in the notice — the sentence already names the crop and the stage. Better reasoning than mine.

---

## Pass 20 — Phase 13, thirteen of fourteen

### The table is a real table

`role="table"` → `rowgroup` → `row` → `columnheader`, with **137 cells** and the chain verified from the header upward. Every header carries `aria-sort`; the active one reads `ascending` on **Band position**, which is the right default. Each sortable header wraps a button with a spoken label — *"Field · sort ascending"*, *"Findings · sort descending"*.

Columns are now **Selection · Shape · Field · Crop · Stage · Band position · Where it sits · Raw index · Findings · Latest**. Crop has its own column. Selection is **13 real checkboxes** — twelve rows and a header — not the shield glyph.

### The raw index refusal is better than the brief asked for

`aria-disabled="true"`, and pressing it produces:

> *"This column will not sort. These readings are not comparable — each one is judged against a different band. **Ranking on the raw index would put a paddy field at 0.72 above an oil palm block at 0.72**, and the two readings do not mean the same thing."*

It refuses **with a worked example**, and names distance-from-band as the alternative. The brief asked for a sentence; this teaches the rule.

### Stage sorting is phenological, and it is correct

In `One list`, sorting by stage groups by crop and then orders within the crop:

| Crop | Order produced |
| --- | --- |
| Oil palm | Prime · Prime · *Archived* |
| Paddy | Tillering · Tillering · Booting · Ripening · *Paused* |
| Pineapple | Late vegetative · Fruit development |
| Rubber | Opening · Prime tapped |

Tillering → Booting → Ripening is right. Opening → Prime tapped is right. **Non-stages sort last within their crop** rather than being interleaved alphabetically, which is the trap the brief named.

### Creation, and the Phase 11 defect that finally closed

**`New estate` refuses in words.** Open since Phase 11 as the one disabled control in the product that said nothing:

> *"This is not yours to create. Creating an estate is an administration action, and this account is signed in as estate manager."*

**It names the role rather than inventing a permission**, which is what the brief asked for where the docs are silent.

**The estate picker has its escape** — a row reading *"The estate is not on this list"*, at the foot of the five.

**Both creation surfaces are centred, at identical geometry** — `left 400 · right 1040 · width 640` in a 1440 viewport, for `New field` and `New check` alike.

**And a refusal nobody asked for, which is the right instinct:**

> *"The boundary is not drawn here. A field's area is measured off its outline, and this form has no map in it. **Nothing here will accept a hectare figure typed by hand, because a number nobody measured would then be carried into every rollup that sums hectares.**"*

That is the product's habit applied to a form field.

### The expansion reuses the component

Expanding *"Show the fields on Muda Granary North"* renders **`FieldTable` (1)** and **`BandGauge` (3)** — the same components as the workspace, three gauges for three fields. Not a second implementation. `aria-expanded` is wired on all five estates.

### Shapes are neutral

26 shape cells, and exactly **one fill across all of them**: `rgba(26,31,28,0.055)`. No good-to-bad ramp, no new tokens. The trap in §4 was avoided cleanly.

### Nothing from Phase 12 regressed

12 `BandGauge` on Fields workspace, zero bare `title` attributes, the counted chips intact with their counts, `Raw index · not comparable` still stated. At **390**: 390 / 390, zero clipped, zero controls under 44 px, and the table still exposes its role.

### E1 · The tablet squeeze got worse

At **834** the field table scrolls **1130 px inside a 458 px container** — 505 elements sit past the right edge, all of them within that scroller, so there is no page overflow and test 13 passes as written.

But it has moved: pass 14 measured **1040 into 514**. Adding `Shape` and `Crop` widened the table by 90 px while the container narrowed by 56, so **672 px of a 1130 px table is now behind a sideways scroll on a tablet** — three-fifths of it, including the raw index, findings and latest columns.

**This is not a Phase 13 defect in the strict sense** — 834 was out of scope in Phase 11 and stayed out here. But the table is the screen, and on a tablet most of it is now hidden. It wants the same treatment 390 got: **drop columns and say which, rather than scroll.**

### Method note — three near-misses in one pass

All three were my probe, not the build. I searched `role="grid"` when the build correctly used `role="table"`; I regex-matched *"add an estate"* when the copy reads *"The estate is not on this list"*; and I clicked an estate **name** when the expander is a separate control labelled *"Show the fields on…"*.

**Enumerate the affordances before asserting one is missing** — `[aria-expanded]`, `[data-sc-name]`, `[role]` — rather than guessing at the string.

---

**Pass 19 · D1 closed.** Zero bare `title` elements on Overview, Estate group and Fields workspace. Every hover-only figure was **promoted to visible text**, not deleted — Overview's crop mix now reads *"Rubber · 276.0 ha · 47.9% · 2 fields"* and the health bars read *"7 fields inside their band · 449.3 ha · 1 field above their band · 3.1 ha · 2 fields below their band · 124.2 ha"* on the surface. Element count rose 845 → 915, which is the tell that content was added rather than removed. 390 still clean.

**Also noted:** Fields workspace now carries **12 `BandGauge` marks** of its own, and the 1440 clipping recorded in pass 14 is gone — zero horizontal scrollers, zero clipped text. The table fits its card.
| **P1** | National overview · Regional forecast | The refusal at 390 carries **no headline verdict** | 10 Aug |
| **P2** | Both, same sentence | *"Every reading below is from the other nine"* — **there are no readings below** | 10 Aug |
| **P3** | All five refused screens | The shared refusal paragraph names a pixel width and states the rule | 10 Aug |
| **P4** | Work done · Where the work is | Desktop **interaction instructions** survive into the phone layout | 10 Aug |
| **P5** | Fields workspace | **Fourteen figures reachable only by hover**, on native `title` | 10 Aug |

**H5 is closed.** The phone layout is built and the ten measured tests pass — see pass 14. **Colour is clean and stayed clean**; H1–H4 and H6 remain closed. **P1–P5 from pass 14 are closed** — see pass 15.

---

## Pass 18 — Phase 12, and the one miss is mine

**Fourteen of fifteen tests pass.** The one that fails does so because the brief pointed at the wrong screen.

### The gauge is built, and it is built properly

**Fifteen `BandGauge` marks** on the crop-health rows — **5 paddy · 4 oil palm · 3 rubber · 3 pineapple**, matching the resolved bands exactly. Test 4 passes on the count alone.

Every one draws its band behind the reading. **No index mark renders without its band.** Measured across all fifteen: `hasBand: true`, band widths from 44 to 111 px on a 437 px track.

**And the three variants are real, not decoration:**

| Variant | How it draws | Where |
| --- | --- | --- |
| Inside the band | Solid green rule | Eleven rows |
| Outside the band | Solid red rule, `rgb(192,57,43)` | Paddy `0.65–0.80 · panicle initiation / booting`, at −0.07 |
| **Saturated** | **Hollow rule** — `background: transparent` | **All three rubber rows** |

**The saturation case is the one that mattered** and it is drawn, not merely stated. Rubber at 0.81 now renders as a reading pressed against a ceiling with a hollow mark, and still says *"Saturated — this figure ranks against nothing."* Drawn **and** said, which is what test 5 asked for.

**A note on method, because I nearly filed this as a failure.** My first probe looked for `<svg>` and found nothing above 24 px. The gauge is built from positioned spans, not SVG. **The probe was wrong, not the build** — this is the fourth near-miss of this class in the project. Query by component (`[data-sc-name]`) before concluding a mark is absent.

### Eighty words became five rows

The paddy block was a single 80-word paragraph. It now reads:

> `0.55–0.75 · tillering · 412.6 ha` — in band
> `0.52–0.72 · tillering · 268.4 ha` — in band
> `0.40–0.65 · no stage resolved · 9.9 ha` — in band
> `0.35–0.60 · tillering · 5.5 ha` — in band
> `0.65–0.80 · panicle initiation / booting · 4.2 ha` — **−0.07**
> **693.3 ha inside · 4.2 below · 3.1 above**

Plus one line: *"Fields here sit at four stages, so no single stage describes this crop."* The stage enumeration and the definition-of-a-band rule are gone from the surface.

**Test 7: *"a band is a definition"* returns zero matches on all five screens checked.** It was on every crop row of every screen.

### §0 — the production figure is fixed, and fixed better than specified

The invented national total is gone. The card now reads:

> **Production forecast · 1 of 5 estates has written one · no total across them**
> Ladang Sabah Timur · **Oil palm** · confidence high · **4,820 t**

Two improvements over the brief. The refusal states its **coverage** — 1 of 5 — rather than just declining. And the one real figure now **names its crop**, so nobody reads 4,820 t as a national number. The `PRODUCTION FORECAST` KPI card that carried 5,410 t is gone from the row entirely.

### Provenance is one line

`Estate group` carried three full-width cards and a four-row confidence breakdown. It now reads:

> *"Confidence medium · Written by the overnight job at 06:00 on 15 Jun · data through 14 Jun"*

Verdict visible, mechanism in the trace. And the freshness **exception** survived, which was the risk: *"KADA Kelantan East is running on imagery through 5 Jun"* is still on the surface, in the headline sentence.

### Also clean

`MEAN INDEX —` is no longer a KPI card. `Ask Semai` overlaps **zero** text elements at 1440. Colour: **zero AA failures** measured at 1440. At **390**: 390 / 390, zero clipped, zero off-screen, zero controls under 44 px.

### D1 · Eighteen figures still reachable only by hover — and I sent the build to the wrong screen

§6 of the brief said *"currently hover-only in fourteen places on **Fields workspace**."*

**Fields workspace is now clean** — 26 `title` attributes, every one on an element that also carries visible text. The build fixed what it was pointed at.

**The figures were on `Overview` and `Estate group`.** They still are:

| Screen | Bare segments with a `title` and no visible text |
| --- | --- |
| **Overview** | **15** |
| **Estate group** | **3** |

> `title="Rubber · 276.0 ha of 576.6 ha · 2 fields"` · `title="2 fields below their band · 124.2 ha"` · `title="1 field above its band · 3.1 ha"`

These are the crop-mix stacked bar and the health-distribution bars. **Band position is on the Visible list by name**, and a native `title` opens on neither tap nor focus — so this fails on a phone and for a keyboard user at every width.

**This is a defect in the brief.** I recorded the screen wrong in pass 14 and carried the error into Phase 12 §6. The fix is unchanged, only the address: **promote the segment labels or drop the segments**, on Overview and Estate group.

---

## Pass 17 — T5 closed, drawer clean

The refusal now sits **above** the list rather than replacing it:

> *"Nothing can be written against this field. Pick a different one below."* → the refusal → **`OR PICK ANOTHER FIELD`** → all five fields → `Cancel`.

Seven controls, all `type="button"`. **Recovery verified without restarting:** from the refused state, selecting Muda B1 goes straight into the full drawer — brief, sample plan, picker. `IN NAME ORDER` holds, no capacity claim, zero submit buttons.

At **390** in the refusal state: 390 / 390, zero clipped, zero off-screen, zero controls under 44 px.

**Origination is done.** Nothing open on this flow.

---

## Pass 16 — T1–T4 re-checked, and one of them was answered better than it was asked

Driven live after a reload. **Three fixes are clean, the fourth introduced a new defect, and the first one is worth reading rather than just ticking.**

### T1 · Closed, and the fix is better than the brief asked for

I said to rank by the documented heuristic — *"shortest recent scout-response time for the field's zone."* The build did not, and was right not to:

> *"2 people can be sent to Muda A2. **Nothing here records how quickly each of them answers, so they are not ranked** — the open checks on each row are context, not an order."*

Header now reads `IN NAME ORDER`.

**That is the correct answer and I had not reached it.** The docs specify a ranking over data the platform does not hold. The two available responses were to substitute a different measure and call it a ranking — which is what the previous build did, and what produced the original error — or to **refuse to rank and say why**. The second is the product's own habit, and nobody had yet applied it to a sort order.

Worth carrying: **a ranking is a claim.** Where the measure that would justify it does not exist, name order plus visible context is honest and a substituted key is not.

### T2 · Closed

*"already at capacity"* is gone from the summary. Rows now read *"4 open checks · carrying a lot"* — a description, not a threshold verdict. No cap is claimed anywhere on the surface.

### T4 · Closed

All sixteen buttons in the drawer are `type="button"`. Enter in the sample-size field no longer fires the close action.

### T3 · Mechanism fixed, and it opened T5

The row is no longer a dimmed dead control. Selecting `KADA E3` now refuses in words, in a `role="status"` node, and the wording is good:

> *"Nothing can be walked on KADA E3 right now. Main Season 2 · 2026 is declared and goes in on 1 Jul. There is no crop on this ground yet, so there is nothing for a check to be about. **It becomes possible the day the season opens.**"*

That last clause is the right instinct — a refusal that names when it stops being one.

### T5 · But the refusal destroys the list it was chosen from

After selecting the closed-cycle field, the drawer contains **the refusal and nothing else**. Measured: `1` button, and it carries no text — the close icon. The field list is gone.

| | Ineligible **person** | Out-of-cycle **field** |
| --- | --- | --- |
| List survives the refusal | **Yes** — 16 buttons, both groups still rendered | **No** — list destroyed |
| Can pick another without restarting | Yes | **No** |
| Escape route | `Cancel` · `Save as a draft` | One unlabelled icon |

**Same drawer, two answers to the same question — and they have swapped which one is worse.** Before this pass the field was silent and the person refused well; now the field refuses well and strands the reader. The person case is the pattern: **refuse beside the list, not instead of it.**

A reader who mis-taps one row of five loses the whole flow.

### Unchanged and still passing

At **390** the drawer holds: body scroll width 390 / 390, zero clipped text, zero elements past the viewport, zero controls under 44 px — measured in the refusal state, which is the narrowest it gets.

---

## Pass 15 — task origination, driven end to end

Built from `TRACK-B-7-task-origination.md`. A check was created from a finding on Muda A2, assigned to Hasan Aziz, and landed on the board. **Sixteen of seventeen acceptance tests pass.**

### What works, and three pieces of it are better than the brief asked for

Test 1, 3, 5, 6, 7, 8, 9, 10, 14, 16 and 17 all pass as written. Specifically: the drawer never opens empty; the brief is written from the finding, not from a template; there is **no due-date input** anywhere and the window is derived (*"Already past its window by 2 days. The 1-day window opened on 12 Jun and closed on 13 Jun."*); a draft saves with no assignee (*"Still missing: somebody to walk it. It can be saved as a draft as it stands."*); send commits with a 5-second undo; the check lands under **Waiting to be accepted** and the count moves 3 → 4. At **390** the drawer is single-column with zero overflow, zero clipping and no control under 44 px. A grep of the rendered copy for twelve spec terms — `assignee_id`, `field_id`, `source`, `mitigation`, `draft`, `tsk_`, `fnd_`, `alert`, `risk card`, `provenance`, `enum`, `severity band` — returns **zero hits**.

**Three things the build got right that were not specified:**

**It refused a spec it could not honestly meet.** The docs ask for *"GPS points — centroids of the affected zones."* The platform does not hold centroids. Rather than fabricate them:

> *"Ranked by how far each is from the field's own band, which is the walking order. The platform holds an area and a reading for a zone and no centroid, so the stops are named rather than given as coordinates."*

**The ineligible-person refusal names the access, not just the fact:**

> *"Faridah Omar cannot see this field at all: their access covers Ladang Perak Hulu, Ladang Johor Selatan only."*

**The escalation sentence carries the clock rule and the fallback together:**

> *"This band pages the on-call cover if nobody accepts within two hours of the message reaching them — the clock starts on delivery, not on this click. Nobody holds on-call on this estate, so it would fall to Siti Rahman instead, and the record says it took the fallback route."*

**P4 from pass 14 is also fixed.** The board now reads *"Open a check to move it — every move is reachable that way, on any screen. On a wide screen a card can be dragged instead."* Tap path first.

### T1 · The candidate ranking uses the heuristic that was explicitly discarded

The picker header reads **`FEWEST OPEN CHECKS FIRST`**.

The docs say something else, and the brief quoted it: *"the three teammates with the **shortest recent scout-response time** for the field's zone."* The brief also recorded fewest-open-checks as **the error from an earlier round** — it put everyone who could not see the field at the top with zero.

Both statements were in the file and neither was used. These are different measures: response time is how quickly somebody answers, open-check count is how much they are already holding. The second is useful **as context in the row**, which is where it already appears — *"Hasan Aziz · 3 open checks"*. It is not the sort key.

### T2 · Two answers to the capacity question, one screen apart

| Where | What it says |
| --- | --- |
| Summary, above the list | *"All 2 people who can go are **already at capacity**."* |
| Detail, on selecting Hasan | *"How much one person is carrying is a workload question rather than a safety one, so this warns and does not refuse. **There is no cap here to state.**"* |

**The detail is exactly right** and matches §6 of the brief, which said not to invent a threshold. The summary invents one anyway: Hasan holds 3 and Ali holds 4, both flagged, so the implied cap is 3 or fewer — a number that exists nowhere in the docs.

Drop the capacity verdict from the summary. The counts are already in the rows and the reader can judge.

### T3 · The closed-cycle row is visually disabled and not actually

`KADA E3` — *"Main Season 2 · 2026 goes in on 1 Jul — nothing is growing yet"*.

| Property | Value |
| --- | --- |
| `cursor` | `not-allowed` |
| `opacity` | `0.7` |
| `disabled` | **false** |
| `aria-disabled` | **absent** |

Clicking it produces nothing — no state change, no refusal, no status message. A sighted user sees a dead control; a screen reader is told it is an enabled button and gets silence.

**The reason is already on the surface**, in the row itself, so this is a small fix: `aria-disabled="true"` plus `aria-describedby` pointing at that sentence. Not `disabled`, which removes it from the tab order and takes the reason with it.

**Note the inconsistency inside one drawer.** An ineligible *person* is fully selectable and then refused in words, naming their access — which is the product's habit and is better. An out-of-cycle *field* is dimmed and silent. **Two answers to "how do we say no", eight inches apart.** Make the field behave like the person.

### T4 · Every control in the dialog is `type="submit"`

All twenty-one, including `Cancel`, `Close without sending`, the four `Where this came from` trace buttons and the symptom chips. Pressing Enter in the sample-size field fires the first submit in the form, which is `Close without sending`.

Everything except the send action should be `type="button"`.

---

## Pass 14 — the phone layout, measured at 390 · 834 · 1440

All sixteen screens, driven live after a reload. Element counts ranged **491 to 1738** and differed per screen, so nothing here is the chrome measured before render.

### What passes

| # | Test | Result |
| --- | --- | --- |
| 1 | `body.scrollWidth === clientWidth` at 390 | **390 / 390 on all sixteen.** Was 770 / 390 |
| 2 | No text element clips | **Zero**, all sixteen |
| 3 | Nothing renders past the viewport | **Zero**, all sixteen |
| 4 | The rail collapses on width, not on a click | **Yes.** Bottom bar — `Checks · Needs you · The board · More`, 98 × 56 — renders at 390 and **not at 834 or 1440** |
| 5 | Every tappable control ≥ 44 px | **Zero under.** `Reset demo` is now 119 × 44 |
| 6 | No refusal truncated | **Zero.** The only two ellipses in the product are a sha256 digest and a `Select…` placeholder |
| 8 | No chart rendered below its readable width | **Zero charts at 390** on the refused screens. Work done renders 12 at 834 and 1440 and none at 390 |
| 10 | Every board transition reachable by tap | **Yes.** Single column, all ten cards carry `Open the check` |
| 11 | No regression at 834 or 1440 | **None.** Zero page overflow, zero clipping, refusals absent, charts back |
| 12 | Colour still passes | **Zero AA failures** measured at 390 and at 1440 |

**`Work done` was built as decided, and the wording is right:**

> *"The timeline needs a wider screen. Every piece of work is drawn against a date axis, and at this width one day is about four pixels — a bar that narrow cannot be read, and cannot be tapped either. The same 33 entries are listed below, in the same groups, with each date written out rather than measured off an axis. The window, the grouping and the filters above still decide what is in it."*

That is the refusal pattern working: name what is withheld, keep everything that survives, say the controls still govern it. **`Field checks · phone`**, the worst screen before this pass, is now clean on every test.

### P1 · Two of the five refused screens give no verdict

Test 7 is *refuse in words **and still give the headline verdict***. Three of five do:

| Screen | What stands where the verdict should be |
| --- | --- |
| **Estate group** | *"Muda Granary North needs attention"* — **correct** |
| **Yield production** | Names the boxplot and why five boxes cannot be read — **correct in kind** |
| **How a figure is built** | *"Every figure here can say where it came from"* — acceptable; the screen has no verdict to give |
| **National overview** | Only the shell's coverage caveat |
| **Regional forecast** | The **same** shell caveat |

A caveat is not a verdict. That the sentence is identical on both is the tell — it belongs to the shell, not to either screen. The brief's own worked example is *"This view needs a wider screen. **Six fields need a decision today** — open it on a desktop to see why."* That verdict exists in the product; it is on `Overview`. At 390 the National overview currently tells an estate manager nothing about the state of the ground.

### P2 · A caveat that outlived its figures

> *"One scheme has not reported since 5 June. Every reading below is from the other nine."*

**There are no readings below.** They have been refused. This is the same failure as a refusal outliving its cause — `CONVENTION-scope-and-density.md` Part 1, rule 3 — one hop sideways: the condition that made the sentence true is the presence of the figures, and the figures are gone. Attach the coverage gap to the verdict, or drop it at this width.

### P3 · The shared refusal is three sentences and one of them is the one that is needed

> *"What it refers to is not on this screen at this width. Open it on a desktop to read it — nothing is hidden from you here, it does not fit honestly at 390, and a figure that no longer lines up with its label is worse than one you have to come back to."*

Three problems, each already paid for once:

- **"What it refers to" has no antecedent.** On the National overview the preceding sentence is about a scheme that has not reported, so the phrase reads as pointing at that.
- **"at 390" is spec vocabulary on a product surface.** A pixel figure is a spec number and a reader does not know what 390 is. Same class as `Track C Brief 3`, cut once already.
- **"a figure that no longer lines up with its label is worse than one you have to come back to"** is the **rule**, not this instance. `CONVENTION-scope-and-density.md` Part 2 sorts that to trace or docs. Same shape as the pineapple refusal's trailing clause, also cut once already.

`Yield production` shows the fix: it names *this* screen's chart and *this* chart's reason. One sentence, screen-specific, no rule and no pixels.

### P4 · The layout refuses the charts and keeps the desktop instructions

One finding in two places.

**`Work done`**, rendered at 390, above a refusal that says there is no timeline:

> *"Drag to pan, scroll to zoom, shift-scroll for the rows, `Today` to come back. Ticks are monthly here. Click a row label to narrow to it; **hover a bar** to trace what it came from."*

No bars, no hover, no timeline. The lead-in also still reads *"drawn against when it happened"*.

**`Where the work is`**: *"Drag a card to start a move."* HTML5 drag does not work on touch, and all ten cards remain `draggable="true"` at 390. The tap path exists on every card, so §7's floor is met — but the copy points at the path that does not work and not at the one that does.

### P5 · Fourteen figures on Fields workspace are hover-only

Twenty-one native `title` tooltips at 390, **fourteen of them on elements with no visible text at all** — bare stacked-bar segments:

> `title="Rubber · 276.0 ha of 576.6 ha · 2 fields"` · `title="2 fields below their band · 124.2 ha"` · `title="1 field above their band · 3.1 ha"`

Two reasons this is worse than an ordinary tooltip miss:

- **Band position is on the Visible list by name** in `CONVENTION-scope-and-density.md` Part 2. It is not tooltip-eligible content on any screen.
- **A native `title` does not open on tap or on focus.** So it also misses the desktop tooltip contract — hover **and** focus **and** tap — which makes this a defect at 1440 too, not only at 390.

### Recorded, not a defect

At **834** the Fields workspace table is **1040 px inside a 514 px scroller**, so 304 elements sit past the right edge, reachable by scrolling sideways within the card. Notification preferences does the same at 640 into 498. Page-level 834 is clean and the brief put 834 out of scope, so this is pre-existing — but roughly half the fields table is behind a horizontal scroll on a tablet.

---

### H6 · One disabled control in the product says nothing

**Zero dead controls.** Every button, tab, option, link and input across eight screens has a live handler, checked on the element and three ancestors. The concern Phase 11 was written around does not exist.

**And the disabled controls almost all explain themselves**, which is the product's habit working:

| Control | What it says on hover and to a screen reader |
| --- | --- |
| Fields workspace, seven controls | *"A viewer can read every field on this screen and record nothing against it."* |
| `Zoom out`, `Whole estate`, `Undo the last corner`, `Clear the outline`, `Import a boundary` | Each names itself |
| `Today` on Work done | Self-evident — already at today |

The Fields workspace line is exactly right: it names the **role**, not the button, so a reader learns why *everything* is off rather than why one thing is.

**The exception is `New estate` on Set up a field.** No `title`, no `aria-label`, no `aria-describedby`, nothing in the surrounding text — hovering it and reading it aloud both produce the same thing: the words *"New estate"* and no reason.

This is the silent-refusal defect, and it has bitten before: an earlier session lost time to *"right now I can't create new estate"* with no explanation on screen. Whatever the rule is — role, entitlement, or not yet built — say it, in the pattern the same screen already uses five times over.

### H5 · There is no phone layout, and it is one number rather than many

Widths used: **1440 · 834 · 390.** No breakpoints are recorded anywhere in the repo, so these are desktop, tablet and phone and the figures below are stated against them.

**834 px is clean.** Zero overflow, zero clipped text, zero off-screen elements across National overview, Regional forecast, Yield production, Where the work is and Work done. Whatever the layout does, it does it well down to tablet.

**390 px overflows by exactly 380 on every screen.** Exactly — the same number on the national overview, the board, the fields workspace and the form. A constant is the signature of a fixed floor, not of content:

| | Width |
| --- | --- |
| Viewport | 390 |
| Body scroll width | **770** |
| Navigation rail | 256, fixed |
| Main column, effective minimum | ~514 |

**The rail never collapses on its own.** Collapsing it by hand takes it 256 → 56 and the body from 770 → 570 — still 180 px past the viewport. So even with the rail out of the way there is no phone layout underneath; the main column alone will not go below roughly 514.

**There is no responsive breakpoint in the product.** The rail is a manual toggle, not a media query, and nothing reflows.

**Where this bites hardest:** the screen named **`Field checks · phone`** — the one surface explicitly for a phone — overflows by 380 px and clips ten labels, including *"Saw something that is not on t…"* and *"P1 · 1 stop · 8 photos · still…"*. A scout in a field is the only user guaranteed to be on a handset.

**Not a defect, but recorded:** `Reset demo` is a 30 px control. That is the design system's smallest size and correct on desktop, but it is below a comfortable touch target if a phone layout ever arrives.

### H3 · Every severity chip in the product fails AA. Not one passes.

The sweep is now complete across all sixteen screens, and this is the headline. White text on the chip fill:

| Chip | Fill | Ratio | AA needs |
| --- | --- | --- | --- |
| **Watch** | amber `rgb(245,158,11)` | **2.15 : 1** | 4.5 |
| **Low** · **For info** | blue `rgb(91,155,213)` | 2.96 : 1 | 4.5 |
| **Urgent** · **High** | red `rgb(239,68,68)` | 3.76 : 1 | 4.5 |

**The whole scale, every value, on every screen that carries one** — Overview, Estate group, What needs you, Notification preferences, Field checks · phone. *Notification preferences* renders the entire ladder together, which is where it is most obvious.

**2.15 : 1 is barely above the 1 : 1 of invisible.** White on amber is the classic failure: amber is already near-white in luminance, so white text has nowhere to go. Blue at 2.96 is no better in kind.

These are the marks that tell a reader **how bad something is**, and they are the first thing the eye lands on. That the entire severity ladder is illegible is not five defects — it is one decision, made once, that white sits on every chip.

**Fix at the token, not the chip.** Dark ink on amber and on blue. Red can keep white only if it darkens enough to clear 4.5, and it is easier to make all three consistent: ink on fill, one rule, no exceptions.

### H4 · Red on the inverted panels, 4.44 : 1, on nine screens

`rgb(239,68,68)` on the dark panel `rgb(26,31,28)` — which appears **in light mode**, on inverted cards. Under AA by a hair, everywhere, and it carries the sentences a reader acts on:

| Text | Screen |
| --- | --- |
| `−0.10`, `−0.03` — negative deltas | Fields workspace |
| *"2 past the window you had"*, *"overdue by 2 days"* | What needs you |
| *"Not confirmed"* | Proof packs, Notification preferences |
| *"Waiting too long"*, *"Off plan"* | Application plans |
| *"overdue by 4 days"*, *"6 points"* | Field checks · phone |
| The band sentences, *"Stays down"* | How a figure is built |

**Every one is a thing that has gone wrong.** The colour reserved for problems is the one colour that does not quite clear the threshold — so the worse the news, the harder it is to read. It misses by 0.06, which makes it a one-token fix rather than a redesign.

G1–G5, E1–E5, D1–D4, F1 and F2 all closed. See passes 7 through 11.

### H1 · The refusals are the least legible text on the screen

`rgb(217, 119, 6)` on white gives **3.19 : 1**. WCAG AA wants **4.5 : 1** for text at this size, and every instance is 13px.

What wears it:

| Text | Screen |
| --- | --- |
| *"NDVI has saturated on this ground — above 0.80 it stops discriminating…"* | National overview |
| *"Ladang Johor Selatan holds one block with more than one open batch…"* | National overview |
| *"Recovery is measured; the value is not"* | National overview |
| *"Nothing declared in this window"* × 2 | Yield production |

**Every one is a refusal or a caveat.** This product's signature is *refuse rather than mislead*, and *a refusal is an answer and renders as one* — and the refusals are rendered in the lowest-contrast colour on the surface. The sentences the whole design rests on are the hardest to read.

Darken the token, or move refusals to body colour and carry the meaning on the icon and the label rather than the text colour. The second is probably better: colour is doing semantic work here that a 13px amber cannot support.

### H2 · The band disclosure is coloured red; the actual exception is not

| Sentence | What it is | Colour, light | Colour, dark |
| --- | --- | --- | --- |
| *"Five bands resolve for paddy — 0.55–0.75 at tillering on 412.6 ha…"* | A **disclosure of method** | `rgb(192,57,43)` red | `rgb(239,68,68)` red |
| *"KADA Blok W4 has not reported since 5 June"* | An **actual exception** | `rgb(26,31,28)` body | body |

**The alarming colour is on the honest disclosure and the neutral colour is on the real problem.** A reader scanning for what is wrong is pulled to three red paragraphs that say the platform declined to average a band — which is the platform working correctly — while the scheme that has gone dark reads as ordinary text.

The band sentence is the product being careful. It should read as careful, not as broken.

In dark mode this compounds: the red resolves to `rgb(239,68,68)`, which is the **same hue as the exception-row tint** `rgba(239,68,68,0.12)` used behind *"KADA Blok W4 has not reported"*. So the disclosure text and the alarm background share a colour. It also lands at **4.44 : 1**, marginally under AA.

**Not a defect, recorded so it is not re-raised:** dark mode does layer its surfaces correctly, via `rgba(255,255,255,0.06)` and `0.07` overlays plus tinted `rgba(239,68,68,0.12)` and `rgba(245,158,11,0.14)`. My first measurement said the background was flat at `rgb(26,31,28)` everywhere — that was my contrast walker skipping backgrounds below 0.5 alpha, not the build. Checked before reporting.

---

One thing recorded rather than opened: the *"Nudge Ali Ismail, or reassign it"* line on the accept refusal is prose, not controls — no handler on it or its ancestors. That satisfies what `TRACK-B-6` asked for (offer the moves in words) and going further was never specified. Worth deciding deliberately if the board grows: a named move the reader cannot take from where they are told about it is half an offer.

### G1 · The undo is promised and absent

The page says, at the top:

> *"…taking someone off commits at once, **with an undo**."*

Verified on a genuine unassign — a card from *Waiting to be accepted* dropped on *To do*, checked 600 ms later, well inside any 5-second window. **No undo button, no undo text, nothing.** The move commits and cannot be taken back from the board.

This is the one transition the guardrails specifically require it for: `/guides/field-scouting` puts *reassign-before-acceptance* among the actions that "commit optimistically with a 5-second undo".

*(Checked twice. My first attempt accidentally dropped a To-do card back on To do, which is a reorder — that produced a refusal rather than a move, and would have been a false finding. The result above is from a card genuinely in Waiting.)*

### G2 · The picker ranks the ineligible above the eligible

Dragging To do → Waiting opens **Who should walk this?**, headed `FEWEST OPEN CHECKS FIRST`, ordered correctly ascending:

| Person | Note shown | Open checks |
| --- | --- | --- |
| Rahim Yusof | No working role on this estate | 0 |
| Ravi Kumar | Cannot see this field | 0 |
| Faridah Omar | Cannot see this field | 1 |
| Mei Ling Tan | Cannot see this field | 2 |
| Hasan Aziz | Cannot see this field | 3 |
| **Ali Ismail** | **4 open checks · at capacity** | 4 |

**Five of the six cannot be assigned, and they are all ranked above the one who can.**

The ordering is doing exactly what was asked and the result is upside down: **someone who cannot see the field has zero open checks on it by definition**, so ineligibility sorts to the top. This is the open-checks decision backfiring against entitlement, and it is mine — the earlier round chose that ordering without checking how it interacts with the access filter.

**Fix:** eligible candidates first, ordered by open checks among themselves; the ineligible below, or summarised as a count with reasons. The product's habit is to refuse visibly, not to rank the refused.

**And say the real answer when there is one.** Here it is *"the only person who can go is at capacity"* — which is a fact the reader has to assemble from six rows. State it.

### G3 · The accept refusal explains but does not offer

> *"Only Ali Ismail can accept this. Acceptance is the assignee's act. If a manager could accept on a scout's behalf, the two-hour escalation on a critical check could be stopped by someone who is not going to the field."*

The reasoning is right and well put. But `TRACK-B-6` §4 and test 7 ask it to **offer nudge or reassign**, and every other refusal on this screen ends with a move — *"Open the check to add one."* This one ends with a lecture.

### G4 · Completion cannot be tested on this seed

**In flight holds zero cards** — *"Nobody is out on a field right now."* So there is nothing to drag into Done, and the completion confirmation cannot be exercised at all.

That is the most contested decision in the brief: with a report the confirmation names the risk-model consequence, without one it names the absence — *"No report, so nothing here reaches the risk model."* Neither branch has been seen.

**Seed two accepted checks**, one with a completed report and one without. Until then the report/no-report split is written but unverified.

---

### F1 · Oil palm groups by scheme, and Felda Wilayah J2 holds two age classes

E1 was fixed on rubber and not on oil palm. The card still reads *"One box per scheme"* and *"Oil palm per hectare, by scheme"*, and Felda Wilayah J2 renders as **one box over 47 fields** with **one rate**.

**It is provable from the platform's own two surfaces that J2 is mixed:**

| Where | What it says |
| --- | --- |
| National overview, oil palm | *"Fields here are spread across two stages — **43 at prime, closed canopy and 6 at young mature**"* — 49 fields |
| Yield production, trace | Ladang Sabah Timur = **2 fields** |

Six young-mature fields cannot fit inside a two-field estate, so **at least four of them sit inside Felda Wilayah J2.**

And the comparison card hardens the wrong assumption rather than disclosing it:

> *"**The stand** is 14 years old in one year and 13 in the other."*

Singular. A 47-field scheme holding both prime closed canopy and young mature is asserted to have one age.

**Why it is the same defect as E1, not a smaller one.** Young mature has not closed canopy — that is what the phase name means — so it carries materially less FFB per hectare than prime by design. J2's box therefore measures **age alongside performance**, exactly as the rubber box measured tapping rhythm. `CLAUDE.md` already names this: *"An estate mixing the two is not underperforming, it is young."*

**And unlike rubber, nothing is hiding it.** Perak Hulu's box was suppressed at n = 2. J2's n = 47, well above k, so the box renders and the spread is on screen now.

Fix is E1's, one crop over: group by scheme × tree-age cohort. The rubber wording transfers almost unchanged — *a row is one cohort, not one scheme; the total above is unaffected, production adds up whatever the age.*

### F2 · Rubber row labels order their parts two ways

```
Ladang Perak Hulu · Panel B · d/3 · Production Year 2026
Ladang Perak Hulu · Panel A · d/2 · Production Year 2026
Felda Wilayah P6  · Production Year 2026 · Panel C · d/2
```

Scheme · panel · cycle · year on two rows, scheme · year · panel · cycle on the third. Introduced by the E1 split. One order.

### E1 · Rubber groups by scheme, and a scheme holds two tapping cycles

Ladang Perak Hulu reports as one row:

> *Production Year 2026 · Panel B · d/3 · Production Year 2026 · Panel A · d/2 · Perak · 2 fields declared · 276.0 ha cut* — **408 kg/ha**, 112,600 kg

Felda Wilayah P6, beside it, is a single cycle: Panel C · d/2 — **222 kg/ha**.

**d/2 taps every second day; d/3 every third.** A d/2 stand out-yields a d/3 stand per hectare by rhythm alone. Perak Hulu's 408 kg/ha is a rate computed across both, and placing it beside P6's 222 invites the reader to a comparison where a large part of the gap is tapping schedule rather than performance.

This is the **structural-gap-as-performance-gap** defect — the same class as Main against Off season for rice, which the product already refuses by name. `CLAUDE.md` states the rule: *rubber comparison requires the same panel **and** the same tapping cycle.*

**It is latent in the boxplot too, and only hidden by luck.** The Perak Hulu box is withheld today because n = 2 is below k. At five fields or more it would render, and it would be measuring tapping rhythm under a heading that asks *"Is rubber coming off evenly"*.

**Fix:** for rubber, the grouping key is scheme × panel × tapping cycle, not scheme. Two cycles on one estate are two rows.

### E2 · Boxplot axis ticks are data-derived, with the decimals shifting mid-axis

| Window | Ticks |
| --- | --- |
| This window | 3.36 · 5.68 · **8.00** · **10.3** · 12.6 |
| Last completed | 8.80 · **13.2** · 17.5 · 21.9 · 26.2 |

Three significant figures, so precision changes partway along a single axis. Round ticks were fixed on every other chart in the product two passes ago; the boxplot missed that pass. The cumulative-line axes on the same screen are already correct — `0 · 5,000 · 10,000 · 15,000`.

### E3 · "Fields per band" collides with the product's most load-bearing word

The histogram under each box is captioned **`FELDA WILAYAH J2 · FIELDS PER BAND`**, and the note reads *"how many fields fall in each band"*.

**"Band" means the agronomic target range for a crop at its stage.** The whole nominal-is-best argument runs through that word, and four build rounds went into teaching the product not to collapse one. Here it means a histogram bin. A reader who has learned the first meaning will read the second as fields-per-health-band.

Call it a bin, a step, or *"fields per t/ha step"*. Anything but band.

### E4 · The window label concatenates

> *Production Year 2026 · Panel B · d/3 · Production Year 2026 · Panel A · d/2 · Perak · 2 fields declared · 276.0 ha cut*

The year appears twice, two distinct groupings are joined by the same middot used *inside* each grouping, and the region and counts follow on the same separator. Unreadable as one line. Fixing **E1** removes most of it, since each cycle becomes its own row.

### E5 · The tile subline repeats verbatim as the card headline

*"13,860 t FFB from 43 of 49 fields with a harvest declared, to 31 May"* appears on the crop tile and again, identically, on the card below it. Same for rubber. One duplicate per crop.

---

### D1 · `One has no growth stage resolved`

**Oil palm, standing alone:**

> *"One has no growth stage resolved, so the estimate is read against the crop-wide curve there."*

One what? Field, block, scheme, estate?

**Paddy, concatenated:**

> *"One scheme has not reported since 5 June, so the range is wider than usual **and One** has no growth stage resolved, so the estimate is read against the crop-wide curve there."*

Capital `O` mid-sentence, two independent clauses joined by *and*, and the reader inherits "scheme" from the first clause — which is wrong. The unresolved ground is 9.9 ha inside Muda Granary North, not a whole scheme.

The fragment is written to open a sentence and is being concatenated into the middle of one. Two sentences, with the noun named:

> *"One scheme has not reported since 5 June, so the range is wider than usual. One field has no growth stage resolved, so the estimate there is read against the crop-wide curve."*

### D2 · Oil palm's caveat has nothing behind it

The regional forecast says oil palm has something with no growth stage resolved. The national overview, **same crop, same scope**, accounts for all of it: all four bands carry a stage, none uses *"with no stage resolved"*, and the distribution sums 43 + 6 = 49 with no remainder.

Paddy is consistent across both surfaces. Rubber and pineapple say the opposite outright. Oil palm is the only crop where the two disagree.

Either the overview is not disclosing unresolved ground, or the caveat is firing on the wrong crop. **Third instance of this class** — two Track C surfaces giving different accounts of one crop at one scope.

### D3 · "Farm owner" in the role selector

There are eight functional roles plus `viewer` as an org role. That set is closed. **"Farm owner" is a persona** — it appears in audience descriptions because that is how the market talks. It has no entitlement scope, no raising ceiling, no acknowledgement rights, and no row in any table in the docs. A selector offering it is offering a role the platform cannot resolve, and whatever it currently resolves to is a silent default nobody chose.

Remove it. If a demo needs an owner-shaped persona, map it explicitly to `estate_manager` or `viewer` and label it as the mapping it is.

### D4 · The two 17s

*"Fields at serious or urgent"* reads 17. *"Findings acted on inside their window"* also reads 17. Different quantities, different denominators — fields versus findings — and no reason to coincide.

They may be genuinely equal on this seed, in which case the fix is to make that visible rather than to change the number: the two traces should show different inputs and different rules. If they do not, one is reading the other's rollup. **Check the call path, not the output** — a coincidence in a demo seed is how a shared reference survives review.

---

## 10 August — pass 13, colour closed. Five of six.

**Zero contrast failures anywhere.** Re-measured Overview, National overview, Notification preferences, Fields workspace and What needs you — 114 to 277 text elements each, light mode. Not one element under threshold. Previously these five carried the whole H1/H3/H4 set.

**H3 was solved better than I specified.** I asked for dark ink on the existing solid fills, and flagged that red would still fail at about 4.0 because `rgb(239,68,68)` is too dark to take ink. The build sidestepped that entirely — **ink on a 15 % tint** rather than ink on a solid:

| Chip | Text | Fill |
| --- | --- | --- |
| Urgent · High | `rgb(26,31,28)` | `rgba(192,57,43,0.15)` |
| Watch | `rgb(26,31,28)` | `rgba(217,119,6,0.15)` |
| Low · For info | `rgb(26,31,28)` | `rgba(0,117,255,0.15)` |

The tint barely darkens the surface, so ink clears comfortably on all three — and **one rule now governs all five chips**, which was acceptance test 2. The constraint I could not resolve was dissolved by changing the shape of the answer rather than the value.

**H6 closed, and the sentence is right.** `New estate` now carries:

> *"Creating an estate is an administration action, and this account is signed in as estate manager. Ask an estate administrator, or an organisation administrator, to create it."*

It names the rule, names the role, and names who to ask — the pattern its neighbours already used, applied properly.

**H1, H2, H4 closed** — no refusal, disclosure or status text now fails, and no disclosure renders in the alarm colour.

**H5 untouched, correctly.** At 390 px: body scroll width still 770, overflow still exactly 380, rail still 256. `Field checks · phone` still clips fifteen labels. The brief said do not attempt this in the same pass; it was not attempted. **This is the whole of what remains of Phase 11.**

## 10 August — pass 12, Phase 11 begins: dark and light across Track C

**Method, because it is not the one the brief assumed.** Phase 11 was written as a visual sweep. Screenshots turn out to be the wrong instrument for most of it — the questions that matter are *what colour is this, against what, at what size*, and those are computed values. So: contrast ratio measured for every text-bearing element on each surface, in both themes, against its effective background, with the AA threshold applied by font size and weight.

117 text elements measured per screen. Both themes.

**Dark mode is broadly sound.** Surfaces layer correctly through low-alpha overlays, the exception tints carry through, and only one colour falls below AA and only marginally.

**The failures are in light mode, and they are semantic rather than cosmetic** — see H1 and H2. Both were invisible to eleven passes of DOM reading because the text was correct; only the colour was wrong.

**Regional forecast and Where the work is are clean** on this pass — no warning-coloured text at all.

**Opened:** H1, H2, and on the wider sweep H3 and H4. The width pass opened **H5**; the dead-control pass opened **H6** and otherwise came back clean.

**Phase 11 is complete.** Three concerns swept — colour in both themes, three widths, dead controls — across all sixteen screens.

**The shape of the result matters more than the count.** Six findings, and not one of them is a screen-by-screen problem:

| | What it actually is |
| --- | --- |
| H1, H3, H4 | **Three colour tokens** used everywhere |
| H2 | **One semantic choice** — what colour a method disclosure wears |
| H5 | **One missing layout** — no breakpoint exists at all |
| H6 | **One control** missing the sentence its five neighbours already have |

There is no long tail. A brief that reads "sweep sixteen screens" would have produced sixteen small fixes; what is actually needed is four token changes, one layout, and one sentence.

**And the thing that did not need fixing:** zero dead controls, and the disabled ones explain themselves in the reader's language — *"A viewer can read every field on this screen and record nothing against it."*

**Widths: 1440 · 834 · 390, chosen because none are recorded.** If the design system names breakpoints, these figures should be restated against them. The app frame is resizable from the parent document, so the measurement is of the real viewport rather than a zoom.

**Coverage: complete. All sixteen screens measured**, light mode, 32–277 text elements each. The inference in the first draft of this entry turned out to be right and then some — the unmeasured six carried the same failures, plus **two chip colours the first ten never showed**: `Low` and `For info` on blue at 2.96 : 1, visible only where the full severity ladder renders together on *Notification preferences*.

**Only one screen is clean: `Set up a field`.** It is also the only screen carrying no severity chip and no red status text, which is the point — the failures are not distributed, they are two tokens used everywhere.

**Two method notes, both my errors, both caught before reporting.**

**A 900 ms settle is too short.** A first batch reported six screens as clean with an identical 32 elements each — that was the chrome, measured before the screen rendered. At 2,200 ms the same screens probe 124–192 elements and three of them have failures. Every "clean" from that batch was discarded. **Use 2 s minimum, and treat a suspiciously uniform element count as a rendering artefact rather than a result.**

**The theme resets on reload.** Two batches were measured after the preview had silently reverted to light, which nearly produced a report mixing themes. Read `body` background before trusting any colour result — light is `rgb(248,249,250)`, dark is `rgb(26,31,28)`.

## 10 August — pass 11, the board is clean. Nothing open.

**G1 closed, and it answered a question the brief deliberately left open.** Unassigning now produces an **Undo** control with:

> *"Undo puts it back with Ali Ismail. Both moves stay in the record."*

`TRACK-B-6` §6 said the audit treatment of an undone move was an audit-contract question, not a board question, and told the build to stop and ask rather than decide it. It decided — **the undo leaves a trace** — and said so on the surface where the reader can see it. That is the right answer and the right place to state it. Verified working: To do 4 → 3 and Waiting 2 → 3 on clicking it.

**G3 closed.** The accept refusal now ends *"Nudge Ali Ismail, or reassign it."* — the reasoning kept intact, the moves added after it.

**G5 closed.** All ten draggable cards compute `cursor: grab`.

*(A note on method: my first read of the G3 fix returned "none" because my DOM query did not reach far enough up from the status node. The text was there. Checked again before reporting — this is the third time in this project that a narrow selector nearly produced a false regression.)*

## 10 August — pass 10, two of four closed

**G4 closed, and it unlocked the decision nobody had seen.** In flight now holds two checks, so the completion confirmation rendered for the first time — both branches, exactly as specified.

**With a report:**

> *"Complete this check? The report filed on 14 Jun covers 2 stops and 5 photos on Muda B1. Completion is append-only. **WHAT THIS SETS OFF** — The report found no symptom, so completing this raises a disagreement with the rule card that fired."*

The consequence is **read from the report, not guessed**, and `raise_contradiction` reaches the reader as *"raises a disagreement"* — the labels file doing its job.

**Without a report:**

> *"Close this check with nothing filed against it? No visit and no photos have been recorded on Muda C4. Completion is append-only, so past this there is no undo — the check reads as done with no evidence behind it. **WHAT REACHES THE RISK MODEL** — No report, so nothing here reaches the risk model. The band stays where it is. Open the check to file what was found first."*

Different title, heavier prompt, and the commit button reads **Close it anyway** rather than **Complete**. The absence is named instead of a consequence, and the better path is offered.

**G2 closed, and better than asked.** The picker now separates eligibility from ranking, and leads with the answer:

> *"All 2 people who can go are already at capacity."*
> `FEWEST OPEN CHECKS FIRST` — Hasan Aziz 3 · Ali Ismail 4
> `4 PEOPLE CANNOT BE SENT TO THIS FIELD` — Faridah Omar, Mei Ling Tan, Rahim Yusof, Ravi Kumar, each with a reason

Eligible ranked, ineligible listed and unranked, and the one-line verdict stated rather than left to be assembled from six rows. Entitlement also resolves per field — Hasan Aziz is eligible on Muda C4 having been ineligible on KADA D1.

**Still open:** G1 and G3, both second time. G5 opened.

## 10 August — pass 9, the board becomes writable

Brief 6 built. **Eight draggable cards, four drop targets, and the refusals are the best copy on the surface.**

The page states the brief's central finding in its own words, which is the right place for it:

> *"Drag a card to start a move. Four of the five open the same confirmation the check does; taking someone off commits at once, with an undo."*

**Every refusal tested lands, and none of them snaps back.**

- **Backwards out of Done** — *"Work already done cannot be moved back. This check was finished on 14 Jun. Completion is append-only — a correction is a new entry, not an edit. Open the check to add one."* Specific to the check, names the rule, offers the move. `role="status"`, so it is announced. The card does not move at all.
- **Accepting for someone else** — *"Only Ali Ismail can accept this. Acceptance is the assignee's act. If a manager could accept on a scout's behalf, the two-hour escalation on a critical check could be stopped by someone who is not going to the field."* The reasoning survived from brief to screen intact.
- **Skipping columns** — *"Nobody has been asked to do this yet. This check has not been sent to anyone and nothing has been walked, so it cannot be marked done. Two steps are missing, and one gesture does not perform both."*
- **Reordering inside a column** — *"The order here is not yours to set. Cards in To do are ordered by what is past it…"* — not asked for by name, correctly refused anyway.

Refusals **clear on the next drag**, not on a timer, as specified.

**Assigning opens the picker rather than completing** — the §2 rule holds. The picker states its ordering, `FEWEST OPEN CHECKS FIRST`, and flags capacity. See **G2** for what is wrong with the ordering itself.

**Unassigning commits instantly** and keeps the history: the card reads *"Taken off Ali Ismail on 15 Jun"*. Counts move correctly, To do 3 → 4 and Waiting 3 → 2. See **G1** for the missing undo.

**No card anywhere asks whether the symptom was found.** Grepped; clean.

**Opened:** G1 through G4. G2 is a decision of mine backfiring, not a build error.

## 10 August — pass 8, F1 and F2 closed. Nothing open.

**F1 fixed, and the split proves the defect was real.** Oil palm now groups by scheme × tree-age cohort, and at the last completed window Felda Wilayah J2 renders as two boxes:

| Cohort | n | Production | Hectares cut | Rate |
| --- | --- | --- | --- | --- |
| Prime, closed canopy · Planted 2011 · Age 14 | 41 | 30,480 t FFB | 1,650.0 | **18.5 t FFB/ha** |
| Young mature · Planted 2019 · Age 6 | 6 | 1,700 t FFB | 214.0 | **≈ 7.9 t FFB/ha** |

**A 2.3× difference between the two cohorts.** The single J2 rate this replaced was 17.3 t FFB/ha — a figure that described neither cohort and made a six-field young stand invisible inside a forty-one-field prime one. That is what the defect was costing.

Two independent confirmations that the split is correct rather than cosmetic:

- **The total holds.** 30,480 + 1,700 = 32,180 ✓, and 32,180 + 2,290 = 34,470 ✓ — unchanged from before the split, which was the boundary condition.
- **It agrees with the other surface.** 214.0 ha for the young-mature cohort matches the national overview's `0.60–0.80 at young mature on 214.0 ha` exactly. Two Track C surfaces now derive the same cohort split independently.

Field counts reconcile: 41 + 6 = 47 (J2) + 1 (Sabah Timur) = 48 of 49 with a completed window ✓.

The copy transferred from rubber as expected: *"A row is one tree-age cohort, not one scheme: young mature palms have not closed canopy and carry less per hectare by definition, so a rate across two ages would read age as performance. The total above is unaffected — production adds up either way."*

**F2 fixed.** Rubber rows now read `scheme · panel · cycle · year` throughout.

**Nothing opened.** One cosmetic inconsistency remains unlogged and does not need a pass of its own: the "coming off evenly" header reads *"One box per scheme and tree-age cohort"* on this window and *"One box per tree-age cohort"* on last completed.

## 10 August — pass 7, all nine closed

**E1 through E5 and D1 through D4 all verified fixed.**

**E1** split correctly and respected the boundary. Ladang Perak Hulu is now two rows — Panel B · d/3 at 83,000 kg and Panel A · d/2 at 29,600 kg — and the card explains itself: *"A row is one panel at one tapping cycle, not one scheme: d/2 taps every second day and d/3 every third, so a rate across both would read tapping rhythm as performance. The total above is unaffected — production adds up whatever the rhythm."* 83,000 + 29,600 = 112,600 ✓, and 112,600 + 21,400 = 134,000 ✓ — the crop total is unchanged, which is the whole point.

**E2** — axis ticks now round and consistent: `2 · 4 · 6 · 8 · 10 · 12 · 14` this window, `5 · 10 · 15 · 20 · 25 · 30` last completed.

**E3** — `FIELDS PER T FFB/HA STEP`. "Band" no longer appears anywhere on the surface. Grepped both windows and all crops: zero matches.

**E4** — resolved by the split, as expected. But see **F2**; the split introduced an ordering inconsistency.

**E5** — the tile now reads *"43 of 49 fields with a harvest declared"* while the card carries the fuller sentence. No exact duplicate.

**D1** — the fragment is gone. Paddy reads *"One scheme has not reported since 5 June, so the range is wider than usual."* and stops.

**D2** — the unsupported caveat is gone from oil palm. Paddy's unresolved 9.9 ha is still disclosed on the national overview, where it belongs; it was never a range-widener, so dropping it from the forecast's coverage line is right rather than lossy.

**D3** — "Farm owner" removed. The selector now offers exactly the seven primary functional roles: Estate manager, Agronomist, Field scout, Approver, Agency officer, Estate admin, Organisation admin.

**D4 — a real bug, found by checking the call path rather than the output.** The two 17s were not a seed coincidence. `FIELDS AT SERIOUS OR URGENT` now reads **16**, with the denominator named: *"A field carrying more than one finding at those bands is one field here — the 17 findings behind these 16 fields are counted where the denominator is findings, which is not this figure."* One figure had been counting findings while claiming to count fields.

**Opened:** F1, F2. F1 is E1 uncorrected on the second crop, and it is the one that produces a misleading number.

## 10 August — pass 6, Yield production (first build)

Brief 4 built. **The two traps the brief was written around were both handled, and one of them properly rather than cosmetically.**

**The boxplot privacy rule holds in the DOM, not just in the copy.** The card says *"No point is drawn for an individual field — a boxplot draws its outliers as separate marks, and an identifiable field on this surface would be a drilldown the rest of Track C refuses."* Checked the SVG: **zero `circle` elements, no `title`, no `aria-label`, no `tabindex`, no pointer cursor.** Six rects and ten lines — box, whiskers, median, histogram bars. Nothing per-field exists to hover.

**Counted and estimated are kept apart.** At the last completed window: *"34,470 t FFB against a forecast of 35,800 t FFB — 1,330 t FFB short"*, then *"34,470 t FFB declared, counted"* and *"35,800 t FFB forecast, 32,910 to 38,690 likely"*. Shortfall in tonnes, not percent. Range symmetric at ±2,890, and the count does sit inside it.

**And it refuses the mid-window shortfall**, which the brief did not ask for: *"Shaping that forecast to today would need the month-by-month production profile this platform does not hold, so no shortfall is stated here. It is stated where a window has closed."* That is the same reasoning that killed pro-rated short horizons in Brief 3, arrived at independently.

Other passes: k refusal on boxes with wording that adapts (*"A box over 1 field is that field"*); n on every box; breadcrumb is a read-only `span` that derives; cross-crop refusal with the unit as its reason at both windows; oil palm comparison refused on tree age (*"14 years old in one year and 13 in the other"*); part-harvested refused against completed; paddy draws no cumulative line because *"a season is cut once"*; the trace carries `Added up · 2 inputs`, `DECLARED THROUGH` correctly adapted from `DATA THROUGH`, exclusions, and the counted/never-zero rule in one sentence with concrete referents — *"a weighbridge ticket, a mill docket, a collection sheet"*.

**Arithmetic reconciles at every scope checked.** Oil palm this window 12,480 + 1,380 = 13,860 ✓ over 41 + 2 = 43 of 49 declared. Last completed 32,180 + 2,290 = 34,470 ✓ over 47 + 1 = 48. Rubber 112,600 + 21,400 = 134,000 ✓, 276.0 + 96.2 = 372.2 ✓. Rates: 32,180 ÷ 1,864.0 = 17.3 ✓, 2,290 ÷ 120.0 = 19.1 ✓, 112,600 ÷ 276.0 = 408 ✓, 21,400 ÷ 96.2 = 222 ✓.

**Opened:** E1 through E5. Only E1 produces a misleading number.

## 10 August — pass 5, Track C

**Fixed.** C1: the cross-crop refusal dropped its stages. C2: the field → estate collapse undone — Felda Wilayah J2's 1,864 ha band split into 1,650 prime + 214 young mature, exactly where the pigeonhole said it had to be.

The split changed the answer, which is how the fix was confirmed real: position by ground moved from *1,960.5 inside · 120.0 below* to *1,742.5 inside · 270.0 below · 68.0 above*. The 150 extra hectares below band are the young palms that had been flattered by a prime-canopy band. It propagated to the forecast — oil palm's range widened to 32,001–44,007 on an unchanged 38,004.

**Regression sweep clean** on both surfaces: no duplicate sentences, no spec vocabulary, all three horizons behave, axis ticks round on every crop, cell traces carry all four spine elements. Narrowing to oil palm withholds the choropleth with a visible refusal and a restated total.

**Opened:** D1, D2.

## 10 August — pass 4

**Fixed.** B1: each band carries its own stage, *"with no stage resolved"* where none does, and the stage spread stated in the row. B2: the per-hectare cell trace gained all four spine elements, including an empty exclusion list that says it is empty.

**Opened:** C1, C2.

## 10 August — pass 3

**Fixed.** A1: bands stopped collapsing between schemes — all four crops read as paddy did, saturation leads the rubber sentence. A2: the Muda Granary North tally reconciles three ways. A3: separator on the input count. A4: the em-dash sentence rewritten. A5: refusal title on the tile, sentence on the card. A6: per-hectare cells open their own trace naming *a ratio of two sums* against the sum above it.

**Opened:** B1 (the stage was never un-collapsed, only the band), B2.

## 10 August — pass 2

**Fixed.** The three priorities: the averaged band on paddy, the pineapple figure published on one surface and refused on the other, and perennial short-horizon figures computed by dividing the annual by the calendar. The §5b national-overview rebuild also landed — one rule sentence in a group header instead of four times, the designer's justification removed, *"every reading below"* replacing *"what follows"*.

**Opened:** A1 through A6.

## 9–10 August — pass 1, the scope and density review

The regional forecast had been rebuilt against `CONVENTION-scope-and-density.md`; the national overview had not. Found the averaged band (provable by arithmetic: area-weighting two children's bands reproduced the parent's exactly), the pineapple contradiction between surfaces, and the pro-rated perennial forecasts. Also the four-times-repeated rule sentence, an exception row with no action, and a verdict whose scope claim was contradicted eight lines below it.

---

## Earlier — the field-creation pass

Kept from `DEFECTS-set-up-a-field.md`, which ran D0a and D0 through D8 against Track B2 and B3. That file is superseded by this one; its content is the record of the field-creation build and its status table was current as of 8 August.

**Notable, because it is the class that keeps recurring:** the name box storing the change event instead of the text, four `Empty` states passing `detail` where the component takes `description`, and a `<label>` with no `for` that made two band-tier selects a single control to anything addressing them by name.

**Three times in that pass I reported the build broken when the fault was mine** — coordinate drift from a screenshot scaled 1568 against a 1512 window, a `fill()` that bypassed the key handler I was meant to be testing, and a standalone Design Component that renders nothing because no parent shell hands it a store. Reload the preview and check a real click before concluding anything is inert.
