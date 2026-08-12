# Track B, Brief 4 — Variable-rate application

**Status: written, stale. Do not build from this file as it stands. Corrected 13 August 2026.**

It was written when every band was `decision_required` and validation was the only route to a permitted rate. Both premises have changed: three oil palm peat rows are now `sourced`, and **attribution** is a second route (PR #5, merged). Three sections are wrong as a result:

- **§4.3** says a rate outside the band is a refusal, full stop. It needs the attributed route.
- **§3** gives `scout` **Generate ✓** and `regional` **Review ✓**. The live docs give `scout` upload only and `regional` nothing, and `viewer` lost its review tick because attaching a comment is a write.
- The **unit** case is missing entirely: the three sourced rows are **per palm**, so a per-hectare submission must refuse on the unit before the min/max check.

Rewrite around attribution before building. `PHASE-17-prescription-creation.md` is the current authority on this surface.

Self-contained. **Attach nothing.** Every fact you need is inline. If something here contradicts your memory of an earlier phase, this file wins.

Run in a **fresh chat**. This is the last Track B workstream.

---

## 1 · What makes this one different

Everything built so far is reversible inside the platform. A finding can be dismissed, a block archived, a season closed. **A prescription leaves the building.** It becomes a file on a machine display, and then it becomes fertiliser on the ground at a rate somebody chose. There is no undo for a hectare that received 150 kg of nitrogen.

So this screen has a different centre of gravity from the other three. The other briefs were about recording things honestly. This one is about **not letting the platform author an agronomic decision it cannot stand behind** — and about making the moment of irreversibility unmissable.

The single most important line in the corpus for this screen: *the advisor pre-fills the draft from the latest calibration; the user reviews rather than authors.* The platform proposes. A person decides. That distinction must be visible on screen, not just true in the code.

---

## 2 · Non-negotiables

Same closed sets as Briefs 1–3.

- **Eight guardrail categories.** No ninth.
- **Nine telemetry event types.** No tenth.
- **No new roles.**
- **Human-readable ids only.** No UUIDs.
- **Compose from existing components.** Preserve base tokens, introduce none.
- **Product mode carries no spec vocabulary.**
- **Anything the resolver computes has exactly one reader.**

---

## 3 · The lifecycle

Six stages. All six exist in the docs; build all six.

| Stage | Who acts | What it means |
| --- | --- | --- |
| **Generated** | System or agronomist | The map exists as a draft. Appears in the field's list. Info-level in the feed. |
| **Reviewed** | Agronomist or supervisor | Zones and rates have been opened and confirmed. Comments attached. |
| **Approved** | Approver | **The map locks.** Export becomes possible. |
| **Exported** | Operator | A machine-format file has been downloaded. Timestamp and target device recorded. |
| **Applied** | Operator, via as-applied upload, or a machinery webhook | What actually happened on the ground. **Resets severity** on any hazard the map targets. |
| **Verified** | System | Cross-checked against the satellite record. Becomes eligible evidence for a proof pack. |

**Approved locks the map.** Editing after export is a confirmation-gated action, not a quiet edit — the file already left.

**Staleness is an alert, not a badge.** A map sitting in Generated or Approved for more than five days, on a field entering a critical growth stage, is promoted to a medium alert in the feed. A prescription written for a stage the crop has now left is worse than no prescription.

### Who may do what

| Role | Generate | Review | Approve | Export | Upload as-applied |
| --- | :-: | :-: | :-: | :-: | :-: |
| `scout` (operator) | ✓ | — | — | ✓ after approval | ✓ |
| `agronomist` | ✓ | ✓ | — | ✓ | ✓ |
| `approver` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `estate_manager` | ✓ | ✓ | ✓ | ✓ | ✓ |
| `regional` (agency officer) | — | ✓ | — | — | — |

**An agronomist can author a prescription but cannot approve their own.** That is the same separation Brief 1 established for findings, and it is here for the same reason. Explain the ceiling on screen; do not just disable the control.

---

## 4 · Building the map

### 4.1 Base index — this choice is agronomic, not cosmetic

The index decides what the zones actually mean. Offer three, each with its use stated:

| Index | Use for |
| --- | --- |
| **NDVI** | Variable seeding rates and general biomass zoning |
| **NDRE** | Variable nitrogen — more sensitive to chlorophyll at high biomass |
| **NDWI** | Irrigation scheduling and fungicide timing by canopy water status |

**NDRE is not an alternative to NDVI for nitrogen — it is the correct one.** On a closed canopy NDVI has saturated and cannot discriminate, so a nitrogen map zoned on NDVI at high biomass is drawing zones out of noise. If the field is at a stage where the chosen index has saturated, say so before the zones are drawn, not after.

### 4.2 Zones

Two to five, three being the usual choice. The platform clusters the pixels and shows the zone map immediately.

**Zones here are computed zones, not blocks.** They are generated per map, from one index, at one moment, and they are discarded with the map. They accumulate no history. A block is a management unit that persists; a zone is a slice of one image. Do not let the UI blur them — Brief 3 turns on the same distinction.

**Zones must cover the target field with no unintentional gaps or overlaps.** An uncovered strip receives nothing and nobody notices until harvest.

### 4.3 Rates — the part the platform must not author

The platform proposes default relative rates scaled from the base application rate. **The person edits them.** Frame it that way on screen: these are a starting point drawn from the latest calibration, not a recommendation the platform is standing behind.

Two guardrails on the numbers themselves:

- **Rates must fall inside the crop-specific agronomic minimum and maximum.** Outside the band is a refusal, not a warning.
- **Rates near the top of the band, but not over, get a soft warning.** Near-maximum on every zone usually means the base rate is wrong rather than the map.

**A finding may motivate a map but must never pre-fill its rates.** Brief 1 established this: a map drafted from a finding opens with that finding attached as evidence and the hazard as the target, and the rates blank. Rates are authored, never inferred from a hazard.

---

## 5 · Export — the irreversible moment

Export is where the decision leaves the platform. Treat it accordingly.

- **Export is a confirmation.** State plainly that the file is leaving and that what happens next is not recorded until as-applied data comes back.
- **Record the format and the target device.** The export formats are equipment-specific; a file in the wrong format is a wasted pass.
- **Fleet apply across multiple fields is a separate, harder confirmation**, and it is rate-limited per action because the blast radius and the external cost both scale with the number of fields.
- **Editing after export is confirmation-gated.** The exported file is already downstream; an edited map that shares its identity is how two different prescriptions end up with one audit trail.

---

## 6 · As-applied — closing the loop

The operator uploads what the machine actually did. The platform overlays target against actual and highlights where they diverged.

**The deviation thresholds are already fixed by the docs — use them exactly:**

| Deviation | Treatment |
| --- | --- |
| Under 10% | Normal. No flag. |
| 10–20% | Soft warning, informational. |
| Over 20% | **Escalates.** |

Overriding the deviation warning is a confirmation, and it enters the audit.

**Deviation is diagnostic, not disciplinary.** The docs are explicit that the comparison exists to find calibration drift, coverage gaps and overlaps, and to feed the next planning session. Write the copy that way — the operator reporting honest as-applied data is doing the right thing, and the screen should not read like an accusation.

**Applied resets severity.** An applied map triggers the binding on any rule card it targets, which typically resets that hazard's severity. This is the loop closing: the platform saw a problem, someone acted, the record reflects it.

**Then the cross-check runs against the next satellite pass**, and only once it passes does the entry become eligible evidence for a proof pack. Until then it is applied but not verified, and those are different words that must not be shown as the same state.

---

## 7 · Copy rules

| Never show | Say instead |
| --- | --- |
| `map.generated`, `map.approved`, `map.applied` | The stage's own name |
| `index_basis` | Which index it was built from |
| `activity_bindings` | Don't show — describe the effect |
| `as-applied` (as a bare term) | "What was actually applied" |
| `k-means`, "clustered" | "The platform grouped the field by how it is reading" |
| `deviation > 10%` | "Applied more than a tenth away from the plan" |

The alert copy already in the product says *"What went on D1 is 24% away from the plan"* — that is the register. Match it.

---

## 8 · Seed

| Fixture | Proves |
| --- | --- |
| A nitrogen map on NDRE, three zones, approved and exported | The straight path. |
| The same map at Generated, six days old, on a field entering a critical stage | The staleness alert fires. |
| A map authored by an agronomist awaiting approval | The author cannot approve their own; the ceiling is explained. |
| A map whose as-applied came back 24% away from plan | Escalation. Pair it with `KADA D1`, which already carries this alert. |
| A map with as-applied 14% away | Soft warning only, informational tone. |
| A map applied but not yet cross-checked | **Applied and verified render as different states.** |
| A nitrogen map attempted on NDVI at closed canopy | The saturation warning fires before zones are drawn. |
| A map drafted from the panel-dryness finding on `field_ldg_perak_r3` | Finding attached as evidence, hazard as target, **rates blank**. |

---

## 9 · Acceptance tests

1. All six lifecycle stages are reachable and visually distinct.
2. An approved map is locked; editing it after export requires a confirmation that states the file has already left.
3. An agronomist cannot approve a map they authored, and is told why rather than shown a dead control.
4. A rate outside the crop's agronomic band is **refused**; a rate near the top of the band gets a soft warning.
5. A map drafted from a finding opens with **blank rates** and the finding attached as evidence.
6. A nitrogen map on NDVI at closed canopy warns about saturation **before** zones are drawn.
7. Zones leaving a gap in the field are refused with a plain reason.
8. Export shows a confirmation naming the format and target device.
9. Fleet apply requires its own confirmation and is rate-limited, with the limit stated.
10. As-applied at 14% is informational; at 24% it escalates. Overriding the warning enters the audit.
11. The deviation view reads as diagnostic, not disciplinary. No blame language.
12. An applied map resets severity on the hazard it targets.
13. **Applied and verified are visibly different states**, and a map that is applied but not cross-checked cannot enter a proof pack.
14. A map sitting five days on a field entering a critical stage raises a medium alert.
15. Zones are never described in a way that could be mistaken for blocks.
16. No screen in Product mode contains an underscore, an enum value or a field name. Grep the rendered copy.
17. Dark and light both pass on every new screen, at all three widths.

---

## 10 · Out of scope

- **Machinery webhooks.** As-applied arrives by upload in this phase. The webhook path is real in the docs but is an integration, not a screen.
- **Authoring the agronomic band** for a crop and product. The min/max comes from the risk model; this screen reads it.
- **Multi-product maps.** One product per prescription.
- **Proof pack assembly.** A verified map becomes eligible evidence; building the bundle is its own surface.
- **Prescription deletion where downstream verification records exist.** The docs make it a confirmation; the retention rule is unresolved and should follow the archive-not-delete precedent from Brief 3 rather than being decided here.

If a decision you need is not in this file and not resolvable from it, **stop and say "Decision required: *question*"** rather than picking a default.
