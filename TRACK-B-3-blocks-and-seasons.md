# Track B, Brief 3 — Blocks and Seasons

**Status: built.**

Self-contained. **Attach nothing.** Every fact you need is inline. If something here contradicts your memory of an earlier phase, this file wins.

Run in a **fresh chat**, after Brief 2's blockers are cleared. These two tabs are paired because they are the two things a field needs before it can be judged at all: *where* the work happens, and *when*.

---

## 1 · Why blocks exist

Three reasons, and the third is the one that makes an operator bother.

**A block stops the field mean lying to you.** A field-level index reading is an area-weighted mean across every pixel in the boundary. If the north half of a field is healthy and the south half is collapsing, the field average reads *slightly below band* — true, and useless. Split it and the south block reports honestly while the north stays quiet. This is the same principle as never averaging an average, applied spatially.

**A block is the unit of work.** Nobody can be dispatched to 4.2 hectares. They are dispatched to North. Scout tasks, prescription maps and findings all need somewhere to land that a person can walk to. A field is an accounting boundary; a block is a place. The product already speaks this way — the seeded alerts say *"Blast is spreading in A2 north block"* and *"P1 division 3 block is short of nitrogen."* This tab is where those come from.

**A block earns better bands.** The band resolver runs block history → practice → variety → water regime → crop default, first match wins. **Block history sits at the top and beats every lookup table below it.** A block observed across several seasons is judged against what actually happened on that ground rather than against a literature default. Drawing blocks is how a farm earns precision over time, and that is the sentence this screen should be built around.

### Block is not zone

The distinction decides the whole design. Do not let the UI blur it.

| | Block | Zone |
| --- | --- | --- |
| What it is | A management unit | A computed unit for variable-rate application |
| Who defines it | The operator, by hand | The platform, by clustering index values |
| Lifespan | Years. Persists across seasons. | One pass. Recomputed and discarded. |
| Accumulates history | **Yes — this is the point** | No |

History needs a stable boundary to accumulate against. If a block can be regenerated like a zone, the resolver's top tier is meaningless. **Zones are out of scope for this brief.**

---

## 2 · Non-negotiables

Same closed sets as Briefs 1 and 2.

- **Eight guardrail categories.** No ninth.
- **Nine telemetry event types.** No tenth.
- **No new roles.** Eight functional roles plus `viewer`, which is an org role.
- **Human-readable ids only.** `blk_`, `ssn_`. No UUIDs.
- **Compose from existing components.** Preserve base tokens, introduce none.
- **Product mode carries no spec vocabulary.** No snake_case, no enum values, no field names on screen.
- **Anything the resolver computes has exactly one reader.** The labels file translates; it never defines.

---

## 3 · Blocks — what to build

### 3.1 What a block carries

| Captured | Rule |
| --- | --- |
| Name | Required. The name used on the ground — "North", "Division 3", "A2". |
| Boundary | Required. Drawn or imported. **Must be contained within the parent field.** Area is calculated, never typed. |
| Crop | Inherited from the field. May be overridden. |
| Water regime | Inherited. May be overridden. |
| Variety | Inherited. May be overridden. |
| Planted date | Per block, because a field is not always planted in one go. |

**An inherited value and an overridden value must not look identical.** The screen already says *"A block inherits the field's crop, water regime and variety, and may override any of them. An override says so."* Honour that literally: label the override, and say what it was overridden from.

### 3.2 Two refusals

**A block outside its parent field is refused.** Already stated on the screen and already in the docs. Keep it.

**Blocks must not overlap each other.** This rule is not yet written down anywhere and it needs to be. Every rollup in the platform is area-weighted; two overlapping blocks double-count the shared hectares in every estate, regional and national figure, and nothing downstream would catch it. Refuse the overlap at draw time with a plain reason, the same way containment is refused.

*Decided here, not found. Record it in the Docs Delta.*

### 3.3 Delete is not delete

This is the one operation on this screen that is not ordinary CRUD, and it is the reason this brief exists rather than a ticket.

**A block is not a drawing. It is an accumulating record.** Deleting one destroys its history, which is the best band evidence that farm owns, and silently drops every future verdict on that ground back down to a lookup table. Nobody clicking a delete button expects to degrade next season's accuracy.

So:

- **Archive, not delete.** The boundary and its history are retained.
- **The confirmation must state what is being given up**, in plain language: this block's own record is what its bands are currently judged against, and archiving it means readings there fall back to the water-regime default.
- **Say what happens to open findings and tasks bound to the block.** This is currently undefined and must not be left to the build to guess. The rule: open findings and tasks are **not** deleted; they remain against the field with the block named as historical. An archived block cannot receive new ones.
- **Archiving is auditable**, per the standard envelope.

**Redrawing a boundary is the same problem in slower motion.** History accumulated against the old shape. A small correction is fine; a wholesale redraw means the history no longer describes the ground it is attached to. Disclose it: readings before today were recorded against a different outline.

### 3.4 Screen work

The tab already renders a field selector, a block list with area and inheritance chips, and a map with the field outline and block outlines. What is missing:

- **Draw a block** — place corners on the map inside the parent field, with the area calculating live and the same self-intersection refusal as the field boundary.
- **Rename**.
- **Redraw**, with the disclosure above.
- **Override** crop, water regime or variety, with the override labelled and its inherited value still visible.
- **Archive**, with the confirmation above.
- **An empty state** for a field with no blocks that says what drawing one would buy — not "No blocks yet", but the reason: the field is currently judged as one average, and blocks are how it earns its own record.

---

## 4 · Seasons — what to build

A season is what makes an index trend mean anything. Without one the platform cannot resolve growth stage, cannot fire emergence alerts, and cannot compare this year to last.

**The cycle family is inherited from the crop and is never chosen by the user.** Three families: cyclical (rice), perennial (oil palm, rubber), rolling continuous (pineapple).

### 4.1 The four crops are genuinely different — do not build one form

| Crop | Season boundary | User picks | Label pattern |
| --- | --- | --- | --- |
| **Rice** | Main Season / Off Season / Ratoon | Which season, cycle index within the year, planting date. Optionally dry or wet sowing. | `Main Season 1 · 2026` |
| **Oil palm** | Production Year | The year. **Planting date is the block's original planting date**, which may be twenty years ago. | `Production Year 2026`, with a chip like `Planted 2018 · Age 8` |
| **Rubber** | Production Year | The year, **the tapping panel (A–D)**, and **the tapping cycle (d/2, d/3, d/4)**. Both are required. | `Production Year 2026 · Panel B · d/3` |
| **Pineapple** | Planting Batch | The batch, its planting date, and its forcing date once known. | `Batch 2026-A · Plant Crop` |

### 4.2 The traps, each of which will produce a wrong number if missed

**The perennial planting-date trap.** For oil palm and rubber, planting date is when the trees went in — often decades ago — not the start of the current production year. One field labelled "planting date" used for both will make every mature block read as newly planted, and every stage resolution will be wrong. Label them separately and say which is which. Tree age derives from the original date and drives the entire yield expectation: immature at 0–3, young mature 4–7, prime 8–18, declining 19–25.

**Rubber cannot have a season without a panel and a cycle.** Comparing a d/2-tapped Panel B against a d/3-tapped Panel C is meaningless, so a season missing either is not comparable to anything. Refuse it.

**Wintering: expect the dip, do not mute the window.** The annual leaf fall drops yield to near zero and is not a hazard. In Peninsular Malaysia the window is typically **February to March**, extending into early April in some clones and in the northern states; Sabah and Sarawak run later and less synchronised. The dip is 0.10–0.25 on NDVI and lasts roughly 6–10 weeks.

**But do not implement this as a date-range suppression**, because the discriminator is recovery, not the calendar:

- Wintering **recovers within 8–10 weeks**.
- A dip **outside** the historical window, or one that **fails to recover** inside 8–10 weeks, is a real stress signal and must still fire.

This matters most on the clone most likely to be on screen. **RRIM 2000-series clones are the ones documented as susceptible to Corynespora leaf fall**, and CLF causes secondary leaf fall *outside* the normal wintering window — so on exactly those blocks, a February-to-April dip that does not fully recover is a strong CLF signal. A build that mutes the window would suppress the disease it most needs to catch.

Compare against the same block in previous years before deciding, per the diagnosis guidance.

**Pineapple has no single "this season".** Batches are planted continuously and coexist at different ages on the same field. Do not build a single-season selector. It needs a batch picker, and any view scoped to "this season" must name which batch it means.

**Rice cycles are numbered within the year.** `Main Season 1 · 2026` — the index matters because an unusual year can run three cycles.

### 4.3 The comparison rules — these are what seasons are *for*

A season that cannot be compared to anything is bookkeeping. Each crop has a rule, and the UI should make the valid comparison the easy one:

- **Rice:** Main to Main, Off to Off. **Never Main to Off** — the 15–25% gap is structural, not agronomic. Offering that comparison at all is a defect.
- **Oil palm:** Production Year to Production Year **at the same tree-age cohort**. A block at year 7 compares to another block at year 7, not to itself last year, because a year of age materially changes yield potential.
- **Rubber:** Production Year to Production Year **on the same panel and the same tapping cycle**.
- **Pineapple:** batches at the **same age from planting**, never the same calendar date. Two batches on the same day are at different growth stages.

**And the axis rule that follows from all four:** comparisons align by **phenology week — days since planting expressed in weeks — not calendar week.** Week 8 of Main 2024 is booting and week 8 of Main 2025 is booting, always. Aligning by calendar compares different biological stages and is misleading.

### 4.4 Screen work

- Create a season on a field, with the form shaped by the crop's cycle family.
- Close a season. Remember from Brief 1: closing a season transitions every open finding on it to season-closed, terminal, disclosed — **and season-closed is not resolved.** No success colouring.
- Show which season is current, and for pineapple, which batch.
- Refuse a second open season on a cyclical field where one is already open.
- An invalid or missing comparison key — a rubber season with no panel — is refused at creation, not discovered later.

---

## 5 · Copy rules

| Never show | Say instead |
| --- | --- |
| `cycle_model`, `cyclical`, `perennial`, `rolling_continuous` | Don't show at all — inherited, not chosen |
| `season_type`, `season_label` | The season's own name |
| `parent_season_id` | "Part of *season name*" |
| `block_history`, `band_source` | "Judged against this block's own record" |
| `area_hectares` | Area, with units |
| `archived` | "Archived on *date*" |

Panel, tapping, wintering, forcing, ratoon, batch, Musim Utama and Musim Luar are the real vocabulary. Use them. Developer jargon is not.

---

## 6 · Seed

Against the existing 12-field seed, make these reachable:

| Fixture | Proves |
| --- | --- |
| A paddy field with North and South blocks where the two read differently | **The argument.** The field mean sits inside band while South sits well below it. One screen, both numbers. |
| A block overriding variety from its parent field | The override is labelled and the inherited value still visible. |
| A block with several seasons of history | Its bands resolve from its own record, and the explain trace says so. |
| An archived block | The confirmation stated what was given up; open findings survived against the field. |
| An oil palm block planted 2008, in Production Year 2026 | Reads as age 18 and prime, not newly planted. |
| A rubber field on Panel B at d/3, and a second on Panel C at d/2 | The two are **not** offered as comparable. |
| A rubber block on an RRIM 2000-series clone, dipping across the wintering window and **recovering** inside 8–10 weeks | Wintering is not reported as a hazard. |
| The same clone, dipping across the window and **not** recovering | Corynespora leaf fall still fires. This is the pair that proves the rule is recovery-based rather than a muted date range. |
| A pineapple field with three overlapping batches | The batch picker; no view claims a single "this season". |
| A rice field with Main 2025 and Off 2025 | **Main-to-Off comparison is not offered.** |

---

## 7 · Acceptance tests

1. A field with North and South blocks shows the field mean **and** both block values, and the divergence is visible without navigating away.
2. A block drawn outside its parent field is refused with a plain reason.
3. A block overlapping an existing block is refused with a plain reason.
4. A block's overridden variety is labelled as an override and shows what it was inherited from.
5. Archiving a block requires a confirmation that names what is being given up, in plain language, with no field names or enum values.
6. After archiving, open findings and tasks still exist against the field, and the block is named as historical.
7. An archived block cannot receive a new finding or task.
8. Redrawing a boundary discloses that earlier readings were recorded against a different outline.
9. A block whose bands resolve from its own record says so in the explain trace.
10. An oil palm block planted in 2008 reads as age 18 and prime in Production Year 2026.
11. A rubber season cannot be created without both a panel and a tapping cycle.
12. Rubber readings across the wintering window are not reported as a hazard, **and a dip that fails to recover within 8–10 weeks still fires.** Both halves must pass — a build that simply mutes the window would pass the first half and suppress Corynespora leaf fall.
13. A pineapple field with three batches offers a batch picker, and no view claims a single "this season".
14. **Main-to-Off comparison is not offered anywhere** for rice.
15. Two seasons compared align by phenology week, not calendar week. Week 8 is the same growth stage in both.
16. Closing a season moves open findings to season-closed with disclosure, and none of them renders as resolved or carries success colouring.
17. An empty block list states what drawing a block would buy, not just that none exist.
18. No screen in Product mode contains an underscore, an enum value or a field name. Grep the rendered copy.
19. Dark and light both pass on every new screen, at all three widths.

---

## 8 · Out of scope

- **Zones.** Computed clustering for variable-rate is its own phase. Blocks may be drawn here; zones are not built.
- **Sub-blocks.** One level of subdivision under field. No nesting.
- **Ratoon crop modelling** beyond the season type existing. Rice ratoon and pineapple ratoon both exist as labels; their yield relationship to the parent crop is not modelled here.
- **Flowering cohorts** for oil palm, and stimulation rounds for rubber. Both are real sub-cycles; both are analytics features, not setup features.
- **Retroactive season creation** on a closed cycle.

Two things this brief decides rather than finds, both flagged for the Docs Delta:

- **Blocks may not overlap.** The corpus does not state it; area-weighted rollups require it.
- **Archive rather than delete**, with open findings surviving against the field. The corpus is silent on block deletion entirely.

If a decision you need is not in this file and not resolvable from it, **stop and say "Decision required: *question*"** rather than picking a default.
