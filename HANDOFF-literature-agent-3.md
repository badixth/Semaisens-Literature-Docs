# Handoff 3 — literature agent

**Status: answered, and the answer was structural.** The bands do not exist to be sourced. See `DECISION-rate-bands-do-not-exist.md`.

One job: establish the **crop-specific agronomic minimum and maximum application rates** that three existing pages already depend on and none of them defines.

This is the last thing blocking Track B4. Unlike the previous two handoffs, this one is mostly agronomy rather than modelling, and the numbers have to come from Malaysian sources.

---

## The gap

`guides/prescription-maps` states in its Input validation guardrail:

> Rates must fall within the crop-specific agronomic min/max declared in the [Risk Model](/concepts/risk-model).

**No such min/max exists in `concepts/risk-model`.** There are no rate bands, no product dimension, nothing to validate against.

It is not one dangling reference. Two more pages depend on the same undefined number:

- `guides/semai-advisor/overview` lists *"rates outside agronomic min/max"* among safety-critical writes requiring confirmation.
- `guides/semai-advisor/failure-modes` triggers a degradation level when a user *"requests an action that would exceed agronomic min/max rates."*

So an unimplementable refusal, a confirmation that cannot fire, and a failure-mode trigger that never triggers — all resting on one absent definition.

**The consequence in the product.** A prescription leaves the platform as a file on a machine display and becomes fertiliser on the ground. The out-of-band rate refusal is the only guardrail between a mistyped rate and a hectare that received it. VRA now ships without it.

---

## What to establish

A rate band per **crop** and per **product**, with the unit stated. Minimum viable coverage, in priority order:

| Crop | Products that matter most |
| --- | --- |
| **Rice** | Nitrogen (urea and compound), phosphorus, potassium |
| **Oil palm** | Nitrogen, potassium (the dominant cost), magnesium, borate |
| **Rubber** | Nitrogen, phosphorus, potassium |
| **Pineapple** | Nitrogen, potassium; forcing agent (ethephon) if it can be sourced safely |

For each: a **minimum** below which the application is pointless, a **maximum** above which it is agronomically or environmentally unsound, and the unit (kg/ha of product, or kg/ha of nutrient — **say which, and be consistent**, because the two differ by a factor that would silently corrupt every figure).

---

## Where the numbers must come from

**Malaysian sources, named per figure.** The recommendations differ enough by country that a general figure is not usable here:

- **MPOB** for oil palm.
- **MARDI** and the **DOA** fertiliser recommendations for rice; the granary schemes (MADA, KADA) publish their own guidance.
- **RRIM / Malaysian Rubber Board** for rubber.
- **MARDI** for pineapple.

**Cite the source on every band.** A rate with no attribution cannot be defended to an agronomist, and this table will be read by people who know these numbers better than the platform does.

---

## The rule that matters more than completeness

**Refuse rather than guess.** If a crop-and-product combination cannot be sourced to a Malaysian recommendation, **leave it undefined and say so explicitly.** The platform's existing habit applies exactly here: an em dash with a stated reason beats a fabricated figure.

An undefined band must produce a **refusal to validate**, not a silent pass. State this on the page: where no band exists for a crop and product, the platform declines to accept a prescription for it rather than accepting any rate. A missing guardrail must fail closed.

That is the difference between "we have not sourced this yet" and "any rate is acceptable here", and the second one puts fertiliser on the ground.

---

## Two questions the bands raise

**Does the band vary by growth stage?** Nitrogen on rice at tillering is not the same recommendation as at panicle initiation. If the sources give stage-specific rates, say so and structure the band accordingly. If they give a season total, say that instead — but do not let a season total be validated as a single-pass rate.

**Does the band vary by the resolver layers Amendment A already carries?** Ecosystem and variety change yield potential, so they plausibly change the sensible rate. If the sources support that, the band should key on the same tiers the band resolver already uses rather than inventing a parallel structure.

Both are worth answering explicitly, even if the answer is "the sources do not distinguish."

---

## Where it lands

`concepts/risk-model`, alongside the yield-impact layers, since that is where all three consuming pages already point.

End with a Docs Delta naming: the new section, the three pages whose references become live, and the fail-closed rule for undefined combinations.

---

## Constraints

Human-readable ids. No new guardrail categories — this fills the existing Input validation and Refusals rows in `prescription-maps`. No new telemetry types. Cite doc pages and external sources. Mark clearly what is sourced versus decided. Keep the product's habit of refusing rather than misleading.

If a decision is needed that the literature does not resolve, **stop and say "Decision required: *question*"** rather than picking a plausible number. On this page a plausible number is the failure mode.
