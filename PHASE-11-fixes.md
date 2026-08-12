# Phase 11 — the fixes

**Status: built and verified.** All five colour findings closed; zero AA failures measured in both themes. See `DEFECTS.md` pass 14.

Six findings from a measured sweep of all sixteen screens: colour in both themes, three widths, dead controls. **Five are small. One is a project.** Read §7 before planning the work.

**Every figure below was measured, not judged.** Contrast is computed against the element's effective background with the AA threshold applied by font size and weight. Where a number appears, it is the real one.

---

## 1 · What was already right, so nobody "fixes" it

- **Zero dead controls.** Every button, tab, option, link and input on eight screens has a live handler. Nothing is inert.
- **Disabled controls explain themselves** — *"A viewer can read every field on this screen and record nothing against it."* That names the **role**, not the button, which is the right shape. One exception, §6.
- **Dark mode is sound.** Surfaces layer properly through low-alpha overlays. The colour failures below are **light mode**, apart from one shared token.
- **834 px is clean** — no overflow, no clipping, no off-screen content.

---

## 2 · The severity chips — every one fails, and it is one decision

White text on the chip fill:

| Chip | Fill | Measured | AA needs |
| --- | --- | --- | --- |
| **Watch** | amber `rgb(245,158,11)` | **2.15 : 1** | 4.5 |
| **Low** · **For info** | blue `rgb(91,155,213)` | 2.96 : 1 | 4.5 |
| **Urgent** · **High** | red `rgb(239,68,68)` | 3.76 : 1 | 4.5 |

**The entire severity ladder, on every screen that carries one** — Overview, Estate group, What needs you, Notification preferences, Field checks · phone. *Notification preferences* renders the whole ladder together and is the fastest place to check a fix.

2.15 : 1 is barely above the 1 : 1 of invisible. White on amber is the classic failure: amber is already near-white in luminance, so white text has nowhere to go.

**Fix: dark ink on the fill, not white.** `rgb(26,31,28)` on amber and on blue clears 4.5 comfortably. **On red it does not** — ink on `rgb(239,68,68)` lands around 4.0, so red needs either a darker fill with white kept, or a darker fill with ink. **Measure it rather than eyeballing; do not ship a fourth value that fails.**

One rule for all five chips. A ladder where three values follow one rule and two follow another is how this comes back.

---

## 3 · The refusals are the least legible text in the product

`rgb(217,119,6)` on white — **3.19 : 1**, at 13 px, needing 4.5.

Everything wearing it is a refusal or a caveat:

- *"NDVI has saturated on this ground — above 0.80 it stops discriminating…"*
- *"Ladang Johor Selatan holds one block with more than one open batch…"*
- *"Recovery is measured; the value is not"*
- *"Nothing declared in this window"* — twice, on Yield production

This product's signature is *refuse rather than mislead*, and *a refusal is an answer and renders as one*. The sentences the whole design rests on are the hardest to read on the surface.

**The floor** is darkening the amber until it clears 4.5. **The better fix** is that refusals stop carrying their meaning in the text colour at all: body colour for the words, meaning on the icon and the label. Thirteen-pixel amber is asking a colour to do semantic work it cannot do at that size, and it will fail the same way wherever it is next used.

---

## 4 · Red on the inverted panels — 4.44 : 1, nine screens, one token

`rgb(239,68,68)` on the dark panel `rgb(26,31,28)`, which appears **in light mode** on inverted cards. It misses by 0.06, so this is a one-token change.

| Text | Screen |
| --- | --- |
| `−0.10`, `−0.03` — negative deltas | Fields workspace |
| *"2 past the window you had"*, *"overdue by 2 days"* | What needs you |
| *"Not confirmed"* | Proof packs, Notification preferences |
| *"Waiting too long"*, *"Off plan"* | Application plans |
| *"overdue by 4 days"*, *"6 points"* | Field checks · phone |

**Every instance is a thing that has gone wrong.** The colour reserved for problems is the one that does not quite clear the threshold, so the worse the news the harder it is to read.

---

## 5 · The band disclosure is coloured as an alarm — and this one is a decision

| Sentence | What it is | Colour, light | Colour, dark |
| --- | --- | --- | --- |
| *"Five bands resolve for paddy — 0.55–0.75 at tillering on 412.6 ha…"* | A **disclosure of method** | `rgb(192,57,43)` red | `rgb(239,68,68)` red |
| *"KADA Blok W4 has not reported since 5 June"* | An **actual exception** | body `rgb(26,31,28)` | body |

**The alarming colour is on the honest disclosure and the neutral colour is on the real problem.** A reader scanning for trouble is pulled to three red paragraphs saying the platform declined to average a band — which is the platform working correctly — while the scheme that has gone dark reads as ordinary text.

In dark mode it compounds: that red is the **same hue as the exception-row tint** `rgba(239,68,68,0.12)` sitting behind the actual exception. Disclosure text and alarm background share a colour.

**Recommended, and open to being overruled: give it no colour at all.**

> **Colour marks a state that needs a decision. A disclosure needs none, so it carries none.**

Body colour, in a recessed note block — the product already has the recess pattern. That leaves red for *something is wrong*, amber for *caution*, and everything else plain, which also removes the double duty red is currently doing. Apply it to every disclosure, not just this sentence.

---

## 6 · One disabled control says nothing

**`New estate` on Set up a field.** No `title`, no `aria-label`, no `aria-describedby`, nothing in the surrounding text. Hovering it and reading it aloud both produce the same thing: the words *"New estate"* and no reason.

This is the silent-refusal defect, and it has cost time already — an earlier session lost a round to *"right now I can't create new estate"* with nothing on screen to explain it. The same page does this correctly five times over on its map controls. Whatever the rule is — role, entitlement, or not yet built — say it in that pattern.

---

## 7 · There is no phone layout, and this is not a token change

**Read this before scheduling the work.** Sections 2 to 6 are a few token edits and a sentence. This one is a design job.

Widths measured: **1440 · 834 · 390.** No breakpoints are recorded anywhere in the repo, so these are desktop, tablet and phone. **If the design system names breakpoints, restate these figures against them before starting.**

**834 is clean. 390 overflows by exactly 380 px on every screen** — the same number on the national overview, the board, the fields workspace and the form. A constant is a fixed floor, not content:

| | Width |
| --- | --- |
| Viewport | 390 |
| Body scroll width | **770** |
| Navigation rail | 256, fixed |
| Main column, effective minimum | ~514 |

**The rail never collapses on its own.** Collapsing it by hand takes it 256 → 56 and the body 770 → 570 — still 180 px past the viewport. There is no phone layout underneath; the main column alone will not go below roughly 514.

**There is no responsive breakpoint anywhere in the product.** The rail is a manual toggle, not a media query.

**Where it bites hardest:** the screen named **`Field checks · phone`** overflows by 380 px and clips ten labels, including *"Saw something that is not on t…"* and *"P1 · 1 stop · 8 photos · still…"*. A scout standing in a field is the only user guaranteed to be on a handset.

**Do not attempt this in the same pass as sections 2 to 6.** Fix the tokens, ship, then treat the phone layout as its own piece of work — starting with `Field checks · phone`, which is the one screen that cannot wait.

Also noted, not a defect yet: `Reset demo` is a 30 px control. Correct on desktop per the design system, below a comfortable touch target if a phone layout arrives.

---

## 8 · Acceptance tests

These were found by measurement, so verify them by measurement. **Do not eyeball a contrast ratio.**

1. Every severity chip clears **4.5 : 1** against its fill. Check all five values on *Notification preferences*, where the whole ladder renders together.
2. One rule governs all five chips. No value uses a different treatment from its neighbours.
3. The refusal colour clears 4.5 : 1, **or** refusals render in body colour with meaning carried by icon and label.
4. `rgb(239,68,68)` on `rgb(26,31,28)` clears 4.5 : 1, or the pairing no longer occurs.
5. No method disclosure renders in the alarm colour, in either theme.
6. In dark mode, no disclosure text shares a hue with the exception-row tint.
7. `New estate` states why it is disabled, in the pattern its neighbours already use.
8. Dark mode still passes everything above. It was sound before this work; do not regress it.
9. **Re-measure every screen, not a sample.** These are token changes, so a fix on one screen is a fix on all of them — and a mistake on one is a mistake on all of them.

**Three method notes, all learned the hard way:**

- **Allow 2 seconds after navigating** before measuring. A 900 ms settle produced six screens reporting an identical 32 elements — that was the chrome, measured before the screen rendered.
- **Read the `body` background before trusting any colour result.** The preview silently reverts to light on reload. Light is `rgb(248,249,250)`, dark is `rgb(26,31,28)`.
- **A suspiciously uniform result is an artefact, not a finding.**

---

## 9 · Out of scope

- **Any copy change.** Nothing here is a wording problem. The sentences are right; the colours are wrong.
- **Any layout change on desktop or tablet.** 834 is clean and 1440 is clean.
- **New tokens.** Change the values of the ones that exist; the design system says introduce none.
- **The phone layout, if you are doing sections 2 to 6.** See §7.

If a decision you need is not in this file and not resolvable from it, **stop and say "Decision required: *question*"** rather than picking a default.
