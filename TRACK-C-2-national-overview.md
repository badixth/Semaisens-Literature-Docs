# Track C, Brief 2 — The national overview

**Status: built, and rebuilt against §5b and `CONVENTION-scope-and-density.md`.** Tested through five passes; see `DEFECTS.md`.

Self-contained. **Attach nothing.** Every fact you need is inline. If something here contradicts your memory of an earlier phase, this file wins.

Run in a **fresh chat**. This is the first Track C surface. It builds on the spine but adds no rules to it.

---

## 1 · The test this screen has to pass

**Can somebody read the headline and close the tab?**

If the answer is a verdict they trust, the screen works. If it is six numbers they have to assemble themselves, it does not — however correctly each one was computed.

That is the whole brief. Everything below serves it.

---

## 2 · What this screen is not

**It is not "How a figure is built."** That screen exists to prove the rules and let anyone open a number to its provenance. Six unrelated figures with no hierarchy is the correct shape for that job and the wrong shape for this one.

**It is not a shelf of totals.** Hectares monitored is context. It is not news, it does not change, and nobody opens a dashboard to learn it.

**It does not compute anything.** Every figure comes from the spine — `job()` writes, this screen reads. If you find yourself adding a rule here, it belongs in the spine and every other surface needs it too.

---

## 3 · Who is reading, and what they actually want

Two audiences, one screen.

**An agency officer** reads across organisations. They want: is the scheme on track, where is it not, and can I evidence that to somebody above me.

**An estate manager or org admin** at organisation scope wants: which of my estates needs me this week.

Both questions are about **exception and change**, not about state. Neither person needs to be told the total area they manage; they already know it.

---

## 4 · The verdict, and the order it resolves in

The headline is one sentence. It resolves in this order, and **the first one that fires wins** — because a verdict computed on ground you cannot see is not a verdict.

### 4.1 Can I see it? — coverage outranks everything

If schemes have not reported, say that first.

> *"Four schemes have not reported since 8 June. What follows covers the other nine."*

**This is the fix for the confidence problem.** "Confidence low" is a modelling word that tells a reader nothing they can act on. Staleness and coverage are the same fact expressed as something a person can chase. A green reading on stale data is not a reassurance, it is a lie with a date on it.

Confidence belongs in Review mode. **Product mode states coverage.**

### 4.2 Is anything past its window?

> *"Three schemes have work past the window it had."*

Overdue outranks unhealthy, because overdue is already a decision someone failed to make. Unhealthy may simply be weather.

### 4.3 Is anything outside its band?

> *"Two schemes are reading outside their band for this stage."*

Band-relative, never a bare index — the rule from the spine. A scheme is not "low", it is outside the band that resolved for its crop at its stage.

### 4.4 Nothing

> *"Nothing needs a decision today. Every scheme in scope reported, and every crop is inside its band for its stage."*

**This must be a real, reachable state, and it must read as an answer rather than an empty screen.** The field-level product already does this — *"Nothing needs you today"* — and it is one of the most valuable things this product says. An executive who can close the tab because the platform told them to is the strongest possible outcome.

---

## 5 · What sits under the verdict

In this order. Nothing above the fold that is not the verdict and its immediate evidence.

**The exceptions, named.** Which schemes, what is wrong, and what the next action is. This is the content. Everything else is context.

**Per-crop health, band-relative.** Four rows, each with its band and stage, saturation flagged where it applies. Never a combined figure across crops — that is refused with the reason, per the spine.

**The totals, quietly.** Hectares, field count, forecast per crop. These are the footer of the story, not its headline. They exist so a reader can orient, and so a figure can be opened to its trace.

**Every figure opens to its trace**, unchanged from the spine: inputs, rule, job run, and every child left out with its reason.

---

## 5b · Layout, weight and components

The first build was correct and heavy. Everything below is about weight, not content — almost no copy needs rewriting, it needs relocating.

### The three failures to avoid, named

**A rule repeated per row.** Four overdue schemes each carrying the same sentence — *"A count of findings on this scheme, never a severity for the scheme itself"* — is that rule stated four times. A rule belongs **once, in the group header**. This is the labels-file discipline applied to layout: state it in one place, never per instance.

**Everything as a card.** Cards are for things acted on individually. Tables are for things scanned and compared. Seven exceptions in seven equal cards read as seven equal decisions, when one scheme going dark outranks one overdue finding. Card-for-everything is why the screen reads flat.

**This screen teaching the rules.** *"There is no field on this list: a national surface that could pick one would unpick the rules that let it hold a national figure at all"* is a designer justifying a decision. **"How a figure is built" already exists for that job.** A screen that teaches while it reports cannot also be closed in ten seconds. Explanations of *why the platform behaves this way* belong there or in Review mode. This screen explains only what a reader must know to trust the number in front of them.

### Three zones, sharply different weight

**Zone 1 — the verdict.** One sentence, the largest type on the page. Nothing competes with it above the fold. The reasoning behind the verdict order becomes a small **"Why this order?"** disclosure, not a paragraph. The job-run and data-through line shrinks to a footnote under it.

**Zone 2 — what to do.** The content of the screen. Not cards: an `ItemGroup` per kind, each with a `GroupHeader` carrying that kind's rule **once**. Three columns per row — scheme, what is wrong, the action.

Grouped as: *Not reporting* · *Past its window* · *Outside band*. Ordered by the §4 verdict order, which the grouping then makes visible without needing to be described.

A row reads: **KADA Blok W4 · 3 findings past their window · Open the queue.** One line, scannable. The rule is still there, once, above the group.

**Zone 3 — what it rests on.** Visually recessive, reasonably collapsed by default. Crop health as a compact four-row table, which it nearly is already. The totals as a single `MetricWell` strip rather than four cards — the design system is explicit that a reading inside a card is a well, a recess rather than another surface, and that **cards never nest**.

### Component mapping

| Zone | Use | Not |
| --- | --- | --- |
| Verdict | `PageHeader` with the sentence as the title | A card |
| Exceptions | `ItemGroup` + `GroupHeader` + `Item` | `SectionCard` per exception |
| Crop health | `Table` | Cards |
| Totals | `MetricWell` in one strip | Four `StatCard`s |
| Every explanation | A disclosure, or the group header | Always-on prose |

The system already carries every one of these. The first build used `SectionCard` for all of it, which is the whole of the heaviness.

### Every row with no action leaves the exception list

Saturation is a caveat about a reading, not something to do. It already appears on the crop-health row where it belongs. **An item under "what needs attention" with no action is a category error** — if there is nothing to do, it is context, and context lives in zone 3.

---

## 6 · Scope, and the line this screen must not cross

**Scope filtering: yes.** Region, scheme, crop, organisation. This is how a reader narrows a national picture to the part they own, and the scope tree already exists.

**Field selection: no.** Track C exists for people who cannot see individual fields. Adding a field picker to a national surface quietly unpicks the k-anonymity and access rules the spine enforces. If a reader is entitled to a field, they reach it through the fields workspace, not through here.

**When scope narrows, the verdict recomputes.** A verdict for "all of Kedah" and a verdict for one scheme are different sentences, and the screen must not carry the national headline over a filtered view.

---

## 7 · Refusals

Reuse the spine's refusal set — do not write new copy for the same cases. The ones that will show on this screen:

| Case | Behaviour |
| --- | --- |
| Combined crop health | Refused, with the reason. Per-crop shown instead. |
| A breakdown that would drop below k | Breakdown refused, total stays, refusal visible. |
| Money below `farm_calibrated` | Physical response shown, ringgit declined with the reason. |
| Mixed delivery policies in scope | Disclosed — counts comparable, timeliness not. |
| Forecast unavailable | Em dash and the reason. Never a zero. |

**A refusal is an answer and renders as one.** Never a blank, never a dash on its own.

---

## 8 · Pointing at the other three

This screen is the container. It should point at the regional forecast, yield production and assistance impact rather than duplicating them.

The rule: **the overview says what needs attention; the other three say why and how much.** If a reader has to leave the overview to find out whether something is wrong, the overview failed. If they leave it to find out the detail of something they already know is wrong, that is correct.

---

## 9 · Copy rules

| Never show | Say instead |
| --- | --- |
| "Confidence low" | What is actually missing — "Muda Granary North last reported 8 June" |
| A bare index | The reading with its band and stage |
| `farm_calibrated`, `by_region` | "This farm's own history has not yet tuned the curve" |
| `k=5`, "k-anonymity" | "Fewer than five fields — the breakdown is withheld, the total stands" |
| An estate-level severity | A count of fields at that band |

Sentence case. The middot as separator. Missing data is an em dash with a reason beside it, never a zero.

---

## 10 · Acceptance tests

1. **The headline is a sentence, and it is the first thing on the screen.** Not a number, not a card.
2. Seed a stale scheme and confirm the **coverage verdict outranks** the health verdict.
3. Seed everything healthy and current, and confirm the screen says so — as an answer, not an empty state.
4. Overdue work outranks out-of-band readings in the verdict order.
5. Every crop health figure carries its band and stage. **No bare index anywhere.**
6. A saturated reading is flagged as uninformative rather than ranked.
7. Narrowing the scope recomputes the verdict; the national headline never survives a filter.
8. No field-level picker exists on this screen.
9. Every figure opens a trace with inputs, rule, job run and exclusions.
10. Every refusal renders with its own words, reused from the spine — grep for a generic "unavailable" and find none.
11. Nothing on this screen computes a rollup. Check the call path.
12. No Product-mode copy contains "confidence", an enum value, an underscore or a field name.
13. Dark and light pass at all three widths.

**Weight and structure — these are what the rebuild is for:**

14. **No rule text appears more than once on the screen.** Grep the rendered copy for any sentence occurring twice; find none.
15. The exceptions render as grouped rows, not as one card each, and **every row fits on one line** at desktop width.
16. **Every row in the exception list has an action.** Anything without one has moved to context.
17. The verdict is the largest type on the page and nothing above the fold competes with it.
18. The totals render as one recessed strip, not as four cards. Cards do not nest anywhere on this screen.
19. Every "why the platform behaves this way" explanation is either a disclosure, a group header, or absent — never always-on prose beside a figure.
20. **Read the screen cold and time it.** If the verdict and the list of what to do cannot be taken in within about ten seconds, the layout has failed regardless of which components were used.

**Also fix, carried from the first build's review:**

21. The verdict's scope claim matches what follows. If a non-reporting scheme still appears in the overdue list, the headline says *"every reading below"*, not *"what follows"*.
22. **The national band for a crop is resolved once for that crop at that stage — never blended from its children's bands.** Averaging bands is averaging an average in band space, and the result always looks plausible. State in the trace where the national band came from.
23. Only one scope control governs the screen, and the app breadcrumb and the page scope row never disagree.

---

## 11 · Out of scope

- **Regional forecast, yield production, assistance impact.** Their own briefs.
- **Changing any spine rule.** If one needs changing, it changes in the spine.
- **A price basis.** Money stays gated and refusing.
- **Field-level drilldown.**

Two things carried in from the spine review and still open — do not paper over them here:

- The **two 17s**: "fields at serious or urgent" and "findings acted on inside their window" are different quantities that currently share a value.
- The **avoided-loss trace** lists hectares as "what fed it" under "no rule applied". A refused figure should show what would have fed it and why it cannot, not a measure it never used.

If a decision you need is not in this file and not resolvable from it, **stop and say "Decision required: *question*"** rather than picking a default.
