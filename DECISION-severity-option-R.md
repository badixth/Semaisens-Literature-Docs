# Decision — human finding severity: Option R

**Status: superseded, kept on purpose.** The acknowledgement model in `concepts/finding-provenance` replaced Option R. Kept so the reasoning that led there stays traceable. Do not build from this file.

Closes Open Questions 1 and 4 on [`concepts/finding-provenance`](concepts/finding-provenance.mdx). Decided 8 August 2026. Supersedes `DECISION-severity-option-P.md`, which was withdrawn — see [Why the earlier P recommendation was wrong](#why-the-earlier-p-recommendation-was-wrong).

---

## The decision

**Option R — Restrictive.** A human finding asserted above a role's direct-commit ceiling enters `pending_review` and requires an agronomist co-signature before it becomes `active`. Friction sits at raise time.

The reason is organisational, not technical: co-signature on a serious assertion is standard practice in the estate hierarchy this platform serves. A gate the organisation already runs informally is cheaper to encode than to fight.

### Raising bounds

**Amended 8 August 2026 — see [Resolution of the raising table](#resolution-of-the-raising-table) at the foot of this file. That section supersedes the table immediately below.**

Original table, retained for the record:

| Role | low / medium | high | critical | May co-sign |
| --- | :-: | :-: | :-: | :-: |
| `scout` | direct | — | — | — |
| `agronomist` | direct | **direct** | `pending_review` | ✓ if not the author |
| `approver` | direct | `pending_review` | `pending_review` | — |
| `estate_manager` | direct | `pending_review` | `pending_review` | — |
| `regional` (agency officer) | direct | — | — | — |
| `estate_admin`, `org_admin` | — | — | — | — |
| `viewer` (org role) | — | — | — | — |
| `on_call` (overlay) | inherits primary role | | | |

**The agronomist commits `high` directly.** This is the load-bearing cell and it is easy to misread. Agronomic judgement is that role's function; the gate exists for people asserting outside their function, and for the `critical` band where the safety floor engages.

---

## Open Question 4 — co-signer scope, answered

**The co-signer must be an `agronomist` whose entitlement scope includes the target estate, and must not be the author.**

**Fallback:** where the target estate has no `agronomist` with activity in the preceding 24 hours, the `estate_manager` in scope may co-sign in their place. The fallback is recorded on the finding and disclosed wherever the co-signature is shown — a manager's signature is not an agronomist's and the record should not imply otherwise.

**Rejected: any in-org agronomist.** It makes a co-signer almost always available at the cost of making the signature meaningless. Someone with no knowledge of that estate's fields signing off on a critical hazard converts a substantive gate into a procedural one, which is worse than no gate because it manufactures false assurance.

**Rejected: strict, no fallback.** A single-agronomist estate would be unable to raise `critical` at all until a second agronomist appeared. The 2-hour escalation would fire on every legitimate critical finding at that estate, training everyone to ignore it.

---

## Open Question 4b — scout ceiling, confirmed at medium

A `scout` raises up to `medium`. Anything they believe is worse is raised at `medium` with evidence attached, and an `agronomist` promotes it.

This matches the report-versus-judge split the organisation already runs, and it keeps the review queue to one path. The scout's observation is not lost — it is recorded, visible, and promotable in one action by the person whose job the judgement is.

---

## The cost to watch

**A `pending_review` finding does not deliver notifications.** The safety floor does not apply to a claim awaiting a signature, which is correct — an unsigned assertion should not page an on-call role. But it means a genuinely urgent `critical` hazard is silent until someone signs, with escalation to `estate_manager` at 2 hours and to the agronomist pool at 4 hours for unsigned `high`.

**Two hours of silence on a critical hazard is a long time.** That number came from the concept page, not from observed practice. It should be checked against how the on-call rotation actually behaves before the build ships, and tightened if the rotation cannot reliably respond inside it. This is the one place R is genuinely worse than the alternative, and it should not be discovered in production.

---

## Why the earlier P recommendation was wrong

Recorded so the reasoning error is not repeated rather than to apologise for it.

The case for P rested on the seed fixture `fnd_seed_rubber_panel_dryness_01` — an agronomist raising panel dryness at `high` on a block reading inside band, the hazard the satellite is correct not to flag. The argument was that R would hold the one finding that proves the human channel necessary.

**It would not.** Under R an agronomist commits `high` directly. The fixture is untouched. The argument was built on a misread cell in the raising table.

Two supporting arguments were overstated for the same reason:

- *"R forces Open Question 4 before anything can ship."* It forces it for `critical` only, not for `high`, and the page already supplies an escalation backstop. A narrower problem than claimed, and now answered above.
- *"P is additive; R is not."* True in the abstract, and pointing the wrong way in practice. Retrofitting `pending_review`, a review queue and escalation timers into a live system already holding findings is the band-model retrofit pattern — the one that cost roughly fifteen phases. If the organisation needs the gate, building it now is cheap and building it later is not.

What survives from the P analysis: rate limits, the input-validation refusal streak, and the append-only audit rules are all still required. They were framed there as compensating controls for the absence of a gate. They are not compensating controls — they are baseline hygiene, and R does not excuse skipping them.

---

## What this builds

| Item | Status under R |
| --- | --- |
| `pending_review` state | **Built.** Human path only. All three of Amendment C's new states are live. |
| Co-sign action and review queue | **Built.** Co-signer must differ from the author. |
| `pending_review → active` and `pending_review → dismissed` | **Built.** Both require an `agronomist` who is not the author. |
| Escalation timers | **Built.** Unsigned `critical` escalates to `estate_manager` at 2 hours; unsigned `high` at 4 hours, matching the safety-floor mute bound. |
| `co_signer_ref`, `co_signed_at` | **Written on every co-signature**, plus a flag when the estate-manager fallback was used. |
| Notification suppression while `pending_review` | **Built.** No delivery until the finding enters `active`. |

## Consequences for the other open questions

- **OQ 2 (expiry as a yield input)** — unaffected, still open. Expiry is visible everywhere and does not move the forecast.
- **OQ 5 (human override of a machine finding)** — unaffected. Contradiction still freezes both sides and routes to agronomy review.
- **Fixture 05** (`fnd_seed_palm_ganoderma_cosign_05`) — keeps its original id. Under R it enters `pending_review` and clears via a second agronomist, which is what the id describes.

---

## Resolution of the raising table

Added 8 August 2026. **This section is the authority on who may assert what.** It supersedes the table earlier in this file, and it resolves a conflict between that table and `concepts/finding-provenance` as rewritten by the literature agent.

### The conflict

When the concept page was updated to record Option R, it also rewrote the rule as: *"A human finding may assert any band from `low` to `high` directly. Only `asserted_severity = critical` requires a co-sign at raise time."*

That is looser than the table this file originally carried, which sent `approver` and `estate_manager` into review at `high` and capped `scout` at `medium`. Two cells moved, and both are tested by Brief 1's acceptance tests. The page changed the table rather than only recording the decision, so the build and the docs actively disagreed.

### The decision

**Neither version as written. The middle position, which takes the correct half of each.**

| Role | low / medium | high | critical |
| --- | :-: | :-: | :-: |
| `scout` | direct | **—** | — |
| `regional` (agency officer) | direct | — | — |
| `agronomist` | direct | **direct** | review |
| `approver` | direct | **direct** | review |
| `estate_manager` | direct | **direct** | review |
| `estate_admin`, `org_admin` | — | — | — |
| `viewer` (org role) | — | — | — |
| `on_call` (overlay) | inherits primary role | | |

**Every role that may assert `critical` goes to review for it, including `agronomist`.** No exceptions. Co-signer rules are unchanged: an in-scope agronomist who is not the author, with the estate manager as fallback where no agronomist has been active for 24 hours.

### Why this and not the other two

**The page is right that `approver` and `estate_manager` should commit `high` directly.** My original table created an authority inversion: an estate manager, who runs the estate's operations and owns overdue tasks and deviation escalations, would have needed a countersignature from an agronomist to say a hazard is serious. That is backwards as an org rule and would have read as an insult in the field. Concede this cell.

**The page is wrong to let a `scout` assert `high`.** Its own opening sentence frames the tension precisely: *"A platform where any user can manufacture an unsuppressible alert has a problem. A platform where a trained agronomist standing in the field cannot mark what they have seen as urgent has a worse one."* The distinction it draws is between *any user* and *a trained agronomist*. A scout's function is ground-truth collection — visits, photos, observations — not diagnosis. Letting that role trip the safety floor alone is the exact failure the sentence warns about.

This matters more than it looks because **the safety floor engages at `high`, not at `critical`.** Signals at severity ≥ `high` cannot be suppressed, muted beyond the bounded window, or personalised away. So `high` is already the band where the platform's least reversible behaviour switches on. Who may reach it alone is the real question; `critical` is simply where everyone stops.

The scout is not silenced. They raise at `medium` with evidence attached and hand it to an agronomist, who promotes it in one action without retyping. That path already exists in Brief 1.

### What changes in the build

Brief 1 shipped with `approver` and `estate_manager` routed into review at `high`. **Those two cells become direct.** Everything else in the built behaviour stands: the scout cap, the agronomist's direct `high`, the universal `critical` gate, the silence while waiting, the expiry clock starting at `active`, and unsigned findings freezing nothing.

### What changes in the docs

`concepts/finding-provenance` should state this table rather than the broader "low to high directly" rule, and should say why the scout is capped — the argument is already in its own lede and just needs connecting.
