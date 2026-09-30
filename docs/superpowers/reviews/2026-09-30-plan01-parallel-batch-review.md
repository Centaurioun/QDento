# Plan 01 Parallel Evidence Batch Review

Date: 2026-09-30

Status: Five-lane evidence batch independently reviewed by the coordinator. Ready for Task 6 synthesis with explicit coordinator rulings below.

Base planning revision used by all five lanes:
`4d21c03c11ecd1f36d05c4347f36eb4d45df4576`

## 1. Branch/commit verification

Each lane is exactly one commit ahead of the common base and changes only its authorized evidence artifact(s).

| Task | Branch | Commit | Authorized diff |
|---|---|---|---|
| T1 | `research/p01-t1-contour-site-orientation` | `e65f92d424a6aa6f9b235b28c9d91f40334d7ab9` | `A-contour-site-orientation.md` only |
| T2 | `research/p01-t2-full-mouth-findings` | `a72f98bf15f0e3754e5ddd1552321b2704a2d597` | `B-full-mouth-findings.md` only |
| T3 | `research/p01-t3-parity-fixtures` | `6cdac5dee07ce1732651ebdfd48bcc0d926abcf1` | `C-parity-fixtures.md` + fixture README only |
| T4 | `research/p01-t4-measurement-edit-semantics` | `f73fb75cebee1e4ede5025dd0b2df2e66ebebba3` | `D-measurement-edit-semantics.md` only |
| T5 | `research/p01-t5-tooth-rendering-assets` | `ebdc26f3aa13963c4b7e47d45cb6d0f7c27ac8fb` | `E-tooth-rendering-assets.md` only |

No application-source changes were found in any lane.

## 2. Task-by-task coordinator assessment

### T1 — Contour/site orientation

Accepted findings:
- all 192 packed measurement offsets are accounted for;
- QDento tooth-index → FDI mapping is explicit;
- triplet → chart item/local-slot mapping is explicit;
- upper source-visible q order is q0 left / q1 middle / q2 right;
- lower transformed visible q order is q0 right / q1 middle / q2 left;
- local baseline 105 and vertical coefficient 3 are source-verified;
- the selected PerioView uses even 48-point spacing rather than the alternative tooth-width spacing path.

Concern:
- QDento source never clinically names q0/q1/q2.
- Runtime sentinel evidence is absent because Qt tooling was unavailable.

Coordinator ruling:
This is NOT allowed to remain `BLOCKS_GEOMETRY` for the new iOS app.

Reason:
The approved design explicitly states that unknown QDento author intent is not automatically a blocker. The new app has its own canonical named-site domain from Clinica and only needs QDento's visible geometry.

Therefore Task 6 must freeze a **new-product canonical anatomical mapping** and label it as such, not as recovered QDento semantics.

### T2 — FMPS/FMBS/BOP

Accepted findings:
- FMPS and FMBS are independent 128-value persisted findings;
- four-wedge geometry is source-verified as left/up/right/down;
- BOP is a distinct 192-value six-site finding;
- legacy summary formulas are now explicit;
- disabled teeth are excluded from QDento parity denominators;
- visible FMPS/HI counts false/unselected FMPS wedges and must not be silently relabeled as a modern plaque-positive percentage.

Concern:
- anatomical meaning of four wedges is undocumented;
- BOP's named-site mapping inherits T1's unnamed q slots;
- final runtime pixel placement is unverified.

Coordinator ruling:
- four-wedge anatomy remains `NONBLOCKING_PRODUCT_DECISION`; preserve neutral left/up/right/down IDs.
- BOP named-site mapping follows the new-product canonical site mapping frozen from T1 geometry.
- runtime pixel capture becomes later parity-test evidence, not a geometry blocker.

### T3 — Deterministic fixtures

Accepted:
- the eight fixtures give a useful shared oracle across QDento evidence, Swift geometry tests, SwiftUI tests, and final screenshots;
- asymmetric values are appropriate for mirror/orientation detection;
- direct CAL edit, AG/recession, BOP, wedge, and missing/implant cases are represented.

Reconciliation needed:
- T3 was written before T4's stronger AG mapping was available.
- T3 therefore calls some attachment-surface naming unresolved even though T4 source-verifies upper/lower surface ownership.

Coordinator ruling:
Task 6 must reconcile the fixture interpretation using T4:
- upper `AG[t]` = facial/buccal applicable;
- upper `AG[t+32]` = palatal/oral disabled/not applicable;
- lower `AG[t]` = facial/buccal;
- lower `AG[t+32]` = lingual/oral.

Do not rewrite the evidence artifact; resolve it in the synthesized contract.

### T4 — PD/CAL/GM + AG/recession

Accepted:
- PD, CAL, and GM are three independent QDento edit paths;
- QDento displayed GM = PD − CAL;
- canonical clinical GM = CAL − PD;
- direct PD keeps CAL;
- direct CAL keeps PD;
- QDento's direct GM path contains legacy mutations/caps that can create model/UI divergence;
- AG mapping and persistence are source-verified;
- recession is read-only, derived, and not persisted.

Coordinator ruling for the new iOS product:
- canonical domain uses clinical GM = CAL − PD;
- parity UI may display/edit QDento-sign GM through an explicit adapter;
- direct PD edit: update PD, keep CAL, recompute both GM representations;
- direct CAL edit: update CAL, keep PD, recompute both GM representations;
- direct QDento-sign GM edit: keep PD constant and compute `CAL = PD - displayGM`;
- permitted display-GM input must be constrained so resulting CAL remains within the accepted 0...19 demo range; equivalently, for current PD `p`, display GM is limited to `p - 19 ... p`;
- do NOT reproduce QDento's pathological branch that mutates PD or permits hidden negative stored PD when the requested GM would make CAL invalid.

This is an explicit `NEW_PRODUCT_RULE` that preserves normal QDento visual algebra while rejecting a legacy model/UI divergence.

AG product rule:
- upper palatal/oral attached gingiva is represented as not applicable rather than persisted as a meaningful assessed value;
- the disabled QDento slot is reference/persistence legacy, not new-domain truth.

Recession:
- derive per surface as max(0, max canonical clinical GM across its three sites);
- do not persist redundant recession.

### T5 — Tooth rendering/assets

Accepted:
- tooth raster and moving contour are separate rendering concerns;
- minimum first-demo states are natural, missing/extracted, implant, and optional periodontal overlay;
- minimum prototype asset candidates are identified;
- a platform-independent replaceable ToothVisualProvider boundary is defined;
- generic QDento treatment artwork is correctly excluded.

Concern:
- individual image authorship/provenance remains unresolved;
- runtime appearance at iPhone size is not verified.

Coordinator ruling:
Both are nonblocking for a private parity demo, provided provenance remains explicit and no distribution-clearance claim is made.
Production/public distribution remains gated on a later asset/licensing review and likely replacement artwork.

## 3. Canonical new-product named-site mapping

This mapping is a **new iOS clinical/product contract**, not a claim about QDento author's undocumented site labels.

Canonical domain:
- facial: MB, B, DB
- oral: ML, L, DL

Screen-anatomy rule:
- middle visible point = B on facial surfaces / L on oral surfaces;
- mesial is always the interproximal point toward the dental midline;
- distal is always away from the midline.

Therefore final visible left/middle/right sites are:

| FDI quadrants | Facial visible L/M/R | Oral visible L/M/R |
|---|---|---|
| Q1 18–11 and Q4 48–41 | DB / B / MB | DL / L / ML |
| Q2 21–28 and Q3 31–38 | MB / B / DB | ML / L / DL |

Combine with T1's QDento visible q order:

### Maxilla
QDento q0/q1/q2 are visible left/middle/right.

- Q1 facial: q0=DB, q1=B, q2=MB
- Q1 oral: q0=DL, q1=L, q2=ML
- Q2 facial: q0=MB, q1=B, q2=DB
- Q2 oral: q0=ML, q1=L, q2=DL

### Mandible
QDento q0/q1/q2 are visible right/middle/left after the QDento transforms.

- Q4 facial: q0=MB, q1=B, q2=DB
- Q4 oral: q0=ML, q1=L, q2=DL
- Q3 facial: q0=DB, q1=B, q2=MB
- Q3 oral: q0=DL, q1=L, q2=ML

Mandibular surface semantics:
Treat the first stored triplet as facial/buccal and the second as oral/lingual. The conflicting QDento `ChartPosition` enum names are an internal rendering-naming anomaly; the UI construction comments plus T4 AG surface mapping are the stronger semantic evidence.

## 4. Runtime evidence disposition

No lane could launch QDento because the available environment lacked Qt/qmake.

This does not block the native iOS foundation after the above product rulings.

Runtime screenshot/sentinel evidence must be retained as:
- `BLOCKS_PARITY_ACCEPTANCE` where exact final visual comparison needs it;
- not `BLOCKS_INITIAL_DOMAIN`;
- not `BLOCKS_GEOMETRY` after the canonical product mapping above is frozen.

The later iOS implementation must still use asymmetric fixtures and simulator evidence so any incorrect transform can be corrected without changing the canonical domain.

## 5. Verdict

**READY_FOR_TASK_6_SYNTHESIS**

Task 6 should integrate the five evidence commits unchanged, apply the coordinator rulings above, and freeze Rendering/Parity Contract v1.

The synthesized contract may finish with nonblocking unresolved items, but it must not carry the unnamed QDento q-site semantics forward as a geometry blocker after adopting the explicit new-product mapping.
