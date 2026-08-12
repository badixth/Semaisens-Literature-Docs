# Set up a field — defects and proposed fixes

**Status: superseded by `DEFECTS.md`, kept as the field-creation build record.** Not updated. Its status table was current at 8 August 2026.

## Status at 9 August, after a full Playwright walk

| Id | What | State |
| --- | --- | --- |
| **D0a** | Hazard picker unclickable | **Fixed, retested.** Full raise flow walked as estate manager and as field scout. |
| **D1** | Field name rejected spaces | **Fixed, retested at the keystroke level.** See the note below — this needed testing twice. |
| **D2** | "Create the field" was a silent no-op | **Fixed, and better than asked.** Names all three missing requirements with a reason each. |
| **D7** | **Water regime cannot be selected** | **New, blocking.** See below. |
| **D3** | Tonnage identical across completeness states | **Blocked by D7** — the fully-resolved state is unreachable, so the comparison cannot be made. |
| **D4** | "Farm owner" in the role selector | **Still open.** Confirmed present. |
| **D5** | Blank landing route | Not retested this pass. |
| **D6** | `Empty` sweep | Reported swept by the build agent; not independently checked. |

**A note on how D1 was verified, because it matters for future retests.** Playwright's `fill()` sets an input's value directly and never dispatches key events, so it would report a space-swallowing key handler as fixed whether or not it was. The real test is `pressSequentially`, which types character by character. D1 passes that test — "Muda E7 North" went in with both spaces intact. **Any future check of an input-level bug must type, not fill.**

### D8 — The role selector's menu is occluded, on more than one screen

**Found 9 Aug via Playwright, at a 1920×1080 viewport.**

**Observed.** Opening the role selector in the top bar and clicking **Organisation admin** fails, with the browser's own hit-test naming the occluder:

- On **Set up a field**: `<div data-dc-tpl="32" role="application" data-canvas="field" aria-label="Draw the field boundary. Each click places a corner. Drag to pan.">` — the boundary map canvas.
- On **Overview**: `data-dc-tpl="32"` again, so it is not specific to the map screen.

**This is the same class as D0a, third instance** — after the header tablist and the advisor drawer. It is not a sandbox effect: a sandbox produces a permission error, not a named element. The browser is reporting that at the option's coordinates, an app-owned element is topmost.

**But it does not reproduce for the user**, who switched roles on this screen successfully at a smaller viewport and 90% zoom. So the layer order appears to hold at some sizes and fail at others — which points at elements *moving* under the menu rather than at a z-index that is plainly wrong. A canvas with `role="application"` is also a poor neighbour for any overlay: it is exactly the kind of element that should never be able to reach above a menu.

**Reproduced at three viewports** — 1920×1080, 1680×1000 and 1440×900 — each time with a *different* occluding element (`data-dc-tpl` 32, then 19, then an unnamed div). Keyboard selection (ArrowUp, Enter) also failed to change the role.

**Best hypothesis: the menu is partially overlapped, not fully covered.** Playwright hit-tests an element's *centre* before clicking and refuses if anything else is on top there. A person clicks wherever they can see the label. That would explain every observation at once: it works for a human, fails for an automated driver, and names a different occluder as the overlap region shifts with layout. The occluders differing by viewport is the tell — a plainly wrong z-index would name the same element every time.

**One check settles it.** For the open menu, compare its bounding box against the elements painted above it and report the overlap. If the menu's centre is covered while part of it is visible, this is real but cosmetic-adjacent — a person can still use it, an automated test cannot, and a keyboard user may be blocked. If nothing overlaps, the fault is in the driver and this entry should be closed.

**Suggested test change.** The existing layer test asserts computed z-index in three mounted states at one viewport. Extend it to two or three widths, add the role selector's menu to the overlays it sweeps, and assert *no overlap of the menu's own rect* rather than only comparing z-index values. Each of the three occlusion instances found so far was invisible to the version of the test that preceded it.

**Consequence for testing.** The field-creation walk cannot be completed by an external driver, because reaching an admin role requires this menu. It remains walkable by hand.

---

### D7 — Both band-tier selects are wired to the same menu, and neither commits

**Screen:** Set up a field → New field → *What decides its bands*.

**Observed, in sequence.**

1. The **Water regime** dropdown opens and lists Not set / Irrigated lowland / Rainfed lowland / Rainfed upland / Flood-prone. Clicking **Rainfed upland** does nothing: the listbox stays open, `Not set` stays selected, the completeness panel stays **Generic** with water regime `—`. Repeated twice.
2. The water regime button remains `[expanded]` — **the menu never closes on selection.**
3. Clicking the **Variety** button while that menu is stuck opens nothing at all.
4. After pressing Escape to clear it, clicking **Variety** opens the **water regime** listbox — the same five water-regime options.

**Diagnosis.** Both selects resolve to one popup instance. The variety trigger opens the water-regime menu, so variety has no menu of its own, and a selection made there commits to nothing visible. It also fails to close, which then swallows the next interaction on the section.

**This is not D0a.** Playwright raised no interception error at any point, so clicks reach their targets. This is wiring, not stacking.

**One root cause, both controls.** Fixing the anchor/instance binding should restore both. Do not treat the variety picker as a separate defect.

**Why it is blocking.** Water regime is one of the two tiers that lift a field out of the generic state. If it cannot be set, **no field can ever reach fully-resolved**, which means:

- The three completeness states cannot all be demonstrated.
- D3 cannot be tested at all — the tonnage comparison needs both ends.
- The whole argument of Brief 2 — that completeness is visible and consequential — is unreachable in the product.

**Check the variety picker at the same time.** It uses the same "Not set" control immediately below and is likely to share the fault.

---


Tested 8 August 2026 in Present mode, as Estate manager and Estate admin. Each item below says how it was observed, so you can tell a verified defect from a suspicion.

**The walkthrough is incomplete.** The build agent's turn was interrupted mid-write and the app began rendering unstyled with no form body. Boundary drawing, crop selection, season creation, and the Blocks and Seasons tabs are **not yet tested**. Section 4 lists what still needs a pass.

---

## 0a · Blocks the raise flow entirely — found 9 Aug via Playwright — **FIXED AND RETESTED**

> **Retest, 9 Aug.** Walked the full path again with Playwright as both estate manager and field scout: Field analytics → Findings → New observation → hazard picked → rationale typed → step 3 reached. The option clicks cleanly; no element intercepts. **D0a is closed.**
>
> The fix also surfaced two more instances of the same class, found by the build agent's own sweep rather than by me: the advisor drawer sat at the same z-index as the menu rung and blocked the picker whenever it was open, and it also covered the raise sheet. The app had four invented z-index numbers and no order; it now has one, quietest first, all beneath the rung. The lesson worth keeping is that the drawer was **invisible to a static sweep because it only exists while open** — so the layer test now sweeps three mounted states, not one.

### D0a — The hazard picker's options cannot be clicked

**Screen:** Field analytics → Findings → **New observation** → step 1, *"What did you see?"*

**Observed.** The dropdown opens and its options render. Playwright reports each option as *"visible, enabled and stable"* — then every click fails because another element is on top:

- On **Blast** (first option): `<div role="tablist"> … intercepts pointer events`
- On **Not listed** (last option): `<div data-dc-tpl="46"> … intercepts pointer events`

Different interceptors at different heights, so the whole listbox is behind other content rather than clipped at one edge. The `tablist` is the field-analytics tab strip (Overview / Trend / Spread / Findings / …).

**Why it is blocking.** A hazard is required to leave step 1 — `Continue` stays disabled without one. So **no finding can be raised through the UI at all.** Not by a scout, not by an agronomist, not on any field.

**Why the acceptance tests did not catch it.** The Deltas pane asserts against the store, not by driving this control. The model layer is correct — every disclosure, ceiling and fallback verified — but the path a person walks to reach it is closed. This is exactly the gap between "21 passing" and "the flow works".

**Likely cause.** A stacking-context problem. The listbox portals to the iframe root but sits below the tab strip and the panel behind it, either because it lacks a z-index above them or because an ancestor creates a stacking context that traps it. Note it is *not* a clipping bug: the options are painted and hit-testable, just not topmost.

**Fix.** Raise the portalled listbox above the page furniture, or render it in the same layer the other overlays use. Then add an acceptance test that **drives the control** rather than asserting the resulting state — the current test set cannot distinguish a working dropdown from an unreachable one.

---

## 0 · Stops everything

### D0 — The main content area does not respond to clicks

Tested on the published artifact `1c94d070`, on a clean browser tab.

**Observed.**

- The **left sidebar works.** Clicking Overview, then Set up a field, navigated correctly and populated the screen.
- **Nothing in the content area responds.** The Edit tab does not switch. The Estate select does not open. The Field name input does not take focus or accept text. Tried single click, double click, and Escape-then-click. The cursor renders at the intended coordinates in every screenshot.

**Why this is the app and not the test harness.** The synthetic clicks are identical in kind. The sidebar button responds; the tab-strip button 115 px below it does not. A harness limitation would not discriminate by position in the DOM. The same controls also worked earlier the same day in the design Present view, so this is a regression rather than a standing limitation.

**Likely window.** The artifact was published from the state in which the build agent's turn was interrupted mid-write. A partially written module leaving an overlay mounted, or a pointer-events rule applied to the content wrapper and never removed, would produce exactly this: sidebar live, content dead.

**Where to look.** Anything that renders a full-bleed layer over the content region — the first-run walkthrough is the obvious candidate, since it mounts on the landing route and this is also the route that renders blank. A backdrop that mounts without its visible card would swallow every click in the content area while leaving the sidebar clickable.

**Until this is fixed, nothing else on this screen can be tested.**

---

## 1 · Blocking

### D1 — The field name input rejects the space character

**Observed.** Typed `Muda D2`, got `MudaD2`. Then pressed space alone with the caret at the end: nothing was inserted and the caret did not move. Two different input methods, only the space lost.

**Why it is blocking.** The placeholder is literally `e.g. Muda D2`, and every seeded field is `Muda A2`, `KADA D1`, `Ladang P1`. **No user can type a name matching the app's own convention.** Field names are the primary human handle for everything downstream — findings, tasks, proof packs — and they will all be malformed.

**Likely cause.** Same family as the change-event bug already fixed on this screen: space is commonly bound to activate a focused control, so a handler is swallowing `keydown` before the input consumes it. Look for a `preventDefault` on space in a key handler that is not scoped to buttons, or a value filter stripping whitespace.

**Fix.** Allow spaces. Trim only leading and trailing whitespace, and only on commit — never mid-typing, or the caret will jump. Then check the same handler is not also eating spaces in the rationale field on the raise flow from Brief 1, where the minimum is 40 characters and a user cannot write 40 characters without one.

### D2 — "Create and edit" is a silent no-op

**Observed.** With a name and an estate set, clicked *Create and edit*: no navigation, no error, no toast, no visible change. Then opened the Edit tab's field picker — it listed Muda A2, Muda B1, Muda C4, KADA D1, KADA E3, Ladang P1, P2, P7. All seeded. **The field was not created.**

**Why it is blocking.** This is the end-to-end path. It cannot currently be walked to completion, so nothing downstream of creation can be reviewed.

**Presumed cause.** A boundary and a crop are almost certainly required and were unmet. That is correct behaviour; the silence is not.

**Fix.** The button must never be a silent no-op. Either:

- disable it while requirements are unmet, **with the reason stated next to it** — "Draw the boundary and choose a crop first"; or
- leave it enabled, and on click scroll to the first unmet requirement and mark it.

The first is preferable and matches the rule already used elsewhere on this screen for the estate ceiling: explain, don't just grey out. A disabled control with no reason reads as a bug — which is exactly how this one read.

---

## 2 · Substantive

### D3 — The completeness gate is demonstrated in wording only, not in quantity

**Observed.** Generic field: *"Sized against 5.2 t/ha for paddy crop-wide default (irrigated lowland)."* Fully resolved field: *"Sized against 5.2 t/ha for irrigated lowland paddy."* **Same number.**

**Why it matters.** With no price basis configured, the tonnage line is the only thing that demonstrates the gate. If the figure is identical either way, an honest reviewer concludes that completeness makes no difference — which is the opposite of the point, and worse than showing nothing, because it actively argues against the feature.

**Fix.** Seed the contrast on an ecosystem whose value genuinely differs from the crop default. **Rainfed upland is the right one** — the literature is explicit that rainfed uplands carry far higher blast pressure than irrigated lowlands, so the numbers should separate. If they don't separate in the data, that is a band-table gap, not a UI gap, and it needs raising rather than papering over.

Add an acceptance test: the tonnage on a generic field and on the same field fully resolved must differ, and the difference must be visible on one screen.

### D4 — The role selector contains a role that does not exist, and is missing the one that changes behaviour

**Observed.** Selector lists: Estate manager, Agronomist, Field scout, Approver, **Farm owner**, Agency officer, Estate admin, Organisation admin.

**Checked against `snippets/role-model.mdx`.** The eight functional roles are `scout`, `agronomist`, `approver`, `estate_manager`, `estate_admin`, `regional`, `org_admin`, `on_call`. **There is no owner role.** "Farm owner" is one of the four personas from the original product brief; persona and functional role have been conflated.

**Why it matters.** Every ceiling, guardrail and escalation resolves by functional role. A Farm owner has no raising ceiling, no co-sign authority and no escalation target — so under Brief 1 the platform has no rule for what they may assert. It will do something, and whatever it does is unspecified. Meanwhile `on_call` is absent, and it is the role that most changes observable behaviour: it overrides quiet hours and is the only holder that receives severity-critical pages.

**Fix.** Remove Farm owner; map that persona to `estate_manager` or `org_admin` by scope and label it accordingly. Add `on_call` as a **time-bounded overlay on a primary role**, not as a ninth primary — a user holds one primary role plus at most one overlay.

### D5 — An unresolvable persisted role takes the whole app down, with no error boundary

**Now reproducible with a precise trigger: the screen renders blank when it is the landing route.**

**Confirmed three times**, including twice on clean browser tabs against the published artifact. On load, `Set up a field` renders its title, its subtitle and a horizontal rule, and nothing else — no tabs, no form, no side panel. Navigating away to Overview and back populates it correctly every time.

So this is a **first-paint fault on the landing route**, not a role fault as originally guessed, and not a random race. It very likely shares a root cause with D0: the same route, the same content region.

The earlier black-screen episode below is retained for the record, but the landing-route repro above is the one to fix against.

---

**Original note — escalated from "rendered blank once" to "will not boot."**

**Observed, in sequence.**

1. Navigating to Set up a field as Estate admin, the header and subtitle rendered and the entire body below was empty — no tabs, no form, no side panel. It populated on the next interaction, so it looked like a mount-time race.
2. Later, on a fresh load of the present URL, the app rendered **black**: only the logo mark and the "Search…" placeholder. Two full navigations with three-second waits, identical result. The shell mounts; everything after it does not.

**Hypothesis, and it is a strong one.** The build agent flagged this exact failure before it happened: *"sessions saved before this turn won't have Zara — the persistence key wants bumping to v3… otherwise the admin viewpoint will resolve to a missing identity."* During testing I switched the role to **Estate admin**, and that selection persists. On the next boot the persisted role resolves to an identity the new seed does not contain, something throws during mount, and there is no error boundary to catch it — so the app dies after the shell and shows black.

That ordering matches what I saw: the blank body was the first symptom, the unbootable app the same fault after the selection had persisted.

**Two fixes, both needed.**

- **Bump the persistence key**, as the agent proposed. That clears the immediate breakage.
- **Add a boot-time guard and an error boundary.** A persisted role that no longer resolves must fall back to a known-good default and say so, not throw. Bumping the key fixes today's stale value; the guard is what stops the next schema change doing this again. The agent offered exactly this — take it.

**And the general rule underneath:** a screen must never render as a bare header, and an app must never render as a black page. If data is resolving, show a loading state. If a viewpoint cannot resolve, show an error naming what is missing. Silence is the one outcome indistinguishable from a broken build — and here it was one.

**Testing consequence.** Everything in §4 remains untested, and no further testing is possible until the app boots.

### D6 — The `detail` / `description` mismatch on `Empty` is unswept

The build agent found four instances on FieldSetup, all rendering as bare titles with no explanatory sentence, and corrected only that file. It has not been swept across the other nine screens.

**Fix.** Do not accept a self-report on this. Ask for the grep output — every `Empty` usage, its prop name and its file. That is checkable evidence and takes seconds; "I swept it and it's fine" is not.

---

## 3 · Working — do not regress these

Recorded so nobody "fixes" something that is already correct.

- **The two money refusals are distinguishable**, which was the open risk from the price decision. Generic: *"A value figure on crop-wide default bands would carry a precision its inputs do not support."* Fully resolved: *"Declined everywhere in this build — no price basis is set for any crop."* Two honest refusals, two different reasons.
- **Estate creation is gated and explained**, not hidden. Greyed for Estate manager with *"Creating an estate is an administration action… ask an estate administrator"*; enabled for Estate admin.
- **The estate copy states the boundary decision** in one line: *"The estate sets who can see this field. It does not constrain where the boundary goes."*
- **Area is an em dash with a reason**, not a typeable box.
- **The destructive-change disclosures are present.** Crop: *"Changing the crop replaces the band table, the stage model and every open finding's hazard reference."* Water regime: *"Changing this re-resolves the bands and discloses that history was read against a different one."*
- **The generic state discloses on the verdict itself**, not in a settings page.

**One line to query rather than change:** variety reads *"It does not move the band on its own."* Variety is a resolver tier in its own right, sitting above ecosystem. Confirm this is deliberate — it may be left over from when variety was only a rule-card multiplier.

---

## 4 · Still untested

The interruption stopped the walkthrough here. None of the following has been exercised:

- Drawing a boundary: corner placement, Undo corner, Clear, Import a boundary, self-intersection refusal, area calculating off the outline
- Crop selection beyond paddy, and the per-crop season differences — oil palm and rubber planting date versus production year, the rubber panel requirement, the pineapple batch picker
- The season creation path at all
- The Blocks tab: block-within-field containment, inheritance and override labelling
- The Edit tab's destructive crop change: confirmation, stated consequence, audit row
- Whether the four tabs actually share one field or merely look like they do
- The scattered smallholder-collective case, which must create cleanly with no proximity warning

**Sequence when the build is stable:** fix D1 and D2 first, since neither the create path nor any name can be completed without them, then run the four tabs as a single field end to end — create it, edit what it carries, add a block inside it, open a season on it. That order tests whether the tabs share state, which clicking controls individually will not reveal.
