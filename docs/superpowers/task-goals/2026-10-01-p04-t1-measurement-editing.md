[$superpowers](app://plugins~Plugin_60aea7460bd4819199fd97a9553a5e12)
[@Build iOS Apps](plugin://build-ios-apps@openai-curated-remote)

# TASK GOAL PACKET — PLAN 04 / TASK 1
# EDITABLE EXAM MODEL + PD/CAL/GM + ATTACHED GINGIVA EDITING

## TASK TYPE

PROMPT for one dedicated Codex controller instance.

This is the FIRST interactive Plan 04 task.

It establishes the editable examination model and the frozen measurement-edit semantics.

It must finish with:
- pure, tested editing rules;
- an observable exam model keyed by ToothID + named site;
- editable PD/CAL/QDento-display-GM rows;
- editable attached gingiva where applicable;
- derived/read-only recession;
- live contour updates;
- focused simulator/UI evidence.

It does NOT implement:
- BOP interaction;
- FMPS/FMBS interaction;
- mobility/furcation/tooth-state interaction;
- persistence;
- summary/risk provider;
- save/reopen.

Those are later Plan 04 tasks.

## REQUIRED WORKFLOW

Before implementation:

1. Invoke Superpowers `using-superpowers`.
2. Invoke `using-git-worktrees`.
3. Invoke `subagent-driven-development`.
4. Use `verification-before-completion`.
5. Use Build iOS Apps.
6. Read/use:
   - `swiftui-ui-patterns`
   - `swiftui-view-refactor`
   - `ios-debugger-agent`
   - `ios-simulator-browser`
7. Use XcodeBuildMCP/current simulator workflow.

STRICT RED-FIRST evidence is required again.
Do not repeat Task 6's missing-RED process deviation.

## MODEL POLICY

ALL sub-agents MUST use GPT-6 Luna only.
Never use GPT-6 Sol for a sub-agent.
No nested sub-agents.

Recommended fresh roles:
1. Measurement Editing Rules Implementer.
2. Editable Exam Model Implementer.
3. Measurement UI Implementer.
4. Frozen Transition Contract Reviewer — read-only.
5. Named-Site/Isolation Reviewer — read-only.
6. SwiftUI Interaction Reviewer — read-only.
7. Simulator Interaction Reviewer — read-only.
8. Final Code Quality Reviewer — read-only.

Keep shared-file writes serialized.

## TARGET REPOSITORY / BASE

Repository:
`Centaurioun/periodontal-ios`

Exact frozen Plan 03 base:
`394bf5133cb452729baa37e55e62db473a7d02b6`

Frozen branch:
`freeze/p03-static-chart-v1`

Create a NEW isolated worktree/branch:

`interaction/p04-t1-measurement-editing`

Do NOT modify the frozen branch.
Do NOT merge.
Do NOT open a PR.
Do NOT begin Task 2.

## AUTHORITATIVE INPUTS

Read:
- `docs/superpowers/plans/2026-09-30-04-interaction-persistence-summary.md`;
- `docs/periodontal-ios/contracts/2026-10-01-rendering-parity-contract-v1.1.md`;
- `docs/superpowers/reviews/2026-09-30-plan01-parallel-batch-review.md`;
- `docs/superpowers/reviews/2026-10-01-p03-plan-acceptance.md`;
- accepted iOS Domain/Parity/Geometry/static-chart source at the frozen HEAD.

The editing transition authority is contract §5 plus the Plan 01 coordinator ruling.

# PART A — PURE MEASUREMENT EDITING RULES

Create:
`PeriodontalIOS/Behavior/MeasurementEditingRules.swift`

Create tests:
`PeriodontalIOSTests/Behavior/MeasurementEditingRulesTests.swift`

Keep rules SwiftUI-free.

## Canonical signs

Domain:
`clinicalGM = CAL - PD`

QDento UI display:
`displayGM = PD - CAL = -clinicalGM`

Use the accepted QDentoDisplayAdapter at UI/display boundaries where appropriate.

Do not store QDento display-GM as domain truth.

## Direct PD edit

For target SiteMeasurement:

Input:
`Int?`

Rules:
- nil = mark PD unassessed;
- nonnil accepted range = 0...19;
- update PD only;
- keep CAL value/source unchanged;
- if new PD and CAL are both assessed:
  - recompute clinical GM = CAL - PD;
- otherwise clinical GM becomes nil;
- preserve BOP exactly.

Direct PD edit MUST NOT change CAL.

## Direct CAL edit

Input:
`Int?`

Rules:
- nil:
  - CAL value=nil;
  - CAL source=nil;
  - clinical GM=nil;
- nonnil accepted range = 0...19;
- set CAL to the entered value;
- set CAL source = `.manual`;
- keep PD unchanged;
- if PD is assessed:
  - clinical GM = CAL - PD;
- otherwise clinical GM=nil;
- preserve BOP.

Direct CAL edit MUST NOT change PD.

## Direct QDento-display-GM edit

Input:
`Int?`

Rules:
- keep PD unchanged;
- nil:
  - clinical GM=nil;
  - CAL value/source=nil;
- nonnil requires assessed PD `p`;
- allowed display-GM integer range:
  `p - 19 ... p`;
- if outside range: return a typed validation error; do NOT silently mutate PD;
- compute:
  `CAL = p - displayGM`;
  `clinicalGM = -displayGM`;
- CAL source = `.derived`;
- preserve BOP.

Examples:
- PD4, displayGM +1 → CAL3, clinicalGM -1;
- PD4, displayGM -2 → CAL6, clinicalGM +2;
- with PD4 valid displayGM range is -15...4;
- -16 and +5 must fail without changing the source measurement.

This is the frozen NEW_PRODUCT_RULE.
Do NOT reproduce QDento's pathological legacy branch that mutates PD.

## Validation

Use typed errors/results.
Do not use `fatalError`/`precondition` for user input.

PD/CAL nonnil allowed:
0...19.

Do not add hidden clamping in the pure behavior rules.

The UI may constrain input, but invalid direct calls must still be safely rejected.

## Immutability

Rules return a new SiteMeasurement.
They must not mutate neighboring sites or findings.

# PART B — ATTACHED GINGIVA EDITING

Add AG behavior either in MeasurementEditingRules or a small focused behavior value in the same Behavior file unless a separate file materially improves clarity.

Input:
`Int?`

Nonnil accepted range:
0...9.

Rules:
- facial AG applicable for maxillary and mandibular natural teeth;
- oral AG applicable only for mandibular natural teeth;
- maxillary oral/palatal AG is NOT APPLICABLE and must reject edit attempts;
- missing/implant positions cannot receive natural-tooth AG edits;
- AG edit does not change PD/CAL/GM/BOP;
- recession remains derived from site clinical GM and therefore does not change solely because AG changed.

# PART C — EDITABLE EXAM MODEL

Create:
`PeriodontalIOS/Features/PeriodontalChart/PeriodontalExamModel.swift`

Use:
`@MainActor @Observable final class PeriodontalExamModel`

It owns:
`private(set) var exam: PeriodontalExam`

Required API equivalent to:

- reset(to exam: PeriodontalExam)
- editPD(toothID:site:value:)
- editCAL(toothID:site:value:)
- editDisplayGM(toothID:site:value:)
- editAttachedGingiva(toothID:surface:value:)

Exact names may vary.

Every edit is keyed by:
- ToothID;
- PeriodontalSite or PeriodontalSurface.

Never expose/accept QDento packed indexes.

## Reconstruction invariants

Because accepted Domain structs are immutable:
- reconstruct only the target SiteMeasurement/NaturalToothRecord/ChartPositionRecord;
- rebuild PeriodontalExam with SAME exam ID;
- preserve SAME examinedAt;
- preserve positions array order;
- preserve every non-target position byte-for-semantic-value;
- preserve BOP, FMPS/FMBS, mobility, furcation, tooth occupancy;
- on any validation failure, published exam remains unchanged.

Wrong target:
- missing or implant where natural edit requested → typed model error;
- unknown ToothID → typed model error.

Do NOT modify accepted Domain types to make them mutable.

# PART D — APP/FIXTURE MODEL WIRING

Modify AppModel minimally so it owns one editable `PeriodontalExamModel` initialized from the selected fixture's `expectedExam`.

On fixture selection change:
- reset the editable exam to the newly selected fixture expectedExam;
- preserve selected arch unless product behavior requires otherwise;
- do not mutate the immutable fixture catalog.

Current available fixtures F01/F02/F06/F07 may remain.
F04/F05 do not need to be exposed in the picker merely to test behavior; use them in unit tests if useful.

The static read-only Task 6 baseline must become live from `examModel.exam`.

# PART E — MEASUREMENT ROW VIEW

Create:
`PeriodontalIOS/Features/PeriodontalChart/MeasurementRowView.swift`

This owns compact READ + EDIT presentation for one measurement kind:
- PD;
- CAL;
- QDento-display GM.

It must use:
- accepted visible tooth order;
- accepted visible site order;
- the editable exam model.

Do NOT put transition formulas in the view.

## Touch strategy

Keep the compact 70/3 visual cells.

Do NOT enlarge hit targets so they overlap neighboring site cells.

A tap may open a native editor surface (alert/sheet/popover) with a larger numeric entry control.

The numeric-entry presentation is replaceable; the behavior rules are not.

The editor must clearly identify:
- measurement kind;
- ToothID;
- site.

Support signed values for display-GM.

Provide Cancel and Apply/Save semantics.

Invalid input:
- show/reject safely;
- must not partially update the model.

## Accessibility

Retain the existing stable IDs:
- `pd-<FDI>-<SITE>`
- `cal-<FDI>-<SITE>`
- `gm-<FDI>-<SITE>`

Add stable editor IDs, e.g.:
- `measurement-editor`
- `measurement-editor-field`
- `measurement-editor-apply`
- `measurement-editor-cancel`

Accessibility label must continue to expose current displayed value.

# PART F — SURFACE SUPPLEMENT ROW VIEW

Create:
`PeriodontalIOS/Features/PeriodontalChart/SurfaceSupplementRowView.swift`

Use it to present/edit:
- attached gingiva;
- recession.

AG:
- editable where applicable;
- same existing AG identifiers;
- tap opens the same/similar bounded numeric editor;
- 0...9 or unassessed.

Recession:
- derived/read-only;
- never directly editable;
- use `NaturalToothRecord.recession(on:)`;
- keep threshold >=1 red.

Maxillary oral AG:
- remains disabled/not-applicable `—`;
- no edit action;
- accessibility must indicate not applicable.

# PART G — LIVE CHART INTEGRATION

Modify the existing chart shell minimally.

Allowed integration files:
- AppModel.swift
- PeriodontalChartScreen.swift
- ArchChartView.swift
- ToothChartColumn.swift only if required to factor read-only cell drawing
- PeriodontalContourView.swift only for accessibility/update evidence, not geometry
- FullMouthWedgeView/BOPMarkerView only if their exam input needs a trivial live-model adapter; do not add their interaction yet.

Replace read-only measurement/supplement rows with the new editable views.

The chart must recompute from `examModel.exam` after an edit.

Contours must still consume accepted ContourGeometry.
Do not duplicate geometry.

BOP and wedge views remain READ-ONLY in Task 1.

# PART H — CONTOUR UPDATE ACCESSIBILITY

The Task 1 UI test must be able to prove a measurement edit changed the corresponding chart state.

Add a concise deterministic accessibility value to each PeriodontalContourView if needed.

Preferred:
- a stable signature derived from the current contour path y values;
- sufficient for the UI test to assert before != after.

Do NOT expose packed QDento indexes.
Do NOT move geometry calculations into SwiftUI.

The visual contour must update live on the simulator.

# STRICT TDD / RED PHASES

## RED A — direct edit rules

Write failing tests BEFORE implementation for:
- PD edit keeps CAL;
- CAL edit keeps PD;
- display-GM edit keeps PD and derives CAL;
- positive and negative display GM;
- dynamic GM range boundaries;
- invalid GM leaves source unchanged;
- PD/CAL 0 and 19 accepted;
- -1/20 rejected;
- BOP preserved;
- nil semantics.

Use F04 or equivalent deterministic values where useful.

## RED B — target isolation/model

Write failing tests for PeriodontalExamModel:
- edit FDI11/B only;
- adjacent FDI11/DB unchanged;
- other teeth unchanged;
- exam ID/date unchanged;
- position order unchanged;
- invalid edit publishes no partial state;
- missing/implant natural edit rejected.

## RED C — attached gingiva

Tests:
- upper facial edit accepted;
- upper oral rejected as not applicable;
- lower facial accepted;
- lower oral accepted;
- 0/9 accepted;
- invalid bounds rejected;
- AG edit leaves recession/site measurements unchanged.

Use F05 where useful.

## RED D — UI flow

Create:
`PeriodontalIOSUITests/MeasurementEditingFlowTests.swift`

The UI flow must, at minimum:
1. launch F01/Upper;
2. target FDI11 B;
3. record adjacent FDI11 DB value;
4. record facial contour accessibility signature;
5. edit PD 0→5;
6. assert:
   - PD B =5;
   - CAL B remains0;
   - displayed GM B becomes5;
   - adjacent DB remains0;
   - contour signature changed;
7. edit CAL B 0→3;
8. assert:
   - PD remains5;
   - CAL becomes3;
   - display GM becomes2;
9. edit display GM B to -1;
10. assert:
   - PD remains5;
   - CAL becomes6;
   - display GM=-1;
11. edit FDI11 facial AG to a valid value;
12. assert AG changes;
13. assert recession did not change merely because AG changed;
14. confirm existing F06/F07 read-only BOP/wedge fixture switching still works.

You may split this into more than one UI-test method if runtime reliability is better.

# SIMULATOR ACCEPTANCE

Mandatory.

Use iPhone 18 Pro / iOS 27.0.

In landscape:
- edit an upper FDI11 site;
- observe numeric value changes;
- observe the contour move live;
- verify neighboring site does not visually/value-wise change;
- edit CAL directly;
- edit a signed GM value;
- edit facial AG;
- confirm recession remains derived/read-only.

Also smoke-check portrait.

Capture evidence:
`docs/superpowers/evidence/p04-t1-editing/F01-upper-pd-cal-gm-edit-landscape.png`

Prefer a frame showing:
- FDI11 header;
- edited numeric rows;
- moved contour in the same visible region.

Create evidence note:
`docs/superpowers/evidence/2026-10-01-p04-t1-measurement-editing-review.md`

Record:
- exact implementation HEAD;
- simulator/runtime;
- edits performed;
- before/after values;
- direct PD/CAL/GM rule outcomes;
- contour live-update observation;
- AG/recession observation;
- any interaction defect/fix;
- statement that BOP/wedge interaction remains deferred.

# ALLOWED NEW FILES

Create:
- `PeriodontalIOS/Behavior/MeasurementEditingRules.swift`
- `PeriodontalIOS/Features/PeriodontalChart/PeriodontalExamModel.swift`
- `PeriodontalIOS/Features/PeriodontalChart/MeasurementRowView.swift`
- `PeriodontalIOS/Features/PeriodontalChart/SurfaceSupplementRowView.swift`
- `PeriodontalIOSTests/Behavior/MeasurementEditingRulesTests.swift`
- `PeriodontalIOSUITests/MeasurementEditingFlowTests.swift`
- Task 1 evidence files.

Modify only the integration files explicitly described above plus Xcode registration.

Do NOT modify accepted Domain/Parity/Geometry/Rendering/provider/prototype raster files.
If a lower-layer change appears necessary, STOP and report.

# REVIEW FOCUS

Independent reviewers must challenge:
1. Direct PD never changes CAL.
2. Direct CAL never changes PD.
3. Display-GM edit never mutates PD.
4. GM sign converts exactly once.
5. Invalid GM cannot leave partial state.
6. Only target ToothID+site changes.
7. BOP is preserved through measurement edits.
8. CAL source semantics are correct: manual for direct CAL, derived for direct GM.
9. nil remains distinct from numeric zero.
10. upper oral AG is still not applicable.
11. recession remains derived/read-only.
12. contours still delegate to accepted geometry.
13. compact cell hit areas do not overlap.
14. Task 6 BOP/wedges remain static/read-only.
15. fixture reset replaces the editable exam deterministically.

# SIX-CYCLE REFINEMENT

Exactly six cumulative cycles:
1. Accuracy & Fundamental Correction
2. Completeness & Gap Analysis
3. Structure & Architecture
4. Adversarial / Critical Review
5. Usability & Goal Fit
6. Final Synthesis, Regression Check & Polish

Cycle 4 must include invalid-input and target-isolation adversarial testing.
Cycle 5 must include real simulator editing of PD, CAL, signed GM, AG.

No seventh cycle.

# VERIFICATION

Run:
1. focused MeasurementEditingRulesTests;
2. any focused exam-model tests added;
3. MeasurementEditingFlowTests;
4. existing Task 5/6 UI flows;
5. all Rendering tests;
6. all Geometry tests;
7. Domain + Parity regression;
8. full unit suite;
9. full UI suite;
10. app build;
11. real simulator editing/capture;
12. `git diff --check`.

# COMMIT

Commit:
`feat: add periodontal measurement editing`

Push:
`interaction/p04-t1-measurement-editing`

Do NOT merge.
Do NOT open a PR.
Do NOT begin Plan 04 Task 2.

# STOP CONDITIONS

STOP rather than improvising if:
- frozen Plan 03 base cannot be checked out exactly;
- implementing the rules appears to require changing accepted Domain semantics;
- live contour updates cannot be achieved without changing accepted Geometry;
- edit isolation cannot be guaranteed;
- simulator interaction cannot be exercised;
- existing Task 5/6 regression cannot remain green.

# RETURN FORMAT

Return only:
- STATUS: DONE / DONE_WITH_CONCERNS / BLOCKED
- repository
- local worktree path
- feature branch
- base commit
- final HEAD
- files changed
- Build iOS Apps skills used
- simulator/tooling use summary
- sub-agent roles/models used
- RED evidence summary
- direct PD verification
- direct CAL verification
- direct display-GM verification
- validation/range verification
- nil semantics verification
- target-isolation verification
- CAL source verification
- attached-gingiva verification
- recession read-only verification
- live contour-update verification
- fixture-reset verification
- accessibility/UI-test result
- Task 5/6 regression result
- screenshot/evidence paths
- focused behavior-test result
- Geometry/Rendering/Domain/Parity regression result
- full unit-test result
- full UI-test result
- build result
- independent review verdict(s)
- visual/interaction findings and remediations
- concerns/blockers
- verification summary
- controller rulings, if any
