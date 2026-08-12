# Phase 15 — three dead screens, two marks, one name

**Status: written, not built.** This is the next thing to paste.

Self-contained. **Attach nothing.** Run in a **fresh chat**.

**Time-boxed pass before a team demo**, and the demo walks every screen. Nothing here changes what a screen claims. It makes screens do what they already say they do.

**Phases 12, 13 and 14 have shipped and are verified.** The gauges, the one-line provenance, the real table, the two restructured overviews. **None of it moves** — see §6.

---

## 1 · The finding, which is one thing in three places

**Three screens render text where controls should be.** Measured: zero interactive elements in the page body on each, once navigation, search and the assistant button are excluded.

| Screen | What it renders | What it promises in its own copy |
| --- | --- | --- |
| **Field analytics** | All ten fields, as static text | *"Picking one here sets it in the scope control at the top, so every other screen follows."* |
| **Application plans** | Two plans, as static text | *"Open the zones and confirm them."* · *"Send the file to the machine that will spread it."* |
| **Proof packs** | One pack, as static text | *"Open Field A2"* — rendered as a label |

**This is the control-that-lies defect at screen scale.** A control that looks pressable and is inert has cost this project time before; a whole screen that describes an interaction it does not have is the same error, larger. It is also the most visible thing in a demo, because all three are nav items and the copy invites the click.

**The content on all three is good and stays.** Application plans in particular already carries the whole attribution argument — *"Semai proposes the rates; a person decides them, and approval is what makes a file possible"*, and *"Drafted by Ali Ismail, confirmed by Ali Ismail, approved by Siti Rahman."* **Do not rewrite it.** Give it the controls it is describing.

---

## 2 · The rule for this pass

> **Every sentence that describes an action either becomes a working control, or the sentence goes.**

Where the action cannot be built in this pass, **the sentence is removed or rewritten to describe what the screen does do** — not left as an instruction pointing at nothing. A screen that quietly does less is honest. A screen that says *"send the file"* beside no button is not.

**Where an action is refused, refuse in words on the surface** — the pattern settled three times now on the closed-cycle field, the ineligible assignee and `New estate`. Pressable, then refused with its reason, beside the thing it refused about.

---

## 3 · Field analytics — make the list a list

The screen renders ten fields with crop, estate, hectares and open findings, ordered with unwatched fields last. **All of it is correct and none of it is clickable.**

**Each field becomes a control.** Selecting one does what the copy already promises: it sets the field in the scope control at the top and the screen renders that field's detail.

**The detail is not new work.** Reuse what exists — the `BandGauge` with the field's own band, its stage, its position by ground, its open findings, its latest capture date. **Query the existing components rather than building a second version**; `FieldTable` and `BandGauge` are both already built and verified.

**If the per-field detail cannot be built in this pass**, then selecting a field sets the scope and the screen says so plainly — *"Muda A2 is now in scope. Open Fields workspace to read it."* **A working handoff beats a promise nobody can keep.**

---

## 4 · Application plans — the actions it names

Two plans render today. One is `Approved`, one is `Generated` and flagged `WAITING TOO LONG`.

| Copy on screen | The control it needs |
| --- | --- |
| *"Open the zones and confirm them."* | Opens the plan's zone table — the rates, the areas, the product. **Read-only in this pass.** |
| *"Send the file to the machine that will spread it."* | **Export.** Confirmations apply: export leaves the platform and is effectively irreversible. |
| The six-stage lifecycle | Each stage names who acted and when. Already in the copy; make the stage list navigable to the plan it belongs to. |

**Do not build prescription creation here.** That is the next brief and it depends on a decision still open — see §7. **This pass makes the two existing plans openable, readable and exportable**, which is the whole of what the screen currently claims.

**One thing to check while you are in there:** the screen's own words are *"Semai proposes the rates; a person decides them."* If a rate on either plan renders without a person's name beside it, that is a defect — the copy already promises attribution.

---

## 5 · Proof packs — one pack, three controls

*"Open Field A2"* is rendered as a label. It becomes a link to that field.

The pack has a reference — `VRF-2026-06-14-MUDA-A2-SCOUT-BLAST` — and a state, *"Satellite check running"*. **The reference should be selectable text at minimum**; an agency officer quoting it in an email is the entire point of it existing.

**The refusal here is already right and must survive:** *"A pack that fails its check cannot be sent"*, and *"The reading fell 0.03 between the two passes, past the 0.02 the check needs, but the check has not finished."* If a send control is added, **it refuses in words while the check is running** rather than being hidden.

---

## 6 · Regional forecast — it never got the Phase 12 marks

Every other reading surface carries the band mark. **National overview 15 · How a figure is built 15 · Fields workspace 12 · Regional forecast 0.** Clicking between two adjacent nav items shows two different products.

**Give it the marks it should have had**, with one rule that is easy to break here:

> **A forecast carries its range. A counted figure does not. They may not converge on one style.**

Use the band-with-spread mark for the projection and a plain line for anything declared. This is the single most likely thing for a restyle to get wrong, and `TRACK-C-4` §3 already settles it: *"one can be audited against a weighbridge ticket and the other cannot."*

**Nothing else on that screen changes.** Its refusals, its horizon rules and its k-suppression are all verified.

---

## 7 · The boxplot on Yield production — drop the box

`Is oil palm coming off evenly` currently draws a single boxplot above a histogram on a shared axis.

**The structure is sound and the mark is the wrong one for this data.** A boxplot earns its place by comparison — several distributions side by side, quartiles that overlap, medians that do not. **There is one box.** With nothing to compare against, a reader is decoding a five-number summary they did not ask for, and the caption has to explain what the drawing means, which is the tell.

Three compounding problems, all measurable:

- **The box is separated from its own axis by the entire histogram.** The scale is at the bottom; the box is at the top; a reader cannot read a value off it without crossing another chart.
- **The label renders twice** — once at the left of the row, once in caps floating inside the plot area.
- **The axis carries no unit.** `2 … 14` with `t FFB/ha` only in the subtitle.

**Fix: keep the histogram, drop the box.** Mark the median and the middle half on the histogram itself if the quartiles are wanted. A histogram answers *is this estate even* in its shape, with no training required — which is the question the card's own title asks.

**Bring the box back when several cohorts clear k = 5** and there is a real comparison to draw. That is the case the mark exists for, and it is not this seed.

**The two sentences that stay visible** are the ones that change what a reader believes: *"No point is drawn for an individual field"*, and *"young mature palms have not closed canopy and carry less per hectare by definition."* The rest is method and belongs in the trace, per Phase 12's triage.

---

## 8 · Rename `Application plans`

The documented entity is a **prescription**, also called a **VRA map**. *"Application plan"* is not evidenced anywhere in the docs, and the industry — including the competitor — says VRA.

**Nav item and screen title become `VRA maps`.** Use *prescription* for the record in specs and traces. **One user-facing name, one spec name, no third.**

---

## 9 · What must not move

- **All 15 gauges** on National overview — 5 paddy, 4 oil palm, 3 rubber, 3 pineapple — with the three rubber rows drawing **hollow** for saturation, and **12** on Fields workspace.
- **The band rows state hectares, not a position.** *"A band row states how much ground resolved to it, not how that ground read"* stays.
- **Every refusal with its reason**, including *"1 of 5 estates has written one · no total across them"* and the VRA refusals on `What needs you`.
- **Zero bare `title` attributes.** Nothing goes back behind hover.
- **Provenance stays one line.** Deviation in both directions stays **one colour**.
- **The table's stated column drop at 834**, and its refused raw-index sort.
- **The `New check` drawer and `New field`**, both centred, both clean.

---

## 10 · Acceptance tests

1. **Field analytics, Application plans and Proof packs each have working controls in the page body.** Count interactive elements excluding nav, search and the assistant — none returns zero.
2. **No sentence on any of the three describes an action that has no control.** Grep the rendered copy for *"open"*, *"send"*, *"pick"*, *"confirm"* and check each against a real control.
3. Selecting a field on Field analytics changes what the screen shows **and** sets the scope, or says plainly where to read it.
4. A plan opens to its zones. Export is present and carries a confirmation.
5. A proof pack whose check is still running **refuses to send, in words**, rather than hiding the control.
6. Regional forecast draws band marks. **A forecast carries its spread; a declared figure does not, and the two are visibly different.**
7. The Yield production card draws **no boxplot**. The histogram carries the median and the middle half.
8. Nav and title read **VRA maps**. Grep for *"Application plan"* — zero hits in Product mode.
9. Every item in §9 re-measured and unchanged.
10. At **390 · 834 · 1440**: zero horizontal overflow, zero clipped text, zero controls under 44 px at 390.
11. Colour passes AA in both themes.

**Method:** measure, do not look. Allow **2 seconds** after navigating. **Enumerate affordances before concluding one is absent** — query `[data-sc-name]`, `[role]`, `[aria-expanded]` rather than guessing at a label.

---

## 11 · Out of scope — and what comes next

**Creating a prescription is the next brief, not this one.** The flow is fully documented — seven creation steps, a role matrix, all eight guardrails — and it was blocked because every rate band is `decision_required` and the fail-closed rule refused everything.

**That block is being lifted.** An open docs PR adds **attribution** as a second route: where no published band exists, a rate may proceed if it carries a named author who was entitled to set it, and it renders as a judgement rather than as a citation. A rate with neither a band nor an author is still refused.

**Do not build against attribution until that PR is merged.** This pass touches only what already exists.

Also out of scope: the reading surfaces beyond §6, the board, the workspace table, the scout drawer, and anything at 834 or 1440 that Phase 13 already closed.

If a decision you need is not in this file and not resolvable from it, **stop and say "Decision required: *question*"** rather than picking a reasonable default.
