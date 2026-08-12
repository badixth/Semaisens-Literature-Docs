# Handoff — Amendment D, three patches

**Status: consumed.** Patches landed in `concepts/effect-methodology`.

For the agent maintaining `/concepts/effect-methodology`. That page ships as written. This file is the delta from `METHODOLOGY-assistance-impact.md` — three findings the page does not carry, one of which is a live defect in its primary baseline.

Do not restructure the page. Do not renumber. These are inserts.

---

## Patch 1 — cohort selection bias runs one way, and it breaks the primary baseline for two crops

**Where:** `## Confounders`, and the cohort row of `### The candidates`.

The page's cohort comparability keys are `crop`, `ecosystem`, `stage`, `region` — Amendment A's band-resolver keys. The `<Note>` argues that reusing them is what makes cohort baselines computable. That argument is right for *what a healthy band looks like* and wrong for *what an untreated arm looks like*.

**Fields that received an intervention were usually the worse ones — that is why they got it.** A cohort matched only on crop, ecosystem, stage and region therefore compares a distressed treated arm against an average untreated arm, and **understates** the effect. The bias is not noise; it has a direction, and it is always the same direction.

This matters more than it would elsewhere because cohort is the **primary** baseline for paddy and for pineapple. Two of four crops rest their strongest available claim on an arm that is systematically not comparable.

**The fix:** cohorts must additionally match on **pre-intervention deviation from band** at the window's opening. Amendment A already computes deviation, so the key is available. State it as a fifth comparability key, and state that a cohort drawn without it cannot support counterfactual impact.

Add to the confounder table as **refused if uncontrollable** — a cohort that cannot be matched on pre-intervention deviation falls back to attributable change.

---

## Patch 2 — Open Question 1 is already answered by the risk model

**Where:** `## Open Questions` item 1, and the avoided-loss rows in `## Effect measures` and `## When no claim can be made`.

The page states that avoided-loss depends on a per-rulecard trust score that "the literature does not define," and leaves the shape open (binary, graded, numeric).

It is defined. `concepts/risk-model.mdx` already resolves yield impact through four layers — `farm_calibrated` → `by_region` → `by_ecosystem` → `literature_v0` — and the resolved layer *is* the trust score. It is graded, it is already computed, and it is already inspectable.

**The gate:** a monetary avoided-loss figure may be reported **only** where yield impact resolved at `farm_calibrated` — the curve tuned on this farm's own history. At `by_region`, `by_ecosystem` or `literature_v0`, the platform reports the physical response (recovery to band, days to recovery) and declines the ringgit figure.

This is stricter than "untrusted curve → refuse." A literature-default curve is not untrusted; it is untrusted *for money*. Publishing a ringgit figure derived from `literature_v0` against an agency disbursement is the single most expensive error available on this page.

Close OQ-1. Move the trust-score question to a narrower one if needed: whether `by_region` should be admissible for avoided-loss with a stated widened error band.

---

## Patch 3 — saturation is a structural constraint, not a confounder row

**Where:** promote out of the `## Confounders` table into its own subsection under `## Effect measures`.

The page carries saturation as one confounder among nine, resolved by "switches to a saturation-tolerant index (NDRE, SAVI) or the claim degrades." That understates it.

Above roughly 0.80, NDVI stops discriminating. A closed oil palm canopy at prime and a tapped rubber block at full canopy sit there permanently. **Two of the four crops cannot demonstrate effect on their default index at their most valuable stage** — not sometimes, not under cloud, structurally and always.

The consequence is not that the claim degrades. It is that **a scheme report that silently ran NDVI on prime oil palm or tapped rubber measured nothing at all** and will report a null effect that looks like an honest null. That is worse than a refusal, because it is indistinguishable from one.

State it as a precondition: effect measurement on oil palm at prime or rubber under tapping **must** run on NDRE. If NDRE is unavailable for the window, the claim is refused, not degraded. Add the corresponding precondition to `## Guardrails`.

---

## What is not being patched, and why

Recorded so the next reader does not reopen these.

**The per-crop baseline decision stands as written.** Cohort primary for paddy and pineapple, prior comparable cycle for oil palm and rubber, before-and-after never primary. The parallel analysis argued a single global primary of before-and-after on deviation-from-band; that was weaker. Stage-normalising removes a confounder, it does not manufacture a counterfactual, and the page's ceiling of **observed change** on before-and-after is the correct call. Patch 1 fixes how cohorts are drawn; it does not disturb which baseline is primary.

**The rulecard-anchored window stands.** Anchoring to `days_to_mitigate * 0.5` … `2 * days_to_mitigate` with per-crop ceilings varies the window per hazard rather than per crop, and reuses a field the risk model already carries. Better than fixed crop-level windows.

**The three claim strengths stand as the spine.** Anchoring every measure, baseline and disclosure to a tier is the right organising axis, and it makes the disclosure envelope enforceable.

**The p-hacking rate limit stands and should be kept visible.** Rate-limiting repeated cohort formation on the same scheme is the guardrail an agency auditor will ask about first. It should not be quietly dropped as a performance concern — it is an integrity control.

---

## Constraints

No new guardrail categories — the eight are sufficient and Patches 1–3 all land in existing ones (Preconditions, Refusals). No new telemetry event types. Human-readable IDs. Keep the no-claim section the largest. Cite doc pages. Mark what is decided versus found.
