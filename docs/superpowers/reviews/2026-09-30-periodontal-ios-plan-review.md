# Periodontal iOS Plan Review — Second Independent Pass

Date: 2026-09-30

Status: Plan review complete; revised plan set requires user review before execution.

Basis:
- approved architectural spec;
- current QDento source;
- Clinica R2 domain/clinical reference;
- Superpowers brainstorming + writing-plans + verification-before-completion principles;
- Improved Six-Cycle Iterative Refinement.

## Executive result

The five-plan decomposition remains sound. No architectural reversal was required.

However, the review found three material omissions and several planning refinements:

1. PD, CAL, and GM are all directly editable in QDento; the initial plan did not fully preserve CAL edit parity.
2. Attached gingiva and derived recession are visible/persisted-or-derived parts of the selected QDento workspace and were missing from the first implementation plan.
3. The central tooth visual system had no dedicated implementation/provenance task even though it is central to the educational value.

Additional refinements:
- landscape-first is now a validation preference rather than a permanent orientation lock;
- final review fan-out is seven independent reviewers;
- a clinical-expert question register is created for intentionally deferred questions such as FMPS/FMBS wedge anatomy;
- prototype asset provenance is a first-demo acceptance concern even though production asset replacement remains later.

## Source-backed corrections

### PD/CAL/GM editability

QDento `PerioView` connects all 192 controls for each field:
- PD → `PerioPresenter::pdChanged`
- CAL → `PerioPresenter::calChanged`
- GM → `PerioPresenter::gmChanged`

The edit paths are not equivalent:
- PD edit changes PD and refreshes the relationship;
- CAL edit changes CAL and refreshes the relationship;
- GM edit may recalculate CAL and contains legacy constraint logic.

Therefore the iOS implementation must not reduce the reference behavior to “PD + GM inputs with CAL display only” without an explicit product decision.

### Attached gingiva / recession

QDento has:
- persisted `ag[64]`;
- two surface-level attached-gingiva controls per tooth position;
- read-only recession controls;
- recession derived from the maximum positive `CAL - PD` relationship across the three sites of the surface.

The iOS model should persist attached gingiva but derive recession.

### Tooth visuals

QDento's selected periodontal view depends on the generic tooth-rendering stack:
- `ToothPainter`;
- `SpriteSheets`;
- tooth/perio/common resources;
- tooth-type widths/index mapping;
- implant/missing visual states.

The new iOS app should isolate this behind a replaceable visual provider so prototype QDento assets can later be replaced without rewriting the domain or contour engine.

## Six-cycle review

### Cycle 1 — Accuracy & Fundamental Correction
Re-read source for measurement edit handlers, AG/recession, and tooth rendering. Corrected plan assumptions that were too Clinica-centric for first-demo parity.

### Cycle 2 — Completeness & Gap Analysis
Added evidence and implementation tasks for edit transitions, surface supplements, tooth visuals, provenance, and expert-review questions.

### Cycle 3 — Structure & Architecture
Kept all new behavior in existing boundaries:
- edit transitions → Behavior;
- attached gingiva → domain surface supplement;
- recession → derived behavior;
- tooth visuals → Rendering provider;
- provenance → documentation/acceptance.

No unrelated subsystem was added.

### Cycle 4 — Adversarial / Critical Review
Tested whether the app could “look mostly right” while:
- CAL editing was impossible;
- recession/attached gingiva rows were missing;
- tooth graphics were hard-wired to GPL raster filenames;
- landscape became an accidental permanent constraint.

Plans were changed to prevent those failure modes.

### Cycle 5 — Usability & Goal Fit
Preserved the student's visible tooth/contour relationship, compact findings, and direct numeric editing. Deferred the exact optimized touch-entry mechanism until an actual iPhone build can be tested.

### Cycle 6 — Final Synthesis / Regression Check
Rechecked the revised plans against the approved design:
- QDento remains visual/interaction authority.
- Clinica remains structured-domain/clinical authority.
- No final Stage/Grade/risk engine moved into the parity milestone.
- New scope is limited to functionality already present in the selected QDento periodontal workspace.
- Deferred wedge anatomy remains nonblocking for the neutral private demo.

## Recommendation

Use the revised plan set.

Before executing Plan 01, the user should review the revised roadmap once more because the plan now explicitly restores PD/CAL/GM edit parity, attached gingiva/recession, and the tooth visual/provenance layer.
