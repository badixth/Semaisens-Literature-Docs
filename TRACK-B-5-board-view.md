# Track B, Brief 5 — The board view

**Status: built, as "Where the work is".** Read-only, as decided. The successor brief `TRACK-B-6-board-writable.md` makes it writable and does not rebuild it.

Self-contained. **Attach nothing.** Every fact you need is inline. If something here contradicts your memory of an earlier phase, this file wins.

Run in a **fresh chat**. This is the last Track B surface, and the smallest.

---

## 1 · The question this screen answers

**Where is the work, and what is stuck?**

"What needs you" already answers *what should I do next* as a list. A board answers a different question — *how is the queue shaped* — which a list cannot, because a list has no columns and therefore no pile-up to see.

If the board does not make a pile-up visible at a glance, it has no reason to exist beside the list.

---

## 2 · The decision that shapes everything below

**The board is read-only. It is decided, not open.**

You cannot drag a card to change its state. Every state transition keeps happening where it already happens — in the task, through the existing lifecycle, with its existing role checks and audit rows.

**This is the hard part of the brief, because every board a reader has ever used is draggable.** A board that looks writable and is not produces a failed drag, and a card that springs back to its old column with no explanation is a control that lies. That is the same defect class as a scope dropdown offering an option that cannot be chosen, and this product has already paid for it once.

So the rule is not just *do not implement drag*. It is:

- **No drag affordance of any kind.** No grab cursor, no lift shadow on hover, no column-edge highlight, no ghost card. If the pointer suggests the card can be picked up, the pointer is lying.
- **Say it once, on the page, before the reader tries.** A short line under the title — *"A view of the queue. Work is moved from the task itself, not from here."* — costs one sentence and saves every reader one failed drag.
- **Never explain it on the card.** Per instance is the repeated-rule defect; once, in the page header.

---

## 3 · The columns, and the gap behind them

**Correction, and read this before the table.** An earlier note in `START-HERE.md` said the task lifecycle "already has all seven states". **It does not, in the docs.** I checked. The docs reference task states in prose and in one audit example, but **nowhere enumerate them as a state machine.** Per the authority rule in `CLAUDE.md`, the docs win over that note, so the note was wrong.

Here is everything the source of truth actually evidences, with citations:

| Evidence | Where |
| --- | --- |
| `scout_task` → `draft`, then → `assigned`, as audit rows | `/snippets/audit-envelope` — the only place two states are named together |
| **Acceptance is a distinct step after assignment** | `/guides/field-scouting` — *"reassign-before-acceptance"*, and *"Severity-critical tasks with no acceptance within 2 hours page the on-call role"* |
| **An assignee can refuse** | same — *"Three refused assignments by the same scout in 7 days escalates to their supervisor"*, *"Assignee has a recent refusal streak"* |
| A task can be **in flight** | same — *"Deleting an in-flight task that has already emitted events"* |
| **Completion is append-only**, and closing is distinct from completing | same — *"Closing a task without a visit or without photos (irreversible: completion is append-only)"* |
| **`task.overdue` is an event, not a state** | `/concepts/risk-model` activity bindings — it fires `promote_severity` |

### The columns

| Column | Holds | Why it earns a column |
| --- | --- | --- |
| **To do** | `draft` — created, no name against it | The backlog |
| **Waiting to be accepted** | Assigned, not yet accepted | **See below. This is the important one.** |
| **In flight** | Accepted and under way | The work actually happening |
| **Done** | Completed or closed, inside the window in §5 | The record, bounded |

**The acceptance gate deserves its own column, and this is the one change the docs forced.**

`assigned` and `accepted` are not the same state, and the difference is load-bearing: *severity-critical tasks with no acceptance within two hours page the on-call role.* A task sitting assigned-but-unaccepted is the exact pile-up a board exists to make visible — it is work nobody has picked up, aimed at someone specific, with an escalation clock running. Collapsing it into a single "Assigned" column hides the one thing the escalation logic already cares about.

**Overdue is a flag on the card, never a column.** The docs settle this rather than my preference: `task.overdue` is an **event** in the risk-model activity bindings that fires `promote_severity`. An event is not a state, and an overdue task is still in one of the four columns — late, not elsewhere. A fifth column would move a card on a clock rather than on an action. The national overview already treats this correctly, counting *"findings past the window it had"* as a property, never as a state.

### Decision required, before you build the columns

> **Where does a refused assignment go?**

The docs establish that an assignee can refuse — they track refusal streaks and escalate on three in seven days — but they never say what state the task lands in. Three readings are all defensible: it returns to **To do** unassigned, it stays in **Waiting to be accepted** flagged as refused, or refusal is its own state.

**Do not pick one.** Stop and ask. A refused task that appears nowhere on the board is worse than an extra column, and a refused task silently shown as still-waiting is worse than both.

### And a docs gap to record, not to fix here

**The task lifecycle is not enumerated anywhere in the docs.** The board is a picture of a state machine, so building one against states inferred from prose guarantees rework the first time someone writes the real enum.

Record this as a Docs Delta in the same shape `concepts/finding-provenance` uses — the page that needs it is `/guides/field-scouting`, which is where every task state is currently mentioned in passing. **Do not write the enum yourself**; naming it in a build brief would make a brief the source of truth for an ontology, which is exactly the inversion `CLAUDE.md` forbids.

---

## 4 · This is an operational surface, and two rules invert here

The last four briefs were Track C. **Do not carry their rules across.** Two in particular flip.

**Scope authority flips.** `CONVENTION-scope-and-density.md` gives Track C the page scope row as the single writable control, with the breadcrumb derived. **Operational surfaces are the reverse:** the app breadcrumb is the writable control and there is **no page scope row**. One writable control per screen either way — which one depends on whether the screen reads down a hierarchy or across one, and a board reads down.

**The access rule flips.** The spine says a field the reader cannot see is **in** the totals and **out** of every list. That is a rollup rule and it does not apply here. **A board is a worklist, not a rollup.** Work outside the reader's entitlement is excluded from the column count *and* from the cards — both, consistently. A count that disagrees with the cards below it is the worse of the two failures, because the reader can see the discrepancy and cannot resolve it.

Say this in the build notes so nobody imports the Track C pattern by habit.

---

## 5 · Done needs a window, or the column is a lie about volume

**Done grows forever.** A column holding every task ever completed says nothing about the shape of the queue, and it will dwarf the other three within a season.

Bound it, and **name the bound on the column** — *"Done · last 14 days"*. An unbounded Done column with no stated window is the same error as a part-harvested total that reads as final: the number looks like an answer to a question nobody asked.

The window is a display choice, not a data one. Everything completed stays in the record; the board shows a slice and says which.

---

## 6 · What is on a card

Enough to decide whether to open it, and no more. A card is a decision to click, not a record.

- **What the task is**, in one line
- **Which field or block**, named
- **Who it is on**, where one is assigned
- **Its window** — and the overdue flag where it has passed
- **Its severity band**, where it originates from a finding

**Nothing computed.** No progress bar, no percentage complete, no estimate. The lifecycle has states, not progress, and inventing a percentage would be a fabricated figure on a surface that has no business producing one.

---

## 7 · Every card exits somewhere

This matters more on a read-only board than it would on a writable one. **If a reader cannot act here, every card must route to where they can** — open the task, open the field, open the queue. A read-only board whose cards go nowhere is a dead end, and the reader's only remaining move is the drag that does not work.

One route per card, the obvious one. Not three.

---

## 8 · Counts, and what they are counts of

Each column header carries a count. Three rules, all inherited:

- **A count renders as an integer.** `2.00 tasks` is not a count of tasks.
- **The count matches the cards rendered.** See §4 — entitlement excludes from both.
- **A count is a count, never a severity.** A column holding four tasks from `critical` findings is a column of four tasks. The board carries no aggregate band, and no column is coloured by the worst thing in it.

---

## 9 · Empty is an answer

An empty column renders as an answer, not as blank space. *"Nothing assigned and unstarted"* is information — it is the pile-up not existing.

And **the all-empty board is a real, reachable state and one of the most valuable things this product says.** The field-level product already does this — *"Nothing needs you today"*. An operator who can close the tab because the platform told them to is the outcome, not the failure case.

---

## 10 · Density

`CONVENTION-scope-and-density.md` applies in full. The three fates for a sentence: visible if it changes what the reader believes, tooltip only if it defines a term and could be deleted safely, trace or docs if it explains method. **Nothing a reader must not miss goes behind hover**, and a tooltip opens on focus and tap, not hover alone.

On this surface specifically: the read-only explanation is a page-level sentence, the column definitions are tooltips at most, and any reasoning about *why* the lifecycle has the states it has belongs in the docs.

**Product mode carries no spec vocabulary.** No snake_case, no enum values, no field names. The lifecycle state names are enum values — the columns carry their labels, and the labels file translates.

---

## 11 · Acceptance tests

1. **No drag affordance anywhere.** No grab cursor on a card, no lift on hover, no column highlight, no ghost. Check the computed cursor, not the appearance.
2. Attempting a drag does nothing and produces no motion. Nothing springs back, because nothing lifts.
3. The read-only sentence appears **once**, at page level, and on no card.
4. Four columns. **No overdue column.** Overdue renders as a flag on cards inside the other four.
5. Every lifecycle state maps to exactly one column. Enumerate them and confirm none is unplaced.
6. **The breadcrumb is the writable scope control and there is no page scope row.** The inverse of Track C, deliberately.
7. Work outside entitlement is absent from both the count and the cards. Seed one and confirm both.
8. `Done` names its window in the column header.
9. Every column count is an integer and equals the cards rendered beneath it.
10. No column is coloured or labelled by the worst severity inside it.
11. No card shows a progress percentage, a completion bar, or any computed figure.
12. Every card routes somewhere, and it is one route.
13. An empty column states what is empty. An all-empty board reads as an answer.
14. No duplicate sentence anywhere on the screen.
15. No underscore, enum value or field name in Product mode.
16. No tooltip holds anything a reader must not miss; every tooltip opens on focus and tap.
17. Dark and light pass at all three widths.
18. **Read it cold and time it.** A reader should see where the pile-up is in about five seconds. This is the fastest screen in the product — it has one job and no figures to reconcile.

---

## 12 · Out of scope

- **Changing state from the board.** Decided: read-only. If it becomes writable later, that is its own brief, and it inherits the full lifecycle contract — role checks, audit rows, confirmations, and refusals in words rather than a silent snap-back.
- **A new lifecycle state, or a fifth column.** If a state will not fit, stop and ask.
- **Filtering beyond what the breadcrumb already gives.** No page-level filter row.
- **Bulk actions.** A read-only board has none by definition.
- **Any rollup rule from Track C.** See §4.

If a decision you need is not in this file and not resolvable from it, **stop and say "Decision required: *question*"** rather than picking a default. The mixed-crop range defect came from a brief that left a gap and a build that filled it reasonably; the reasonable fill was a 28-month window and a crushed axis.
