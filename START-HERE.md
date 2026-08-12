# Start here — what to run, in what order

**Status: live. Rewritten 10 August 2026.**

Read this first. It tells you what state the project is in, what to paste next, and what to check when it comes back. `CLAUDE.md` is the agent-facing companion — where truth lives and what not to reopen.

---

## Where things stand

| Track | What it is | State |
| --- | --- | --- |
| **A** | The diagnostic surface | Built through Phase 10 |
| **B** | Operational — findings, fields, blocks, VRA | B1–B3, B5, B6, B7 built. **B4 stale** — written around validation only, needs rewriting around attribution. |
| **C** | Executive and agency — the rollup surfaces | C1–C3 built and tested. **C4 written, not built.** C5 not written. |

---

## The two things to paste right now

They go to **different agents** and do not depend on each other.

### 1 → a fresh build chat

**Paste:** `TRACK-C-4-yield-production.md`

**Attach nothing.** The brief is self-contained by design. Attaching the docs repo is what saturated three earlier chats — the file carries every fact the builder needs inline.

Also send `DEFECTS.md`, which has four open items against the surfaces already built. Two are new, two have outlived several reviews.

### 2 → your literature agent, or an agronomist

**Read first:** `DECISION-rate-bands-do-not-exist.md`

This one is not a research task any more. The research came back and the answer is structural: **no Malaysian authority publishes a min/max for any crop-product pair.** DOA, RISDA and MPIB all publish point recommendations. `guides/prescription-maps` asks for a number that has to be *set*, not found.

The file carries the sourced point recommendations that did come back — rice from DOA Rice Check 2022, rubber from RISDA, pineapple from MPIB — and states the decision. Someone with agronomy authority has to answer it before VRA can accept a single prescription.

---

## What is deliberately not running yet

**Track C brief 5, assistance impact.** Last by design. Without a price basis it ships as mostly refusals, and the price basis runs through `farm_calibrated`, which runs through corroboration history.

**Track B5, the board view.** Written, read-only, ready to paste. **Correcting what this file used to say:** it claimed the task lifecycle "already has all seven states". It does not — the docs never enumerate them. See `TRACK-B-5-board-view.md` §3, which cites what the docs do evidence and records the gap.

**Phase 11, the final sweep.** Dark and light parity, three widths, no dead controls, across every screen. Hold it until Track C stops moving and do one pass over everything.

---

## What to check when brief 4 comes back

Four things, in this order.

**1. No production figure carries a range.** A count is counted. If a spread appears beside a harvest figure, something upstream is treating it as a forecast. The whole point of the surface is that actual and modelled are different classes of claim.

**2. Boxplot outliers must identify no field.** This is the subtle one. A boxplot renders outliers as individual points by construction, and a hoverable outlier is a field-level drilldown through the back door — on the surface where a field picker was refused precisely to protect that rule. Either suppress them or make them unlabelled and unhoverable.

**3. A boxplot over fewer than five fields is a scatter plot of named farms.** The box hides nothing when n is small. Refuse it in words and keep the total, reusing the choropleth's existing wording.

**4. Bands and stages are still not collapsed.** This has been fixed three times at three different levels — between schemes, then between crops, then inside a single scheme. Check the level below wherever you last looked.

If the build reports something the brief did not cover, its closing instruction tells it to stop and say *"Decision required"* rather than guess. Send me that question rather than answering it yourself — that is how the 28-month axis defect happened, and it came from a reasonable guess filling a gap the brief left.

---

## How to read this folder

Every root file carries a status line under its title. Four kinds live here and they decay at very different rates:

| Prefix | What it is | Half-life |
| --- | --- | --- |
| `CLAUDE.md`, `CONVENTION-*`, `CHARTS-product-map` | Standing law. Read before acting. | Indefinite |
| `TRACK-*` | One-shot work orders. Consumed once built. | Until built |
| `DECISION-*` | Settled positions, including two deliberately-kept wrong turns | Indefinite |
| `DEFECTS.md`, `HANDOFF-*` | Point-in-time observations | Hours to days |

`DEFECTS.md` is append-only with dates. Do not overwrite it — the record of what was fixed when is worth more than a tidy current list.

`archive/` is in `.gitignore`. Moving a file there removes it from the repo, so do not use it for consumed briefs; they are the record of why things are the way they are.
