# Phase 11 — the phone layout

**Status: built and verified.** Zero overflow at 390 on all sixteen screens, bottom bar on width, no control under 44 px. See `DEFECTS.md` passes 14 and 18.

`PHASE-11-fixes.md` closed five findings and deliberately left this one. Do not reopen those; colour is verified clean.

---

## 1 · What was measured

Widths: **1440 · 834 · 390.** No breakpoints are recorded in the repo. If the design system names some, restate these against them before starting.

**834 is clean** — no overflow, no clipping, no off-screen content, on every screen. **390 fails identically everywhere:**

| | Width |
| --- | --- |
| Viewport | 390 |
| Body scroll width | **770** |
| Navigation rail | 256, fixed |
| Main column, effective minimum | ~514 |

**Exactly 380 px of overflow on every screen** — the national overview, the board, the fields workspace, the form. A constant is a fixed floor, not content.

Collapsing the rail by hand takes it 256 → 56 and the body 770 → 570. **Still 180 px past the viewport.** There is no phone layout underneath the rail; the main column alone will not go below roughly 514.

**There is no responsive breakpoint anywhere in the product.** The rail is a manual toggle, not a media query.

---

## 2 · The decision that shapes the whole job

**Do not build sixteen phone layouts.**

Ask who is actually holding a phone. A scout standing in a field, and an estate manager checking something between places. Nobody reads a national forecast on a handset — the agency officer and the executive are at a desk, and Track C exists precisely for people looking across estates with time to look.

So the work splits three ways:

| | Screens | What happens at 390 |
| --- | --- | --- |
| **Must work** | `Field checks · phone`, `What needs you`, `Where the work is` | A real phone layout |
| **Should work** | `Fields workspace`, `Set up a field`, `Work done` | Usable, single column, degraded gracefully |
| **Says it needs a wider screen** | `National overview`, `Regional forecast`, `Yield production`, `Estate group`, `How a figure is built` | **Refuse in words.** Do not overflow, do not shrink |

**The third row is the product's own habit applied to layout.** A screen that cannot be read honestly at this width should say so, the way every other refusal does — not silently overflow and let the reader scroll sideways through a figure that no longer lines up with its label.

> *"This view needs a wider screen. Six fields need a decision today — open it on a desktop to see why."*

Give the headline verdict and the route. **A refusal is an answer and renders as one**, and that rule does not stop applying because the constraint is pixels.

**`Field checks · phone` is not on the "should" list by accident.** Its entire reason for existing is a handset, and it currently overflows by 380 px and clips fifteen labels including *"Saw something that is not on t…"*. Start there.

---

## 3 · The rail

At phone width the rail is not a rail. Options in order of preference:

1. **A bottom bar** carrying the three or four destinations a phone user actually needs, with the rest behind an overflow.
2. **A drawer** off a header control, which is honest but costs a tap on every navigation.

**It must be automatic.** The current collapse is a manual button, and a reader who does not find it sees 380 px of horizontal scroll. A media query, not a preference.

**Do not simply hide it.** A phone user still needs to move between checks and the board.

---

## 4 · On a phone there is no hover, so the three fates become two

`CONVENTION-scope-and-density.md` sorts every sentence into visible, tooltip, or trace. **On touch, the middle one does not exist.**

That convention already says *nothing a reader must not miss goes behind hover*, and it already requires tooltips to open on tap. But at phone width the honest position is stronger: **a tooltip is either promoted to visible or dropped.** Do not ship a phone layout whose explanations require discovering that a word is tappable.

**Refusals never truncate.** They are the load-bearing sentences in this product, and they are longer than most labels — *"No report, so nothing here reaches the risk model. The band stays where it is."* Wrap them. A clipped refusal is worse than no refusal, because a half-read reason reads as a complete one.

---

## 5 · Charts at 390

`CHARTS-product-map.md` sets a budget per surface. At 390 that budget shrinks, and the rule is the same one everywhere else in this product:

> **A chart that cannot be read at this width is refused in words, not shrunk.**

State what it would have shown and keep the figure it sits beside:

> *"The spread across five schemes needs a wider screen. The total, 534,800 kg, is unchanged."*

That is the k-suppression pattern — *refuse the breakdown, keep the total, say so* — applied to a physical constraint instead of a privacy one. Reuse the wording.

**Specifically:** a boxplot with five boxes, a choropleth, and a grouped bar are all unreadable at 390. A sparkline and a single cumulative line survive. **Do not shrink a boxplot until its quartiles are three pixels apart** — that is a chart that lies about being legible.

---

## 6 · Scope controls at 390

Track C carries four dropdowns in a row — `All regions · All schemes · All crops · All organisations` — plus a horizon selector. That does not fit, and wrapping it into four stacked rows buries the screen's actual content below the fold.

If a Track C surface gets a phone layout at all (see §2, most should not), the scope row collapses to **one control that opens a sheet**, with the resolved-scope line still stated in words above the fold. That line — *"124 fields · 10 schemes · Kedah, Kelantan, Sabah, Perak and Johor · three organisations"* — matters more on a phone, not less, because there is less on screen to orient from.

**One writable scope control per screen still holds.** Collapsing four dropdowns into one sheet does not create a second authority.

---

## 7 · Touch

**44 px minimum for anything tappable.** The design system already names 30 / 36 / 44 and 44 is the touch size — use it, do not invent a fifth.

`Reset demo` currently renders at 30 px. Correct on desktop, too small on a phone.

**Every drag on `Where the work is` needs a touch path or an alternative.** HTML5 drag-and-drop does not work on touch. The board already routes every transition through **Open the check**, so the floor is met — but confirm it, because a board that looks draggable and cannot be dragged on the device it is being held on is the control-that-lies defect in its purest form.

---

## 8 · Acceptance tests

Measured, not eyeballed. At **390**, on every screen:

1. `document.body.scrollWidth` equals `clientWidth`. **Zero horizontal overflow.** This is the one number that says the job is done.
2. No text element has `scrollWidth > clientWidth`. Nothing clips.
3. Nothing renders with `right` beyond the viewport.
4. The rail collapses **without a click** — driven by width, not preference.
5. Every tappable control is ≥ 44 px in its smaller dimension.
6. No refusal is truncated. Grep the rendered copy for an ellipsis inside one; find none.
7. Every screen in the third row of §2 states, in words, that it needs a wider screen — and still gives its headline verdict.
8. No chart is rendered below the width at which it can be read. Where one is withheld, the figure beside it is restated.
9. No content is reachable only by hover.
10. Every drag-reachable transition on the board is reachable by tap.

At **834 and 1440**, on every screen:

11. **Nothing changed.** Both widths were clean before this work; re-measure and confirm no regression. This is the test most likely to be skipped and most likely to fail.

12. Colour still passes everywhere. `PHASE-11-fixes.md` closed five findings; do not undo them with a layout change.

**Method notes, learned the hard way:**

- Allow **2 seconds** after navigating before measuring. A 900 ms settle once produced six screens reporting an identical 32 elements — that was the chrome, measured before render.
- **Read the `body` background before trusting a colour result.** The preview reverts to light on reload.
- The app frame is resizable from the parent document, so measure the real viewport rather than zooming.

---

## 9 · Out of scope

- **Anything at 834 or 1440.** Both are clean. Do not touch them.
- **The colour work.** Closed and verified.
- **A phone layout for the Track C reading surfaces**, unless §2 is overruled. They refuse instead.
- **New breakpoints beyond the one this needs.** One phone breakpoint, not a system.
- **Redesigning any screen's content.** This is layout at a narrow width, not a rethink of what the screen says.

If a decision you need is not in this file and not resolvable from it, **stop and say "Decision required: *question*"** rather than picking a default.
