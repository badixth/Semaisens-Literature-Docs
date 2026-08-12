# Track B, Brief 2 — Field creation

**Status: built.** Defects from its test pass are in `DEFECTS.md`.

Self-contained. **Attach nothing.** Every fact you need is inline. If something here contradicts your memory of an earlier phase, this file wins.

Run this in a **fresh chat**, after Brief 1 lands. Do not run both in one session.

---

## 1 · The defect this fixes

The field record marks `ecosystem` and `variety_group` as **optional**.

The band resolver reads them as tiers, first match wins:

```
block history  →  practice  →  variety  →  ecosystem  →  crop default
```

So an optional field that nobody fills does not produce a gap. It produces a **silent fall-through to the crop default** — the weakest, most generic band the platform has — and the field then displays a health verdict computed against it with no indication that anything is missing.

The same two fields feed the yield-impact resolver, where the layer that resolves *is* the trust score for every downstream monetary claim. A field created without an ecosystem is not merely less precise. It is a field whose every band verdict, every yield estimate and every avoided-loss figure rests on a default nobody chose, presented at the same visual confidence as a fully specified one.

**This is the same class of defect as the linear NDVI ramp**, which was agronomically wrong and looked authoritative. The fix is the same shape: make the resolution visible, and refuse to imply precision the inputs do not support.

---

## 2 · What this brief builds

A field creation and editing flow that:

- captures what the resolvers actually need, rather than what the schema marks required
- shows, at creation time, what the field will and will not be able to tell you
- gets the season right for each of four crops with genuinely different cycle shapes
- never silently accepts a default in a place where a default degrades a verdict

---

## 3 · Non-negotiables

Same closed sets as Brief 1. Do not extend them.

- **Eight guardrail categories**: Input validation, Preconditions, Refusals, Confirmations, Soft warnings, Rate and scope limits, Audit, Escalation. No ninth.
- **Nine telemetry event types.** No tenth.
- **No new roles.**
- **Human-readable ids only.** `fld_`, `blk_`, `zon_`, `ssn_`, `obs_`. No UUIDs.
- **Compose from existing components.** Preserve base tokens, introduce none. Manrope, 4px spacing family, controls at 30 / 36 / 44px.
- **Product mode carries no spec vocabulary.** No snake_case, no field names, no enum values on screen.
- **Anything the resolver computes has exactly one reader.** The labels file translates; it never defines. This rule came from five duplicated-authority defects and is not negotiable.

---

## 4 · What a field carries

| Captured | Required? | Why it matters |
| --- | --- | --- |
| Name | Yes | — |
| Estate | Yes | Sets entitlement scope for everything downstream. |
| Boundary | Yes | Area is **calculated** from the polygon, never typed. |
| Crop | Yes | Determines the cycle family, the stage model and the whole band table. |
| **Ecosystem** | **Effectively yes — see §5** | Resolver tier. Same disease behaves very differently across water regimes. |
| **Variety group** | **Effectively yes — see §5** | Resolver tier, and a rule-card multiplier. |
| Planting date | Yes | Every stage resolution keys off it. |
| Harvest date | Yes (may be projected) | Cycle boundary. |
| Status | Defaulted | `active` or `archived`. |

Area is derived. Do not offer a text input for it — a typed area that disagrees with the polygon is a defect generator, and every per-hectare figure downstream inherits the error.

**Blocks and zones inherit from the field and may override.** A block may carry its own crop, ecosystem or variety group; a zone is either drawn or computed. When a block overrides, say so on the block — an inherited value and an overridden value must not look identical.

---

## 5 · The completeness gate

This is the heart of the brief.

**Ecosystem and variety group stay structurally optional** — you cannot always know them, particularly at an agency importing third-party boundaries, and forcing a guess produces confidently wrong data, which is worse than absent data.

**But the consequence becomes visible.** Build a completeness state on the field, shown at creation, on the field record, and anywhere the field's health verdict is displayed.

| Provided | State | What the platform says |
| --- | --- | --- |
| Crop + ecosystem + variety | Fully resolved | Nothing. Silence is the reward. |
| Crop + ecosystem | Partly resolved | "Variety not set — bands use the ecosystem default for this crop." |
| Crop only | **Generic** | "Ecosystem and variety not set — this field's bands are the crop-wide default and will not reflect local conditions." |

Rules:

- **The generic state is disclosed on the health verdict itself**, not buried in a settings page. Someone reading a band gauge must be able to see that the band is generic.
- **It is a soft warning at creation, never a refusal.** The field must be creatable.
- **It resolves the moment the values are supplied** — no re-verification step.
- **Do not invent a score, percentage or progress ring.** Three named states, plain sentences. A percentage implies a precision the thing does not have.

**The refusal that does belong here:** a field in the generic state must not produce a **monetary** yield-impact or avoided-loss figure. It reports the physical response and declines the ringgit figure, consistent with how the platform already refuses rather than misleads. Show an em dash and the reason.

**No price basis exists yet, so demonstrate the gate one layer down.** Price is a Track C decision — which source per crop, what refresh cadence, how staleness is disclosed — and it is not settled here. Do not seed a provisional constant: a placeholder labelled in Review mode still renders a ringgit number in Product mode, and prototypes get screenshotted.

That leaves the ringgit line as an em dash on **every** field, which would make the completeness gate invisible if nothing else changed. It doesn't have to be money. The gate needs a quantity that moves with completeness, and yield impact already gives one — percentage impact, and tonnage once multiplied by expected yield and area. So:

| Field | Ringgit line | Tonnage line |
| --- | --- | --- |
| Fully resolved | Em dash · "no price basis configured" | Shown |
| Generic | Em dash · the completeness reason | Shown, with the crop-default disclosure |

The two refusals must be **distinguishable by their stated reason**, and the tonnage contrast is what proves the gate works.

---

## 6 · Seasons, per crop

A season ties a crop cycle to a field. Without one, the platform cannot resolve growth stage, cannot fire emergence alerts, and cannot compare this year's canopy to last year's.

**The cycle family is inherited from the crop, never chosen by the user.** Three families:

| Family | Crops | Shape |
| --- | --- | --- |
| `cyclical` | Rice | Two short cycles a year. Clear boundaries. |
| `perennial` | Oil palm, rubber | One continuous producing life. The "cycle" is an accounting year. |
| `rolling_continuous` | Pineapple | Batches that never share a boundary. |

### What to ask, per crop

| Crop | Season label | What the user picks | What is derived |
| --- | --- | --- | --- |
| **Rice** | Main Season / Off Season / Ratoon | Which season, and the planting date | Harvest date projected from the crop model; stage resolves from planting date |
| **Oil palm** | Production Year | The year boundary | Planting date is **the block's original planting date, not this year's**. Tree age derives from it and drives everything. |
| **Rubber** | Production Year, with tapping panel | The year, the **panel**, and the tapping system | Panel year history is what makes year-on-year comparison valid. Without the panel, the comparison is meaningless. |
| **Pineapple** | Planting Batch | The batch, and its forcing date if known | Batch age, not calendar date, drives stage. Batches overlap by design. |

**The perennial planting-date trap.** For oil palm and rubber, the planting date is when the trees went in — often twenty years ago — not the start of the current production year. A UI that labels one field "planting date" and uses it for both will silently make every mature block look newly planted, and every stage resolution will be wrong. Label them separately and say which is which.

**Pineapple has no single "this season".** Batches overlap; a field can hold several at once. Do not build a single-season selector for pineapple — it needs a batch picker, and any view scoped to "this season" must state which batch it means.

**A mixed-crop scope has no single season.** If a selection spans crops, do not compute a union of cycles. That is precisely the defect that produced a 28-month window and a crushed axis: the union was a reasonable fill for a gap the brief left. Refuse the aggregate, disable the control, and say why.

---

## 7 · Screen work

### 7.1 Create

Progressive, not a wall of fields. Suggested order: identity and estate → boundary → crop → the two resolver tiers → season. The completeness state appears live from the moment the crop is chosen and updates as tiers are supplied — the user should see the generic state resolve as they fill it.

**Boundary.** Draw or import. Show calculated area as it changes.

Refuse **self-intersecting polygons** only, with a plain reason. That one is arithmetic: area is calculated from the polygon, a self-intersecting polygon has no well-defined area, and every per-hectare figure downstream inherits the error.

**Do not validate the boundary's location against the estate.** There is no estate geometry in the docs — an Estate is *"a named operating unit (an estate group, a cooperative branch, a smallholder collective)"*, which is organisational, not spatial. A collective's plots are scattered by definition, so a containment check would refuse legitimate fields for most of Malaysian agriculture. A proximity warning is no better: it is silent on the first field of a new estate, which is exactly when a misdrawn boundary is most likely, and noisy on scattered holdings, which trains people to dismiss warnings.

The boundary is drawn or imported on a map. Seeing it on the map is the check.

### 7.2 Edit

Changing the crop on a field with an open season and existing findings is a **destructive change**, because the band table, stage model and every open finding's hazard reference all belong to the old crop. Confirm it explicitly, state what happens to the open season and to open findings, and audit it. Do not silently re-resolve.

Changing ecosystem or variety **re-resolves the bands**. Not destructive, but the field's history now reads against a different band. Disclose it: *"Bands re-resolved from irrigated lowland. Readings before today were assessed against the crop default."*

### 7.3 Blocks

Create within a field boundary — a block outside its parent is refused. Blocks may override crop, ecosystem or variety; an override is labelled as one wherever it is shown.

---

## 8 · Copy rules

| Never show | Say instead |
| --- | --- |
| `ecosystem`, `irrigated_lowland` | Water regime; "Irrigated lowland" |
| `variety_group` | Variety |
| `cycle_model`, `cyclical`, `perennial`, `rolling_continuous` | Don't show at all — it is inherited, not chosen |
| `season_type`, `season_label` | The season's own name, as the workspace already renders it |
| `parent_season_id` | "Part of *season name*" |
| `crop_default`, `by_ecosystem`, `band_source` | "Crop-wide default" / "Set for irrigated lowland" |
| `area_hectares` | Area, with units |
| `boundary`, GeoJSON | Field boundary |

Malaysian farming terms where they are the words people use. Panel, tapping, forcing, ratoon and batch are the real vocabulary here — use them.

---

## 9 · Seed

The existing seed has 12 fields across four crops. Add, or confirm reachable:

| Fixture | Purpose |
| --- | --- |
| A rice field with crop only, no ecosystem or variety | **The generic state.** Its band gauge must disclose that the band is crop-wide, and its yield panel must show an em dash where a ringgit figure would go. |
| A rice field fully specified | The silent, fully-resolved case, for contrast on the same screen. |
| An oil palm block planted 18 years ago, in the current production year | The perennial planting-date trap. Tree age must read 18, not zero. |
| A rubber field with two panels, one tapped | Panel selection, and year-on-year comparison against the same panel. |
| A pineapple field with three overlapping batches | The batch picker, and the "this season" ambiguity. |
| A mixed-crop selection spanning rice and oil palm | The refused aggregate. |
| A smallholder collective estate holding three scattered, non-adjacent rice plots | **No estate containment.** Proves a legitimate collective creates cleanly. This is most of Malaysian agriculture, not an edge case. |

---

## 10 · Acceptance tests

1. A field created with crop only is creatable, and its health verdict states on its face that the band is the crop-wide default.
2. That same field shows an em dash instead of a monetary figure, giving the **completeness** reason — while a fully-specified field also shows an em dash, giving the **no price basis** reason. The two are distinguishable by their text.
2b. Both fields show a tonnage figure, and the generic one carries the crop-default disclosure. This is the contrast that proves the gate, since no ringgit figure exists anywhere.
3. Supplying the ecosystem resolves the disclosure immediately, with no re-verification step.
4. An 18-year-old oil palm block reads as 18 years old, not as newly planted, after a production year is created.
5. A rubber season cannot be created without a panel.
6. A pineapple field with three batches offers a batch picker, and no view claims a single "this season".
7. A mixed-crop selection refuses the aggregate and says why. **No 28-month window appears anywhere.**
8. Area is never typeable and always matches the polygon.
9. A block drawn outside its parent field is refused with a plain reason. (This containment rule is real and stays — it is in the docs.)
9b. A field drawn far from its estate's other fields is **accepted, with no warning and no refusal.** The scattered-collective fixture must create cleanly. There is no estate geometry, so there is nothing to validate against.
10. Changing crop on a field with an open season and open findings requires confirmation that states the consequence, and writes an audit row.
11. Changing ecosystem discloses that historical readings were assessed against a different band.
12. No screen in Product mode contains an underscore, an enum value or a field name. Grep the rendered copy.
13. Dark and light both pass on every new screen, at all three widths.

---

## 11 · Out of scope

- **Zone computation.** Clustering into zones is its own phase. Zones may be drawn here; computed zones are not built.
- **Bulk field import.** Agencies onboarding hundreds of boundaries need their own path with different validation and a different completeness story.
- **Editing a closed season.** Refused; not a UI to build.
- **Auto-inferring ecosystem from geography.** Tempting and plausible — irrigation scheme boundaries are known — but an inferred value that looks identical to a stated one is exactly the silent-default problem this brief exists to fix. If it is ever built, an inferred value must be visibly inferred.

If a decision you need is not in this file and not resolvable from it, **stop and say "Decision required: *question*"** rather than picking a default.
