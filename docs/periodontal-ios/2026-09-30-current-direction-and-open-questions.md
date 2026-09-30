# Current Direction and Open Questions for the QDento-Inspired iOS Periodontal App

Date: 2026-09-30

Status: Current brainstorming synthesis.

This document is intentionally shorter and more operational than the decision log. It describes what we currently believe, what evidence supports it, and what remains open before a real implementation plan is written.

## A. Current direction

### Product
Build a private native iPhone demo first.

Primary use case:
student-friendly periodontal charting with immediate visual feedback.

Primary success criterion:
a student changes measurements and can immediately understand the corresponding periodontal visual change around the correct tooth/site.

### Visual authority
QDento.

Preserve:
- chart information hierarchy;
- tooth-centered presentation;
- moving gingival/CAL contours;
- BOP marker behavior;
- compact FMPS/FMBS visual controls;
- mobility/furcation presentation;
- risk/statistics panel concept.

### Domain authority
Clinica, especially the accepted R2 clinical lineage, with a fresh authority check before implementation.

Use explicit named sites:
MB, B, DB, ML, L, DL.

Keep natural teeth and implants separate.

Use explicit null/not-assessed semantics.

Use explicit mobility and furcation records.

### Rendering authority
QDento behavior, not Clinica's current SVG geometry.

Clinica's current renderer is useful as a source example but does not match the desired QDento visual result.

### Native architecture
New Swift/SwiftUI implementation.

Do not recreate Qt framework architecture.

Keep:
- domain;
- behavior;
- geometry;
- SwiftUI views;
- persistence;
- clinical interpretation

as separable responsibilities.

## B. Important translation rules

### Six sites
Canonical domain order:
MB, B, DB, ML, L, DL.

Facial:
MB, B, DB.

Oral:
ML, L, DL.

### Gingival margin sign convention
Clinica:
CAL = PD + GM.
Positive GM = recession/apical to CEJ.

QDento source behavior:
GM = PD - CAL.

Therefore a QDento-style visual adapter may need to negate Clinica GM before applying QDento-like geometry.

This must be runtime-verified.

### Contours
QDento source analysis indicates:
- baseline about 105;
- 3 display units per mm;
- different GM/CAL y-direction formulas;
- surrounding Qt transforms by arch/surface.

Do not freeze final iOS geometry until runtime screenshots confirm the net visible result.

## C. FMPS / FMBS current decision

Keep the QDento compact four-part per-tooth visual control for the parity experiment.

Reasons:
- compact;
- understandable;
- touch-friendly;
- immediate feedback;
- useful for students;
- updates summary indices visibly.

Do not assume the four QDento positions are identical to Clinica's six site findings.

The exact wedge-to-anatomy mapping is still an evidence question.

Do not label it incorrectly merely to finish the model.

## D. BOP current decision

Preserve a direct visual BOP marker such as the QDento blood-drop presentation.

The exact artwork can change later, but the state should be visible at the correct site and update immediately.

## E. Mobility and furcation

Prefer Clinica domain semantics.

Do not reproduce QDento's mobility persistence disconnect.

Use the QDento presentation only as a visual reference.

## F. Risk and clinical interpretation

Keep the QDento risk/statistics presentation as a reference.

Do not treat QDento's PerioStatistic implementation as final clinical authority.

Use a replaceable calculation boundary.

Later calculation authority should come from the accepted Clinica clinical lineage plus any explicitly reviewed literature updates.

## G. Remaining evidence before implementation planning

The remaining research should be a narrow QDento runtime parity closure.

Required evidence:

1. One-to-one site-to-visible-position mapping for MB/B/DB and ML/L/DL.
2. Net upper/lower and facial/oral contour transforms.
3. Four FMPS/FMBS wedge mapping and toggle order.
4. BOP marker placement.
5. Deterministic screenshots using sentinel values.
6. Reference screenshots for at least:
   - healthy baseline;
   - asymmetric three-site contour;
   - recession case;
   - BOP case;
   - FMPS/FMBS selected-sector case;
   - missing/implant state.

This should be treated as a visual/rendering closure, not a second full QDento analysis.

## H. What no longer blocks the first domain design

The following earlier blockers can be retired for the new app:

- six-site clinical names: Clinica explicitly defines them;
- anonymous QDento six-slot storage: do not use it as the new canonical domain;
- QDento mobility serialization gap: do not reproduce it;
- uncertainty about which clinical engine to prefer: use the accepted Clinica clinical lineage rather than QDento risk/diagnostic code;
- QDento desktop shell dependencies: irrelevant to native iOS architecture.

## I. What remains intentionally undecided

These decisions should wait for evidence or a later phase:

- exact final FMPS/FMBS clinical data representation;
- whether the parity demo exposes both QDento-style FMPS/FMBS and six-site plaque simultaneously;
- final portrait versus landscape interaction strategy;
- whether selected-tooth enlargement or popovers are needed;
- final commercial asset replacement;
- final risk-assessment labels and formulas;
- exact persistence technology;
- final iOS deployment target;
- Android architecture.

## J. Xcode / Codex working model

Recommended workflow:

Codex app / CLI:
- main orchestration;
- repository-wide changes;
- multi-agent work;
- cross-repo analysis;
- review and verification.

Xcode:
- source of truth for build/runtime;
- simulator;
- previews;
- compiler and Instruments.

Codex in Xcode:
- focused local edits and debugging.

All operate on the same future iOS Git repository.

## K. Build iOS Apps skill usage

From the beginning:
- swiftui-ui-patterns;
- swiftui-view-refactor.

After an Xcode project and working screen exist:
- ios-debugger-agent;
- ios-simulator-browser.

After correctness:
- swiftui-performance-audit;
- ios-ettrace-performance when evidence calls for it;
- ios-memgraph-leaks when evidence calls for it.

Defer:
- App Intents;
- Liquid Glass.

## L. Stop condition for brainstorming

Do not start implementation merely because this document exists.

The next major gate is:
- finish the narrow QDento runtime visual parity closure;
- update/freeze the portable contract;
- review the written design specification;
- only then invoke Superpowers writing-plans.

Until that happens, this branch remains a research/brainstorming branch.
