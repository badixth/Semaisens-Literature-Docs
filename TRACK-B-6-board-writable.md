# Track B, Brief 6 — Making the board writable

**Status: built and verified clean.** Drag-writable, five transitions, refusals in words. Tested across passes 9–11; see `DEFECTS.md`. One thing the build decided that this file left open: an undone move **leaves a trace** — *"Both moves stay in the record."*

Self-contained. **Attach nothing.** Every fact you need is inline. If something here contradicts your memory of an earlier phase, this file wins.

Run in a **fresh chat**. This is a successor to `TRACK-B-5-board-view.md`, which shipped as **Where the work is** and is read-only. **Do not rebuild that screen.** This brief changes one thing about it.

**What changed since the first draft of this file.** It previously said the lifecycle enum must land before this is built, and separately forbade this build from defining it — a contradiction that made the brief unbuildable. That is resolved: **the enum is now enumerated in the docs**, at `/guides/field-scouting` → *Task lifecycle*, and §2 and §4 below are rewritten against it. Three earlier open questions are settled and marked where they appear.

---

## 1 · What already exists, verified in the build

Four columns, and they are correct. Do not renegotiate them.

| Column | Count in the seed |
| --- | --- |
| **To do** | 3 |
| **Waiting to be accepted** | 3 |
| **In flight** | 0 — *"Nobody is out on a field right now"* |
| **Done · last 7 days** | 2 |

Also already right, and all of it survives this brief unchanged:

- The page headline is a verdict — *"Three checks are waiting to be accepted."*
- **Overdue is a flag, not a column.** A card in Waiting to be accepted reads *"overdue by 3 days"*.
- **A refused assignment sits in To do**, carrying *"Refused by Hasan Aziz on 13 Jun"*.
- Entitlement is disclosed rather than silent: *"One check sits on ground outside what you can see. It is in neither a column nor a count here."*
- One route per card — **Open the check**.
- The user-facing noun is **check**, never `task`.
- Zero drag affordance anywhere. Verified: no `draggable` attribute, no `onDragStart`, no grab cursor.

---

## 2 · The finding that shapes this brief

**Almost no drag on this board can complete on its own.**

Work through the transitions the four columns imply and the reason becomes obvious:

| Drag | What it actually is | Can it complete silently? |
| --- | --- | --- |
| To do → Waiting to be accepted | **Assigning** | **No.** A drag cannot name an assignee. |
| Waiting to be accepted → In flight | **Accepting** | **No.** Acceptance is the assignee's act, not the viewer's. |
| In flight → Done | **Completing** | **No.** Completion is append-only and confirmation-gated. |
| Waiting to be accepted → To do | **Unassigning** | **Yes.** Nothing is needed but the removal. |
| Anything → backwards out of Done | **Un-completing** | **Never.** Refuse. |

**Exactly one drag out of five completes without a dialogue.**

So the honest design is: **a drag is an entry point to a transition, not the transition itself.** Dropping a card opens the same confirmation the check itself would, pre-filled with the move you intended. That is still worth building — it is faster than opening the check and finding the control — but it is not the instant board a builder will otherwise reach for.

Build it as an instant board and you will either skip the confirmations the lifecycle requires, or produce a card that flies to a column and bounces back when the dialogue is cancelled. Both are worse than the read-only board you already have.

---

## 3 · The rule that matters most

**An illegal drag refuses in words, on the card, with the reason. It never snaps back silently.**

A card springing to its old column with no explanation is a control that lies. This product has already paid for that class of defect once, in a scope dropdown that offered an option which could not be chosen, and the fix was the same: say why.

Concretely:

- The card **stays where the reader dropped it** long enough to carry the refusal, or does not move at all — never moves and then reverts.
- The reason is **specific to this check and this move**, not generic. *"Ali Ismail has not accepted this yet, so it cannot be marked started"* beats *"Invalid move"*.
- The refusal is **visible, not a toast.** A toast is hover's cousin: it disappears, and a reader who looked away has no idea what happened. Same rule as `CONVENTION-scope-and-density.md` — anything a reader must not miss cannot vanish.
- **It clears on the next interaction**, not on a timer.

**And the columns must show what they will accept before the drag begins.** During a drag, a column that cannot take this card is visibly not a target. Refusing after the fact is the fallback, not the plan — the best refusal is the one the reader never triggers.

---

## 4 · Each transition, in full

### To do → Waiting to be accepted · assigning

Opens an assignee picker, pre-filled with nothing.

**Ordered by open checks, fewest first — settled, and not what the docs say.** `/guides/field-scouting` specifies *"the three teammates with the shortest recent scout-response time for the field's zone"*. **No response-time data exists anywhere in the corpus or the seed.** The seed carries `open_tasks`, `scout_capacity_flag` and `last_active_at`, and none of those is response time.

Order by `open_tasks` ascending, because it is **the same number the existing soft warning already reads** — *"Scout already has many open tasks"*. One quantity, one reader. Do **not** substitute `last_active_at`: recency of activity is a plausible-but-wrong proxy for responsiveness, since someone active five minutes ago may be mid-task and the worst person to hand work to. Do **not** add a response-time field to the seed to satisfy the docs — inventing data to justify a design is backwards.

**Say what the ordering is** on the picker — *"Fewest open checks first"*. An unexplained ordering implies an authority it does not have; a reader will otherwise assume it means "best person for this".

**This is a Docs Delta, not a silent substitution.** Record that `/guides/field-scouting` specifies an ordering the platform cannot compute.

**Refuse** where the chosen assignee has no access to the field. That refusal already exists in the field-scouting guardrails; use its words.

**Confirm** on bulk — assignment across estates is confirmation-gated because of notification blast radius, and there is an org-configurable cap on tasks per action.

**Soft-warn**, do not refuse, where the assignee already holds many open checks or has a recent refusal streak. Volume is a warning, never a block.

### Waiting to be accepted → In flight · accepting

**This is the one to get right, and the instinct is wrong.**

Acceptance is the assignee's act. A manager dragging a card into In flight is accepting on someone else's behalf, and the whole escalation rule depends on acceptance meaning what it says: *severity-critical checks with no acceptance within two hours page the on-call role.* If a manager can accept for a scout, that clock can be silenced by someone who is not going to the field.

**So: refuse this drag for anyone other than the assignee**, and say so — *"Only Ali Ismail can accept this. Nudge, or reassign it."* Offer the two things they can actually do.

**Decision required if you disagree:** if there is a legitimate "mark as started on their behalf" action, it is a different verb from acceptance, it needs its own audit distinction, and it must not stop the escalation clock. Stop and ask rather than folding it into accept.

### In flight → Done · completing

**Always confirmation-gated, and irreversible past the undo window.** Completion is append-only.

**Two completions, not one — and this supersedes what was recorded as P-40.** The earlier round settled this as "name no risk-model consequence", on the grounds that the escalation-or-contradiction branch is decided by the scout report and a card has no report. That reasoning is right and the conclusion is half right. The guardrails already split these two cases; follow the split.

**Where the report is complete.** The drag completes, and the confirmation **names the consequence**, because the report already says whether the symptom was found. There is no branch to guess — a confirmed symptom escalates severity, no symptom found raises a contradiction, and the reader is entitled to know which before they commit. They are not filing a card, they are moving a hazard band.

**Where there is no report.** This is the *"closing without a visit or without photos"* case the guardrails single out. Confirm heavily, irreversibly, and **name the absence rather than a consequence**:

> *"No report, so nothing here reaches the risk model. The band stays where it is."*

That is true, and it is the strongest argument the reader could be given for going into the check instead.

**What not to do.** Do not ask "was the symptom found?" on the card. A scout report carries photos, GPS points and counts at stops; a yes/no on a board card would create a second, impoverished path to `scout.completed` with `lesions_confirmed` set and no evidence behind it — which is precisely what the without-photos confirmation exists to make deliberate.

### Waiting to be accepted → To do · unassigning

**The only instant one.** Commits optimistically with the **5-second undo** the product already uses for reassign-before-acceptance. Keep the assignee's name and the date on the card afterwards, the same way a refusal is kept — the history is what the refusal-streak rule reads.

### Any drag backwards out of Done

**Refuse, always.** Completion is append-only; a correction is a new entry, not an edit. **Work done** already states this rule on its own screen — *"Work already done cannot be edited here — a correction is a new entry"* — and the board must not contradict a sibling screen.

### Drags that skip a column

To do → In flight, or To do → Done. **Refuse.** Each names the step that is missing: *"Nobody has been asked to do this yet."* Do not silently perform two transitions from one gesture.

---

## 5 · What a drag can never do

- **Touch a check the reader cannot see.** The entitlement-excluded check is in no column, so it cannot be a source or a target. Confirm it stays that way.
- **Remove the overdue flag.** Overdue is computed from the clock; there is no gesture that makes a late check on-time.
- **Change severity.** The band comes from the finding behind the check. Dragging a card does not touch it — although completing one may, via the risk model, which is why §4 says the confirmation names that.
- **Reorder within a column.** There is no manual priority here. If ordering matters, it is computed, and a drag that appears to reorder but does not persist is another control that lies.

---

## 6 · Audit, and one thing that is easy to drop

**Every drag that changes state writes an audit row**, identical in shape to the same transition performed inside the check. Same envelope, same fields.

**The one thing to get right: `source`.** A move made by dragging is still a human action from the UI, and the audit must not let a board drag look like something the system did. If the envelope distinguishes UI from API from agent from system, a drag is UI, with the actor named.

An undone move — the 5-second window — should leave the record honest. Follow whatever the platform already does for optimistic undo elsewhere rather than inventing a board-specific rule; **if that is not established, stop and ask** rather than deciding it here. Whether an undone action leaves a trace is an audit-contract question, not a board question.

---

## 7 · Keyboard, because a board is the worst offender

Drag-and-drop is the least accessible interaction in any product, and this one governs field safety work.

**Every transition available by drag is available without one.** The card's existing **Open the check** route already reaches all of them, so the floor is met — but state it as a requirement and test it, because the temptation is to add drag and consider the job done.

A card must be reachable and actionable by keyboard alone. If you add a keyboard move affordance, it announces the same refusals in the same words.

---

## 8 · The lifecycle now exists, and one thing to do with it

**The enum landed.** `/guides/field-scouting` → *Task lifecycle* enumerates four resting states — `draft`, `assigned`, `accepted`, `completed` — each cited to behaviour the platform already had, plus the transitions and who may perform them. It is the authority; `boardColumnOf` is not.

**Read the section before you touch a drag**, and one part in particular:

> **A board's columns are not the four states.** Any surface grouping tasks reads state **plus two attributes**. The split between *To do* and *Waiting to be accepted* is **whether the assignee has been asked**, not a state difference.

That is why a check assigned for next Tuesday sits in To do — it has an assignee and no running acceptance clock. It is also why a refused check sits in To do: refusal returns it to `draft` carrying who refused and when, because the refusal-streak escalation reads that history. **Neither refusal nor overdue is a state.**

The three things the section leaves open are the same three this brief leaves open: mark-started-on-behalf, a per-role permission matrix, and whether the lifecycle governs work items beyond scout tasks. Do not close any of them here.

**The vocabulary reconciliation — settled, and it is a labels-file change, not a screen change.**

*Where the work is* uses To do · Waiting to be accepted · In flight · Done. *Work done* filters by Scheduled · In progress · Done · Overdue. One label set, read by both. **The board's names win** — they describe a person's situation, where the timeline's describe an abstract status and lose the acceptance gate entirely.

And **Overdue comes out of that filter.** It sits there as a peer of the other three, but it is orthogonal: a check can be overdue *and* in progress. It belongs as its own toggle, matching how the board already treats it as a flag. Do this in the labels file; change neither screen's grouping.

---

## 9 · Acceptance tests

**Refusals**

1. An illegal drag refuses **in words, on the card**, naming this check and this move. Grep for a generic "invalid" and find none.
2. No card ever moves and then returns. Either it stays where dropped carrying the refusal, or it does not move.
3. A refusal persists until the next interaction. It is not a toast and it does not time out.
4. During a drag, columns that cannot accept the card are visibly not targets.

**Transitions**

5. To do → Waiting to be accepted opens an assignee picker ordered by **open checks ascending**, with capacity flags, and the ordering is stated on the picker.
5b. No response-time ordering exists anywhere. Confirm the Docs Delta is recorded rather than the ordering quietly substituted.
6. Assigning to someone without field access refuses, in the guardrail's existing words.
7. **Waiting to be accepted → In flight refuses for anyone but the assignee**, and offers nudge or reassign.
8. In flight → Done always confirms, and the prompt is stronger without a visit or photos.
9. **With a report:** the confirmation names the consequence — escalation or contradiction — read from the report, not guessed.
9b. **Without a report:** the confirmation names the absence — *"nothing here reaches the risk model"* — and no branch is stated.
9c. **No card ever asks whether the symptom was found.** Grep for it; find nothing.
10. Waiting to be accepted → To do commits instantly with a 5-second undo, and keeps who was asked and when.
11. Every backwards drag out of Done refuses. Completion stays append-only.
12. Every column-skipping drag refuses and names the missing step.

**Invariants**

13. The entitlement-excluded check is neither a drag source nor a target.
14. No gesture clears an overdue flag or changes a severity band.
15. No drag reorders within a column.
16. Every drag-reachable transition is reachable without a drag, by keyboard alone.

**Audit**

17. A drag writes the same audit row as the same transition made inside the check, with `source` identifying the UI and the actor named.
18. Nothing about a board move is recorded as a system action.

**Unchanged from B5 — confirm no regression**

19. Four columns, overdue still a flag, refused still lands in To do with who refused.
20. Done still names its 7-day window.
21. Counts are integers and equal the cards rendered.
22. The entitlement sentence still renders.
23. No duplicate sentence, no underscore, no enum value, no field name in Product mode.
24. Dark and light pass at all three widths.

---

## 10 · Out of scope

- **Rebuilding the board.** It exists and it is right. This changes one thing.
- **Changing the four columns**, or promoting overdue to one.
- **Changing the lifecycle enum.** It now exists at `/guides/field-scouting` → *Task lifecycle* and it is the authority. If a drag needs a state that is not there, **stop and ask** — do not add one here. A build brief must never become the source of truth for an ontology, which is what made the first draft of this file unbuildable.
- **Closing any of the three questions that section leaves open** — mark-started-on-behalf, the per-role permission matrix, the scope of the lifecycle beyond scout tasks.
- **The vocabulary reconciliation itself.** §8 settles what it should be; the change is in the labels file and is not part of this build.
- **Bulk drag.** One card at a time. Bulk assignment already has a confirmed, capped flow and does not need a gesture.

If a decision you need is not in this file and not resolvable from it, **stop and say "Decision required: *question*"** rather than picking a default. The mixed-crop range defect came from a brief that left a gap and a build that filled it reasonably; the reasonable fill was a 28-month window and a crushed axis.
