# Phase 13 — the table, and folding creation into it

**Status: built and verified.** Real table semantics, guarded sorts, creation folded in; E1 closed. See `DEFECTS.md` passes 20 and 21.

Self-contained. **Attach nothing.** Run in a **fresh chat**.

Two screens: **`Fields workspace`** and **`Estate group`**. One component underneath both, and two creation flows folded in so a person can act where they are looking.

**Phase 12 has shipped and is verified.** The band gauge, the one-line provenance and the promoted bar labels are all in place and clean. **Do not undo any of it** — see §7.

---

## 1 · What is actually wrong

Nothing here is broken. The workspace works, it fits its card, and its figures are right. **The problems are affordance and reach.**

**The table is not a table.** It renders as `div`s — zero `<table>` elements, no column semantics. So there is nothing for a screen reader to navigate by column, and nothing for a header to hang a sort on.

**Sorting is a segmented control off to one side.** `RANK BY: Distance from band · Field name · Freshness` sits above the grid rather than on the columns it orders. It works, and it is invisible as an affordance to anyone who has used a table before.

**Crop is buried.** It lives in the field subline — *"Paddy · 4.2 ha · Muda Granary North"* — which means a reader cannot scan crops down a column, and the filter chips are the only way to see the crop split.

**Selection reads as status.** The leading column is a shield glyph in a circle. It looks like a state, not a checkbox.

**And you cannot create anything from either screen.** `Set up a field` is a separate destination under CONFIGURE, disconnected from the two screens where a person looks at fields and estates.

---

## 2 · The one rule that constrains the rebuild

**A sort is a claim.** Pass 16 settled this on the assignee picker, and it applies harder here.

The workspace shows a raw index column — `0.55`, `0.72` — beside a label that already says **`Raw index · not comparable`**. Four crops at different stages sit in one list. **Sorting by that column would rank paddy at tillering against oil palm at prime, which is the defect the entire band programme exists to prevent.**

So:

> **Every column gets a sort header. The raw index column gets one too, and it is refused in words.**

Not hidden, not silently inert. A header that says why it will not sort:

> *"These readings are not comparable — each is against a different band. Sort by distance from band instead."*

**Sort by distance from band is the honest version of the same intent**, and it already exists. The refusal should point at it.

The same rule covers `WHERE IT SITS`, which is prose derived from distance — sort it by the distance it describes, not alphabetically.

---

## 3 · The table

Rebuild as a real table with column semantics. `<table>` with `<th scope="col">`, or `role="grid"` with proper roles. **The sort state must be announced** — `aria-sort` on the active header.

**Columns, in order:**

| Column | Sortable | Notes |
| --- | --- | --- |
| Selection | — | **Checkbox.** Not a glyph. Header checkbox selects the visible rows and says how many. |
| Shape | — | See §4 |
| **Field** | Yes, by name | Name plus estate. Crop leaves this subline. |
| **Crop** | Yes | **New column.** |
| Stage | Yes, by the crop's own phenological order — not alphabetically | `Booting` before `Ripening`, not after |
| Band position | Yes, by **signed distance** | The gauge stays. This is the default sort. |
| Where it sits | Yes, by the same distance | Prose derived from the column above |
| Raw index | **Refused, in words** | §2 |
| Findings | Yes, by count | |
| Latest | Yes, by date | |

**Stage sorting is a real trap.** Stages are ordered by phenology, and that order differs per crop. In a mixed-crop list, sorting by stage means grouping by crop then ordering within it. If that cannot be done honestly, **refuse the sort and say so** rather than sorting the labels alphabetically.

### Filters — keep what carries counts

**Do not replace the chips with dropdowns.** `Paddy 5 · Oil palm 3 · Rubber 2 · Pineapple 2` and `Below its band 2 · Inside its band 7 · Above its band 1 · Not watched 2` tell a reader the **distribution before they filter**. A dropdown hides that, and the distribution is the answer to half the questions this screen is for.

**Add**, alongside them: a text filter on field name, and nothing else. Per-column filter rows suit thirty columns of flat data; there are nine here and three already carry counts.

---

## 4 · Field shapes, and the colour trap

Add a shape thumbnail per row. A manager recognises a field by outline faster than by name, and the boundary already exists — `Set up a field` draws it.

**One hard rule: the thumbnail must not be filled by a good/bad colour ramp.**

A green-fill-good, red-fill-bad thumbnail reintroduces the exact error the whole band programme exists to kill. **Nominal-is-best: a high index on paddy at ripening means late, not healthy.** A green polygon would say the opposite of what the gauge two columns over is saying.

**Either** render the shape neutral — outline and a single wash — **or** fill it from the divergent band scale already in the design system, which encodes *distance from optimum in both directions*. Never a sequential green-to-red ramp. Introduce no new tokens.

---

## 5 · Creation, folded in

### `New field` on Fields workspace

`Set up a field` becomes an action **on** the workspace. Keep the destination if you like — the form is already built and works — but the entry point belongs where fields are looked at.

**The pattern is already set by `New check`**, which shipped and is clean: an action in the header, opening a drawer, pre-filled where the platform knows something, refusing in words where it does not. **Reuse it rather than inventing a second interaction.**

### `New estate` on Estate group

This control exists and is the one disabled control in the whole product that gives no reason. It has been open since Phase 11:

> No `title`, no `aria-label`, no `aria-describedby`, nothing in the surrounding text. Hovering it and reading it aloud both produce the words *"New estate"* and no reason.

**Fix it in the pattern the drawer now uses**, which is better than the one Phase 11 proposed: **let it be pressed, then refuse in words, beside the thing it refused about.** That is what pass 17 settled for the closed-cycle field, and what the ineligible-person picker did before it. The refusal names the rule — role, entitlement, or not yet built — and stays next to the list rather than replacing it.

**Do not simply add a tooltip.** Hover does not survive touch, and this pattern is now established twice.

### The estate picker inside `New field` needs an escape, and it is probably a refusal

The `New field` form asks *"which estate"* and lists the five that exist. **A person whose estate is not on that list has nowhere to go** — they must abandon the form, find the estate flow elsewhere, and start again.

**Add the option. Expect it to refuse.** Creating an estate is almost certainly not an `estate_manager` act — it sits with `estate_admin` or `org_admin`, the same place the `on_call` overlay is set. So the row at the foot of the estate list reads as an action and, for this reader, answers:

> *"Adding an estate is an administrator's job. Ask your organisation admin."*

Name the person if the platform holds one. **An absent option teaches nothing; a refusing option teaches the rule.** This is the third surface to need the same answer, after the closed-cycle field and the ineligible assignee.

**Do not nest a create-estate modal inside the create-field modal**, even for a role that may do it. A stack of two half-finished forms is where work gets lost. If the role permits it, the row expands **in place** — a name and a region — or the flow hands off cleanly and returns.

### Centre the creation surfaces

**`New field` is currently a right-anchored panel covering most of the screen.** What remains visible behind it is fragments — `IR OWN BA`, `urthest ou`, `HERE IT S`. That is not context; it is noise. The panel is paying a drawer's cost without a drawer's benefit.

**Decide it properly and apply it to both creation surfaces:**

- **Centre a modal** where the form is a committing act the reader should finish. Both of these are.
- **Narrow a drawer** only where the list behind must stay readable — which is not the case here, since the estate is chosen inside the form.

**Recommendation: centre both `New field` and `New check`**, at a width that lets the form breathe rather than one that spans the viewport.

**One consequence to take deliberately:** the boundary step then goes **full-screen**, not centred. Drawing a polygon needs room, and a map in a 600 px box is the shrink-a-chart mistake wearing different clothes.

**Whatever is chosen, both must match.** `New check` is a drawer today. Two creation patterns for two creation acts is a difference no reader could name a reason for.

---

## 6 · Estate group — expand without forking the list

Each estate row gets an expand that reveals its fields with their band position.

**One condition, and it is the whole point:** the expansion renders **the same field-row component** the workspace uses, filtered to that estate. Not a second implementation.

Two lists of fields with band positions, built separately, will drift — and the band rules are the most expensive thing in this project to get wrong twice. If reusing the component is impractical, **make the `Fields →` button deep-link to the workspace pre-filtered instead.** Either is acceptable. Two implementations is not.

---

## 7 · What must not regress

Phase 12 is verified. Re-measure all of it after this pass.

- **Fifteen `BandGauge` marks** on National overview — 5 paddy, 4 oil palm, 3 rubber, 3 pineapple — plus **12** on Fields workspace. Every one draws its band.
- **The three rubber rows draw hollow.** Saturation is drawn *and* said.
- **Zero bare `title` attributes.** Every figure that was hover-only is now visible text. Do not reintroduce one on a shape thumbnail or a truncated cell.
- **Provenance stays one line**, with the confidence verdict visible and its mechanism in the trace.
- *"A band is a definition rather than a measurement"* stays **off** the surface.
- **`Raw index · not comparable`** stays, and `MEAN INDEX` does not return to the KPI row.
- The Overview production card still refuses: *"1 of 5 estates has written one · no total across them."*

---

## 8 · Acceptance tests

1. The grid exposes column semantics — `<th scope="col">` or equivalent roles — and the active sort is announced via `aria-sort`.
2. Every sortable header sorts both directions, and the state is visible.
3. **The raw index header refuses in words and names the alternative.** It is not hidden and not silently inert.
4. Sorting by stage groups by crop and orders by phenology, **or** refuses in words. It never orders stage labels alphabetically.
5. `Crop` is its own column and is sortable.
6. Selection is a checkbox. The header checkbox states how many rows it took.
7. Every row shows a field shape. **No thumbnail is filled by a sequential good-to-bad ramp.** Grep the fills against the token set; no new tokens.
8. The counted filter chips survive, with their counts.
9. `New field` opens from the workspace. `New estate` opens from Estate group, and when it refuses it **says why, in words, beside the list**.
9a. The estate picker inside `New field` offers a way to add one, and **refuses in words naming who can** where the reader may not. No nested modal.
9b. `New field` and `New check` use the **same placement**. The boundary step is full-screen.
10. Expanding an estate renders the same field-row component as the workspace — verify by component name, not by eye.
11. Every item in §7 re-measured and unchanged.
12. At **390**: 390 / 390, zero clipped, zero controls under 44 px. The table becomes cards or refuses; it does not scroll sideways.
13. At **834 and 1440**: zero horizontal overflow, zero clipped text. Pass 19 measured this clean — keep it.
14. Colour passes AA in both themes.

**Method:** measure, do not look. Allow **2 seconds** after navigating. **Query marks by `[data-sc-name]`, not by tag** — a probe for `<svg>` missed the entire gauge component in pass 18, because it is built from positioned spans.

---

## 9 · Out of scope

- **Anything on the Track C reading surfaces** beyond what §7 protects.
- **The Overview and National overview restructure** — ranking regions by hectares outside their own band, cutting the queue card. That is the next and last pass in this sequence.
- **Prescriptions and VRA.** Still blocked on the rate bands.
- **New tokens or new components.** The gauge, the drawer, the refusal pattern and the divergent scale all exist.
- **Renaming `Application plans`** to the documented noun, `prescription`. Recorded, separate.

If a decision you need is not in this file and not resolvable from it, **stop and say "Decision required: *question*"** rather than picking a reasonable default.
