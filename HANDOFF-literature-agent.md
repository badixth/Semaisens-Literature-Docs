# Handoff — literature agent

**Status: consumed.** Both jobs landed.

Two jobs. Job A corrects `concepts/effect-methodology` (Amendment D). Job B updates `concepts/finding-provenance` (Amendment C) now that its open severity question is decided.

Both are edits to pages you already wrote. Neither is a rewrite. Keep the structure, keep the voice, keep the no-claim section the largest thing on the effect page.

Standing constraints, unchanged: no new guardrail categories beyond the eight, no new telemetry event types beyond the nine, no new roles, human-readable ids only, cite doc pages, mark clearly what is decided versus found, end with Open Questions and a Docs Delta.

---

# Job A — three corrections to `concepts/effect-methodology`

The page is good and ships. The per-crop baseline decision, the rulecard-anchored attribution window and the three-claim-strength spine are all better than the parallel analysis they were compared against, and none of them changes. These are three inserts and one closed question.

## A1 — cohort selection bias, and why it breaks two crops

**Where:** the `## Confounders` table, and the cohort row of `### The candidates`.

The page's cohort comparability keys are `crop`, `ecosystem`, `stage`, `region` — Amendment A's band-resolver keys. The `<Note>` argues that reusing them is what makes cohort baselines computable. That argument is correct for *what a healthy band looks like* and incorrect for *what an untreated arm looks like*.

**Fields that received an intervention were usually the worse ones — that is why they got it.** A cohort matched only on those four keys compares a distressed treated arm against an average untreated arm and therefore **understates** the effect. This is not noise. It has a direction, and the direction is always the same.

It matters more here than it would elsewhere because cohort is the **primary** baseline for paddy and for pineapple. Two of four crops rest their strongest available claim on an arm that is systematically not comparable.

**The fix:** cohorts must additionally match on **pre-intervention deviation from band** at the window's opening. Amendment A already computes deviation, so the key is available and costs nothing new. State it as a fifth comparability key. State that a cohort drawn without it cannot support counterfactual impact — it ceilings at attributable change.

Add the corresponding row to the confounder table as **refused if uncontrollable**.

## A2 — Open Question 1 is already answered by the risk model

**Where:** `## Open Questions` item 1, plus the avoided-loss rows in `## Effect measures` and `## When no claim can be made`.

The page says avoided-loss depends on a per-rulecard trust score that "the literature does not define," and leaves its shape open — binary, graded, or numeric.

It is already defined. `concepts/risk-model` resolves yield impact through four layers — `farm_calibrated` → `by_region` → `by_ecosystem` → `literature_v0` — and **the resolved layer is the trust score.** It is graded, it is already computed, and it is already inspectable.

**The gate:** a monetary avoided-loss figure may be reported **only** where yield impact resolved at `farm_calibrated` — the curve tuned on this farm's own history. At `by_region`, `by_ecosystem` or `literature_v0`, report the physical response (recovery to band, days to recovery) and decline the ringgit figure.

This is stricter than the page's current "untrusted curve → refuse". A literature-default curve is not untrusted in general; it is untrusted **for money**. Publishing a ringgit figure derived from `literature_v0` against an agency disbursement is the most expensive single error available on this page.

Close OQ 1. If a narrower question remains, it is whether `by_region` should be admissible for avoided-loss with an explicitly widened error band — that one is worth keeping open.

## A3 — saturation is a structural constraint, not a confounder row

**Where:** promote it out of the `## Confounders` table into its own subsection under `## Effect measures`.

The page carries saturation as one confounder among nine, resolved by "switches to a saturation-tolerant index (NDRE, SAVI) or the claim degrades." That understates it badly.

Above roughly 0.80, NDVI stops discriminating. A closed oil palm canopy at prime and a tapped rubber block at full canopy sit there permanently. **Two of the four crops cannot demonstrate effect on their default index at their most valuable stage** — not occasionally, not under cloud. Structurally, and always.

The consequence is not that the claim degrades. It is that **a scheme report that silently ran NDVI on prime oil palm or tapped rubber measured nothing at all**, and will report a null effect that looks exactly like an honest null. That is worse than a refusal, because it is indistinguishable from one.

State it as a precondition: effect measurement on oil palm at prime or rubber under tapping **must** run on NDRE. If NDRE is unavailable for the window, the claim is refused, not degraded. Add the matching precondition to `## Guardrails`.

## Not changing — recorded so nobody reopens it

- **The per-crop baseline decision stands.** Cohort primary for paddy and pineapple, prior comparable cycle for oil palm and rubber, before-and-after never primary. A1 changes how cohorts are drawn; it does not change which baseline is primary.
- **The rulecard-anchored window stands.** Varying by hazard rather than by crop is right, and it reuses a field the risk model already carries.
- **The three claim strengths stay as the spine.**
- **The p-hacking rate limit stays and stays visible.** Rate-limiting repeated cohort formation on the same scheme is the first thing an agency auditor will ask about. It is an integrity control, not a performance concern.

---

# Job B — update `concepts/finding-provenance` for the decided severity option

The page correctly presented Options R and P side by side and declined to pick. The call has now been made above the page, so the page should record it.

## B1 — Option R is decided

**Change the framing.** `## Severity and the safety floor` currently presents two live options. Rewrite so **Option R is the decision**, with Option P retired to a short "considered and rejected" note that keeps the reasoning — the trade is real and future readers deserve to see why it went the way it did.

Retire Open Question 1. Replace it with the note that the choice was made on organisational grounds: co-signature on a serious assertion is standard practice in the estate hierarchy the platform serves, and a gate the organisation already runs informally is cheaper to encode than to fight.

**Do not soften the agronomist-at-`high` cell.** Under R an `agronomist` still commits `high` directly; only `critical` requires a signature for that role. This is load-bearing — the page's own seed fixture 01 is an agronomist at `high`, and gating it would break the argument the page is built on. Consider adding a sentence making this explicit, because it is the cell most likely to be misread. It was misread once already during the decision.

## B2 — Open Question 4 is answered

**The co-signer must be an `agronomist` whose scope includes the target estate, and must not be the author.**

**Fallback:** where the estate has no agronomist with activity in the preceding 24 hours, the in-scope `estate_manager` may sign instead. Record that the fallback was used, and disclose it wherever the signature is shown — a manager's signature is not an agronomist's and the record must not imply otherwise.

Both alternatives were considered and rejected; keep the reasoning on the page:

- *Any in-org agronomist* makes a co-signer almost always available at the cost of making the signature meaningless. Someone with no knowledge of that estate's fields signing off on a critical hazard converts a substantive gate into a procedural one — worse than no gate, because it manufactures false assurance.
- *Strict, no fallback* leaves a single-agronomist estate unable to raise `critical` at all. The 2-hour escalation would then fire on every legitimate critical finding at that estate, training everyone to ignore it.

Also confirm the scout ceiling at `medium`, with promotion by an agronomist as the path upward.

## B3 — fixture 04 is arithmetically impossible

`## Seed` lists fixture 03 as machine `high` and fixture 04 as human `agronomist` `high`, and states fixture 04's purpose is "a two-band-different severity" to force `raise_contradiction`. The gap between `high` and `high` is zero. The fixture cannot do its stated job.

**Correct fixture 04 to `low`.** The intent is unambiguous so this is an arithmetic correction, not a reopened decision. It also makes it the better fixture: the engine reads blast at `high`, the agronomist walks the field and says it is not blast. That is a contradiction anyone can picture.

## B4 — two consequences of R the page does not yet cover

Both are new. Neither could arise under P, because under P every human finding was `active` on arrival.

**An unsigned finding must never expire.** The expiry clock starts when a finding enters `active`, not when it is raised. Expiry means *the estate was told and did nothing* — but a finding in `pending_review` has been delivered to nobody, since the page correctly suppresses notification while a claim awaits signature. Letting it expire would silently kill a hazard nobody was ever told about, and would do so fastest at `critical`, where the window is shortest. Unsigned findings are governed by the escalation timers and nothing else.

This also resolves a field-level ambiguity: the expiry windows key off `severity_effective`, and a finding awaiting signature has only an `asserted_severity`. Starting the clock at `active` is what gives the window something real to measure.

**An unsigned finding must freeze nothing.** `raise_contradiction` should engage only once a human finding is `active`. Otherwise anyone entitled to raise could mute a live machine alert by asserting an unsigned finding against it, and the review gate becomes a weapon. An unsigned claim has no authority over a machine finding until someone with the authority signs it.

## B5 — one number to flag, not to answer

The page sets escalation for an unsigned `critical` at 2 hours. Under R that is the **only** backstop for a hazard that reaches nobody until it is signed. Two hours of silence on a critical hazard is a long time, and the number appears to come from the page rather than from observed on-call practice.

Do not change it. Add it to Open Questions as a number that should be validated against how the on-call rotation actually behaves before the module ships.

---

## Docs Delta expected from both jobs

Both pages end with a Docs Delta. Add rows for: the fifth cohort comparability key, the `farm_calibrated` gate on monetary claims, the NDRE precondition for prime oil palm and tapped rubber, the Option R decision, the co-signer scope and fallback, the fixture 04 correction, the expiry-clock start condition, and the contradiction precondition.

Where a delta touches a page other than the one you are editing, name that page. The docs repo is source of truth for every downstream brief; an amendment that never reaches it is invisible to the next reader.
