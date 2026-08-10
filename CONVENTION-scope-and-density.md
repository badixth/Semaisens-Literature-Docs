# Convention — scope authority and text density

**Status: standing law.** Applied to the national overview and regional forecast and verified in the build. Every surface after them inherits it.

Cross-surface. Applies to every Track C screen and to any screen built after it. Self-contained: **attach nothing.**

Two problems, raised together because they have the same root — the screen is carrying more authority than it should. One control too many, and one voice too many.

---

# Part 1 · Scope has exactly one authority

## The observation

The regional forecast carries two scope controls.

**In the app chrome:** an organisation → estate breadcrumb (`Semai Demo Organisation › … › Ladang P2`, or `All fields`), beside the role selector.

**On the page:** `All regions` · `All schemes` · `All organisations`.

They can disagree, and on the current screen they do. The breadcrumb pins **Ladang P2**. The page says **All organisations**. The figure below is 3,178 t across 59 fields in 4 schemes in 2 states — which is neither of them.

## Why this is not cosmetic

This is the **duplicated-authority defect** in a new place. The standing convention already names it: *anything the resolver computes has exactly one reader.* Two writable controls over one piece of state is two writers, and the project has paid for that five times in the labels file.

Three concrete consequences:

- **`All organisations` while an organisation is pinned** either widens scope past the reader's context — an access problem — or is inert, which is a control that lies. Both are worse than the control being absent.
- **Spine §4.1 keys the delivery-policy disclosure off scope mixing organisations.** If the page control is real, that disclosure is missing from this screen. If it is not real, the disclosure logic is watching a control that never changes. Neither is a working state.
- **National overview test 23 already requires one control.** This is the same failure on a second surface, which means it is a convention problem, not a screen problem.

## The decision

**On Track C surfaces the page scope row is the only writable control. The breadcrumb renders the resolved scope, read-only, derived from it.**

Three reasons:

1. Track C exists for readers who work **across** estates. An estate-path breadcrumb is the wrong shape for the job — it navigates down when this reader reads sideways.
2. The three axes — region × scheme × organisation — do not express as a path. A breadcrumb can hold one of them.
3. One writer, one derived reader is the convention already in force everywhere else in this product.

**On the operational surfaces** — fields workspace, what needs you, set up a field, field analytics — the breadcrumb stays writable and there is **no page scope row**. One writable control per screen, either way. Which one it is depends on whether the screen reads down a hierarchy or across one.

## Three rules that hold whichever control wins

**1 · State the resolved scope in words, once, above the fold.**

`All regions · All schemes · All organisations` is three controls at rest, and a reader has to assemble them into a mental picture. The screen never says what is actually in view. Add one line:

> *59 fields · 4 schemes · Kedah and Kelantan · one organisation*

That sentence is the answer to "what am I looking at", and it is currently nowhere on the screen.

**2 · A control never offers a scope the reader cannot reach.**

If exactly one organisation is in scope for this reader, the organisation control is **a label, not a dropdown**. An option that cannot be chosen is not a filter; it is furniture that implies the data is broader than it is.

**3 · Narrowing recomputes the refusals, not just the figures.**

Narrow to one crop and the *"No long-horizon total across crops"* banner must **disappear** — it is no longer true. A refusal that outlives the condition that produced it is a stale claim, and on this product a stale refusal is worse than a stale figure, because refusals are what the reader is being asked to trust.

---

# Part 2 · Density — three fates for a sentence

## Tooltips are part of the answer, not the answer

Hover does not exist on touch. It does not survive a screenshot pasted into an agency report. It does not reach a keyboard. So:

> **Anything a reader must not miss cannot go behind hover.**

The product's own line settles most of this without further argument: *a refusal is an answer and renders as one.*

And a second trap: a tooltip holding a methodology paragraph is the same paragraph, now harder to read and impossible to re-read. Moving heavy prose into hover reduces the words **on** the screen and not the words **in** the product. The weight has to leave, not hide.

## The triage

Sort every sentence by what the reader would do with it.

| Fate | What qualifies | Treatment | The test |
| --- | --- | --- | --- |
| **Visible** | Changes what the reader believes about the number in front of them — coverage gaps, refusals and their reason, units, band position, k-suppression | Stays on the surface, always | *Would a reader draw a wrong conclusion without it?* |
| **Tooltip** | Defines a term. Noun-shaped. No consequence if never read | Hover **and** focus **and** tap. ≤ 12 words | *Could you delete it and still read the figure correctly?* |
| **Trace or docs** | Answers *why the platform behaves this way*, or *how this figure was computed* | "How this was built", or "How a figure is built" | *Is it about the method rather than the result?* |

Most of the weight goes to the third row. That is the finding: this screen is not over-glossed, it is **teaching while it reports**, and the teaching has a page of its own already.

## Worked, on the regional forecast

| Text as it stands | Fate |
| --- | --- |
| *"Track C Brief 3. How much is coming, in what, and with how much certainty. Every projection is a range rather than a point, each crop is judged at its own long horizon, and an invalid comparison is refused in words."* | **Cut.** `Track C Brief 3` is spec vocabulary on a product surface — a Product-mode violation on its own. The rest is the brief describing itself to the reader. Replace with the resolved-scope line from Part 1, rule 1. |
| Card sublines — *"this season's harvest, with the season named · 700.6 ha"* | **Split.** Hectares stay visible. The horizon gloss becomes a tooltip on `END OF SEASON`: *"This season's harvest. Main Season 1 · 2026."* |
| Pineapple refusal, 60 words | **Visible, shortened.** Keep *"No range, so no figure"* and one sentence of cause: *"One block at Ladang Johor Selatan holds more than one open batch, and production is not split between them."* The trailing clause — *a central estimate with no range beside it is not a weaker answer, it is a different and false one* — is the **rule**, not this instance. Once, in a group header, or in docs. |
| *"No long-horizon total across crops"* banner, 55 words | **Headline visible, reason disclosed.** Keep the title plus *"The four crops share no date to state a total at."* The per-crop enumeration restates the four cards directly above it. |
| *"One scheme has not reported since 5 June, so the range is wider than usual."* | **Visible.** Highest-value sentence on the card. This is coverage stated as something chaseable, exactly per the brief. |
| *"Ranges are added rather than combined in quadrature: schemes in one region sit under one weather system, so their errors are not independent."* | **Trace.** Method, and good method — it belongs behind *How this was built*, which is where a reader who doubts the range goes. |
| Chart caption, 55 words | **Split three ways.** (a) *"It stops moving at the last capture"* — drop the flat dashed segment after today and the sentence goes with it. (b) *"MADA Blok C9 and KADA Blok W4 report figures rather than captures…"* — this is an **exclusions row**, which spine §5 already requires the trace to carry. (c) What is left is an axis label. |
| *"One row per scheme, at its own long horizon, with the direction its captures have moved beside the figure rather than instead of it. No scheme carries a growth stage of its own."* | **Cut, then state once.** *"beside the figure rather than instead of it"* is a designer defending a decision. *"No scheme carries a growth stage"* is said again on the distribution card two sections down. |
| Choropleth caption, 45 words | **Cut to docs.** *"a drawn outline would be a picture with nothing behind it"* is the reasoning, not the reading. The reader needs five words under the tiles: *"Equal-area tiles, not a map."* The one-crop-at-a-time clause is already enforced by the card showing one crop. |
| *"Every cell here holds at least 5 fields, so none of them names the farm it came from."* | **Visible, shortened.** k-suppression is required disclosure. *"Every cell holds at least five fields."* |
| Distribution — *"A region has no growth stage… production adds up across them, condition does not"* **and** *"Fields at 4 stages, summed for production and never averaged for condition."* | **Defect.** One rule, stated twice, on one card. National overview test 14 forbids exactly this. Keep one, in the header. |
| *"What paddy may be compared with"* caption, 50 words | **Header keeps the rule once**, trimmed: *"Seasons line up by week from planting, not by date."* The refused pairing's own reason **stays visible** — that is a refusal, and refusals never move. |

Roughly 400 words of prose on the screen now; roughly 150 after. Nothing a reader needs to reach a correct conclusion has left the surface.

## Tooltip mechanics, so they do not become the next defect

- **Hover, keyboard focus and tap all open it.** Hover-only content is desktop-only content.
- **Twelve words is the ceiling.** Longer than that wanted to be a disclosure, which persists and can be read twice.
- **A tooltip never holds a number that appears nowhere else.** A figure inside hover cannot be checked, screenshotted or exported — and this product's figures have to survive being pasted into an agency report.
- **Never a refusal, a coverage gap, a unit, or a band position.** Those four are what a wrong conclusion is made of.
- **One per element, never nested.** Stacked tooltips mean the content wanted to be a disclosure.
- **Product mode applies inside the tooltip.** No enum values, no field names, no `P10`. Hover is not Review mode.

---

# Acceptance tests

**Scope**

1. Exactly one **writable** scope control exists per screen. Grep the rendered markup for a second.
2. The breadcrumb and the page scope row never state different scopes. Narrow the page row; confirm the breadcrumb follows within the same frame.
3. A scope the reader cannot reach renders as a label, never as an inert dropdown option.
4. Narrowing to a single crop **removes** the cross-crop refusal banner.
5. One line above the fold states the resolved scope in words — fields, schemes, states, organisations.
6. Scope spanning more than one organisation fires the delivery-policy disclosure. Scope pinned to one does not.

**Density**

7. No sentence appears twice on the screen. Grep the rendered copy; find no duplicate.
8. No tooltip contains a refusal, a coverage gap, a unit, or a band position.
9. No tooltip contains a number that appears nowhere else on the screen.
10. Every tooltip opens on keyboard focus and on tap, not on hover alone.
11. No tooltip exceeds twelve words. Anything longer is a disclosure.
12. Every *"why the platform behaves this way"* sentence is in a trace, in a group header, or absent.
13. **The screenshot test.** With every tooltip closed and every disclosure collapsed, screenshot the screen. Every conclusion a reader would draw from it is still supported by what is visible. If a reader could be wrong about a number from the screenshot alone, something moved that should not have.
14. Read cold and timed: still "which crop, how much, how certain" in about fifteen seconds. The density work must not have removed a fact needed to get there — it should have removed the ones that were in the way.

---

If a decision you need is not in this file and not resolvable from it, **stop and say "Decision required: *question*"** rather than picking a default.
