# Track B, Brief 7 — Task origination: create, brief, assign

**Status: built and verified clean.** Create, brief and assign all work; T1–T5 closed. See `DEFECTS.md` passes 15–17.

Self-contained. **Attach nothing.** Every fact you need is inline, and every quotation below was pulled from the live docs on 10 August 2026. If something here contradicts your memory of an earlier phase, this file wins.

Run in a **fresh chat**.

---

## 0 · Read this part first, because it changes what you thought you knew

**The product can move work it cannot make.** `Where the work is` is drag-writable across four columns. `Field checks · phone` executes a scout's queue. `What needs you` ranks decisions. Nothing anywhere creates a task.

**And the docs already specify the flow that is missing.** This brief is not an invention. `/guides/field-scouting` carries a seven-step creation flow, a required-field list, an assignee-ranking heuristic, a capacity rule, and all eight guardrail rows. The build simply never happened. **Where this brief quotes the docs, build to the quote.** Where it decides something the docs leave open, §5 says so explicitly and gives the reasoning.

**Three docs defects surfaced while writing this. Do not build around them silently — they are listed in §9 and one of them affects you directly.**

---

## 1 · The question this flow answers

**Something is wrong in a field. How does that become work a named person has agreed to do?**

Today the answer is: it does not. A finding sits at `high`, its window closes, and the only surface that could act on it is a board with nothing to move. **`draft` is a state with no origin** — the lifecycle was enumerated before anything could produce its first state, which is the wrong order and it was my sequencing, not the build's.

---

## 2 · What is inherited, and is not up for renegotiation

**The eight functional roles.** `scout`, `agronomist`, `approver`, `estate_manager`, `estate_admin`, `regional`, `org_admin`, `on_call` (an overlay, not a primary). Plus `viewer`, which is an **org** role. There is no ninth, and "Farm owner" is a persona.

**One entity, `finding`,** with a required `provenance` discriminator. A person may originate one. There is no separate "risk" or "alert" entity — see §5.1, where this collides with an older enum.

**Human-readable ids only** — `fld_`, `blk_`, `zon_`, `ssn_`, `obs_`, `fnd_`. Tasks will need one; use `tsk_`, which the docs already use (`"task": "tsk_01j8xyz"` in the activity feed example).

**Product mode carries no spec vocabulary.** No `assignee_id`, no `source`, no enum values anywhere a user can see. Review mode may.

**Refuse rather than mislead.** A refusal is an answer and renders as one.

**From `CONVENTION-scope-and-density.md`:** one writable scope control per screen — on operational surfaces that is the breadcrumb, and there is **no page scope row**. Three fates for a sentence: visible if it changes what the reader believes, tooltip only if it defines a term and could be deleted safely, trace or docs if it explains method.

**From the board work (TRACK-B-6), settled and built:** **acceptance is the assignee's act.** A manager dragging a card to In flight is refused, because acceptance is what stops the two-hour on-call escalation. And **an undone move leaves a trace** — *"Both moves stay in the record."*

**From the assignee-ordering error, learned the hard way:** ranking candidates by fewest open checks put everyone who **cannot see the field** at the top with zero, and the only eligible person last, at capacity. **Eligible people are ranked; ineligible people are listed below, unranked, with the reason.** Never one list.

---

## 3 · What the docs already specify — build to these

Every line in this section is quoted from `/guides/field-scouting`, `/guides/activity-and-alerts`, `/guides/fields-workspace/overview` or `/snippets/role-model`. **Cite them; do not improve them.**

### 3.1 Two origination paths, and a third entry point

> *"Every scout task is either created from an alert or risk card (with an AI-generated brief) or from a scouting plan (a route derived from anomaly-ranked zones)."*

**Path 1 — from a card.** *"Click **Open scout task** on any Risk Monitoring card or field alert. The platform pre-populates the task with an AI brief (why, what, when, how) drawn from the matching rule card and diagnosis page. You pick the assignee and the task is ready."*

**Path 2 — from a route.** *"Generate a full-field route from the latest NDVI map. The system ranks anomalous zones and lays out a time-efficient walking order. Use this for periodic scouting or when no specific risk is driving the visit."*

**The third entry point is already named:** the fields-workspace FAB — *"**New scout task** | Opens the Field Scouting new-task drawer, pre-filled with the selected geometry and any active rule card."* **A drawer, not a page.** Build it as one.

### 3.2 The required-field set — this is the hard statement

Input validation, verbatim:

> *"Task requires: `field_id`, `assignee_id`, `source` (alert, risk, plan), and a populated why/what/when/how brief. Geometry of the target zone must be within the field boundary. The advisor pre-fills all of these from Sense + Interpret; **the user reviews rather than authors**."*

**That last clause is the design**, and `/guides/semai-advisor/overview` states it again in full, naming every field that arrives pre-filled:

> *"By the time the advisor presents a draft, every input it could infer from Sense and Interpret is already filled: **field, block, cycle, firing rule card, severity, suggested assignee, why/what/when/how brief, target zone.** The user sees a pre-filled draft, not an empty form. They review, adjust, confirm."*

**Note that the assignee is on that list.** The drawer opens with a suggestion already in it, and §3.5's three-candidate picker is how the reader changes it — not how they make the first choice.

This is not a blank form with an assist button. The platform knows why the task exists — a finding with its reading, its band, its stage and its date — and the human's job is to review, adjust and name someone. **A blank form would be the product inventing a reason, which is the one thing it exists not to do.**

`/guides/semai-advisor/intent-taxonomy` confirms the draft is a real object the advisor produces: *"Assign scouting to Ali for Blok A2" → Task → Field Scouting draft*.

### 3.3 The brief has four fixed sections, each with a stated source

| Section | What fills it |
| --- | --- |
| **Why** | *"The alert or risk summary, the rule card `drivers` that fired, and the current severity band."* |
| **What** | *"The `mitigation.actions` from the rule card, filtered to scout-relevant steps."* |
| **When** | *"`mitigation.window_days_by_severity` for the current severity, minus any elapsed time. Renders as a countdown."* |
| **How** | *"The crop-specific field-walk-protocol and diagnosis page excerpt from `literature_ref` on the rule card."* |

**The brief is not a frozen snapshot:** *"The AI brief is generated once when the task is created, and re-generated whenever the underlying rule card's severity changes. The assignee always sees the current window and current action list, not a stale copy."*

**There is no user-set due date.** Time is always derived from the mitigation window. The docs render it absolutely — *"3 days remaining. Complete before Fri 14 Jun."* — but that is a rendering of the window, not an input. **Do not add a date picker.**

### 3.4 The sample plan, editable before assigning

> *"**Sample size** — default from `drivers` confidence (e.g., 8 tillers for a blast confirmation). **GPS points** — centroids of the affected zones, ranked by severity. **What to record** — a checklist of the specific symptoms named in the diagnosis page (e.g., \"diamond-shaped lesions\", \"collar rot\", \"whiteheads\")."*

> *"Adjust any of these before assigning."*

### 3.5 Choosing the assignee — the heuristic is specified

> *"Select a teammate. The platform surfaces the three teammates with the shortest recent scout-response time for the field's zone, and flags anyone already at task capacity. Change assignee at any time before completion."*

**Three candidates. Ranked by recent scout-response time in that zone. Capacity is a flag, never a block:**

> *"No hard cap on total open tasks per scout; workflow volume is a soft warning, not a refusal."*

**Eligibility is a hard precondition:** *"Assignee must have read access to the field and belong to the same estate or an authorized cross-estate role."* Assigning to a user without field access is a **refusal**.

**This is where the ordering rule from §2 binds.** Rank the three eligible candidates by response time. Anyone ineligible appears below, unranked, with the reason — *"cannot see this field"* — and cannot be selected.

### 3.6 What happens on assign

> *"Click **Assign**. The task appears in the assignee's mobile app queue immediately, and a `task.assigned` entry lands in their Activity & Alerts feed. Delivery channel follows the assignee's Notification Preferences."*

**Create, reassign-before-acceptance and edit-brief all commit optimistically with a 5-second undo.** Quoted twice, in field-scouting and in the advisor's tier table.

### 3.7 The escalations, all three

> *"Severity-critical tasks with **no acceptance within 2 hours** page the on-call role."*
> *"Task overdue past `mitigation.window_days_by_severity` escalates to the estate manager."*
> *"Three refused assignments by the same scout in 7 days escalates to their supervisor."*

`on_call` resolves per `/snippets/role-model`: *"If no `on_call` holder is configured for an estate at the moment an escalation fires, the escalation falls back to `estate_manager` and records `delivery_outcome: delivered_via_fallback`."* Both the overdue and refusal-streak escalations also land on `estate_manager`.

### 3.8 Preconditions and refusals, verbatim

**Preconditions:** *"Field must have an active crop cycle."* Plus the assignee access rule in §3.5.

**Refusals:** *"Creating a task on a closed cycle."* *"Assigning to a user without field access."*

**And one more precondition, from `/guides/semai-advisor/failure-modes`:** *"The advisor will not draft a scout task or VRA on **stale imagery**. It offers to notify the user when fresh data arrives."*

This one matters because it shapes the drawer's empty state. **Stale imagery does not block a human from raising a task — it blocks the pre-fill.** The honest behaviour is to say so and offer the notify, rather than to open a drawer whose *Why* section is built on a reading nobody should act on.

---

## 4 · The screens

Three surfaces. **Two are new; the third is an edit.**

### 4.1 The new-task drawer

Opens from three places, all of which already exist in the product or the docs: the fields-workspace FAB, a finding on `What needs you`, and a `New check` action on `Where the work is`.

**It opens pre-filled or it does not open.** If there is no rule card, no finding and no route behind it, the drawer opens on Path 2 — pick a field, generate a route — never on an empty form.

Order down the drawer, which is also the reading order:

1. **The field and the zone**, stated in words. Geometry shown as a shape, not coordinates.
2. **The brief, four sections**, editable, with each section's origin visible on demand rather than inline. *Why* is the highest-value sentence on the screen — it is what the assignee will read on a phone in a field.
3. **The sample plan** — sample size, points, what to record.
4. **The assignee**, per §3.5 and the ranking rule.
5. **Assign.**

**The countdown is derived and read-only.** Show it; do not offer to set it.

### 4.2 The assignee picker

Its own component, because the rule is subtle enough to get wrong twice.

> **Eligible, ranked** — three by shortest recent response time in this zone, each with their open-check count and a capacity flag where it applies.
> **Not eligible, listed** — unranked, greyed, each carrying its reason. *"Cannot see Muda C4."*

**Never sort the two groups into one list.** A person with zero open checks who cannot see the field is not the best candidate; they are not a candidate.

### 4.3 The edit — `Where the work is` gains an origin

The board currently starts with three checks waiting to be accepted and no way to add a fourth. Add **`New check`** to the board header, opening the drawer of §4.1 with the board's current scope pre-filled.

**And fix the copy while you are there.** The board's intro line reads *"Drag a card to start a move."* At phone width there is no drag. Lead with the tap path; mention drag second.

---

## 5 · Decisions I am making, and the reasoning

The docs are silent on each of these. I am deciding rather than stopping, because each is resolvable from the closed sets. **Each is marked so a later reader can find and overturn it.**

### 5.1 `source` gains `finding`, and this is a Docs Delta

The documented enum is `source (alert, risk, plan)`. **The settled model has one entity, `finding`, with a `provenance` discriminator, and no "risk" or "alert" entity at all.** The guide predates that work.

**Decision: the drawer records `source: finding | plan`, and `/guides/field-scouting` is patched.** The closed-set rule outranks an older guide's enum — CLAUDE.md names the one-entity model as settled and non-negotiable, and two vocabularies for one thing is the duplicated-authority defect this project has paid for five times.

**Record it as a Docs Delta.** Do not patch the guide from inside the build.

### 5.2 `draft` persists, and validation runs at assign

The docs require `assignee_id` at Input validation, which cannot be true of a draft that has no assignee yet. The two statements are only compatible one way.

**Decision: a draft persists, may carry a null assignee, and Input validation runs at the assign transition rather than at save.** Two reasons. The validation list names `assignee_id`, which is knowable only at assign. And refusal returns a task to `draft` **carrying its history**, which requires draft to be a real, persisted thing rather than an unsaved form.

**The alternative** — draft is unsaved local state and the task exists only from assign — is defensible and simpler, but it loses a refused task's history. If you take it, say so loudly; it changes what refusal means.

### 5.3 Who may create and who may assign

Not evidenced anywhere. TRACK-B-6 left the per-role permission matrix open; **this brief closes it.**

| Role | Create a draft | Assign | Accept | Refuse |
| --- | --- | --- | --- | --- |
| `scout` | Yes | **No** | Yes — their own | Yes — their own |
| `agronomist` | Yes | Yes | No | No |
| `approver` | Yes | Yes | No | No |
| `estate_manager` | Yes | Yes | No | No |
| `estate_admin` | No | No | No | No |
| `regional` | No | No | No | No |
| `org_admin` | No | No | No | No |
| `viewer` (org) | No | No | No | No |

**Two rules carry the weight.** A scout may raise a draft — they are standing in the field and are the person most likely to see something — but may not assign, including to themselves, because assignment is what starts the escalation clock. And **acceptance stays the assignee's act**, which is settled and built.

`estate_admin` sets the `on_call` overlay; it is an administrative role, not an operational one.

### 5.4 Refusal captures a reason, and the reason travels

Not evidenced. The docs count refusals — three in seven days escalates — but never describe the act.

**Decision: refusing requires a reason from a short list plus optional free text, the task returns to `draft` carrying its full history, and the assigner is notified.** A refusal that vanishes is indistinguishable from a task nobody looked at, and the escalation rule is meaningless if the platform cannot tell the two apart.

**Note the term collision and do not repeat it on the surface.** "Refusals" is one of the eight guardrail categories, meaning *the system refuses a write*. A scout declining an assignment is a workflow act. **Two meanings, one word, and both appear in the same guardrails table today.** On the surface, call the scout's act **declining**.

### 5.5 The two-hour clock starts at delivery

Not evidenced. **Decision: at notification delivery, not at assign.** The escalation exists because nobody has agreed to do the work; a scout who has not been reached has not failed to respond. This matches the delivery-policy model, where an organisation's channel choice shapes when a person actually hears.

---

## 6 · Decisions I am not making

**Stop and say "Decision required: *question*" rather than filling either of these.**

**Capacity has no number.** The docs say *"flags anyone already at task capacity"* and *"no hard cap"*, and never state a threshold. Do not pick one. Show the open-check count and let the reader judge; the flag can wait for a policy.

**Whether a non-`scout` role may be an assignee.** `/snippets/role-model` says `scout` is *"Assignee on scout tasks"* but does not say *only* `scout`. An agronomist walking a field is plausible and unevidenced. Assume `scout` only, and surface the question rather than widening it quietly.

---

## 7 · Guardrails — all eight, and none of them new

There is no ninth category. Every row below is either quoted or derived from a quoted rule.

| Category | What applies here |
| --- | --- |
| **Input validation** | `field_id`, `assignee_id`, `source`, a populated four-section brief, and target geometry inside the field boundary. Runs at assign, per §5.2. |
| **Preconditions** | Field has an active crop cycle. Assignee has read access and belongs to the same estate or an authorised cross-estate role. |
| **Refusals** | Creating a task on a closed cycle. Assigning to a user without field access. Both refuse **in words on the surface**, never by disabling a control silently. |
| **Confirmations** | None on create. *"Everything else, including create, reassign-before-acceptance, and edit-brief, commits optimistically with a 5-second undo."* |
| **Soft warnings** | *"Scout already has many open tasks. Field has received many tasks recently. Assignee has a recent refusal streak. Duplicate brief detected against a recent task on the same rule card version."* All four surface and none block. |
| **Rate and scope limits** | *"Bulk assignment capped at N tasks per action (org-configurable) because of notification blast radius."* N is org policy — do not invent it. |
| **Audit** | *"Every task write logs actor, source (UI, API, agent), rule card ID and version, assignee, timestamps, and any severity band changes."* A decline writes its own row. |
| **Escalation** | The three in §3.7, unchanged. |

**Telemetry stays at nine event types.** `task.assigned`, `task.overdue` and `scout.completed` are named in the docs. **A drawer opening is not telemetry.** If declining needs an event, that is a Decision required, not a tenth type invented in the build.

---

## 8 · Acceptance tests

Measured or driven, not eyeballed.

1. A task can be created from a finding on `What needs you`, and the resulting brief's **Why** cites that finding's reading, band and stage — not generic text.
2. A task can be created from a route on a field with no finding, via Path 2.
3. **The drawer never opens empty.** With no card, no finding and no route, it opens on field selection.
4. Creating a task on a **closed cycle** refuses in words, on the surface, naming the cycle.
5. Assigning to someone **without field access** refuses in words, naming the field they cannot see.
6. The assignee picker renders **two groups**. Eligible are ranked by response time; ineligible are unranked, unselectable, each carrying its reason.
7. A person with **zero open checks who cannot see the field** appears in the lower group, never at the top of the upper one.
8. There is **no due-date input** anywhere in the flow. The countdown derives from the mitigation window.
9. Assign commits with a **5-second undo**, and the undone assignment leaves a trace.
10. A newly assigned task appears in `Where the work is` under **Waiting to be accepted**, and in `Field checks · phone` for that assignee.
11. **A manager cannot accept on the assignee's behalf** — still refused, per TRACK-B-6.
12. Declining requires a reason, returns the task to `draft` **with its history intact**, and notifies the assigner.
13. A third decline by the same scout within 7 days surfaces the escalation to `estate_manager`.
14. **No spec vocabulary in Product mode.** Grep the rendered copy for `assignee_id`, `field_id`, `source`, `alert`, `risk`, `plan`, `draft`, `mitigation`.
15. `New check` on the board opens the drawer with the board's scope pre-filled.
16. At **390 px** the drawer is usable: single column, every control ≥ 44 px, zero horizontal overflow, and no refusal truncated.
17. At **834 and 1440**, nothing else regressed.

---

## 9 · Three docs defects found while writing this — one affects you

**`/guides/field-scouting` has no Task lifecycle section.** Verified against `main` at `2cf4748` on 10 August 2026: a search for *Task lifecycle* returns **zero matches**, and `draft` appears once in the whole guide, as prose. The branch `admin-mcp/task-lifecycle-enum-2cf4748` exists and is **unmerged**. `TRACK-B-6-board-writable.md` asserts twice that the enum landed. It did not. TRACK-B-5 has it right: *"The docs reference task states in prose and in one audit example, but nowhere enumerate them as a state machine."*

**This is the one that affects you: build to this brief, not to a lifecycle section you will not find.** Correcting TRACK-B-6 and merging the PR is a separate job.

**`/guides/prescription-maps` names four roles that are not in the closed set of eight** — Operator, Agronomist, Approver/Manager, Read-only. `Read-only` is `viewer`; **`Operator` is not any of the eight.** Recorded, not patched here.

**Two different fields are both called `source`** in the field-scouting guardrails table — one is `alert | risk | plan`, the other is the audit channel `UI | API | agent`. Recorded.

---

## 10 · Out of scope

- **Prescriptions and VRA.** Their creation flow is fully documented and **blocked**: every band is `status: decision_required`, and the fail-closed rule means validation refuses every prescription. See `DECISION-rate-bands-do-not-exist.md`. That flow is worth building as a separate brief precisely *because* it ends in a refusal — but not in this one.
- **The board's columns, drag behaviour and refusals.** Built and settled in TRACK-B-6.
- **Acceptance mechanics.** Settled; this brief only produces the state that precedes them.
- **Any Track C surface.**
- **The phone layout of existing screens.** Phase 11 closed it. The drawer must meet the same bar, which is test 16 — that is the whole of the phone work here.
- **Renaming `Application plans`.** The docs' entity is a **prescription** or **VRA map**; "application plan" is not evidenced anywhere. Real, and a separate pass.

If a decision you need is not in this file and not resolvable from it, **stop and say "Decision required: *question*"** rather than picking a reasonable default.
