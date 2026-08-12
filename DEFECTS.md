# Defects — the running log

**Status: live. Append-only.**

One log for the whole prototype. Newest pass at the top. **Do not overwrite a pass** — the record of what was fixed, and how many attempts it took, is worth more than a tidy current list. Three earlier defect files were collapsed into this one on 10 August 2026.

Everything here was driven live in the prototype with Playwright after a full reload, not read from screenshots. Figures were re-derived by hand from the rendered DOM.

---

## Open

| Id | Surface | What | Since |
| --- | --- | --- | --- |
| **D1** | Regional forecast | `One has no growth stage resolved` — fragment with no noun, glued mid-sentence | 10 Aug |
| **D2** | Both Track C surfaces | Oil palm carries an unresolved-stage caveat the overview does not support | 10 Aug |
| **D3** | Role selector | "Farm owner" is a persona, not a functional role | flagged 4× |
| **D4** | National overview | The two 17s share a value and should not | flagged 3× |

### D1 · `One has no growth stage resolved`

**Oil palm, standing alone:**

> *"One has no growth stage resolved, so the estimate is read against the crop-wide curve there."*

One what? Field, block, scheme, estate?

**Paddy, concatenated:**

> *"One scheme has not reported since 5 June, so the range is wider than usual **and One** has no growth stage resolved, so the estimate is read against the crop-wide curve there."*

Capital `O` mid-sentence, two independent clauses joined by *and*, and the reader inherits "scheme" from the first clause — which is wrong. The unresolved ground is 9.9 ha inside Muda Granary North, not a whole scheme.

The fragment is written to open a sentence and is being concatenated into the middle of one. Two sentences, with the noun named:

> *"One scheme has not reported since 5 June, so the range is wider than usual. One field has no growth stage resolved, so the estimate there is read against the crop-wide curve."*

### D2 · Oil palm's caveat has nothing behind it

The regional forecast says oil palm has something with no growth stage resolved. The national overview, **same crop, same scope**, accounts for all of it: all four bands carry a stage, none uses *"with no stage resolved"*, and the distribution sums 43 + 6 = 49 with no remainder.

Paddy is consistent across both surfaces. Rubber and pineapple say the opposite outright. Oil palm is the only crop where the two disagree.

Either the overview is not disclosing unresolved ground, or the caveat is firing on the wrong crop. **Third instance of this class** — two Track C surfaces giving different accounts of one crop at one scope.

### D3 · "Farm owner" in the role selector

There are eight functional roles plus `viewer` as an org role. That set is closed. **"Farm owner" is a persona** — it appears in audience descriptions because that is how the market talks. It has no entitlement scope, no raising ceiling, no acknowledgement rights, and no row in any table in the docs. A selector offering it is offering a role the platform cannot resolve, and whatever it currently resolves to is a silent default nobody chose.

Remove it. If a demo needs an owner-shaped persona, map it explicitly to `estate_manager` or `viewer` and label it as the mapping it is.

### D4 · The two 17s

*"Fields at serious or urgent"* reads 17. *"Findings acted on inside their window"* also reads 17. Different quantities, different denominators — fields versus findings — and no reason to coincide.

They may be genuinely equal on this seed, in which case the fix is to make that visible rather than to change the number: the two traces should show different inputs and different rules. If they do not, one is reading the other's rollup. **Check the call path, not the output** — a coincidence in a demo seed is how a shared reference survives review.

---

## 10 August — pass 5, Track C

**Fixed.** C1: the cross-crop refusal dropped its stages. C2: the field → estate collapse undone — Felda Wilayah J2's 1,864 ha band split into 1,650 prime + 214 young mature, exactly where the pigeonhole said it had to be.

The split changed the answer, which is how the fix was confirmed real: position by ground moved from *1,960.5 inside · 120.0 below* to *1,742.5 inside · 270.0 below · 68.0 above*. The 150 extra hectares below band are the young palms that had been flattered by a prime-canopy band. It propagated to the forecast — oil palm's range widened to 32,001–44,007 on an unchanged 38,004.

**Regression sweep clean** on both surfaces: no duplicate sentences, no spec vocabulary, all three horizons behave, axis ticks round on every crop, cell traces carry all four spine elements. Narrowing to oil palm withholds the choropleth with a visible refusal and a restated total.

**Opened:** D1, D2.

## 10 August — pass 4

**Fixed.** B1: each band carries its own stage, *"with no stage resolved"* where none does, and the stage spread stated in the row. B2: the per-hectare cell trace gained all four spine elements, including an empty exclusion list that says it is empty.

**Opened:** C1, C2.

## 10 August — pass 3

**Fixed.** A1: bands stopped collapsing between schemes — all four crops read as paddy did, saturation leads the rubber sentence. A2: the Muda Granary North tally reconciles three ways. A3: separator on the input count. A4: the em-dash sentence rewritten. A5: refusal title on the tile, sentence on the card. A6: per-hectare cells open their own trace naming *a ratio of two sums* against the sum above it.

**Opened:** B1 (the stage was never un-collapsed, only the band), B2.

## 10 August — pass 2

**Fixed.** The three priorities: the averaged band on paddy, the pineapple figure published on one surface and refused on the other, and perennial short-horizon figures computed by dividing the annual by the calendar. The §5b national-overview rebuild also landed — one rule sentence in a group header instead of four times, the designer's justification removed, *"every reading below"* replacing *"what follows"*.

**Opened:** A1 through A6.

## 9–10 August — pass 1, the scope and density review

The regional forecast had been rebuilt against `CONVENTION-scope-and-density.md`; the national overview had not. Found the averaged band (provable by arithmetic: area-weighting two children's bands reproduced the parent's exactly), the pineapple contradiction between surfaces, and the pro-rated perennial forecasts. Also the four-times-repeated rule sentence, an exception row with no action, and a verdict whose scope claim was contradicted eight lines below it.

---

## Earlier — the field-creation pass

Kept from `DEFECTS-set-up-a-field.md`, which ran D0a and D0 through D8 against Track B2 and B3. That file is superseded by this one; its content is the record of the field-creation build and its status table was current as of 8 August.

**Notable, because it is the class that keeps recurring:** the name box storing the change event instead of the text, four `Empty` states passing `detail` where the component takes `description`, and a `<label>` with no `for` that made two band-tier selects a single control to anything addressing them by name.

**Three times in that pass I reported the build broken when the fault was mine** — coordinate drift from a screenshot scaled 1568 against a 1512 window, a `fill()` that bypassed the key handler I was meant to be testing, and a standalone Design Component that renders nothing because no parent shell hands it a store. Reload the preview and check a real click before concluding anything is inert.
