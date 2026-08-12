# WITHDRAWN — superseded by DECISION-severity-option-R.md

**Status: withdrawn, kept on purpose.** Do not build from this file.

This file recommended **Option P (permissive)** for human finding severity. It was withdrawn on 8 August 2026 and replaced by [`DECISION-severity-option-R.md`](DECISION-severity-option-R.md), which decides **Option R (restrictive)**.

**Do not build from this file.** It is kept only so the reasoning error is traceable.

## Why it was withdrawn

Two reasons, in order of weight.

**1. It was factually wrong about what R costs.** The argument for P rested on the seed fixture `fnd_seed_rubber_panel_dryness_01` — an agronomist raising panel dryness at `high` on a block reading inside band — and claimed Option R would hold that finding in `pending_review` until a peer signed. It would not. Under R an `agronomist` commits `high` directly; only `critical` requires a co-signature for that role. The one fixture that proves the human channel is necessary is untouched by R. The argument was built on a misread cell in the raising table.

Two supporting claims failed for the same reason: R blocks shipping on Open Question 4 for `critical` only, not for `high`; and the reversibility argument points the other way in practice, because retrofitting `pending_review`, a review queue and escalation timers into a live system already holding findings is the band-model retrofit pattern.

**2. Co-signature on a serious assertion is standard practice in the organisation.** That constraint was not available when P was recommended. A gate the organisation already runs informally is cheaper to encode than to fight.

## What survived

The rate limits, the input-validation refusal streak, and the append-only audit rules. This file framed them as compensating controls for the absence of a raise-time gate. They are not compensating controls — they are baseline hygiene, and Option R does not excuse skipping them. They carry forward into the R decision unchanged.
