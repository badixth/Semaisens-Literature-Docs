# Handoff 2 — literature agent

**Status: consumed.** The acknowledgement model is live on `concepts/finding-provenance`.

One job: amend `concepts/finding-provenance` (Amendment C) to replace the raise-time gate with an acknowledgement model plus an organisation-level delivery policy.

This supersedes the severity mechanism you wrote in Handoff 1. **The raising table and the co-signer scope survive; the state machine does not.**

Keep the page's structure and voice. Keep the no-claim discipline. No new guardrail categories beyond the eight, no new telemetry event types beyond the nine, no new roles. Human-readable ids. End with Open Questions and a Docs Delta.

---

## Why this is changing

Amendment C currently gates a serious assertion at raise time: the finding is created in `pending_review` and does not exist as a live signal until a peer signs.

That produced eight rules — the state itself, the review queue, co-signer scope, the estate-manager fallback, two escalation timers, notification suppression, a rule that the expiry clock does not run while waiting, and a rule that a waiting finding freezes nothing. Two of those were only discovered by going looking for them, and both were bugs on their way to shipping.

It also produced one behaviour nobody wanted: **an unsigned critical finding is silent.** A hazard reaching nobody until someone signs, with escalation as the only backstop. The design then needed four further rules to stop that silence causing harm.

The organisational requirement behind the gate is real — co-signature on a serious assertion is standard practice in the estate hierarchy the platform serves. What follows satisfies that requirement without the state machine.

---

## 1 · The raising table stays, and needs stating explicitly

The page currently says *"A human finding may assert any band from `low` to `high` directly."* That is too broad. State the per-role table:

| Role | low / medium | high | critical |
| --- | :-: | :-: | :-: |
| `scout` | ✓ | — | — |
| `regional` (agency officer) | ✓ | — | — |
| `agronomist` | ✓ | ✓ | ✓ |
| `approver` | ✓ | ✓ | ✓ |
| `estate_manager` | ✓ | ✓ | ✓ |
| `estate_admin`, `org_admin` | — | — | — |
| `viewer` (org role) | — | — | — |
| `on_call` (overlay) | inherits primary role | | |

**Two things to argue on the page, both drawn from material already there.**

**Why `approver` and `estate_manager` reach `high` directly.** An estate manager runs the estate's day-to-day operations and owns overdue tasks and deviation escalations. Requiring an agronomist to countersign before that role may call a hazard serious inverts the hierarchy.

**Why `scout` cannot reach `high` at all.** Connect this to the page's own lede, which already frames the tension exactly: *"A platform where any user can manufacture an unsuppressible alert has a problem. A platform where a trained agronomist standing in the field cannot mark what they have seen as urgent has a worse one."* The distinction drawn there is between *any user* and *a trained agronomist*. A scout's documented function is ground-truth work — visits, photos, observations — not diagnosis.

**And state the reason the band matters:** the safety floor engages at `high`, not at `critical`. Signals at severity ≥ `high` cannot be suppressed, muted beyond the bounded window, or personalised away. So `high` is where the platform's least reversible behaviour switches on, and who may reach it alone is the real question. `critical` is simply where every role stops without a second name on the record.

A capped scout is not silenced: they raise at `medium` with evidence and an agronomist promotes it in one action.

---

## 2 · `pending_review` stops being a state

**Delete it from the state list.** Amendment C then adds two states, not three: `expired` and `closed_with_cycle`.

Replace it with two fields on the finding:

| Field | Meaning |
| --- | --- |
| `acknowledged_by` | The in-scope `agronomist` who confirmed the assertion. Must not be the author. Null until confirmed. |
| `acknowledged_at` | When. Null until confirmed. |

**A human finding always enters `active`.** It is a live signal from the moment it is created. Acknowledgement is a property recorded against it, not a gate in front of it.

The co-signer scope rule from Handoff 1 survives unchanged, now as *who may acknowledge*: an `agronomist` whose scope includes the target estate and who is not the author, with the in-scope `estate_manager` as fallback where no agronomist has been active for 24 hours. The fallback is recorded and disclosed wherever the acknowledgement is shown.

**Four rules can now be deleted outright**, because each existed only as a consequence of `pending_review` being a state:

- The rule that a waiting finding freezes nothing. A finding is always active, so it always participates in contradiction.
- The rule that the expiry clock does not run while waiting. The clock always runs from creation.
- Notification suppression as a property of the state. It becomes a delivery policy — see §3.
- The review queue as a place findings live. It becomes a filter on unacknowledged findings.

---

## 3 · Delivery policy is an organisation setting, bounded by the safety floor

The platform serves federal agencies, estate groups, cooperatives and smallholder collectives. Their governance genuinely differs, and one delivery policy for all of them is the wrong kind of opinionated. Set per organisation by an `estate_admin`, alongside verification presets and notification defaults.

Two settings. **Name them for what the administrator is choosing, not in spec vocabulary** — no "restrictive" or "permissive" anywhere a user can see.

| Setting | Behaviour |
| --- | --- |
| **Send immediately, confirm after** | Every human finding delivers on the routing its effective severity earns. Acknowledgement is recorded when it happens and escalates if it does not. |
| **Confirm before sending** | A human finding at `high` holds delivery until acknowledged, **or until its mute bound elapses, whichever comes first**. Every other band delivers immediately. |

### The floor is the bound, and this is the important part

**A held finding is an active finding, so the safety floor applies to it in full.** The earlier design escaped this by arguing the floor does not cover a claim awaiting a signature; once the finding is always active, that argument is gone and should not be reinstated.

The floor's own mute-bound table then settles what may be held and for how long — `critical` never, `high` 4 hours, `medium` 7 days, `low` 30 days:

- **`critical` is never held, under either setting.** It delivers immediately, always. Acknowledgement is recorded after the fact and escalates if absent.
- **`high` may be held for at most 4 hours**, its existing mute bound, and delivers automatically when that elapses whether or not anyone has acknowledged it.

So the organisation setting is **not an exception to the safety floor. It is an instance of it.** Say that on the page in those terms, because a future reader will otherwise assume a governance toggle can override a safety rule, and it cannot.

Note the consequence plainly, because it reads as counter-intuitive and is not: **the gate lands on `high`, not on `critical`.** The most severe band is the one that must never wait. Noise at `critical` is answered by the audit trail, the escalation and the per-role rate limits — not by delaying the page.

---

## 4 · The mode travels with the finding

Record the delivery policy in force on the finding itself, and **disclose it wherever the finding crosses an organisational boundary** — every verification bundle, and every rollup in the [Aggregation Model](/concepts/aggregation-model) and [Effect Methodology](/concepts/effect-methodology).

Without this, a national dashboard aggregates findings that passed different governance bars and presents them as one number. That is the same class of error as averaging across crops, and the same discipline applies: disclose rather than hide, and never present two differently-constituted quantities as comparable without saying so.

---

## 5 · Guardrails to revise

Stay inside the eight categories.

- **Confirmations** — remove co-signing a `pending_review` finding into `active`. Add acknowledging a finding at `high` or above, which remains safety-critical. Raising `critical` remains a confirmation.
- **Refusals** — remove the Option R raise-time refusals. Add: holding a `critical` finding's delivery under any setting; holding a `high` finding beyond its 4-hour mute bound; acknowledging one's own finding.
- **Escalation** — an unacknowledged `critical` escalates to `estate_manager` at 2 hours and copies `org_admin`; an unacknowledged `high` escalates to the agronomist pool at 4 hours, coinciding with automatic delivery. Keep the existing refusal-streak escalation.
- **Audit** — log `acknowledged_by`, `acknowledged_at`, whether the estate-manager fallback was used, and the delivery policy in force at creation.
- **Rate and scope limits** — unchanged, and now carrying more weight. Per-actor per-role rolling caps on `high` and `critical` raises are the primary defence against a manufactured alert, since delivery is no longer gated at `critical`. Say so explicitly.

---

## 6 · Seed

- `fnd_seed_palm_ganoderma_cosign_05` — rename away from co-sign. It now commits `active`, delivers immediately because it is `critical`, and is acknowledged afterwards.
- `fnd_seed_palm_ganoderma_fallback_05b` — keeps its purpose: acknowledged by the estate manager because no agronomist has been active for 24 hours, with the fallback disclosed.
- **Add one fixture** on an organisation set to *confirm before sending*: a `high` finding held, then delivering automatically at the 4-hour bound without acknowledgement. This is the fixture that proves the hold is bounded by the floor rather than by a person.

---

## 7 · Open Questions

**Retire:** Open Question 1 (severity option) and Open Question 4 (co-signer scope) — both are now decided.

**Add:**

1. Whether the delivery policy is settable per estate as well as per organisation. Estates within one agency may have different on-call maturity.
2. Whether an unacknowledged finding older than its expiry window should carry the lack of acknowledgement into the expiry disclosure, or expire silently on the same terms as any other unactioned finding.
3. Whether `regional` officers reading across organisations should see the delivery policy inline on every finding, or only on export. Inline is more honest and noisier.

---

## 8 · Docs Delta

Rows for: the per-role raising table; `pending_review` removed from the state list; `acknowledged_by` and `acknowledged_at` added; the organisation-level delivery policy and its two settings; the safety floor as the bound on delivery holds; `critical` never held; the disclosure requirement at organisational boundaries; and the four deleted rules.

Where a delta touches a page other than this one — the aggregation model and the effect methodology both gain a disclosure requirement — name that page.

---

# Addendum — the six points raised in review

All resolved. Proceed with the edit on these.

## A · The third field, named

Correct catch — §4 and §5 both depend on a field the fields table never declares.

| Field | Type | Applies to | Source |
| --- | --- | --- | --- |
| `delivery_policy_snapshot` | enum: `immediate` \| `hold_until_acknowledged` | human only | Decided. The organisation's delivery policy **as it stood when the finding was created.** Snapshot semantics, matching `author_role`. |

**It must read as frozen, and the name is doing that work.** If an organisation changes its setting, the disclosure on a historical finding must not change with it — otherwise a proof pack exported today describes a governance regime that was not in force when the finding was raised. Do not name it `delivery_policy`; a future reader will wire it to the live setting.

List it alongside `acknowledged_by` and `acknowledged_at` in the fields table and in the Docs Delta.

## B · Migration — none

Amendment C has not shipped. It supersedes its own mechanism before ship, so there is **no migration section and no migration rule.**

The only artifact carrying `pending_review` is the prototype, and unwinding that is a build change rather than a data migration. State this in one line on the page so a future reader does not go looking for a migration path that never existed.

## C · Fixture rename — take yours

`fnd_seed_palm_ganoderma_ack_05`. Your instinct, and naming it here rather than leaving it to the writer is the right call.

Its behaviour under the new model: `critical`, commits `active`, **delivers immediately under either setting**, acknowledged afterwards by a second in-scope agronomist.

## D · The two four-hour timers — they are one deadline

Not a coincidence, and not two timers. **The `high` mute bound is the deadline; delivery and escalation are both consequences of it expiring.** Write it as one rule with two effects, because a reader who sees two independent four-hour timers will eventually "fix" one of them.

Precisely:

- Under **hold_until_acknowledged**, the four-hour mark releases delivery **and** escalates to the agronomist pool.
- Under **immediate**, the finding has already delivered, so the same four-hour mark escalates only.

Same deadline in both settings. One extra consequence in one of them.

## E · Guardrail density — two genuinely go, one is re-keyed

You are right that categories staying at eight is not the same as rules staying flat. Specifically:

- *"A waiting finding freezes nothing"* — **genuinely deleted.** Not reworded. A finding is always `active`, so it always participates in `raise_contradiction`. There is nothing left to except.
- *"Any actor asserting `critical` without a co-signer identified"* — **genuinely deleted.** No co-signer exists at raise time.
- *"A `scout` asserting `high` or `critical`"* — **survives, re-keyed.** It is no longer an Option R artifact; it is the raising table being enforced. Cite the table, not the retired option.

On the Confirmations row growing to six: **consolidate rather than enumerate.** Acknowledging a finding at `high` or above and raising `critical` are the same underlying rule — a serious assertion requires a named human on the record. Write it once. The page is already long and the reader's attention is the scarce resource, not the row count.

## F · Open Question 1 — scope stated, not left ambiguous

**Amendment C ships at organisation granularity.** Estate-level delivery policy is deferred, and it is genuinely open rather than merely unwritten: an agency holding many estates of differing on-call maturity is a real case that a single organisation-wide setting serves badly.

Say both halves — what ships, and that the deferral is a real question rather than an oversight.
