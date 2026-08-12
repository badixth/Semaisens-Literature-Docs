# Track B, Brief 1 — Manual findings, end to end

**Status: built.**

Self-contained. **Attach nothing** — not the docs repo, not the four spec layers, not the concept pages. They are large and attaching them is what saturated three earlier chats. If something here contradicts your memory of an earlier phase, this file wins.

**This supersedes the earlier version of this brief.** The severity mechanism changed after that version was built — see §15 for exactly what to unbuild.

---

## 1 · Why this is first

A finding raised by a person is an **ontology change**, not a screen. It touches all four spec layers at once: a new entity shape, a lifecycle that nothing fires, a question of who may assert what, and seed rows that must show both origins.

The band model taught us what happens when an ontology change arrives late — roughly fifteen phases as a retrofit, purely because the ontology had no crop, ecosystem or stage. Build the entity once and discover its consequences together.

---

## 2 · The gap

The platform today can produce a finding exactly one way: the risk engine matches a rule card against the signal layer and fires. A person can **escalate, demote or contradict** a finding that already exists — but cannot **originate** one.

Two nearby actions are easy to confuse. **`New note`** is the inert one: explicitly not a task, not delivered as a notification. **`New observation`** is not inert — it logs a ground-truth observation and feeds scout-completion semantics. Its problem is different, and it is the one this brief solves: the workspace can raise a `New observation` with no scout task open to complete, and what that means has never been defined. Giving it a finding to attach to *is* the definition.

The gap bites hardest where the satellite is weakest. Panel dryness shows as a slow, block-wide decline over one to three years. Phosphorus and potassium are indirect only and need a soil test. Multispectral cannot separate ion species at all. The literature documents these hazards in full and the platform cannot record a conclusion about any of them.

---

## 3 · The shape

**One entity, `finding`, with a required `provenance` discriminator.** Not two entities behind a shared interface.

- `provenance = machine` — the existing rule-card firing, renamed. Carries the rule card reference and version, drivers, computed severity, resolved yield impact, and a mitigation window.
- `provenance = human` — carries an author, a role snapshot, a rationale, attached evidence, an asserted severity, and the delivery policy in force when it was raised. **All machine fields are null.**

Every consumer must read the null case honestly and show an em dash rather than fabricate a comparable figure. Same habit as the mean-index panel.

**Provenance is not "who typed it."** An agronomist who accepts or annotates a machine finding does not change its provenance. Provenance records where the *claim* originated.

---

## 4 · Non-negotiables

Closed sets. Do not extend them.

- **Eight guardrail categories.** Input validation, Preconditions, Refusals, Confirmations, Soft warnings, Rate and scope limits, Audit, Escalation. No ninth.
- **Nine telemetry event types.** No tenth.
- **No new roles.** Eight functional roles — `scout`, `agronomist`, `approver`, `estate_manager`, `estate_admin`, `regional`, `org_admin`, `on_call` — plus `viewer`, an **org** role.
- **Append-only once active.** Origin, author, rule-card reference and delivery policy cannot be edited after commit. Corrections are new rows. **One exemption:** promotion, which writes an append row — see §8.
- **Only the system writes a machine finding**, and the system may not author a human one.
- **Human-readable ids only.** `fnd_` prefix. No UUIDs.
- **Compose from existing components.** Preserve base tokens, introduce none. Manrope, 4px spacing family, controls at 30 / 36 / 44px.
- **Product mode carries no spec vocabulary.** §12 has the translation table.

---

## 5 · States

Seven. Two of them — `expired` and `closed_with_cycle` — are new work.

| State | Machine | Human | Enters when |
| --- | :-: | :-: | --- |
| `dormant` | ✓ | — | Rule card exists for this crop and stage but drivers do not match. |
| `active` | ✓ | ✓ | Machine: drivers matched and bindings fired. **Human: on raise, at any band the author's role permits.** |
| `frozen` | ✓ | ✓ | A contradiction was raised. Both sides freeze until agronomy review. |
| `resolved` | ✓ | ✓ | Machine: reset by an applied map. Human: author or an in-scope agronomist / estate manager marks it resolved with a note. |
| `dismissed` | ✓ | ✓ | Dismissed, with 14-day suppression on the same hazard, field and cycle. |
| `expired` | ✓ | ✓ | Sat in `active` past its severity-scaled window without action. See §10. |
| `closed_with_cycle` | ✓ | ✓ | The parent season closed while the finding was still open. Terminal. |

**There is no waiting state.** A human finding is live from the moment it is created. Nothing is held back pending approval. If you find yourself building a queue that findings *live in*, stop — the queue is a filter, not a state (§7.3).

**A human finding is never `dormant`.** Nothing fires it; someone writes it.

**`closed_with_cycle` is not `resolved`.** A resolved finding was acted on. A cycle-closed finding was not, and the record must not claim otherwise. Different chip, different copy, no green.

---

## 6 · Who may assert what

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

**Every finding commits immediately, at every band, for every role in the table.** There is no raise-time gate.

**Why `scout` stops at `medium`.** The safety floor engages at `high` — signals at severity ≥ high cannot be suppressed, muted beyond the bounded window, or personalised away. So `high` is where the platform's least reversible behaviour switches on. A scout's function is ground-truth work: visits, photos, observations. Diagnosis is not in that lane, and a platform where any user can manufacture an unsuppressible alert has a problem.

**Why `approver` and `estate_manager` reach `high` and `critical` directly.** An estate manager runs the estate's operations and owns overdue tasks and deviation escalations. Requiring a countersignature before that role may call a hazard serious inverts the hierarchy the platform is deployed into.

**Read-only means read-only for operations, not observations.** An agency officer cannot assign a scout task, approve a map or export a bundle, but can record what they saw at low or medium.

**Explain every ceiling in the UI. Do not merely grey the control out** — a disabled control with no reason reads as a bug.

---

## 7 · Acknowledgement, and how delivery works

This is the part that changed. Read it carefully even if you built the earlier version.

### 7.1 Acknowledgement is a property, not a gate

A serious assertion should have a second name against it. That is recorded **after** the finding is live, never in front of it.

| Field | Meaning |
| --- | --- |
| `acknowledged_by` | Who confirmed it. Must not be the author. Null until acknowledged. |
| `acknowledged_by_role` | `agronomist` on the normal path, `estate_manager` under the fallback. |
| `acknowledged_at` | When. Null until acknowledged. |

**Who may acknowledge:** an in-scope `agronomist` who is not the author. **Fallback:** an in-scope `estate_manager` where **no *other* in-scope agronomist** has been active in the preceding 24 hours.

The word *other* is load-bearing. The author is barred from acknowledging their own finding, so their own activity must not count as the activity that blocks the fallback — otherwise a single-agronomist estate could never acknowledge anything. Most smallholder collectives and cooperative branches have one agronomist or none. **Build the fallback test to exclude the author.**

Record the fallback and disclose it wherever the acknowledgement appears. A manager's acknowledgement is not an agronomist's.

### 7.2 Delivery policy is an organisation setting

Federal agencies, estate groups, cooperatives and smallholder collectives have genuinely different governance. Set per organisation by an `estate_admin`, alongside verification presets and notification defaults.

| Setting shown to the admin | Enum | Behaviour |
| --- | --- | --- |
| **Send immediately, confirm after** | `immediate` | Every band delivers on the routing its severity earns. Acknowledgement recorded when it happens. |
| **Confirm before sending** | `hold_until_acknowledged` | A `high` finding holds delivery until acknowledged **or until 4 hours elapse, whichever comes first.** Every other band delivers immediately. |

The enum values are the schema; the sentences are labels owned by the labels file. Do not make the label the value.

Snapshot the policy onto each finding as `delivery_policy_at_creation`, **immutable**. If the organisation changes its setting later, a historical finding must not retroactively describe a regime that was not in force.

### 7.3 The bound is the safety floor, not a preference

**`critical` is never held, under either setting.** It delivers immediately, always.

**`high` may be held for at most 4 hours** — its existing mute bound — and delivers automatically when that elapses, acknowledged or not.

A held finding is an active finding, so the safety floor applies to it in full. The floor's mute bounds are `critical` never, `high` 4 hours, `medium` 7 days, `low` 30 days. So the organisation setting is **not an exception to the safety floor; it is an instance of it.**

**The gate lands on `high`, not on `critical`.** This reads backwards and is not: the most severe band is the one that must never wait. Noise at `critical` is answered by the audit trail, the escalation and the rate caps — not by delaying the page.

**The unacknowledged queue is a filter**, not a state: active findings where `acknowledged_by` is null.

### 7.4 One deadline, two effects

The four hours is the safety floor's `high` mute bound. Under `hold_until_acknowledged` it releases delivery **and** escalates to the agronomist pool. Under `immediate` the finding already delivered, so it escalates only.

**Do not build two timers.** If the floor's `high` bound ever moves, both behaviours move with it.

An unacknowledged `critical` escalates to `estate_manager` at 2 hours, copying `org_admin`.

### 7.5 Rate caps are the primary defence

Delivery is not gated at `critical`, so nothing stops a manufactured alert except these. All three ship with the raise flow:

1. **Per-actor, per-role rolling caps** on `high` and `critical` raises, org-configurable, with the limit and reset time stated when hit.
2. **A per-actor hourly cap on bulk raising**, any band.
3. **Three input-validation refusals from one actor inside 24 hours escalates** to the estate manager. This is what catches someone probing the validator.

These are not belt-and-braces. They are the whole abuse story.

---

## 8 · Promotion

A scout who believes what they are seeing warrants more than `medium` raises at their ceiling with evidence, and an in-scope agronomist promotes in one action.

**Promotion reuses `promote_severity`**, the effect the risk model already defines — one band up — and already uses for `task.overdue`. Do not invent a parallel path.

**`author_ref` does not change.** The scout stays the author of the observation; the agronomist is recorded as the promoter. A promoted finding shows both the band its author asserted and the band the platform now acts on.

Promotion writes an append row: `promoted_by`, `promoted_by_role`, `promoted_at`, `severity_before_promotion`. It is the one stated exemption to the append-only rule on effective severity.

Build promotion as one action from the finding — never a re-raise. The scout's observation must not be lost or retyped.

---

## 9 · Screen work

### 9.1 Raise flow

Entry point: the existing `New observation` action. Preserve its placement; change what it opens.

| Captured | Rule |
| --- | --- |
| Field and cycle | Inside the actor's scope. The field must have an **open** cycle. |
| What was seen | Plain-language hazard picker from the crop's literature. An "I can't find it here" escape hatch is required. |
| When it was seen | Defaults to now. Back-dating capped at 72 hours (§14). |
| Why they think so | Free text, **minimum 40 characters**, required. This is the whole accountability story. |
| Evidence | At least one item at medium and above. |
| How serious | The four-band ladder, bounded by role per §6. |

Growth stage resolves from planting date and observation time. Do not ask, do not allow an override.

**The unclassified path.** Warn before commit: the finding will not benefit from automatic rule-card behaviour and will not feed the farm's own history until corroborated. Soft warning only — the escape hatch must work, because the hazards the satellite cannot see are precisely the ones with no rule card.

**Signal disagreement.** If the asserted band sits two or more bands above the signal layer, warn that the signal does not corroborate it. Also soft. The panel-dryness fixture trips this deliberately and must commit anyway.

**No agronomist available.** If no *other* in-scope agronomist has been active in 24 hours, say so at raise time: acknowledgement will fall to the estate manager and the escalation timer is running.

### 9.2 Origin disclosure

| Origin | Chip | Detail view also shows |
| --- | --- | --- |
| Machine | Detected by satellite | The rule card and **its version** |
| Human | Reported by *name*, *role* | The author's **identity**, and **when they say they saw it** — not when they logged it |

An unacknowledged finding must be **visually distinct** from an acknowledged one on every surface, including notification bodies. Unacknowledged is a property; the finding is live either way.

A held `high` finding that delivered automatically at the bound displays that fact: *"delivered on safety-floor bound, unacknowledged at 4 hours."*

### 9.3 Feed placement

A human finding lands in the activity feed as its own row. Notification routing follows effective severity, the recipient's preferences, and the organisation's delivery policy.

Known taxonomy tension, flagged so you do not resolve it silently: the feed splits alerts (from data) from notifications (from humans). A human-raised field-level finding is both. **Put it in the alert lane with the origin chip** and note the tension in your handoff. Do not invent a third lane.

### 9.4 Duplicate detection

When raised on a field with an open finding on the same hazard inside 72 hours, the platform **asks**. Never merges silently.

- Merge authority: agronomist, estate manager, estate admin.
- A `scout` is offered "add your evidence to the existing finding" instead.
- If declined, both rows stay visible and the hazard count carries both.

### 9.5 Contradiction

If a human and a machine finding on the same hazard and field disagree by two or more bands, **both freeze** and route to agronomy review. Neither wins on screen. Show both bands side by side with their origin chips.

**Every human finding participates from the moment it exists**, because there is no waiting state. What stops this being weaponised is the raising table and the rate caps — not a state gate.

---

## 10 · Expiry

| Effective severity | Window |
| --- | --- |
| low | 30 days |
| medium | 14 days |
| high | 72 hours |
| critical | 24 hours |

**The clock starts at `active`**, which for a human finding is the moment of raise. Acknowledgement does not gate, pause or restart it; the unacknowledged case is handled by the escalation timers running in parallel.

**These windows are unrelated to the mute bounds.** The mute bound governs how long notification may be suppressed; the expiry window governs how long an unactioned finding survives. Neither bounds the other — do not derive one from the other.

**Expiry is disclosed, never silent:** *"Expired unactioned on 12 Jul · 72-hour window at high."*

**Expiry does not move the yield estimate in this phase.** Build the state, the windows, the disclosure. Do not touch the forecast.

---

## 11 · Two refusals to widen

1. The scouting refusal on suppressing a task reads *"originates from a severity ≥ high rule card."* As written, a human high finding is suppressible while a machine one is not. Widen to any finding at effective severity high or above, **whatever its origin.**
2. Muting a finding at effective severity high or above beyond the safety floor's bounded window is refused, identically for both origins.

Dismissal is the same for both: 14-day suppression on the same hazard, field and cycle. A dismissed **critical** finding still surfaces every re-raise during the window as a new activity entry — only the notification is suppressed.

---

## 12 · Copy rules

| Never show | Say instead |
| --- | --- |
| `provenance`, `machine`, `human` | Detected by satellite / Reported by *name*, *role* |
| `severity_effective`, `asserted_severity` | The severity chip labels already in the risk feed |
| `acknowledged_by` | "Confirmed by *name*" / "Not yet confirmed" |
| `delivery_policy_at_creation`, `hold_until_acknowledged` | "Urgent findings are confirmed before they are sent" |
| `promote_severity`, `promoted_by` | "Raised to *band* by *name*" |
| `hazard_ref`, `unclassified` | The hazard's plain name; "Not listed" |
| `closed_with_cycle` | "Season closed before this was actioned" |
| `expired` | "Expired unactioned on *date*" |
| `corroboration_ref` | "Confirmed by a field check" |

Malaysian farming terms where they are the words people use. Developer jargon is not.

---

## 13 · Seed fixtures

| Id | Field | Origin | Band | Proves |
| --- | --- | --- | --- | --- |
| `fnd_seed_rubber_panel_dryness_01` | `field_ldg_perak_r3` | human, agronomist | high | **The argument.** Panel dryness the satellite is correct not to flag — it surfaces over one to three years and R3 reads inside band. |
| `fnd_seed_rice_blb_duplicate_02` | `field_muda_b1` | human, scout | medium | The duplicate prompt. Left unmerged so both rows stay. |
| `fnd_seed_rice_blast_machine_03` | Rice field 1 | machine | high | Baseline firing. Pairs with 04. |
| `fnd_seed_rice_blast_paired_04` | Rice field 1 | human, agronomist | **low** | Two bands below 03 — contradiction freeze. The engine reads blast; the agronomist walks the field and says it is not. |
| `fnd_seed_palm_ganoderma_ack_05` | Oil palm field 1 | human, agronomist | critical | Delivers **immediately** because critical is never held; a peer agronomist acknowledges afterwards. |
| `fnd_seed_palm_ganoderma_ack_fallback_05b` | Oil palm field 3 | human, agronomist | critical | **Single-agronomist estate.** The author is the only agronomist, so the estate manager acknowledges. Proves the author is excluded from the fallback test. |
| `fnd_seed_palm_expired_06` | Oil palm field 2 | human, scout | medium | Expires at 14 days with disclosure. |
| `fnd_seed_pine_forcing_corroborated_07` | Pineapple batch 1 | human, agronomist | low | Resolved and corroborated. **Feeds** the farm's history. |
| `fnd_seed_pine_forcing_uncorroborated_08` | Pineapple batch 2 | human, agronomist | low | Resolved, never corroborated. **Does not feed** history. |
| `fnd_seed_rubber_closed_with_cycle_09` | Rubber field 2 | human, scout | medium | Season closes while open. Terminal, disclosed. |
| `fnd_seed_regional_observation_10` | Rice field 3 | human, regional | low | The agency-officer lane. Refused at high and critical. |
| `fnd_seed_rice_blast_hold_bound_11` | Rice field 2 | human, agronomist | high, org on `hold_until_acknowledged` | **Held 4 hours, then delivers automatically with nobody having acknowledged it.** Proves the hold is bounded by the safety floor, not by a person's availability. |

Also add a promotion fixture: a scout raises at `medium`, an agronomist promotes to `high`, and the record shows the scout as author and the agronomist as promoter.

Bundle fixture 01 through verification end to end at **weak** grade, so the weak-flag disclosure is exercised on a human-origin claim.

---

## 14 · Acceptance tests

Run 1 and 2 first.

1. **No finding is ever held from view pending approval.** Every raise, at every permitted band, commits to active immediately.
2. A **single-agronomist estate** can acknowledge a `critical` finding raised by that agronomist — the estate manager records it, and the fallback is disclosed.
3. `agronomist`, `approver` and `estate_manager` each raise `high` and `critical` directly.
4. A `scout` is capped at medium, told why, and can be promoted by an agronomist in one action **without the observation being retyped**.
5. After promotion, **the scout is still the author** and the agronomist is shown as promoter. Both bands are visible.
6. A `regional` officer raises low and medium, and is told plainly why high is unavailable.
7. Fixture 05 (`critical`) **delivers immediately** under both delivery settings.
8. Fixture 11 (`high`, hold setting) holds, then delivers automatically at 4 hours with `acknowledged_by` still null, and says so on screen.
9. Nobody can acknowledge their own finding.
10. An unacknowledged finding is visually distinct from an acknowledged one on every surface, including notification bodies.
11. Changing the organisation's delivery setting does **not** change the disclosure on findings raised before the change.
12. Fixtures 03 and 04 both land in `frozen`, neither displays as winning, both bands shown side by side.
13. Exceeding the rolling cap on `high` raises blocks the commit with a stated limit and reset time.
14. Three input-validation refusals from one actor in 24 hours escalates to the estate manager.
15. A committed finding offers no edit control for origin, author, rule card or delivery policy. Promotion is the only severity change, and it appends.
16. Fixture 06 expires at 14 days with the date and window stated.
17. Fixture 09 renders as season-closed, not resolved, with no success colouring.
18. Fixture 07 appears in the band-resolver explain trace; fixture 08 does not.
19. A task spawned from any finding at high or above cannot be suppressed, regardless of origin.
20. No screen in Product mode contains an underscore, an enum value or a field name. Grep the rendered copy.
21. Dark and light both pass on every new screen, at all three widths.

---

## 15 · What to unbuild

The earlier version of this brief specified a raise-time gate. If you built it, remove:

- **The `pending_review` state.** Findings are never held from view.
- **The co-sign review queue as a place findings live.** It becomes a filter on unacknowledged active findings.
- **Notification suppression as a property of the state.** It is now a delivery policy, and it applies only to `high`, never to `critical`.
- **The rule that a waiting finding freezes nothing.** Every finding participates in contradiction from creation.
- **The rule that the expiry clock does not run while waiting.** The clock always runs from `active`.
- **`co_signer_ref` and `co_signed_at`**, replaced by the three acknowledgement fields.

This is deletion, not restructuring — a state comes out of the machine, no branch goes in. The waiting badge becomes an unacknowledged badge.

---

## 16 · Out of scope

- **Expiry affecting the yield forecast.** Open. State and disclosure only.
- **Per-estate delivery policy.** Ships at organisation granularity. Per-estate is deferred and genuinely open.
- **Bulk import of retrospective findings.**
- **Repeated unclassified labels becoming rule-card proposals.** Note where the hook goes; do not build the queue.
- **Back-dating beyond 72 hours.** Cap it, say so, flag it.

Two places this brief picks rather than stops, both flagged upstream as tensions and neither settled by a decision record. If you disagree, say so rather than working around it:

- §9.3 puts human findings in the **alert lane**. A third lane splits the feed for every reader to describe a source they can already see on the chip.
- §9.1 caps back-dating at **72 hours**, matching the existing late-log rule. The smaller assumption.

If a decision you need is not in this file and not resolvable from it, **stop and say "Decision required: *question*"** rather than picking a default. The mixed-crop range defect came from a brief that left a gap and a build that filled it reasonably; the reasonable fill was a 28-month window and a crushed axis.
