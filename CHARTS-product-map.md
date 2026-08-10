# Charts — the product map

**Status: standing law.** Read before adding any chart to any surface. Companion to `guidelines/CHARTS.md` in the design system.

A companion to `guidelines/CHARTS.md`, not a replacement.

`CHARTS.md` answers *what exists and how it behaves*. This answers *which mark belongs where in this product, and why*. It exists so four surfaces do not each invent their own allocation, and so a screen built after this one inherits a chart budget rather than a chart library.

---

## The rule that governs all of it

**A chart is something a reader interprets. A verdict is something a reader reads.**

Twenty-nine chart types and a goal of *read the headline and close the tab* pull against each other. The instinct after acquiring a chart set is to use it, and on the surfaces where speed matters most, most charts slow a reader down.

So the budget is deliberately uneven. The national overview gets the fewest marks in the product. The analytical surfaces get the most.

**And the corollary, which matters more than it sounds:** where a mark can replace a sentence, it should. The screens are heavy with words partly because they say in prose what a mark shows at a glance. Reducing words and adding the right marks are the same piece of work, not two.

---

## The five questions a planter actually has

Everything below hangs off these. If a chart does not answer one of them, it needs a reason to exist.

| # | The question | Where it is answered | The mark |
| --- | --- | --- | --- |
| 1 | **Is anything wrong, and where?** | National overview · What needs you | `Sparkline` only |
| 2 | **Is my crop tracking where it should be for its stage?** | Field analytics · National overview | `ChartGauge` with the band behind it |
| 3 | **Will I hit the yield I expect?** | Regional forecast · Yield production | `ChartBand`, never a point |
| 4 | **Did what I did work?** | Assistance impact | `ChartBullet`, `ChartScatter` |
| 5 | **Can I prove it?** | Proof packs · Verification | No chart. A bundle is evidence, not a picture. |

Question 5 having no chart is a decision, not an omission. An auditor wants the rows, the hashes and the exclusions. A chart of a proof pack would be decoration on the one artifact that must not be decorated.

---

## Per surface

### National overview — two marks, and they are both small

The budget is deliberately tiny. This screen is read in ten seconds and closed.

**`Sparkline`** in exception rows and crop rows. Your own decision table has it right: *a mark, not a chart; value printed beside it.* It answers *rising or falling* without an axis to read.

**`ChartGauge`** on each crop-health row, one per row, band drawn behind the mark.

This is the highest-value chart in the product and the reason is nominal-is-best. The row currently reads *"Inside the 0.54–0.74 band for paddy at panicle initiation / booting"* — nine words a reader assembles into a mental picture. The gauge **is** that picture. It also makes deviation-in-either-direction obvious, which prose never manages: a reader told "0.81, inside the band" still thinks higher is better.

**Saturation is drawn, not just said.** Where the index has saturated, the gauge shows the reading pressed against a ceiling it cannot discriminate past. Rubber at 0.81 should look inconclusive, not excellent.

**Nothing else.** No trend lines, no distribution, no composition. If a figure on this screen needs a chart to be understood, it is the wrong figure for this screen.

### Regional forecast — the band is the point

**`ChartBand`** as the primary mark, and this is more than a styling choice.

The forecast currently reads **3,178 t** — a single hard number to four significant figures. This product refuses to fabricate figures, takes confidence as the minimum of its inputs, and will not price a tonne without calibration. Then it states a yield forecast as a point.

`CHARTS.md` already has the argument: *the spread is data, so draw it.* **A forecast drawn as a band is both more honest and more useful than a point**, and it is the same instinct that produced every other refusal in this product. A planter who sees a band knows what to plan against; a planter who sees 3,178 t plans against a number nobody stands behind.

`ChartLine` for the history behind it. One band, one line, one card.

### Yield production — spread, not means

**`ChartBoxplot`** where estates are compared. Your table's line is exactly right and worth keeping: *a bar of means cannot answer "is this estate uniform?"*

That question is the whole point of a production surface. An estate averaging 4.5 t/ha with every block at 4.5 is a different business from one averaging 4.5 with half its blocks at 3 and half at 6, and only one of them has a problem to fix.

**`ChartBarGrouped`** for the same measure across two or three things — season against season, panel against panel. Never four series; your table already caps it.

**`ChartHistogram`** where the shape of one estate's blocks matters. Again per your own note: *a mean hides whether a block is uniform or half-dead.*

### Assistance impact — the most careful surface

**`ChartBullet`** for measured against expected. It stacks and it is small, which matters because this surface carries many claims.

**`ChartScatter`** where two measures are compared — with the caveat `CHARTS.md` already states, that **`trend` is a fit, never a cause.** On the surface that routes public money, that caption is not optional.

**No monetary chart until a price basis exists and the resolver reaches `farm_calibrated`.** A ringgit axis is the fastest way to make an unearned number look authoritative. The refusal stays a refusal, in words.

---

## Deliberately left as text

Recorded so nobody "improves" these later.

- **Verification acceptance.** 84% is context, not news. A meter adds furniture to a figure nobody opens the screen for.
- **Hectares monitored.** A total that does not move is not a trend.
- **Any refusal.** A refusal is a sentence with a reason. Drawing it gives a reader a shape to interpret when what they need is to be told why.
- **Counts of fields at a severity.** A count is a count. `CHARTS.md` says it plainly: bars imply each period is a separate thing.

---

## Two rules that will otherwise be broken

**Never a bare index on a scale.** A chart with a 0-to-1 axis and no band drawn is the nominal-is-best defect in visual form, and worse than the prose version because a scale implies more is better. Every index mark carries its band or it does not ship.

**Two charts in one card share their left padding.** Straight from `CHARTS.md`, and repeated here because it is the most common way a dashboard looks broken and the easiest to miss in review.

---

## What to check in review

1. The national overview carries **no chart larger than a row**.
2. Every index mark shows its band. Grep for a gauge or sparkline without one.
3. A saturated reading is drawn as inconclusive, not as high.
4. Every forecast is a band or carries its spread in words. No bare point estimates.
5. No monetary axis exists anywhere.
6. Any `ChartScatter` carries the fit-not-cause caption.
7. Charts in the same card share left padding and their baselines line up.
8. Every chart answers one of the five questions. If it does not, it has a stated reason or it goes.
