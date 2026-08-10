# Handoff — finding provenance, decisions and findings

**Status: consumed.** The page it describes is live at `concepts/finding-provenance`.

For the agent writing `/concepts/finding-provenance`. Read alongside `AMENDMENT-C-finding-provenance.md`, which is the analysis this page should be built from.

---

## Confirmations

**Placement — yes, `/concepts/finding-provenance`.** Alongside `field-data-model`, `risk-model` and `verification-model`. The docs repo is source of truth for every downstream brief; an amendment that never reaches it is invisible to the next reader.

**Severity — yes, option (c).** Present two named options side by side and leave the call. Ground them in what follows rather than inventing the space fresh — the analysis in Amendment C §C4 sets out why both plain answers fail.

---

## Six decisions now settled

These were Open Questions in Amendment C §C9. They are answered. Two of them change the shape of the page.

| # | Question | Decision |
| --- | --- | --- |
| 1 | Can a repeated manual finding become a rule-card proposal? | **Yes — desired.** When the same hazard label is raised across fields, it should surface as a **Proposal** artifact into the human review queue. This is the documented tier-2 path (`guides/semai-advisor/overview.mdx`) and it turns the manual channel into a route by which the knowledge base learns what it is missing. |
| 2 | Does a manual finding feed `farm_history`? | **Yes, by default, and optionally switchable.** See the guard below — this is the most consequential answer of the six. |
| 3 | Should duplicate manual findings merge? | **Yes, RBAC-gated, AI-proposed.** The platform detects a probable duplicate on the same field and hazard and **asks** — it does not merge silently. Merge authority follows role. |
| 4 | What happens when the crop cycle closes? | **Stops firing; kept in the record.** The finding closes with the cycle and remains in the log for the season's history. |
| 5 | Should an unactioned finding expire? | **Yes.** Expiry is a modelling event, not housekeeping — see below. |
| 6 | Can a read-only agency officer raise one? | **Yes.** |

---

## Three consequences the page must handle

### `farm_history` needs a guard — a wrong human finding poisons the model

`concepts/risk-model.mdx` defines `farm_history` as the top layer of yield-impact resolution — *"auto-tuned by the platform after 1–2 seasons of observations… the platform's own record of what actually happened on this farm."* Amendment A makes the same layer the top of the band resolver.

Letting manual findings calibrate it means a mistaken one degrades every future yield-impact and band resolution on that farm, **silently and for seasons**. Three guards:

- **Dismissed and expired findings never calibrate.** Only findings that reached `resolved`, or were corroborated by a field check, contribute.
- **A finding contradicted by a later check is excluded retroactively**, and any calibration derived from it is recomputed. The `raise_contradiction` mechanism already exists for machine findings; it must reach back here.
- **Calibration provenance is inspectable.** `band_source: block_history` should be able to say which observations built it. The resolver already explains itself; this extends that.

### Expiry changes the yield calculation, so it must explain itself

An unactioned finding affecting yield is defensible — unattended problems do cost yield. But it makes expiry a **measurement event**, which brings three requirements:

- **Windows differ by severity.** A `watch` finding left for a month is normal. An `urgent` one left for a month is a different fact and should cost more.
- **Expiry is disclosed, not silent.** *"Expired unactioned on 12 Jul · counted against this season's yield estimate."*
- **It must be explainable**, per `concepts/aggregation-model.mdx#reproducibility`. A yield figure moved by an expired finding has to be able to say so.

### Read-only means read-only for *operations*, not for *observations*

Answer 6 changes what the role means. An agency officer cannot assign work, approve a plan or export a proof pack — but they can record what they saw.

State it plainly on the page, because "read-only" now carries an exception. And it interacts with severity: whichever option is chosen, be explicit about whether a read-only role can raise **at** the band that trips the safety floor, or only below it.

---

## Findings from Amendment C worth carrying

Not opinions — things found in the corpus that the page should state.

**The docs distinguish evidence from judgement without ever saying so.** `New observation` is evidence. `New note` is *"not a task; not delivered as a notification"* — deliberately inert. `scout.completed` can promote, demote or contradict a finding **that already exists**. So: **humans can escalate, but cannot originate.** That is the gap in one sentence and it belongs in the lede.

**It bites hardest where the satellite is weakest.** Panel dryness *"shows as a slow, block-wide NDVI decline over 1–3 years"*; phosphorus and potassium are *"indirect only… requires soil test"*; ion species are *"No — multispectral cannot separate ion species"*. The literature documents these hazards in full and the platform cannot record a conclusion about any of them.

**The ontology is narrower than the product.** The UI, and the design system's `FindingCard`, have said *Findings* since Pass 2 while the spec carried only `rulecard_firing`. The general word was in use before the general entity existed — which is the argument for promoting `finding` rather than adding a sibling.

**A live wording bug.** `guides/field-scouting.mdx` refuses *"suppressing a task that originates from a severity ≥ high **rule card**"*. Manual findings fall outside it as written, so human high findings would be suppressible while machine ones are not. Widen to *"whatever its origin."*

**The alert/notification taxonomy has no slot for this.** `guides/activity-and-alerts.mdx` splits the feed by source — alerts from *data*, notifications from *humans*. A human-raised field-level finding is field-critical like an alert and human-sourced like a notification. Worth naming in the Docs Delta.

**`New observation` has an undefined completion path.** It *"feeds `scout.completed` semantics"*, but the workspace FAB can raise one with no scout task open to complete. What that event means with no task is unstated — and it is the closest thing the corpus has to a human-originated signal.

---

## The seed fixture that makes the argument

> **Panel dryness on `field_ldg_perak_r3`**, raised by an agronomist at `high`. Bark inspection on Panel B shows dryness over roughly a fifth of the tapped length. **The satellite has not flagged it, and is correct not to** — the literature says panel dryness surfaces over one to three years, and R3's readings are inside band.

A machine-only platform sees nothing there for another year. That single row says the human channel is not a fallback for when the model is wrong — it is the channel for what the instrument cannot see.

Add a weaker second fixture on `field_muda_b1` that duplicates an open machine finding, so the duplicate-detection prompt from decision 3 is reachable.

---

## Constraints

Human-readable IDs per Layer 1 §3. No new guardrail categories — the eight in `snippets/guardrails-template.mdx` are sufficient. No new aggregation categories. Cite doc pages; mark clearly what is decided rather than found. Keep the product's habit of refusing rather than misleading. End with Open Questions and a Docs Delta.
