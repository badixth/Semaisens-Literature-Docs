# Track C, Brief 1 — The aggregation spine

**Status: built.** The five rules it settles are now inherited by every Track C surface.

Self-contained. **Attach nothing.** Every fact you need is inline. If something here contradicts your memory of an earlier phase, this file wins.

Run in a **fresh chat**. This is the first Track C brief and it builds no screen.

---

## 1 · Why this comes before any screen

Track C is four surfaces: national overview, regional forecast, yield production, assistance impact. All four do the same thing — take numbers computed on individual fields and present them to somebody who cannot see those fields.

**That crossing is where every Track C mistake will happen.** Not in a chart. In the moment a figure leaves the estate that produced it.

There are already four separate rules governing that crossing, written on four different pages, and none of them assembled: k-anonymity, the averaging rules, the delivery-policy disclosure, and the monetary gate. Four screens implementing those independently is how you end up with four subtly different answers to the same question — and on an agency-facing surface, the differences are the part that gets noticed.

So this brief settles the crossing once. The four screens are then built against it.

**Findings went quickly because provenance was settled before the screen. The band model cost roughly fifteen phases because it was not.** Track C has the same shape.

---

## 2 · Non-negotiables

- **Eight guardrail categories.** No ninth.
- **Nine telemetry event types.** No tenth.
- **No new roles.** `regional` is the agency officer: reads across estates, rollup-only.
- **Human-readable ids only.**
- **Compose from existing components.** Preserve base tokens, introduce none.
- **Product mode carries no spec vocabulary.**
- **Higher layers never query field data live.** They read pre-computed rollups written by a scheduled job. This is what makes the numbers reproducible for audit, and it is not negotiable for performance reasons or any other.

---

## 3 · The five rules of the crossing

Every number on every Track C surface obeys all five. Build them once, in one place, and have the screens call it.

### 3.1 Never average an average

Area-weighted, always: `sum(value × area) / sum(area)`. Never `mean(values)`.

Mixing the two silently biases small fields upward, and it is invisible in the output — a wrong national figure looks exactly like a right one.

**And never average across crops.** Paddy at ripening and oil palm at prime are not the same quantity. A national "average crop health" across four crops is not a weak number, it is not a number.

**And never sum across units.** This is the same rule and it bites harder, because the output looks plausible. Yield rates are not held in one unit — paddy and oil palm in tonnes per hectare, dry rubber in kilogrammes. Summed naively, a national forecast read **434,337 t, of which 273,000 came from 182 ha of rubber** — about 1,500 t/ha, a thousandfold error that made one crop 63% of the national figure.

So: **a total carries its unit, or it is not a total.** Forecast is reported per crop with the crop in the label, and a mixed-crop scope carries no tonnage at all. Before summing anything, assert the inputs share a unit and refuse if they do not — do not convert silently, because a silent conversion is indistinguishable from a correct sum in the output.

### 3.1b An index never appears without its band position

**A bare index is not a health figure.** The band is the target; deviation in either direction is loss. Paddy falls through ripening, so a high reading there means late, not healthy.

Four crops shown as 0.58, 0.78, 0.81, 0.65 under a heading reading "crop health" will be read as a league table, every time. The field-level UI already does this correctly — *"0.55 · slightly below"*, the band gauge, the nominal-is-best legend. A national surface that regresses to a bare number is the more dangerous of the two, because its reader has no field context to correct it.

**Rule:** the rollup returns the reading **and** its position against the band that resolved for that crop and stage. No surface renders one without the other.

**And watch saturation.** NDVI stops discriminating above roughly 0.80, and tapped rubber sits there permanently. A rubber figure of 0.81 is not the healthiest crop on the page; it is the least informative number on it. Where the index has saturated, say so rather than ranking it.

### 3.2 Confidence is the minimum of inputs, never the mean

One weak input drags the whole answer down, deliberately. An executive should see the weakest link, not an optimistic blend.

### 3.3 k-anonymity at k = 5

Any slice with fewer than five fields collapses into "Other" until the count is met.

**The trap that matters more than the rule:** a refused *breakdown* must not silently become an aggregate. If breaking down by ecosystem would drop a bucket below k, refuse the breakdown and say so — while keeping the total available. *"Breakdown by ecosystem withheld at k=5; the scheme total remains."* Silence here reads as "no data", which is a different and false claim.

### 3.4 What does not roll up, stays down

Rolls up: area totals, area-weighted indices, health distribution buckets, counts by severity and category, production forecast, verification counts and acceptance rate.

Does not roll up: individual severity, task assignees, specific prescriptions, named recommendations.

A field is HIGH. **An estate is not HIGH.** An estate has twelve fields at HIGH. Higher layers see counts and rates of these things, never the things themselves.

### 3.5 Access does not relax on the way up

If a viewer cannot see a field, that field is still **included** in sums and area-weighted means — they see the correct total — and **excluded** from every drilldown, list, filter and export that would reveal it.

This is what lets a national total be correct while an individual estate's performance stays private from its neighbours.

---

## 4 · Two disclosures that are new, and easy to miss

Both come from work that landed after the aggregation model was written. Neither is optional.

### 4.1 Delivery policy travels with the finding

Organisations choose whether a serious human finding is sent immediately or held until confirmed. That choice is recorded on each finding as it is raised.

**A rollup mixing organisations is mixing governance regimes.** Two findings that reached their audience under different rules are not the same quantity, and presenting them as one number without saying so is the same class of error as averaging across crops.

So: any rollup or dashboard that mixes findings from more than one organisation **discloses the delivery policies in play.** Inside a single organisation it is a property of the organisation and needs no per-row chip.

### 4.2 Money is gated at `farm_calibrated`

A monetary figure — avoided loss, value at risk, assistance impact in ringgit — may only be shown where the yield-impact resolver landed at `farm_calibrated`, the layer tuned on that farm's own history.

At `by_region`, `by_ecosystem` or `literature_v0`, report the **physical response** and decline the ringgit figure with the reason.

This is stricter than "untrusted curve, refuse". A literature-default curve is not untrusted in general; it is untrusted **for money**. And the chain that unlocks it runs: corroborated finding → feeds the farm's own history → enables `farm_calibrated` on that hazard → unlocks the monetary claim. A farm with no corroboration history sees physical response only, however many interventions it has logged.

**Publishing a ringgit figure derived from a literature default against an agency disbursement is the single most expensive error this platform can make.** Build the gate before you build the surface that would show the number.

---

## 5 · Every number explains itself

Any figure on any Track C surface can be opened to show:

1. The child rollups that fed it.
2. The rule applied — sum, area-weighted mean, minimum.
3. The job run timestamp and the `data through` cutoff.
4. **Every child excluded, and why** — stale, paused, below k, missing phenology.

The fourth is the one that gets dropped and the one auditors want. A national figure that cannot say what it left out is not reproducible, and reproducibility is the whole reason agencies can use it.

**Four rules on how a trace renders**, because the four surfaces will copy whatever shape this sets:

- **Every value carries its unit.** A trace row reading `73.3` is unreadable — the same column position holds hectares on one figure and tonnes on the next. This is the tonnes-versus-kilogrammes defect wearing different clothes, and it is worse in a trace than in a headline because a trace is what somebody checks the headline against.
- **A count renders as an integer.** `2.00 fields` is not a count of fields.
- **The rule label must match the operation.** A percentage is not "Counted". An acceptance rate is `sum(accepted) ÷ sum(submitted)` — a ratio of two sums, and the trace needs a category that says so. Labelling it as a count hides the one thing worth checking: that it is not a mean of per-child rates, which would be averaging an average.
- **Withheld rows say what is actually true of them.** "In the total, not in this list" is contradicted by the row being visible in the list. The true statement is that it is counted and cannot be opened.

The same explanation embeds in every verification bundle exported at estate, regional or national layer.

---

## 6 · What to build

Not a screen. A **module** the four Track C surfaces call, plus a demonstration of it.

| Piece | What it does |
| --- | --- |
| **The rollup reader** | Reads pre-computed rollups. Never queries field data live. |
| **The five rules** | §3, implemented once. Area-weighting, minimum-confidence, k-suppression, the roll-up/stays-down split, and access-preserving inclusion. |
| **The two disclosures** | §4. Delivery-policy mixing, and the monetary gate. |
| **The explain trace** | §5. One shape, used by every figure. |
| **The refusal set** | Every case where a number is withheld, each with its own stated reason — never a blank, never a zero, never silence. |

**The demonstration:** one screen — call it whatever fits — that renders a handful of national figures and lets you open each one's trace. It exists to prove the spine works and to be the thing the four real surfaces are checked against. It is not the national overview.

---

## 7 · The refusals, each with its own words

Use these as distinct states, not one generic "unavailable". Copy is illustrative; the distinctions are not.

| Case | What it says |
| --- | --- |
| Below k | "Breakdown by *dimension* withheld — fewer than five fields. The total is unaffected." |
| Cross-crop average requested | "Paddy at ripening and oil palm at prime are not the same measurement. No combined figure will be shown." |
| Monetary below `farm_calibrated` | "Recovery is measured; the value is not. This farm's own history has not yet tuned the curve." |
| Mixed delivery policies | "These organisations confirm urgent findings differently. Counts are comparable; how quickly they reached someone is not." |
| Stale or paused children excluded | "Three fields excluded — monitoring paused. The figure covers the remaining 47." |
| Forecast unavailable | An em dash and the reason. Never a zero. |

---

## 8 · Acceptance tests

1. An area-weighted index computed from unequal fields differs from the plain mean, and the trace names which rule was used.
2. Requesting a cross-crop average is refused, in words, on every surface that could offer it.
3. A slice with four fields collapses to "Other"; the parent total is unchanged and says the breakdown was withheld.
4. A refused breakdown is visibly refused, not absent.
5. Confidence on a card equals the **minimum** of its inputs. Seed one weak input and confirm it drags the card.
6. A viewer without access to a field still sees the correct total, and that field appears in no list, filter, drilldown or export.
7. No surface displays an estate-level severity. Counts only.
8. A monetary figure appears only where the resolver reached `farm_calibrated`; everywhere else the physical response shows with the reason stated.
9. A rollup mixing two organisations with different delivery policies discloses it.
10. Every figure opens a trace showing inputs, rule, timestamp, **and exclusions**.
11. Each refusal in §7 renders with its own words. Grep for a generic "unavailable" and find none.
12. No screen queries field data live. Check the call path, not the output.
12b. **Summing values in different units is refused**, not silently converted. Seed the rubber-in-kilogrammes case and confirm no combined tonnage appears.
12c. **At least one test reads the rendered screen, not the module.** Assert the figures are present in the DOM, that at least two carry refusals, and that opening one produces a trace with inputs, rule, timestamp and exclusions. A suite that only exercises `job()` and `read()` will pass against a blank screen — which is the same blind spot that let an unclickable dropdown through an earlier suite.
13. No Product-mode screen contains an underscore, an enum value or a field name.
14. Dark and light pass at all three widths.

---

## 9 · Out of scope

- **The four real surfaces.** Their briefs come after this.
- **Cross-scheme cohort formation** for effect claims. Open question on the effect methodology; national claims are summations of scheme-level claims until it is answered.
- **Per-estate delivery policy.** Ships at organisation granularity.
- **Changing k.** Five is the default and this brief does not renegotiate it.

If a decision you need is not in this file and not resolvable from it, **stop and say "Decision required: *question*"** rather than picking a default. The mixed-crop range defect came from a brief that left a gap and a build that filled it reasonably; the reasonable fill was a 28-month window and a crushed axis.
